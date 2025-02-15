---
title: Подписка на события
description: Архитектура микрослужб .NET для контейнерных приложений .NET | Общие сведения о публикации событий интеграции и подписке на них.
ms.date: 10/02/2018
ms.openlocfilehash: 208b0f27aa1e6ceb6686e9e846b6e31d9f1c74df
ms.sourcegitcommit: ad800f019ac976cb669e635fb0ea49db740e6890
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73035636"
---
# <a name="subscribing-to-events"></a>Подписка на события

Первый шаг при использовании шины событий — подписать микрослужбы на события, которые они должны получать. Это нужно сделать для микрослужб-получателей.

В следующем простом коде видно, что должна реализовать каждая микрослужба-получатель при запуске службы (то есть в классе `Startup`), чтобы подписаться на нужные события. В этом случае микрослужба `basket.api` должна подписаться на сообщения `ProductPriceChangedIntegrationEvent` и `OrderStartedIntegrationEvent`.

Например, при подписке на событие `ProductPriceChangedIntegrationEvent` микрослужба корзины узнает об изменении цены товара и уведомляет пользователя об этом изменении, если товар находится в корзине пользователя.

```csharp
var eventBus = app.ApplicationServices.GetRequiredService<IEventBus>();

eventBus.Subscribe<ProductPriceChangedIntegrationEvent,
                   ProductPriceChangedIntegrationEventHandler>();

eventBus.Subscribe<OrderStartedIntegrationEvent,
                   OrderStartedIntegrationEventHandler>();

```

После выполнения этого кода микрослужба-подписчик будет ожидать передачи данных по каналам RabbitMQ. При поступлении сообщения типа ProductPriceChangedIntegrationEvent код вызывает обработчика событий, который передается ему и обрабатывает событие.

## <a name="publishing-events-through-the-event-bus"></a>Публикация событий через шину событий

Наконец, отправитель сообщения (исходная микрослужба) публикует события интеграции с кодом, как в приведенном ниже примере. (Это упрощенный пример, не учитывающий атомарность.) Применяйте аналогичный код в случае, когда событие необходимо распространить по нескольким микрослужбам, обычно сразу после фиксации данных или транзакций из исходной микрослужбы.

Сначала объект реализации шины событий (на основе RabbitMQ или служебной шины) будет внедрен в конструктор контроллера, как в следующем коде:

```csharp
[Route("api/v1/[controller]")]
public class CatalogController : ControllerBase
{
    private readonly CatalogContext _context;
    private readonly IOptionsSnapshot<Settings> _settings;
    private readonly IEventBus _eventBus;

    public CatalogController(CatalogContext context,
        IOptionsSnapshot<Settings> settings,
        IEventBus eventBus)
    {
        _context = context;
        _settings = settings;
        _eventBus = eventBus;
    }
    // ...
}
```

Затем вы используете его из методов контроллера, например UpdateProduct:

```csharp
[Route("items")]
[HttpPost]
public async Task<IActionResult> UpdateProduct([FromBody]CatalogItem product)
{
    var item = await _context.CatalogItems.SingleOrDefaultAsync(
        i => i.Id == product.Id);
    // ...
    if (item.Price != product.Price)
    {
        var oldPrice = item.Price;
        item.Price = product.Price;
        _context.CatalogItems.Update(item);
        var @event = new ProductPriceChangedIntegrationEvent(item.Id,
            item.Price,
            oldPrice);
        // Commit changes in original transaction
        await _context.SaveChangesAsync();
        // Publish integration event to the event bus
        // (RabbitMQ or a service bus underneath)
        _eventBus.Publish(@event);
        // ...
    }
    // ...
}
```

В этом случае, поскольку источником является простая микрослужба CRUD, этот код помещается прямо в контроллер веб-API.

В более сложных микрослужбах, например на основе подходов CQRS, это можно реализовать в классе `CommandHandler` в методе `Handle()`.

### <a name="designing-atomicity-and-resiliency-when-publishing-to-the-event-bus"></a>Проектирование атомарности и устойчивости при публикации в шине событий

При публикации событий интеграции с помощью распределенной системы обмена сообщениями, такой как шина событий, вы сталкиваетесь с проблемой атомарного обновления исходной базы данных и публикации события (таким образом, выполняются либо обе операции, либо ни одна из них). Например, в упрощенном примере, приведенном выше, код фиксирует данные в базе данных, когда меняется цена продукта, а затем публикует сообщение ProductPriceChangedIntegrationEvent. Сначала может показаться важным, чтобы обе операции выполнялись атомарно. Однако при использовании распределенных транзакций с базами данных и брокером сообщений, как в прежних системах, например [очередь сообщений (Майкрософт)](https://msdn.microsoft.com/library/windows/desktop/ms711472(v=vs.85).aspx), это не рекомендуется по причинам, описанным в [теореме CAP](https://www.quora.com/What-Is-CAP-Theorem-1).

По сути, вы используете микрослужбы для создания масштабируемых систем с высокой доступностью. В упрощенном виде теорема CAP гласит, что невозможно создать (распределенную) базу данных (или микрослужбу, владеющую своей моделью), которая была бы всегда доступной, строго согласованной *и* устойчивой к разделению. Можно выбрать только два свойства из трех.

В архитектурах на основе микрослужб рекомендуется выбирать доступность и устойчивость в ущерб строгой согласованности. Поэтому в большинстве современных приложений на базе микрослужб вы обычно не используете распределенные транзакции при обмене сообщениями, как вы это делаете при реализации [распределенных транзакций](https://docs.microsoft.com/previous-versions/windows/desktop/ms681205(v=vs.85)) на основе координатора распределенных транзакций Windows (DTC) с [MSMQ](https://msdn.microsoft.com/library/windows/desktop/ms711472(v=vs.85).aspx).

Давайте вернемся к начальной проблеме и примеру. Если служба аварийно завершает работу после обновления базы данных (в этом случае сразу после строки кода с \_context.SaveChangesAsync()), но до публикации события интеграции, вся система может утратить согласованность. Это может быть критически важным для бизнеса в зависимости от конкретной операции.

Как уже упоминалось в разделе об архитектуре, существует несколько подходов к решению этой проблемы:

- Использование полной модели [источников событий](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing).

- [Интеллектуальный анализ данных журнала транзакций](https://www.scoop.it/t/sql-server-transaction-log-mining).

- Использование [шаблона Outbox](https://www.kamilgrzybek.com/design/the-outbox-pattern/). Это таблица транзакций, в которой хранятся события интеграции (расширяющие локальную транзакцию).

В этом сценарии одним из лучших, если не *лучшим*, подходом будет использование полного шаблона источников событий. Однако часто вы не можете реализовать полную систему источников событий. Источник событий подразумевает хранение в базе данных о транзакциях только событий в предметной области, а не данных о текущем состоянии. Хранить только события в предметной области очень удобно. Например, у вас есть история системы, и вы можете определить состояние системы в любой момент в прошлом. Однако реализация полной системы источников событий требует перестройки большей части системы и приводит к другим сложностям и требованиям. В частности, придется использовать базу данных, созданную специально для источников событий, например [хранилище событий](https://eventstore.org/), или документоориентированную базу данных, например Azure Cosmos DB, MongoDB, Cassandra, CouchDB или RavenDB. Модель источников событий может стать отличным решением этой проблемы, но не самым простым, особенно если вы еще не знакомы с этой концепцией.

Вариант с анализом журнала транзакций, на первый взгляд, выглядит понятным. Но такой подход подразумевает соединение микрослужбы с журналом транзакций реляционной СУБД, например журналом транзакций SQL Server. Иногда это нежелательно. Еще один недостаток заключается в том, что обновления низкого уровня, записанные в журнал транзакций, могут не соответствовать событиям интеграции высокого уровня. Это может затруднять реконструирование операций в журнале транзакций.

В качестве компромисса можно использовать комбинацию таблицы базы данных о транзакциях и упрощенного шаблона источников событий. Например, вы можете использовать состояние "готово к публикации события", которое вы задаете в исходном событии при его фиксации в таблице событий интеграции. Затем вы пытаетесь опубликовать событие в шине события. Если действие публикации события выполняется успешно, вы запускаете еще одну транзакцию в исходной службе и изменяете состояние с "готово к публикации события" на "событие опубликовано".

Если действие публикации события в шине событий не выполняется, данные не утратят согласованность в исходной микрослужбе — они останутся в состоянии "готово к публикации события", и это будет согласовываться с остальными службами. Вы всегда можете запустить фоновые задания для проверки состояния транзакций или событий интеграции. Когда задание находит событие в состоянии "готово к публикации события", оно пытается повторно опубликовать событие в шине событий.

При таком подходе вы сохраняете только события интеграции для каждой исходной микрослужбы и только события, которые вы хотите передать другим микрослужбам или внешним системам. В отличие от полной системы источников событий, когда вы также храните все события в предметной области.

Такой сбалансированный подход — это упрощенная система источников событий. Вам нужен список событий интеграции с их текущим состоянием ("готово к публикации" или "опубликовано"). Но эти состояния нужно применять только к событиям интеграции. При таком подходе вам не нужно хранить все данные предметной области в виде событий в базе данных о транзакциях, как в полной системе источников событий.

Если вы уже используете реляционную базу данных, можете хранить события интеграции в таблице транзакций. Чтобы приложение было атомарным, используйте двухэтапный процесс на основе локальных транзакций. Фактически, таблица IntegrationEvent хранится в той же базе денных, что и сущности предметной области. Эта таблица используется для гарантии атомарности, чтобы вы включали хранимые события интеграции в те же транзакции, которые фиксируются в данных предметной области.

Пошаговый процесс выглядит следующим образом:

1. Приложение запускает транзакцию в локальной базе данных.

2. Затем оно обновляет состояние сущностей предметной области и вставляет событие в таблицу событий интеграции.

3. Наконец, оно фиксирует транзакцию. Вы получаете необходимую атомарность и

4. публикуете событие каким-либо образом (далее).

Для выполнения этапов публикации события у вас есть следующие варианты:

- Опубликуйте событие интеграции сразу после фиксации транзакции и используйте другую локальную транзакцию, чтобы отметить события в таблице как опубликованные. Затем используйте таблицу просто как объект для отслеживания событий интеграции в случае проблем в удаленных микрослужбах и выполняйте необходимые действия, основываясь на хранящихся событиях интеграции.

- Используйте таблицу как очередь. Отдельный поток или процесс приложения обращается к таблице событий интеграции, публикует события в шине событий, а затем выполняет местную транзакцию, чтобы отметить события как опубликованные.

На рисунке 6-22 показана архитектура для первого из этих подходов.

![Один подход к обработке атомарности при публикации событий заключается в том, чтобы использовать одну транзакцию для фиксации события в таблице журнала событий, а другую — для публикации (используется в eShopOnContainers)](./media/image23.png)

**Рис. 6-22**. Атомарность при публикации события в шине событий

В подходе, проиллюстрированном на рисунке 6-22, отсутствует дополнительная рабочая микрослужба, которая проверяет и подтверждает успешную публикацию событий интеграции. В случае сбоя эта дополнительная проверяющая микрослужба может считать события из таблицы и опубликовать их повторно, то есть повторить шаг 2.

При втором подходе вы используете таблицу EventLog в качестве очереди и всегда применяете рабочую микрослужбу для публикации сообщений. Подобный процесс показан на рисунке 6-23. Здесь изображена дополнительная микрослужба, и таблица является единственным источником при публикации событий.

![Другой подход к обработке атомарности: опубликовать в таблицу журнала событий и затем использовать другую микрослужбу (фоновый рабочий процесс) для публикации события.](./media/image24.png)

**Рис. 6-23**. Атомарность при публикации события в шине событий с рабочей микрослужбой

Для простоты в примере приложения eShopOnContainers используется первый подход (без дополнительных процессов или проверяющих микрослужб) и шина событий. Однако eShopOnContainers не обрабатывает все возможные случаи сбоев. В реальном приложении, развернутом в облаке, вы должны учитывать факт неизбежности сбоев и реализовывать эту логику проверки и повторной отправки. Таблица в качестве очереди эффективнее первого подхода, если эта таблица является единственным источником событий при их публикации (с помощью рабочего процесса) через шину событий.

### <a name="implementing-atomicity-when-publishing-integration-events-through-the-event-bus"></a>Реализация атомарности при публикации события интеграции через шину событий

В следующем коде показано, как можно создать одну транзакцию с несколькими объектами DbContext — один контекст связан с исходными обновляемыми данными, а второй — с таблицей IntegrationEventLog.

Обратите внимание, что транзакция в приведенном ниже примере кода не будет устойчивой, если при выполнении кода возникнут проблемы с подключением к базе данных. Это может произойти в облачных системах, таких как база данных SQL Azure, которые могут перемещать базы данных между серверами. Реализация устойчивых транзакций в нескольких контекстах описана в разделе [Реализация устойчивых SQL-соединений с платформой Entity Framework Core](../implement-resilient-applications/implement-resilient-entity-framework-core-sql-connections.md).

Чтобы было понятнее, в следующем примере показан весь процесс в одном фрагменте кода. Но в приложении eShopOnContainers для простоты выполнен рефакторинг и разделение этой логики на несколько классов.

```csharp
// Update Product from the Catalog microservice
//
public async Task<IActionResult> UpdateProduct([FromBody]CatalogItem productToUpdate)
{
  var catalogItem =
       await _catalogContext.CatalogItems.SingleOrDefaultAsync(i => i.Id ==
                                                               productToUpdate.Id);
  if (catalogItem == null) return NotFound();

  bool raiseProductPriceChangedEvent = false;
  IntegrationEvent priceChangedEvent = null;

  if (catalogItem.Price != productToUpdate.Price)
          raiseProductPriceChangedEvent = true;

  if (raiseProductPriceChangedEvent) // Create event if price has changed
  {
      var oldPrice = catalogItem.Price;
      priceChangedEvent = new ProductPriceChangedIntegrationEvent(catalogItem.Id,
                                                                  productToUpdate.Price,
                                                                  oldPrice);
  }
  // Update current product
  catalogItem = productToUpdate;

  // Just save the updated product if the Product's Price hasn't changed.
  if (!raiseProductPriceChangedEvent)
  {
      await _catalogContext.SaveChangesAsync();
  }
  else  // Publish to event bus only if product price changed
  {
        // Achieving atomicity between original DB and the IntegrationEventLog
        // with a local transaction
        using (var transaction = _catalogContext.Database.BeginTransaction())
        {
           _catalogContext.CatalogItems.Update(catalogItem);
           await _catalogContext.SaveChangesAsync();

           // Save to EventLog only if product price changed
           if(raiseProductPriceChangedEvent)
               await _integrationEventLogService.SaveEventAsync(priceChangedEvent);

           transaction.Commit();
        }

      // Publish the integration event through the event bus
      _eventBus.Publish(priceChangedEvent);

      integrationEventLogService.MarkEventAsPublishedAsync(
                                                priceChangedEvent);
  }

  return Ok();
}

```

После создания события интеграции ProductPriceChangedIntegrationEvent транзакция, хранящая операцию в исходной предметной области (обновление позиции каталога), также включает сохранение события в таблице EventLog. В результате получается одна транзакция, и вы всегда можете проверить, были ли отправлены сообщения о событии.

Таблица журнала событий обновляется атомарно операцией исходной базы данных с помощью локальной транзакции в этой же базе данных. Если происходит сбой операции, выдается исключение, и транзакция откатывает выполненную операцию, поддерживая согласованность между операциями предметной области и сообщениями о событии, сохраненными в таблице.

### <a name="receiving-messages-from-subscriptions-event-handlers-in-receiver-microservices"></a>Получение сообщений из подписок: обработчики событий в микрослужбах-получателях

Помимо логики подписки на события вам необходимо реализовать внутренний код для обработчиков событий интеграции (например, метод обратного вызова). В обработчике событий вы указываете, где будут получаться и обрабатываться сообщения о событиях определенного типа.

Сначала обработчик событий получает экземпляр события от шины событий. Затем он выполняет поиск компонента, нуждающегося в обработке и связанного с этим событием интеграции, распространяя и сохраняя событие как изменение состояния в микрослужбе-получателе. Например если событие ProductPriceChanged инициируется в микрослужбе каталога, оно обрабатывается в микрослужбе корзины и изменяет состояние в этой микрослужбе-получателе, как показано в следующем коде.

```csharp
namespace Microsoft.eShopOnContainers.Services.Basket.API.IntegrationEvents.EventHandling
{
    public class ProductPriceChangedIntegrationEventHandler :
        IIntegrationEventHandler<ProductPriceChangedIntegrationEvent>
    {
        private readonly IBasketRepository _repository;

        public ProductPriceChangedIntegrationEventHandler(
            IBasketRepository repository)
        {
            _repository = repository;
        }

        public async Task Handle(ProductPriceChangedIntegrationEvent @event)
        {
            var userIds = await _repository.GetUsers();
            foreach (var id in userIds)
            {
                var basket = await _repository.GetBasket(id);
                await UpdatePriceInBasketItems(@event.ProductId, @event.NewPrice, basket);
            }
        }

        private async Task UpdatePriceInBasketItems(int productId, decimal newPrice,
            CustomerBasket basket)
        {
            var itemsToUpdate = basket?.Items?.Where(x => int.Parse(x.ProductId) ==
                productId).ToList();
            if (itemsToUpdate != null)
            {
                foreach (var item in itemsToUpdate)
                {
                    if(item.UnitPrice != newPrice)
                    {
                        var originalPrice = item.UnitPrice;
                        item.UnitPrice = newPrice;
                        item.OldUnitPrice = originalPrice;
                    }
                }
                await _repository.UpdateBasket(basket);
            }
        }
    }
}
```

Обработчик событий должен проверить, существует ли товар в экземплярах корзины. Он также обновляет цену товара в каждой соответствующей позиции строки корзины. Наконец, он создает для пользователя предупреждение об изменении цены, как показано на рисунке 6-24.

![Представление браузера с уведомлением об изменении цены в корзине пользователя.](./media/image25.png)

**Рис. 6-24**. Отображение изменения цены товара в корзине на основе данных о событиях интеграции

## <a name="idempotency-in-update-message-events"></a>Идемпотентность в событиях сообщения об обновлении

Важный аспект событий сообщения об обновлении заключается в том, что при сбое взаимодействия сообщение должно отправляться повторно. В противном случае фоновая задача попытается опубликовать уже опубликованное событие, создавая состояние гонки. Убедитесь, что обновления либо являются идемпотентными, любо предоставляют достаточно информации, чтобы вы могли найти дублирующие данные, отменить их и отправить обратно только один ответ.

Как отмечалось ранее, идемпотентность означает, что операция может выполняться несколько раз без изменения результатов. В среде обмена сообщениями, например при передаче событий, событие является идемпотентным, если его можно доставить несколько раз без изменения результатов для микрослужбы-получателя. Это необходимо из-за особенностей самого события или из-за способа обработки событий в системе. Идемпотентность сообщений важна в любом приложении, где используется обмен сообщениями, а не только в приложениях с шаблоном шины событий.

Пример идемпотентной операции — инструкция SQL, которая вставляет данные в таблицу только в том случае, если этих данных еще нет в таблице. Сколько бы раз вы ни выполняли эту инструкцию SQL по вставке, результат не изменится — таблица будет содержать эти данные. Такая идемпотентность также может быть необходима при работе с сообщениями, если сообщения могут быть отправлены и, следовательно, обработаны несколько раз. Например, если по логике повтора отправитель несколько раз посылает одно и то же сообщение, необходимо обеспечить идемпотентность.

Вы можете создать идемпотентные сообщения. Например, можно создать событие, которое предписывает "установить цену товара 25 рублей" вместо "увеличить цену товара на 5 рублей". Первое сообщение можно безопасно обрабатывать сколько угодно раз, результат не изменится. Со вторым сообщением будет иначе. Но даже в первом случае можно отказаться от обработки первого события, поскольку система уже могла послать обновленное событие изменения цены, и тогда будет изменена уже новая цена.

Еще один пример — событие выполнения заказа, распространяемое нескольким подписчикам. Важно обновлять сведения о заказах в других системах однократно, даже если существуют дублирующие события сообщений для одного события выполнения заказа.

У события должен быть какой-то идентификатор, чтобы вы могли создать логику, по которой каждое событие обрабатывается получателем только один раз.

Иногда обработка сообщений является идемпотентной сама по себе. Например, если система создает эскизы изображений, неважно, сколько раз будет обработано сообщение о созданном эскизе, результат неизменен — эскизы созданы. С другой стороны, такие операции, как вызов шлюза оплаты для списания средств с кредитной карты, могут быть совсем не идемпотентными. В этих случаях необходимо гарантировать, что обработка сообщения несколько раз приведет к ожидаемым результатам.

### <a name="additional-resources"></a>Дополнительные ресурсы

- **Соблюдение идемпотентности сообщений**  
  <https://docs.microsoft.com/previous-versions/msp-n-p/jj591565(v=pandp.10)#honoring-message-idempotency>

## <a name="deduplicating-integration-event-messages"></a>Дедупликация сообщений о событиях интеграции

Вы можете гарантировать, что события сообщения будут отправлены или обработаны только один раз для подписчика, на разных уровнях. Например, можно использовать функцию дедупликации в вашей инфраструктуре обмена сообщениями. Или можно применить пользовательскую логику в микрослужбе назначения. Лучший вариант — проверки и на уровне транспортировки, и на уровне приложения.

### <a name="deduplicating-message-events-at-the-eventhandler-level"></a>Дедупликация событий сообщения на уровне EventHandler

Один из способов убедиться, что событие будет обработано получателями только один раз, — применить определенную логику при обработке событий сообщения в обработчиках событий. Например, такой подход используется в приложении eShopOnContainers, как видно в [исходном коде класса UserCheckoutAcceptedIntegrationEventHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/IntegrationEvents/EventHandling/UserCheckoutAcceptedIntegrationEventHandler.cs), который получает событие интеграции UserCheckoutAcceptedIntegrationEvent. (В данном случае мы заключаем CreateOrderCommand в оболочку IdentifiedCommand, используя eventMsg.RequestId в качестве идентификатора, прежде чем отправлять его в обработчик команд.)

### <a name="deduplicating-messages-when-using-rabbitmq"></a>Дедупликация сообщений при использовании RabbitMQ

В случае периодических сбоев в сети сообщения могут дублироваться, и получатель сообщений должен быть готов к обработке повторяющихся сообщений. По возможности получатели должны обрабатывать сообщения идемпотентно. Это лучше, чем явная обработка с дедупликацией.

Согласно [документации RabbitMQ](https://www.rabbitmq.com/reliability.html#consumer), если сообщение доставлено получателю, а затем повторно поставлено в очередь (например, оно не было подтверждено до разрыва соединения с получателем), RabbitMQ пометит его как доставляемое повторно, когда оно снова будет доставляться (этому же получателю или другому).

Если сообщение имеет метку "доставляется повторно", получатель должен учитывать это, поскольку он мог уже обработать это сообщение. Но это необязательно. Сообщение могло так и не дойти до получателя после отправки из брокера сообщений, например из-за проблем в сети. С другой стороны, если нет метки "доставляется повторно", сообщение гарантированно было отправлено только один раз. Таким образом, получатель должен дедуплицировать или обрабатывать сообщения идемпотентно только в том случае, если они имеют метку "доставляется повторно".

### <a name="additional-resources"></a>Дополнительные ресурсы

- **Ветвление eShopOnContainers с использованием NServiceBus (Particular Software)**  \
    <https://go.particular.net/eShopOnContainers>

- **Обмен сообщениями на основе событий** \
    <https://patterns.arcitura.com/soa-patterns/design_patterns/event_driven_messaging>

- **Джимми Богард (Jimmy Bogard). Рефакторинг для обеспечения отказоустойчивости: оценка взаимозависимости** \
    <https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/>

- **Канал публикации и подписки** \
    <https://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html>

- **Взаимодействие между ограниченными контекстами** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/jj591572(v=pandp.10)>

- **Итоговая согласованность** \
    [https://en.wikipedia.org/wiki/Eventual\_consistency](https://en.wikipedia.org/wiki/Eventual_consistency)

- **Филип Браун (Philip Brown). Стратегии интеграции ограниченных контекстов** \
    <https://www.culttt.com/2014/11/26/strategies-integrating-bounded-contexts/>

- **Крис Ричардсон (Chris Richardson). Разработка транзакционных микрослужб с помощью агрегатов, порождения событий и CQRS. Часть 2** \
    <https://www.infoq.com/articles/microservices-aggregates-events-cqrs-part-2-richardson>

- **Крис Ричардсон (Chris Richardson). Шаблон порождения событий** \
    <https://microservices.io/patterns/data/event-sourcing.html>

- **Знакомство с порождением событий** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/jj591559(v=pandp.10)>

- **База данных хранилища событий**. Официальный сайт \
    <https://geteventstore.com/>

- **Патрик Номменсен (Patrick Nommensen). Управление данными на основе событий для микрослужб** \
    <https://dzone.com/articles/event-driven-data-management-for-microservices-1>

- **Теорема CAP** \
    [https://en.wikipedia.org/wiki/CAP\_theorem](https://en.wikipedia.org/wiki/CAP_theorem)

- **Что такое теорема CAP?** \
    <https://www.quora.com/What-Is-CAP-Theorem-1>

- **Основные сведения о согласованности данных** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/dn589800(v=pandp.10)>

- **Рик Сейлинг (Rick Saling). Теорема CAP: почему в случае с облачными средами и Интернетом все иначе** \
    <https://blogs.msdn.microsoft.com/rickatmicrosoft/2013/01/03/the-cap-theorem-why-everything-is-different-with-the-cloud-and-internet/>

- **Эрик Брюер (Eric Brewer). CAP двенадцать лет спустя: Как изменились "правила"**  \
    <https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed>

- **Служебная шина Azure. Обмен сообщениями через брокер: обнаружение повторений**  \
    <https://code.msdn.microsoft.com/Brokered-Messaging-c0acea25>

- **Руководство по обеспечению надежности** (документация RabbitMQ) \
    [https://www.rabbitmq.com/reliability.html\#consumer](https://www.rabbitmq.com/reliability.html#consumer)

> [!div class="step-by-step"]
> [Назад](rabbitmq-event-bus-development-test-environment.md)
> [Вперед](test-aspnet-core-services-web-apps.md)
