---
title: Общие сведения о преобразователях типов для XAML
ms.date: 03/30/2017
helpviewer_keywords:
- XAML [XAML Services], type converters
- XAML [XAML Services], TypeConverter
- type conversion for XAML [XAML Services]
ms.assetid: 51a65860-efcb-4fe0-95a0-1c679cde66b7
ms.openlocfilehash: b54731cc1aba1a47ed6b11f2bff5c596a53fd4b5
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458508"
---
# <a name="type-converters-for-xaml-overview"></a>Общие сведения о преобразователях типов для XAML
Преобразователи типов предоставляют логику для средства записи объекта, преобразующую строку в разметке XAML в конкретные объекты в графе объектов. В службах XAML .NET Framework преобразователь типов должен быть классом, производным от <xref:System.ComponentModel.TypeConverter>. Некоторые преобразователи также поддерживают путь сохранения XAML и могут использоваться для сериализации объекта в виде строки в разметке сериализации. В этом разделе описывается, как и когда вызываются преобразователи типов в XAML, а также представлены рекомендации по реализации переопределений методов класса <xref:System.ComponentModel.TypeConverter>.  
  
<a name="type_conversion_concepts"></a>   
## <a name="type-conversion-concepts"></a>Понятия преобразования типов  
 В следующих разделах рассматриваются некоторые основные вопросы использования строк в языке XAML, а также использование преобразователей типов средствами записи объектов служб XAML .NET Framework для обработки некоторых строковых значений, которые встречаются в источнике XAML.  
  
### <a name="xaml-and-string-values"></a>XAML и строковые значения  
 При установке значения атрибута в файле XAML начальный тип этого значения — это строка в общем смысле и значение строкового атрибута в контексте XML. Даже другие примитивные типы, такие как <xref:System.Double> , изначально являются строками для обработчика XAML.  
  
 В большинстве случаев XAML-обработчику требуется два компонента для обработки значения атрибута. Первый из них — это тип значения свойства, которое должно быть задано. Любую строку, которая определяет значение атрибута и обрабатывается в XAML, в конечном счете необходимо преобразовать или разрешить в значение этого типа. Если значение — примитивный тип, понятный средству синтаксического анализа XAML (например, числовое значение), выполняется попытка прямого преобразования строки. Если значение атрибута ссылается на перечисление, предоставленная строка проверяется на совпадение имени с именем константы в этом перечислении. Если значение не является ни примитивом, понятным средству синтаксического анализа, ни именем константы из перечисления, применимый тип должен предоставить значение или ссылку на основе преобразованной строки.  
  
> [!NOTE]
> Директивы языка XAML не используют преобразователи типов.  
  
### <a name="type-converters-and-markup-extensions"></a>Преобразователи типов и расширения разметки  
 Процессор XAML должен обработать расширения разметки, прежде чем он проверит тип свойства и другие аспекты. Например, если устанавливаемое как атрибут свойство обычно использует преобразования типов, однако в конкретном случае задается расширение разметки, сначала обрабатывается расширение разметки. Одна из распространенных ситуаций, где необходимо расширение разметки — указание ссылки на объект, который уже существует. Для этого сценария преобразователь типов без состояния может создать только новый экземпляр, что может быть нежелательно. Дополнительные сведения о расширениях разметки см. в разделе [Markup Extensions for XAML Overview](markup-extensions-for-xaml-overview.md).  
  
### <a name="native-type-converters"></a>Собственные преобразователи типа  
 В реализациях служб XAML WPF и .NET существуют определенные типы CLR, которые имеют собственную обработку преобразования типов, однако эти типы CLR не считаются примитивами. Пример такого типа — <xref:System.DateTime>. Одна из причин этого — работа архитектуры платформы .NET Framework: тип <xref:System.DateTime> определяется в mscorlib, в основной библиотеке .NET. Для<xref:System.DateTime> не разрешается атрибут, поступающий из другой сборки, который представляет зависимость (<xref:System.ComponentModel.TypeConverterAttribute> из системы). Поэтому обычный механизм обнаружения преобразователя типов по атрибуту не поддерживается. Вместо этого средство синтаксического анализа XAML использует список типов, требующих собственный обработки. Эти типы обрабатываются как настоящие примитивы. В случае <xref:System.DateTime>при обработке вызывается <xref:System.DateTime.Parse%2A>.  
  
<a name="Implementing_a_Type_Converter"></a>   
## <a name="implementing-a-type-converter"></a>Реализация преобразователя типов  
 В следующих разделах рассматриваются API-интерфейс класса <xref:System.ComponentModel.TypeConverter> .  
  
### <a name="typeconverter"></a>TypeConverter  
 В службах XAML .NET Framework все типы преобразователей, используемые для нужд XAML — это классы, производные от базового класса <xref:System.ComponentModel.TypeConverter>. Класс <xref:System.ComponentModel.TypeConverter> существовал в версиях .NET Framework до появления XAML. Один исходный сценарии <xref:System.ComponentModel.TypeConverter> использовался для преобразование строк в редакторах свойств визуальных конструкторов.  
  
 Для XAML роль <xref:System.ComponentModel.TypeConverter> расширяется. В XAML <xref:System.ComponentModel.TypeConverter> — это базовый класс для поддержки определенных преобразований в строку и из строки. Преобразование из строки позволяет выполнить анализ значения строкового атрибута из XAML. Преобразование в строку позволяет преобразовать значение определенного свойства объекта во время выполнения в атрибут в XAML для сериализации.  
  
 В<xref:System.ComponentModel.TypeConverter> определены четыре элемента, относящихся к преобразованию в строку и из строки для обработки XAML:  
  
- <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A>  
  
- <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A>  
  
- <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>  
  
- <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A>  
  
 Наиболее важный из этих элементов — это метод <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A>, который преобразует входную строку в требуемый тип объекта. Метод <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> можно реализовать для преобразования более широкого диапазона типов в требуемый конечный тип преобразователя. Таким образом его можно использовать в целях, выходящих за пределы языка XAML, например для поддержки преобразований во время выполнения. Однако для XAML важен только путь кода, который может обрабатывать входные данные <xref:System.String> .  
  
 Второй наиболее важный метод — <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>. Если приложение преобразуется в представление разметки (например, если оно сохранено в файл XAML), <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> участвует в более масштабном сценарии создания модулями записи текста XAML представления разметки. В этом случае важный путь кода XAML — передача вызывающим объектом `destinationType` из <xref:System.String>.  
  
 <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> и <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A> — это вспомогательные методы, используемые, когда служба запрашивает возможности реализации <xref:System.ComponentModel.TypeConverter> . Вам необходимо реализовать эти методы для возврата `true` для определенных типов. Они аналогичны методам преобразования для поддержки вашего преобразователя. В целях XAML обычно это означает тип <xref:System.String> .  
  
### <a name="culture-information-and-type-converters-for-xaml"></a>Сведения о языке и преобразователи типов для XAML  
 Каждая реализация <xref:System.ComponentModel.TypeConverter> может однозначно интерпретировать допустимую строку для преобразования, а также может использовать или игнорировать описание типа, переданного в качестве параметров. Важный аспект для региональных параметров и преобразования типов XAML: хотя использование локализуемых строк в качестве значений атрибутов поддерживается в XAML, невозможно применять эти локализуемые строки в качестве входных данных преобразователя типов с определенными требованиями для региональных параметров. Это ограничение вызвано тем, что преобразователи типов для значений атрибутов XAML обязательно используют фиксированное поведение обработки XAML, для чего применяются региональные параметры `en-US` . Дополнительные сведения о причинах этого ограничения см. в статье спецификация языка XAML ([\[MS-XAML\]](https://go.microsoft.com/fwlink/?LinkId=114525)) или в разделе [Общие сведения о глобализации и локализации WPF](../wpf/advanced/wpf-globalization-and-localization-overview.md).  
  
 Вот пример проблемы с языком и региональными параметрами: в некоторых языках в качестве десятичного разделителя для чисел в виде строки вместо точки используется запятая. Это противоречит поведению многих существующих преобразователей типов, которые используют запятую в качестве разделителя. Передача языка через `xml:lang` в XAML не решает проблему.  
  
### <a name="implementing-convertfrom"></a>Реализация ConvertFrom  
 Для использования в качестве реализации <xref:System.ComponentModel.TypeConverter> , поддерживающей XAML, метод <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> данного преобразователя должен принимать строку как параметр `value` . Если строка представлена в допустимом формате и может быть преобразована реализацией <xref:System.ComponentModel.TypeConverter> , возвращаемый объект должен поддерживать приведение к типу, ожидаемому свойством. В противном случае реализация <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> должна возвращать `null`.  
  
 Каждая реализация <xref:System.ComponentModel.TypeConverter> может однозначно интерпретировать допустимую строку для преобразования, а также может использовать или игнорировать описание типа или контекст языка, переданного в качестве параметров. Однако обработка XAML в WPF может не передавать значения в контекст описания типа во всех случаях, а также может не передавать язык на основе `xml:lang`.  
  
> [!NOTE]
> Не используйте фигурные скобки ({}), в частности открывающую фигурную скобку ({), как элемент формата строки. Эти символы зарезервированы как вход и выход для последовательности расширения разметки.  
  
 Можно вызывать исключение, если преобразователь типов должен иметь доступ к службе XAML из средства записи объектов служб XAML .NET Framework, но вызов <xref:System.IServiceProvider.GetService%2A> , который выполняется в контексте, не возвращает эту службу.  
  
### <a name="implementing-convertto"></a>Реализация ConvertTo  
 <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> может использоваться для поддержки сериализации. Поддержка сериализации с помощью <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> для пользовательского типа и его преобразователя типов не обязательна. Тем не менее, при реализации элемента управления или использовании сериализации как часть функций или класса, необходимо реализовать <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>.  
  
 Для использования в качестве реализации <xref:System.ComponentModel.TypeConverter> , поддерживающей XAML, метод <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> данного преобразователя должен принимать экземпляр поддерживаемого типа (или значение) как параметр `value` . Если тип параметра `destinationType` — тип <xref:System.String>, возвращаемый объект должен иметь возможность приведения к <xref:System.String>. Возвращаемая строка должна представлять сериализованное значение `value`. В идеальном случае выбранный формат сериализации должен позволять создавать то же значение, как и при передаче строки реализации <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> того же преобразователя без значительной потери информации.  
  
 Если значение не может быть сериализовано или преобразователь не поддерживает сериализацию, реализация <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> должна возвращать `null` и может вызывать исключение. Однако при создании исключений, следует сообщать о невозможности использовать это преобразование в вашей реализации <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> , поэтому рекомендуется проверять с помощью <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> , чтобы предотвратить исключения.  
  
 Если параметр `destinationType` не относится к типу <xref:System.String>, можно выбрать собственную обработку преобразователя. Как правило, вы возвращаетесь к базовой обработке реализации, которая с помощью <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> вызывает определенное исключение.  
  
 Можно вызывать исключение, если преобразователь типов должен иметь доступ к службе XAML из средства записи объектов служб XAML .NET Framework, но вызов <xref:System.IServiceProvider.GetService%2A> , который выполняется в контексте, не возвращает эту службу.  
  
### <a name="implementing-canconvertfrom"></a>Реализация CanConvertFrom  
 Ваша реализация <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A> должна возвращать `true` для `sourceType` типа <xref:System.String> , а в противном случае обращаться к базовой реализации. Не вызывайте исключения из <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A>.  
  
### <a name="implementing-canconvertto"></a>Реализация CanConvertTo  
 Ваша реализация <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> должна возвращать `true` для `destinationType` типа <xref:System.String>, а в противном случае обращаться к базовой реализации. Не вызывайте исключения из <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A>.  
  
<a name="Applying_the_TypeConverterAttribute"></a>   
## <a name="applying-the-typeconverterattribute"></a>Применение TypeConverterAttribute  
 Чтобы пользовательский преобразователь типов использовался в качестве преобразователя действующего типа для пользовательского класса .NET Framework служб XAML, необходимо применить <xref:System.ComponentModel.TypeConverterAttribute> к определению класса. Имя <xref:System.ComponentModel.TypeConverterAttribute.ConverterTypeName%2A> , указываемое в атрибуте, должно быть именем типа пользовательского преобразователя типов. Если этот атрибут применяется, обработчик XAML может передать строки и вернуть экземпляры объекта, когда он обрабатывает значения, в которых тип свойства применяет тип пользовательского класса.  
  
 Вы также можете предоставить преобразователь типов для отдельных свойств. Вместо применения <xref:System.ComponentModel.TypeConverterAttribute> к определению класса примените его к определению свойства (основное определение, а не `get`/`set` реализации внутри него). Тип свойства должен соответствовать типу, который обрабатывается пользовательским преобразователем типов. Если этот атрибут применяется, обработчик XAML может обработать входные строки и вернуть экземпляры объекта при работе со значениями этого свойства. Использование преобразователя типов для каждого свойства особенно полезно, если вы решили использовать тип свойства из Microsoft .NET Framework или из другой библиотеки, где нельзя управлять определением класса и не применять <xref:System.ComponentModel.TypeConverterAttribute> там.  
  
 Чтобы реализовать поведение преобразования типов для пользовательского присоединенного члена, примените <xref:System.ComponentModel.TypeConverterAttribute> к методу доступа `Get` шаблона реализации присоединенного члена.  
  
<a name="accessing_service_provider_context_from_a_markup_extension_implementation"></a>   
## <a name="accessing-service-provider-context-from-a-markup-extension-implementation"></a>Доступ к контексту поставщика службы из реализации расширения разметки  
 Доступные службы одинаковы для всех преобразователей значений. Разница заключается в том, как преобразователь значения получает контекст службы. Доступ к службам и предоставляемые службы описаны в разделе [Type Converters and Markup Extensions for XAML](type-converters-and-markup-extensions-for-xaml.md).  
  
<a name="type_converters_in_the_xaml_node_stream"></a>   
## <a name="type-converters-in-the-xaml-node-stream"></a>Преобразователи значений в потоке узлов XAML  
 При работе с потоком узлов XAML действие преобразователя типов еще не выполнено (или результат не достигнут). В пути загрузки строка атрибута, которой со временем потребуется преобразовать для загрузки, остается текстовым значением в пределах начального и конечного элемента. Преобразователь типов, который потребуется для этой операции, можно определить с помощью свойства <xref:System.Xaml.XamlMember.TypeConverter%2A?displayProperty=nameWithType> . Однако для получения допустимого значения из <xref:System.Xaml.XamlMember.TypeConverter%2A?displayProperty=nameWithType> необходим контекст схемы XAML, который может получить доступ к такой информации через базовый член, или тип объектного значения, используемого этим членом. Для вызова преобразования типов также необходим контекст схемы XAML, так как для этого требуется сопоставление типов и создание экземпляра преобразователя.  
  
## <a name="see-also"></a>См. также

- <xref:System.ComponentModel.TypeConverterAttribute>
- [Преобразователи типов или расширения разметки для XAML](type-converters-and-markup-extensions-for-xaml.md)
- [Общие сведения о языке XAML (WPF)](../../desktop-wpf/fundamentals/xaml.md)
