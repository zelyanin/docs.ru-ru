---
title: Разработка с использованием ссылочных типов, допускающих значение null
description: В этом расширенном руководстве содержатся общие сведения о ссылочных типах, допускающих значение null. Вы научитесь выражать проектный замысел относительно того, когда ссылочные значения могут иметь значение null, и используете компилятор, который будет применять соответствующее поведение, когда они не могут иметь значение null.
ms.date: 02/19/2019
ms.technology: csharp-null-safety
ms.custom: mvc
ms.openlocfilehash: 9cb9ac1b292e61d6a8a5f84be29a6a6c323725fc
ms.sourcegitcommit: ad800f019ac976cb669e635fb0ea49db740e6890
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73039690"
---
# <a name="tutorial-migrate-existing-code-with-nullable-reference-types"></a>Учебник. Перенос существующего кода со ссылочными типами, допускающими значение null

В C# 8 вводятся **ссылочные типы, допускающие значение null**, которые дополняют ссылочные типы таким же образом, как типы значений, допускающие значение null, дополняют другие типы значений. Чтобы объявить переменную как имеющую **ссылочный тип, допускающий значение null**, добавьте `?` к типу. Например, `string?` соответствует типу `string`, допускающему значение null. Эти новые типы можно использовать для более четкого выражения проектного замысла: некоторые переменные *всегда должны содержать значение*, а в других *они могут отсутствовать*. Любые существующие переменные ссылочного типа будут интерпретироваться как не допускающие значение null. 

В этом руководстве вы узнаете, как:

> [!div class="checklist"]
>
> - включить проверку пустых ссылок при работе с кодом;
> - выполнить диагностику и устранение различных предупреждений, связанных со значениями null;
> - управлять интерфейсом между контекстами, допускающими значение null и не допускающими его;
> - контролировать контекст заметок, допускающих значение null.

## <a name="prerequisites"></a>Предварительные требования

Вам нужно настроить свой компьютер для выполнения .NET Core, включая компилятор C# 8.0. Компилятор C# 8 доступен, начиная с [версии 16.3 Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) или [в пакете SDK .NET Core 3.0](https://dotnet.microsoft.com/download).

В этом руководстве предполагается, что вы знакомы с C# и .NET, включая Visual Studio или .NET Core CLI.

## <a name="explore-the-sample-application"></a>Изучение примера приложения

Пример приложения, который вы будете переносить, представляет собой веб-приложение для чтения RSS-каналов. Оно читает один RSS-канал и отображает сводки самых последних статей. Вы можете щелкнуть любую статью, чтобы перейти на страницу сайта. Приложение относительно новое, но было написано до того, как ссылочные типы, допускающие значение null, стали доступными. Проектные решения для приложения представляют разумные принципы, но не используют преимущества этой важной функции языка.

Пример приложения содержит библиотеку модульных тестов, которые проверяют основную функциональность приложения. Этот проект облегчит безопасное обновление, если вы измените любую из реализаций на основе сгенерированных предупреждений. Скачать начальный код можно из репозитория GitHub [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/nullable-reference-migration/start).

Целью переноса проекта является использование новых возможностей языка, позволяющих четко выразить свои намерения относительно переменных, допускающих значение null, таким образом, чтобы компилятор не генерировал предупреждения в случае контекста заметок, допускающих значение null, и установки для контекста предупреждений о допустимости значения null значения `enabled`.

## <a name="upgrade-the-projects-to-c-8"></a>Переход проекта на C# 8

Сначала рекомендуется определить область задачи перехода. В первую очередь мы перейдем на использование C# 8.0 (или более поздней версии). Добавьте элемент `LangVersion` в два CSPROJ-файла: веб-проекта и проекта модульных тестов.

```xml
<LangVersion>8.0</LangVersion>
```

Версия языка обновляется до C# 8.0, но без включения контекста заметок, допускающих значение null, или контекста предупреждений о допустимости значения null. Перестройте проект так, чтобы сборка выполнялась без предупреждений.

Следующим шагом является включение контекста заметок, допускающих значение null, и просмотр количества генерируемых предупреждений. Добавьте следующий элемент прямо под элементом `LangVersion` в оба CSPROJ-файла решения:

```xml
<Nullable>enable</Nullable>
```

Выполните тестовую сборку и обратите внимание на список предупреждений. В этом небольшом приложении компилятор генерирует пять предупреждений, так что, скорее всего, вы оставите контекст заметок, допускающих значение null, включенным и начнете исправлять предупреждения для всего проекта.

Эта стратегия работает только для небольших проектов. Для крупных проектов из-за включения контекста заметок, допускающих значение null, для всей базы кода генерируется большое количество предупреждений. Это затрудняет систематическое исправление этих предупреждений. Для крупных корпоративных проектов часто нужно будет переносить один проект за раз. В каждом проекте переносите один класс или файл за раз.

## <a name="warnings-help-discover-original-design-intent"></a>Предупреждения помогают определить исходное намерение проекта

Существует два класса, которые генерируют предупреждения. Начнем с класса `NewsStoryViewModel`. Удалите элемент `Nullable` из обоих CSPROJ-файлов, чтобы ограничить область предупреждений до фрагментов кода, над которыми вы работаете. Откройте файл *NewsStoryViewModel.cs* и добавьте следующие директивы, чтобы включить контекст заметок, допускающих значение null, для класса `NewsStoryViewModel`, и восстановите класс в соответствии со следующим определением:

```csharp
#nullable enable
public class NewsStoryViewModel
{
    public DateTimeOffset Published { get; set; }
    public string Title { get; set; }
    public string Uri { get; set; }
}
#nullable restore
```

Эти две директивы помогут сфокусировать трудозатраты перехода. Предупреждения о допустимости значений null генерируются для той области кода, над которой вы активно работаете. Оставьте так до тех пор, пока не будете готовы включить предупреждения для всего проекта. Необходимо использовать значение `restore`, а не `disable`, чтобы случайно не отключить контекст, когда вы включите заметки, допускающие значение null, для всего проекта. После включения контекста заметок, допускающих значение null, для всего проекта вы можете удалить все прагмы-директивы `#nullable` из проекта.

Класс `NewsStoryViewModel` является объектом передачи данных (DTO), а двое из его свойств — строками чтения и записи:

[!code-csharp[InitialViewModel](~/samples/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/ViewModels/NewsStoryViewModel.cs#StarterViewModel)]

Эти два свойства вызывают ошибку `CS8618`: "Non-nullable property is uninitialized" (Отменена инициализация свойства, которое не является свойством необязательной определенности). Это очевидно: оба свойства `string` при создании объекта `NewsStoryViewModel` имеют значение по умолчанию `null`. Важно выяснить, как создаются объекты `NewsStoryViewModel`. Глядя на этот класс, невозможно определить то, является ли значение `null` частью решения разработки, или то, устанавливаются ли для этих объектов ненулевые значения во время их создания. Новости (newsStory) создаются в методе `GetNews` класса `NewsService`:

[!code-csharp[StarterCreateNewsItem](~/samples/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Services/NewsService.cs#CreateNewsItem)]

В предыдущем блоке кода довольно много важного. Это приложение использует пакет NuGet [AutoMapper](https://automapper.org/) для создания элемента новостей на основе элемента `ISyndicationItem`. Видно также, что в этом одном операторе и создаются элементы новостей, и задаются свойства. Это означает, что проектное решение класса `NewsStoryViewModel` указывает, что эти свойства никогда не должны иметь значение `null`. Эти свойства должны быть **ссылочного типа, не допускающего значение null**. Это лучше всего выражает исходное намерение проекта. Фактически, любой объект `NewsStoryViewModel` *правильно* создается с ненулевыми значениями. Следующий код инициализации является допустимым исправлением:

```csharp
public class NewsStoryViewModel
{
    public DateTimeOffset Published { get; set; }
    public string Title { get; set; } = default!;
    public string Uri { get; set; } = default!;
}
```

Присвоение строкам `Title` и `Uri` значения `default`, которое является `null` для типа `string`, не изменяет поведение программы во время выполнения. Объекты класса `NewsStoryViewModel` все еще создаются с нулевыми значениями, но теперь компилятор не выдает предупреждений. **Оператором, разрешающим значение null**, является символ `!`. В этом примере он стоит после выражения `default` и сообщает компилятору, что предыдущее выражение не является нулевым. Эта методика может быть целесообразной, если другие изменения вызывают гораздо большие изменения базы кода, но в этом приложении есть лучшее и относительно быстрое решение. Оно состоит в том, чтобы сделать класс `NewsStoryViewModel` неизменным типом, где все свойства устанавливаются в конструкторе. Внесите следующие изменения в класс `NewsStoryViewModel`:

[!code-csharp[FinishedViewModel](~/samples/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/ViewModels/NewsStoryViewModel.cs#FinishedViewModel)]

После этого нужно обновить код, который настраивает AutoMapper так, чтобы тот использовал конструктор, а не инициализировал свойства. Откройте файл `NewsService.cs` и найдите следующий код в нижней части:

[!code-csharp[StarterAutoMapper](~/samples/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Services/NewsService.cs#ConfigureAutoMapper)]

Этот код сопоставляет свойства объектов `ISyndicationItem` и `NewsStoryViewModel`. Нам нужно, чтобы AutoMapper предоставил сопоставление, используя вместо этого конструктор. Замените приведенный выше код на следующую конфигурацию AutoMapper:

[!code-csharp[FinishedViewModel](~/samples/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Services/NewsService.cs#ConfigureAutoMapper)]

Обратите внимание, так как этот класс небольшой и вы внимательно изучили его, необходимо включить директиву `#nullable enable` над объявлением класса. Изменение в конструкторе могло нарушить какой-то код, поэтому стоит выполнить все тесты и протестировать приложение, прежде чем двигаться дальше.

Первый набор изменений показал, как определить, что в исходном намерении для переменных не должно устанавливаться значение `null`. Это методика называется **правильным конструированием**. Вы объявляете, что объект и его свойства не могут иметь значение `null` при создании. Анализ потока компилятором гарантирует, что для этих свойств не будет задано значение `null` после создания. Обратите внимание, что конструктор вызывается внешним кодом, который **не обращает внимания на допустимость значений null**. Новый синтаксис не обеспечивает проверку во время выполнения. Внешний код может обойти анализ потока компилятором. 

В других случаях структура класса предоставляет различные подсказки к пониманию намерения. Откройте файл *Error.cshtml.cs* в папке *Pages*. Класс `ErrorViewModel` содержит следующий код:

[!code-csharp[StarterErrorModel](~/samples/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Pages/Error.cshtml.cs#StartErrorModel)]

Добавьте директиву `#nullable enable` перед объявлением класса и директиву `#nullable restore` после объявления. Вы получите одно предупреждение о том, что `RequestId` не инициализирован. Посмотрев на класс, вы должны решить, что в некоторых случаях свойство `RequestId` должно быть со значением null. Наличие свойства `ShowRequestId` указывает, что возможны пропущенные значения. Так как значение `null` является допустимым, добавьте `?` в тип `string`, чтобы указать, что свойство `RequestId` является *ссылочного типа, допускающего значение null*. Окончательное определение класса выглядит следующим образом:

[!code-csharp[FinishedErrorModel](~/samples/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Error.cshtml.cs#ErrorModel)]

Проверьте использование свойства. Вы увидите, что перед отображением свойства в исправлении оно проверяется на значение null на соответствующей странице. Это безопасное использование ссылочного типа, допускающего значение null, поэтому мы закончим работу с этим классом.

## <a name="fixing-nulls-causes-change"></a>Исправление значений null приводит к изменениям

Часто исправление одного набора предупреждений создает новые предупреждения в связанном коде. Давайте посмотрим на предупреждения в действии, исправив класс `index.cshtml.cs`. Откройте файл `index.cshtml.cs` и изучите код. В этом файле содержится код для страницы индексов:

[!code-csharp[StarterIndexModel](~/samples/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Pages/Index.cshtml.cs#IndexModelStart)]

Добавьте директиву `#nullable enable`, и вы увидите два предупреждения. Ни свойство `ErrorText`, ни свойство `NewsItems` не инициализированы. Изучив этот класс, вы придете к выводу, что оба свойства должны быть ссылочного типа, допускающего значение null: оба имеют частные методы задания. Один из этих методов назначается в методе `OnGet`. Прежде чем вносить изменения, посмотрите на объекты-получатели обоих свойств. На самой странице объект `ErrorText` проверяется на значение null перед созданием исправления ошибок. Коллекция `NewsItems` проверяется на значение `null`, а также на наличие в ней элементов. Чтобы быстро решить эту проблему, можно присвоить этим свойствам ссылочные типы, допускающие значение null. Но лучше было бы сделать коллекцию ссылочного типа, не допускающего значение null, и добавлять элементы в существующую коллекцию при получении новостей. Первое исправление заключается в добавлении `?` к типу `string` для свойства `ErrorText`:

[!code-csharp[UpdateErrorText](~/samples/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Index.cshtml.cs#UpdateErrorText)]

Это изменение не будет распространяться на другой код, потому что любой доступ к свойству `ErrorText` уже защищен проверками на значение null. Далее инициализируйте список `NewsItems` и удалите метод задания свойств, сделав этот список свойством, доступным только для чтения:

[!code-csharp[InitializeNewsItems](~/samples/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Index.cshtml.cs#InitializeNewsItems)]

Это исправило предупреждение, но внесло ошибку. Список `NewsItems` теперь **правильно сконструирован**, но код, который устанавливает список в `OnGet`, нужно изменить, чтобы соответствовать новому API. Вместо присваивания вызовите метод `AddRange`, чтобы добавить элементы новостей в существующий список:

[!code-csharp[AddRange](~/samples/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Index.cshtml.cs#AddRange)]

Использование метода `AddRange` вместо присваивания означает, что метод `GetNews` может вернуть `IEnumerable` вместо `List`. Это экономит одно распределение. Измените сигнатуру метода и удалите вызов `ToList`, как показано в следующем примере кода:

[!code-csharp[GetNews](~/samples/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Services/NewsService.cs#GetNewsFinished)]

Изменение сигнатуры также прерывает один из тестов. Откройте файл `NewsServiceTests.cs` в папке `Services` проекта `SimpleFeedReader.Tests`. Перейдите к тесту `Returns_News_Stories_Given_Valid_Uri` и измените тип переменной `result` на `IEnumerable<NewsItem>`. Изменение типа означает, что свойство `Count` больше недоступно, поэтому замените свойство `Count` в классе `Assert` вызовом метода `Any()`:

[!code-csharp[FixTests](~/samples/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader.Tests/Services/NewsServiceTests.cs#FixTestSignature)]

Необходимо также добавить оператор `using System.Linq` в начало файла.

Этот набор изменений уделяет особое внимание обновлению кода, который включает создание универсальных экземпляров. И список, и элементы в списке типов, не допускающих значение null. Любой из них или оба могут быть типа, допускающего значение null. Разрешены следующие объявления:

- `List<NewsStoryViewModel>`: список, не допускающий значение null, моделей представлений, не допускающих значение null;
- `List<NewsStoryViewModel?>`: список, не допускающий значение null, моделей представлений, допускающих значение null;
- `List<NewsStoryViewModel>?`: список, допускающий значение null, моделей представлений, не допускающих значение null;
- `List<NewsStoryViewModel?>?`: список, допускающий значение null, моделей представлений, допускающих значение null.

## <a name="interfaces-with-external-code"></a>Интерфейсы с внешним кодом

Вы внесли изменения в класс `NewsService`, поэтому включите для него заметку `#nullable enable`. После этого новые предупреждения больше не будут генерироваться. Однако тщательное изучение класса помогает проиллюстрировать некоторые ограничения анализа потоков компилятором. Изучите следующий конструктор:

[!code-csharp[ServiceConstructor](~/samples/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Services/NewsService.cs#ServiceConstructor)]

Параметр `IMapper` вводится в качестве ссылки, не допускающей значение null. Он вызывается кодом инфраструктуры ASP.NET Core, поэтому компилятор не знает, что `IMapper` никогда не будет иметь значение null. Контейнер по умолчанию для внедрения зависимостей ASP.NET Core генерирует исключение, если не может устранить необходимую службу, чтобы код оставался правильным. Компилятор не может проверить все вызовы ваших общедоступных API, даже если код скомпилирован с включенными контекстами заметок, допускающих значение null. Кроме того, библиотеки могут использоваться в проектах, которые еще не перешли на ссылочные типы, допускающие значение null. Проверяйте входные данные для общедоступных API, даже если вы объявили их как типы, не допускающие значение null.

## <a name="get-the-code"></a>Получите код

Вы исправили предупреждения, полученные в начальной тестовой компиляции, поэтому теперь можете включить контекст заметок, допускающих значение null, в обоих проектах. Перестройте проекты. Компилятор не выдаст ни одного предупреждения. Получить код готового проекта можно в репозитории GitHub [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/nullable-reference-migration/finished).

Новые возможности, поддерживающие ссылочные типы, допускающие значение null, помогают находить и исправлять потенциальные ошибки в обработке значений `null` в коде. Включение контекста заметок, допускающих значение null, позволяет выразить намерение проекта: некоторые переменные никогда не должны быть нулевыми, тогда как другие могут. Используя эти возможности, проще выразить намерение проекта. Аналогичным образом контекст предупреждений о допустимости значения null указывает компилятору выдавать предупреждения при нарушении намерения проекта. Благодаря этим предупреждениям вы сможете внести обновления, которые сделают код более устойчивым и уменьшат вероятность создания исключений `NullReferenceException` во время выполнения. Вы можете контролировать область этих контекстов, чтобы сосредоточиться на локальных областях кода, которые нужно перенести, пока оставшаяся база кода остается без изменений. На практике можно сделать эту задачу перехода частью регулярного обслуживания классов. В этом руководстве продемонстрирован процесс перехода приложения на использование ссылочных типов, допускающих значение null. Вы можете ознакомиться с гораздо большим реальным примером этого процесса, изучив запрос на вытягивание [Джона Скита](https://github.com/jskeet), созданный для перехода на ссылочные типы, допускающие значение null, в [NodaTime](https://github.com/nodatime/nodatime/pull/1240/commits).
