---
title: Поддержка приложений для Магазина Windows и среды выполнения Windows в .NET Framework
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- Windows Store apps, .NET Framework support for
- Windows Runtime, .NET Framework support for
- .NET for Windows Store apps
- .NET Framework, and Windows Store apps
- .NET Framework, and Windows Runtime
ms.assetid: 6fa7d044-ae12-4c54-b8ee-50915607a565
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 2f46d21123ecfda4bd336edbbd79fabf01e002a4
ms.sourcegitcommit: 8a0fe8a2227af612f8b8941bdb8b19d6268748e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/03/2019
ms.locfileid: "71835330"
---
# <a name="net-framework-support-for-windows-store-apps-and-windows-runtime"></a>Поддержка приложений для Магазина Windows и среды выполнения Windows в .NET Framework

.NET Framework 4,5 поддерживает несколько сценариев разработки программного обеспечения с среда выполнения Windows. Эти способы можно разделить на три категории.

- Разработка приложений [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] с элементами управления XAML, как описано в статье [Путеводитель по приложениям для C# магазина Windows с помощью или Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10)) [, а также в](https://docs.microsoft.com/previous-versions/windows/apps/br229566(v=win.10))статье [Общие сведения о приложениях для приложений](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))для Магазина Windows.

- Разработка библиотек классов для использования в приложениях [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)], создаваемых с помощью .NET Framework.

- Разработка компонентов среда выполнения Windows, упакованных в. WinMD-файлы, которые могут использоваться любым языком программирования, поддерживающим среда выполнения Windows. Например, см. раздел [создание среда выполнения Windows компонентов C# в и Visual Basic](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic).

В этом разделе описывается поддержка, предоставляемая .NET Framework для всех трех категорий, а также описываются сценарии для среда выполнения Windows компонентов. Первый раздел содержит основные сведения о связи между .NET Framework и среда выполнения Windows и объясняет некоторые странности, которые могут возникнуть в справочной системе и интегрированной среде разработки. [Во втором разделе](#WindowsRuntimeComponents) обсуждаются сценарии разработки компонентов Среда выполнения Windows.

## <a name="the-basics"></a>Основы

.NET Framework поддерживает три сценария разработки, приведенных выше, предоставляя [!INCLUDE[net_win8_profile](../../../includes/net-win8-profile-md.md)] и поддерживая среда выполнения Windows.

- [Пространства имен .NET Framework и среда выполнения Windows](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)#net-framework-and-windows-runtime-namespaces) предоставляют упрощенное представление библиотек классов .NET Framework и включают только типы и члены, которые можно использовать для создания приложений [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] и среда выполнения Windows компонентов.

  - При использовании Visual Studio (Visual Studio 2012 или более поздней версии) для разработки приложения [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] или компонента среда выполнения Windows набор ссылочных сборок гарантирует, что будут видны только соответствующие типы и члены.

  - Упрощенный набор API упрощается за счет удаления функций, которые дублируются в .NET Framework или дублирования среда выполнения Windowsных функций. Например, он содержит только универсальные версии типов коллекций, а объектная модель XML-документа исключается в пользу среда выполнения Windows набора API XML.

  - Функции, которые просто заключают в оболочку API операционной системы, также удаляются, поскольку среда выполнения Windows легко вызывать из управляемого кода.

  Дополнительные сведения о [!INCLUDE[net_win8_profile](../../../includes/net-win8-profile-md.md)] см. в статье [Общие сведения о приложениях для Магазина Windows в .NET](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)). Сведения о процессе выбора API см. в разделе " [.NET для приложений в стиле Metro](https://devblogs.microsoft.com/dotnet/net-for-metro-style-apps/) " в блоге .NET.

- [Среда выполнения Windows](/uwp/api/) предоставляет элементы пользовательского интерфейса для создания приложений [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] и предоставляет доступ к функциям операционной системы. Как C# и .NET Framework, среда выполнения Windows содержит метаданные, позволяющие компиляторам и Visual Basic использовать среда выполнения Windows, используя .NET Framework библиотеки классов. .NET Framework упрощает использование среда выполнения Windows, скрывая некоторые различия:

  - Некоторые различия в шаблонах программирования между .NET Framework и среда выполнения Windows, например шаблон для добавления и удаления обработчиков событий, скрыты. Можно просто использовать шаблон .NET Framework.

  - Некоторые отличия часто используемых типов (например, примитивных типов и коллекций) также скрыты. Вы просто используете тип .NET Framework, как описано в разделе [различия, отображаемые в интегрированной среде разработки](#DifferencesVisibleInIDE)далее в этой статье.

В большинстве случаев .NET Framework поддержка среда выполнения Windows прозрачна. В следующем разделе обсуждаются некоторые очевидные различия между управляемым кодом и среда выполнения Windows.

<a name="AboutReferenceDocumentation"></a>

### <a name="the-net-framework-and-the-windows-runtime-reference-documentation"></a>Справочная документация по .NET Framework и среда выполнения Windows

Среда выполнения Windows и .NET Framework наборы документации являются отдельными. При нажатии клавиши F1 для отображения справки для типа или члена отображается справочная документация из соответствующего набора. Тем не менее, если вы просматриваете [ссылку на среда выполнения Windows](/uwp/api/) , вы можете столкнуться с примерами, которые выглядят замешательство:

- Такие темы, как интерфейс <xref:Windows.Foundation.Collections.IIterable%601>, не имеют синтаксиса объявления для C#Visual Basic или. Вместо этого в разделе синтаксиса появится Примечание (в данном случае ".NET: Этот интерфейс отображается как System. Collections. Generic. IEnumerable @ no__t-0T > "). Это обусловлено тем, что .NET Framework и среда выполнения Windows предоставляют аналогичные функции с разными интерфейсами. Кроме того, существуют отличия в поведении: `IIterable` содержит метод `First` вместо метода <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> для возврата перечислителя. Вместо того чтобы приступать к изучению другого способа выполнения распространенной задачи, .NET Framework поддерживает среда выполнения Windows, так как управляемый код будет использовать тип, с которым вы знакомы. Вы не видите интерфейс `IIterable` в интегрированной среде разработки, и поэтому единственный способ ее встретиться в справочной документации по среда выполнения Windows — это прямое обращение к этой документации.

- В документации по <xref:Windows.Web.Syndication.SyndicationFeed.%23ctor(System.String,System.String,Windows.Foundation.Uri)> показана тесно связанная с этим ситуация: Типы параметров для разных языков выглядят иначе. Для C# и Visual Basic типами параметров являются <xref:System.String?displayProperty=nameWithType> и <xref:System.Uri?displayProperty=nameWithType>. Это происходит потому, что платформа .NET Framework имеет собственные типы `String` и `Uri`, и для таких часто используемых типов не имеет смысла принуждать пользователей .NET Framework использовать другой способ выполнения действий. В интегрированной среде разработки .NET Framework скрывает соответствующие типы среда выполнения Windows.

- В некоторых случаях, таких как структура <xref:Windows.UI.Xaml.GridLength>, .NET Framework предоставляет тип с тем же именем, но с дополнительными функциональными возможностями. Например, разделы о конструкторе и свойствах связаны с `GridLength`, но они имеют блоков синтаксиса только для Visual Basic и C#, так как эти члены доступны только в управляемом коде. В среда выполнения Windows структуры имеют только поля. Для структуры среда выполнения Windows требуется вспомогательный класс <xref:Windows.UI.Xaml.GridLengthHelper>, чтобы обеспечить эквивалентную функциональность. Вы не увидите этот вспомогательный класс в интегрированной среде разработки при создании управляемого кода.

- В интегрированной среде разработки типы среда выполнения Windows являются производными от <xref:System.Object?displayProperty=nameWithType>. У них могут быть члены, унаследованные от <xref:System.Object>, например <xref:System.Object.ToString%2A?displayProperty=nameWithType>. Эти члены работают так, как если бы типы, фактически унаследованные от <xref:System.Object>, и среда выполнения Windows типы могли быть приведены к <xref:System.Object>. Эта функция является частью поддержки, предоставляемой .NET Framework для среда выполнения Windows. Однако если просмотреть типы в справочной документации по среда выполнения Windows, такие члены отображаться не будут. Документация для этих унаследованных членов предоставляется в документации по <xref:System.Object?displayProperty=nameWithType>.

<a name="DifferencesVisibleInIDE"></a>

### <a name="differences-that-are-visible-in-the-ide"></a>Различия, которые видимы в интегрированной среде разработки

В более сложных сценариях программирования, таких как использование среда выполнения Windowsного компонента, C# написанного в, для предоставления логики приложения для приложения [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)], созданного для Windows с помощью JavaScript, такие различия очевидны в интегрированной среде разработки, а также в документации. Когда компонент возвращает `IDictionary<int, string>` в JavaScript и вы взгляните на него в отладчике JavaScript, вы увидите методы `IMap<int, string>`, так как JavaScript использует тип среда выполнения Windows. Несколько часто используемых типов коллекций, которые по-разному представляются в этих двух языках, показаны в следующей таблице.

|Тип среды выполнения Windows|Соответствующий тип платформы .NET Framework|
|--------------------------------------------------------------|---------------------------------------|
|`IIterable<T>`|`IEnumerable<T>`|
|`IIterator<T>`|`IEnumerator<T>`|
|`IVector<T>`|`IList<T>`|
|`IVectorView<T>`|`IReadOnlyList<T>`|
|`IMap<K, V>`|`IDictionary<TKey, TValue>`|
|`IMapView<K, V>`|`IReadOnlyDictionary<TKey, TValue>`|
|`IBindableIterable`|`IEnumerable`|
|`IBindableVector`|`IList`|
|`Windows.UI.Xaml.Data.INotifyPropertyChanged`|`System.ComponentModel.INotifyPropertyChanged`|
|`Windows.UI.Xaml.Data.PropertyChangedEventHandler`|`System.ComponentModel.PropertyChangedEventHandler`|
|`Windows.UI.Xaml.Data.PropertyChangedEventArgs`|`System.ComponentModel.PropertyChangedEventArgs`|

В среда выполнения Windows `IMap<K, V>` и `IMapView<K, V>` просматриваются с помощью `IKeyValuePair`. При передаче этих типов в управляемый код они представляются как `IDictionary<TKey, TValue>` и `IReadOnlyDictionary<TKey, TValue>`, поэтому для их перебора можно обычным образом использовать `System.Collections.Generic.KeyValuePair<TKey, TValue>`.

Представление интерфейсов в управляемом коде влияет на представление типов, реализующих эти интерфейсы. Например, класс `PropertySet` реализует `IMap<K, V>`, который представлен в управляемом коде как `IDictionary<TKey, TValue>`. `PropertySet` представляет себя как реализующего `IDictionary<TKey, TValue>` вместо `IMap<K, V>`, поэтому в управляемом коде у него присутствует метод `Add`, который ведет себя как метод `Add` в словарях .NET Framework. А метода `Insert` у этого типа нет.

Дополнительные сведения об использовании .NET Framework для создания среда выполнения Windows компонента и пошаговое руководство, в котором показано, как использовать такой компонент с JavaScript, см. [в разделе Создание компонентов C# среда выполнения Windows в и Visual Basic](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic).

### <a name="primitive-types"></a>Типы-примитивы

Чтобы обеспечить естественное использование среда выполнения Windows в управляемом коде, .NET Framework примитивные типы отображаются вместо среда выполнения Windows типов-примитивов в коде. В .NET Framework у простых типов, таких как структура `Int32`, имеется множество полезных свойств и методов, например метод `Int32.TryParse`. Напротив, типы-примитивы и структуры в среда выполнения Windows имеют только поля. При использовании простых типов в управляемом коде они представляются как типы платформы .NET Framework, и можно как обычно использовать свойства и методы типов платформы .NET Framework. В следующем списке приводятся сводные данные.

- Для примитивов среда выполнения Windows `Int32`, `Int64`, `Single`, `Double`, `Boolean`, `String` (неизменяемая коллекция символов Юникода), `Enum`, `UInt32`, `UInt64` и `Guid`, используйте тип с тем же именем в пространстве имен 0.

- вместо `UInt8` используется тип `System.Byte`;

- вместо `Char16` используется тип `System.Char`;

- вместо интерфейса `IInspectable` используется тип `System.Object`.

- Для `HRESULT` используйте структуру с одним членом `System.Int32`.

Как и в случае с типами интерфейсов, вы можете видеть свидетельство этого представления, если проект .NET Framework является компонентом среда выполнения Windows, который используется приложением [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)], созданным с помощью JavaScript.

Другие базовые, часто используемые среда выполнения Windows типы, отображаемые в управляемом коде в качестве их .NET Framework эквивалентов, включают структуру `Windows.Foundation.DateTime`, которая отображается в управляемом коде как структура <xref:System.DateTimeOffset?displayProperty=nameWithType>, и структура `Windows.Foundation.TimeSpan`, которая отображается как <xref:System.TimeSpan?displayProperty=nameWithType>. дереве.

### <a name="other-differences"></a>Другие различия

В некоторых случаях .NET Framework типы отображаются в коде, а не среда выполнения Windows типы требуют действия в вашей части. Например, класс <xref:Windows.Foundation.Uri?displayProperty=nameWithType> отображается как <xref:System.Uri?displayProperty=nameWithType> в .NET Frameworkном коде. <xref:System.Uri?displayProperty=nameWithType> разрешает относительный URI, но для <xref:Windows.Foundation.Uri?displayProperty=nameWithType> требуется абсолютный URI. Поэтому при передаче URI в метод среда выполнения Windows необходимо убедиться, что он является абсолютным. См. раздел [Передача URI в среда выполнения Windows](../../../docs/standard/cross-platform/passing-a-uri-to-the-windows-runtime.md).

<a name="WindowsRuntimeComponents"></a>

## <a name="scenarios-for-developing-windows-runtime-components"></a>Сценарии для разработки компонентов среды выполнения Windows

Сценарии, поддерживаемые для управляемых компонентов среда выполнения Windows, зависят от следующих общих принципов.

- Среда выполнения Windows компоненты, созданные с помощью .NET Framework, не имеют очевидных отличий от других Рунтимелибрариес Windows. Например, при повторной реализации собственного среда выполнения Windows компонента с помощью управляемого кода эти два компонента становятся неразличимыми. Дело в том, что компонент, написанный в управляемом коде, остается незаметным для кода, который его использует, даже если сам код является управляемым. Однако на внутреннем уровне компонент является настоящим управляемым кодом и работает в среде CLR.

- Компоненты могут содержать типы, реализующие логику приложения, элементы управления пользовательского интерфейса [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] или и те, и другие.

  > [!NOTE]
  > Рекомендуется отделять элементы пользовательского интерфейса от логики приложения. Кроме того, нельзя использовать элементы управления пользовательского интерфейса [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] в приложении [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)], созданном для Windows с помощью JavaScript и HTML.

- Компонент может быть проектом внутри решения Visual Studio для приложения [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] или многоразовым компонентом, который можно добавить к нескольким решениям.

  > [!NOTE]
  > Если компонент будет использоваться только с C# или Visual Basic, нет причин сделать его компонентом Среда выполнения Windows. Если вместо этого вы сделаете ее обычной .NET Frameworkой библиотекой классов, вам не нужно ограничивать область ее открытого API среда выполнения Windows типами.

- Можно выпустить версии многократно используемых компонентов с помощью атрибута среда выполнения Windows <xref:Windows.Foundation.Metadata.VersionAttribute>, чтобы узнать, какие типы (и какие элементы в типе) были добавлены в разные версии.

- Типы в компоненте могут быть производными от типов среда выполнения Windows. Элементы управления могут быть производными от примитивных типов элементов управления в пространстве имен <xref:Windows.UI.Xaml.Controls.Primitives> или из более законченных элементов управления, таких как <xref:Windows.UI.Xaml.Controls.Button>.

  > [!IMPORTANT]
  > Начиная с [!INCLUDE[win8](../../../includes/win8-md.md)] и .NET Framework 4,5 все открытые типы в управляемом компоненте среда выполнения Windows должны быть запечатанными. Тип в другом компоненте среда выполнения Windows не может быть производным от них. Если необходимо предоставить полиморфное расширения функциональности в компоненте, можно создать интерфейс и реализовать его в полиморфных типах.

- Все типы параметров и возвращаемых типов в общедоступных типах в компоненте должны быть среда выполнения Windows типами (включая типы среда выполнения Windows, определяемые компонентом).

Следующие разделы содержат примеры распространенных сценариев.

### <a name="application-logic-for-a-includewin8_appname_longincludeswin8-appname-long-mdmd-app-with-javascript"></a>Логика приложения для приложения [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] с JavaScript

При создании приложения [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] для Windows с помощью JavaScript может оказаться, что некоторые части логики приложения работают лучше в управляемом коде или их легче так разработать. Javascript не может использовать библиотеки классов платформы .NET Framework непосредственно, но можно сделать библиотекой классов файл .WinMD. В этом сценарии компонент среда выполнения Windows является неотъемлемой частью приложения, поэтому не имеет смысла предоставлять атрибуты версии.

### <a name="reusable-includewin8_appname_longincludeswin8-appname-long-mdmd-ui-controls"></a>Повторно используемые элементы управления пользовательского интерфейса [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)]

Набор связанных элементов управления пользовательского интерфейса можно упаковать в многократно используемый среда выполнения Windows компонент. Компонент может распространяться сам по себе или использоваться как элемент в приложениях, которые вы создаете. В этом сценарии имеет смысл использовать атрибут среда выполнения Windows <xref:Windows.Foundation.Metadata.VersionAttribute> для улучшения совместимости.

### <a name="reusable-application-logic-from-existing-net-framework-apps"></a>Логика приложения с возможностью повторного использования существующих приложений платформы .NET Framework

Вы можете упаковать управляемый код из существующих классических приложений в качестве автономного компонента среда выполнения Windows. Это позволит использовать его в приложениях [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)], созданных с помощью C++ или JavaScript, а также в приложениях [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)], созданных с помощью C# или Visual Basic. Используйте управление версиями, если этот код будет использоваться в нескольких местах.

## <a name="related-topics"></a>См. также

|Заголовок|Описание|
|-----------|-----------------|
|[Общие сведения о платформе .NET для приложений Магазина Windows](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))|Описание типов .NET Framework и членов, которые можно использовать для создания приложений [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] и Windows Рунтимекомпонентс. (В Центре разработки для Windows.)|
|[Путеводитель по приложениям Магазина Windows с C# помощью или Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))|Предоставляет основные ресурсы, помогающие начать разрабатывать приложения [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] с помощью C# или Visual Basic, а также многие разделы краткого руководства, правила и рекомендации. (В Центре разработки для Windows.)|
|[Инструкции (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/br229566(v=win.10))|Предоставляет основные ресурсы, помогающие начать разрабатывать приложения [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] с помощью C# или Visual Basic, а также многие разделы краткого руководства, правила и рекомендации. (В Центре разработки для Windows.)|
|[Создание компонентов среды выполнения Windows в C# и Visual Basic](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic)|Описание процесса создания среда выполнения Windows компонента с помощью .NET Framework, объясняется, как использовать его как часть приложения [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)], созданного для Windows с помощью JavaScript, и описывается отладка сочетания с Visual Studio. (В Центре разработки для Windows.)|
|[Справочник по среда выполнения Windows](/uwp/api/)|Справочная документация по среда выполнения Windows. (В Центре разработки для Windows.)|
|[Передача URI в среду выполнения Windows](../../../docs/standard/cross-platform/passing-a-uri-to-the-windows-runtime.md)|В этой статье описывается проблема, которая может возникнуть при передаче URI из управляемого кода в среда выполнения Windows и о том, как ее избежать.|
