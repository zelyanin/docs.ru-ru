---
title: Рекомендации по архитектуре бессерверных приложений
description: Узнайте о проблемах проектирования бессерверных приложений, от управления состоянием и постоянного хранилища до масштабирования, ведения журнала, трассировки и диагностики.
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: c856683cf6910be98661e634246cd003b93a6d76
ms.sourcegitcommit: 4f4a32a5c16a75724920fa9627c59985c41e173c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72522424"
---
# <a name="serverless-architecture-considerations"></a>Рекомендации по бессерверной архитектуре

Внедрение бессерверной архитектуры поступает с определенными проблемами. В этом разделе рассматриваются некоторые из наиболее распространенных вопросов, о которых следует помнить. Все эти проблемы имеют решения. Как и в случае со всеми вариантами архитектуры, решение о переходе на бессерверные решения должно выполняться только после тщательного рассмотрения достоинств и недостатков. В зависимости от потребностей приложения вы можете решить, что бессерверная реализация не является верным решением для определенных компонентов.

## <a name="managing-state"></a>Управление состоянием

Бессерверные функции, как и в большинстве микрослужб, по умолчанию не имеют состояния. Предотвращение состояния позволяет бессерверно создавать временные, масштабируемые и обеспечивать устойчивость без центральной точки отказа. В некоторых случаях для бизнес-процессов требуется состояние. Если процесс требует состояния, у вас есть два варианта. Можно использовать модель, отличную от сервера, или взаимодействовать с отдельной службой, которая предоставляет состояние. Добавление состояния может усложнить решение и сделать его более трудным для масштабирования и, возможно, создать единую точку отказа. Тщательно продумайте, требуется ли для функции абсолютное состояние. Если вы ответили «да», определите, имеет ли он возможность реализовать его без сервера.

Существует несколько решений, которые можно принять, не нарушая преимущества сервера. Ниже перечислены некоторые из наиболее популярных решений.

- Использование временного хранилища данных или распределенного кэша, например Redis
- Сохранение состояния в базе данных, например SQL или CosmosDB
- Обработку состояния через механизм рабочих процессов, например устойчивые функции

В конце концов, необходимо учитывать потребность в любом управлении состоянием в процессах, которые вы собираетесь реализовать с бессерверными.

## <a name="long-running-processes"></a>Длительные процессы

Многие преимущества бессерверных процессов полагаются на временные процессы. Короткое время выполнения упрощает бессерверный поставщик для высвобождения ресурсов, так как функции завершаются и совместно используют функции на разных узлах. Большинство поставщиков облачных служб ограничивают общее время, в течение которого функция может выполняться около 10 минут. Если процесс может занять больше времени, вы можете рассмотреть альтернативную реализацию.

Существует несколько исключений и решений. Одним из решений может быть разбиение процесса на небольшие компоненты, выполнение которых по отдельности занимает меньше времени. Если процесс выполняется долго из-за зависимостей, можно также использовать асинхронный рабочий процесс, используя такое решение, как устойчивые функции. Устойчивые функции приостанавливают и сохраняют состояние процесса, пока он ожидает завершения внешнего процесса. Асинхронная обработка сокращает время выполнения фактического процесса.

## <a name="startup-time"></a>Время запуска

Одна из потенциальных проблем, связанная с бессерверными реализациями, — время запуска. Для экономии ресурсов многие поставщики, не имеющие сервера, создают инфраструктуру "по запросу". Когда функция без сервера активируется по истечении определенного периода времени, может потребоваться создать или перезапустить ресурсы для размещения функции. В некоторых ситуациях холодный запуск может привести к задержкам в несколько секунд. Время запуска зависит от поставщиков и уровней обслуживания. Существует несколько подходов к времени запуска, если важно, чтобы они были минимальны для успешного выполнения приложения.

- Некоторые поставщики позволяют пользователям платить за уровни обслуживания, гарантирующие, что инфраструктура всегда включена.
- Реализуйте механизм поддержания активности (проверьте связь с конечной точкой, чтобы она оставалась в спящем режиме).
- Используйте функцию оркестрации, например Kubernetes, с использованием контейнерной функции (узел уже работает, так что вращение новых экземпляров чрезвычайно быстрым).

## <a name="database-updates-and-migrations"></a>Обновления базы данных и миграция

Преимуществом бессерверного кода является возможность выпуска новых функций без необходимости повторного развертывания всего приложения. Это преимущество может стать недостатком, если есть связанная реляционная база данных. Изменения в схемах баз данных трудно синхронизировать с обновлениями без сервера. Дополнительные трудности возникают в случае неправильной передачи, поэтому необходимо выполнить откат изменений. Целостность данных — одна из причин, по которой в микрослужбах и бессерверных функциях рекомендуется использовать собственные данные. Изменения можно развернуть как единую единицу вычислений и данных. В реальности многие устаревшие системы имеют большую серверную базу данных, которая должна быть согласована с архитектурой без сервера.

Распространенный подход к решению управления версиями схемы — никогда не изменяет существующие свойства и столбцы, а вместо этого добавляет новые сведения. Например, рассмотрим переход с логического флага "завершено" для списка дел на "дату завершения". Вместо удаления старого поля изменение базы данных будет следующим:

1. Добавьте новое поле "Дата завершения".
1. Преобразование логического поля "завершено" в вычисляемую функцию, которая оценивает, является ли Дата завершения датой окончания текущей даты.
1. Добавьте триггер, чтобы присвоить дату завершения текущей дате, когда для выполненного логического значения задано значение true.

Последовательность изменений гарантирует, что устаревший код продолжит выполняться "как есть", а новые функции без сервера могут воспользоваться преимуществами нового поля.

Дополнительные сведения о данных в бессерверных архитектурах см. в разделе [проблемы и решения для управления распределенными данными](../microservices/architect-microservice-container-applications/distributed-data-management.md).

## <a name="scaling"></a>Масштабирование

Это распространенное заблуждение, что означает отсутствие сервера. На самом деле это «меньше сервер». Фактическая инфраструктура является резервной, чтобы понять, когда дело доходит до масштабирования. Большинство несерверных платформ предоставляют набор элементов управления, позволяющих выполнять масштабирование инфраструктуры при увеличении плотности событий. Вы можете выбрать один из различных вариантов, но в зависимости от функции ваша стратегия может отличаться. Более того, функции обычно выполняются под связанным узлом, поэтому функции на одном узле имеют одинаковые параметры масштабирования. Поэтому необходимо организовать и разработать, какие функции размещаются вместе на основе требований к масштабированию.

Правила часто определяют способ масштабирования (увеличение ресурсов узла) и горизонтальное масштабирование (увеличение количества экземпляров узлов) на основе различных параметров. Триггеры для масштабирования могут включать расписание, частоту запросов, загрузку ЦП и использование памяти. Более высокая производительность часто имеет более высокую стоимость. Менее дорогие подходы, основанные на потреблении потребления, могут не масштабироваться так быстро, когда скорость запроса внезапно увеличивается. Существует компромисс между оплатой "Страховая стоимость" по сравнению с оплатой по мере использования и риском медленных ответов по причине внезапного роста спроса.

## <a name="monitoring-tracing-and-logging"></a>Мониторинг, трассировка и ведение журнала

Часто пропорциями DevOps является мониторинг приложений после их развертывания. Важно иметь стратегию для мониторинга бессерверных функций. Крупнейший проблемой часто является корреляция или распознавание, когда пользователь вызывает несколько функций в рамках одного и того же взаимодействия. Большинство независимых от сервера платформ позволяют вести журнал консоли, который можно импортировать в сторонние средства. Существуют также параметры для автоматизации сбора данных телеметрии, создания и отслеживания идентификаторов корреляции и отслеживания определенных действий для получения подробных сведений. Azure предоставляет расширенную [платформу Application Insights](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) для мониторинга и анализа.

## <a name="inter-service-dependencies"></a>Зависимости между службами

Бессерверная архитектура может включать функции, зависящие от других функций. На самом деле, нередко в бессерверной архитектуре, чтобы несколько служб вызывали друг друга как часть взаимодействия или распределенной транзакции. Во избежание сильной взаимосвязи рекомендуется, чтобы службы не ссылались на друг друга напрямую. Если необходимо изменить конечную точку службы, прямые ссылки могут привести к значительному рефакторингу. Предлагаемое решение — предоставить механизм обнаружения служб, например реестр, который предоставляет соответствующую конечную точку для типа запроса. Другим решением является использование таких служб обмена сообщениями, как очереди или разделы, для обмена данными между службами.

## <a name="managing-failure-and-providing-resiliency"></a>Управление сбоем и обеспечение устойчивости

Также важно учитывать *шаблон автоматического деостановки*: Если по какой-то причине служба продолжит работу, не рекомендуется многократно вызывать эту службу. Вместо этого вызывается альтернативная служба или сообщение, возвращенное до тех пор, пока не будет восстановлена работоспособность зависимой службы. Бессерверная архитектура должна учитывать стратегию разрешения и управления зависимостями между службами.

Чтобы продолжить шаблон автоматического деостановки, службы должны быть отказоустойчивыми и устойчивыми. Отказоустойчивость означает возможность продолжения работы приложения даже после непредвиденных исключений или при обнаружении недопустимых состояний. Отказоустойчивость обычно является функцией самого кода и ее написанием для управления исключениями. Устойчивость — это способ восстановления приложения после сбоев. Устойчивость часто управляется бессерверной платформой. Платформа должна иметь возможность запуска нового экземпляра функции, бессерверной, в случае сбоя существующего. Платформа также должна быть достаточно интеллектуальной, чтобы при сбое каждого нового экземпляра останавливаются новые экземпляры.

Дополнительные сведения см. [в разделе Реализация шаблона](../microservices/implement-resilient-applications/implement-circuit-breaker-pattern.md)автоматического выключения.

## <a name="versioning-and-greenblue-deployments"></a>Управление версиями и зеленые/синие развертывания

Основным преимуществом бессерверных приложений является возможность обновления определенной функции без необходимости повторного развертывания всего приложения. Для успешного обновления функции должны иметь версию, чтобы службы, вызывающие их, находились в правильной версии кода. Также важна стратегия развертывания новых версий. Распространенным подходом является использование "зеленых/синих развертываний". Зеленым развертыванием является текущая функция. Новая версия "Blue" развертывается в рабочей среде и тестируется. При проведении тестирования зеленые и синие версии меняются, так что новая версия вступает в динамическую версию. При возникновении каких либо проблем они могут быть переключены обратно. Поддержка управления версиями и зеленым/синим развертывания требует сочетания функций для изменения версий и работы с бессерверной платформой для обработки развертываний. Одним из возможных подходов является использование прокси-серверов, которые описаны в разделе [несерверная Платформа Azure](azure-functions.md#proxies) .

>[!div class="step-by-step"]
>[Назад](serverless-architecture.md)
>[Вперед](serverless-design-examples.md)
