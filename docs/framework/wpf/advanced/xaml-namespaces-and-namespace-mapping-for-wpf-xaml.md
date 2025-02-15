---
title: Пространства имен XAML и сопоставление пространств имен для WPF XAML
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- custom classes [WPF], mapping namespaces to
- XAML [WPF], namespaces
- namespace mapping [WPF]
- assemblies [WPF], mapping namespaces to
- mapping namespaces [WPF]
- XAML [WPF], namespace mapping
- classes [WPF], mapping namespaces to
- namespaces [WPF]
ms.assetid: 5c0854e3-7470-435d-9fe2-93eec9d3634e
ms.openlocfilehash: 8f381a06aa916be378052d00f0d65f37ef910433
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2019
ms.locfileid: "73740660"
---
# <a name="xaml-namespaces-and-namespace-mapping-for-wpf-xaml"></a>Пространства имен XAML и сопоставление пространств имен для WPF XAML
В этом разделе обосновывается наличие и рассматриваются цели сопоставления двух пространств имен XAML, обычно содержащихся в корневом теге файла WPF XAML. Также описываются способы создания аналогичных сопоставлений для использования элементов, заданных в коде и/или в отдельных сборках.  

## <a name="what-is-a-xaml-namespace"></a>Общие сведения о пространстве имен XAML  
 Пространство имен XAML — это расширение концепции пространства имен XML. При указании пространства имен XAML используется синтаксис пространства имен XML: соблюдается принцип использования универсальных кодов ресурсов (URI) в качестве идентификаторов пространств имен, использование префиксов, позволяющих указать несколько пространств имен с общим исходным кодом разметки и т. п. Основное нововведение в определении пространства имен XML, используемом в XAML, заключается в том, что пространство имен XAML неявно задает область уникальности используемой разметки и при этом влияет на возможное дополнение сущностей разметки определенными пространствами имен CLR и указанными в них сборками. Последний фактор обусловлен в том числе природой контекста схемы XAML. Впрочем, в отношении взаимодействия WPF с пространствами имен XAML можно рассматривать пространства имен XAML с учетом пространства имен XAML по умолчанию, пространства имен языка XAML и любых других пространств имен XAML, напрямую сопоставленных в разметке XAML с дополняющими их пространствами имен CLR и указанными в них сборками.  
  
<a name="The_WPF_and_XAML_Namespace_Declarations"></a>   
## <a name="the-wpf-and-xaml-namespace-declarations"></a>WPF и объявления пространства имен XAML  
 В объявлении пространства имен в корневом теге многих XAML-файлов, как правило, содержатся два объявления пространства имен XML. Первое объявление задает сопоставление для общего пространства имен клиента WPF и платформы XAML как для пространства имен по умолчанию.  
  
 `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`  
  
 Второе объявление сопоставляет отдельное пространство имен XAML, сопоставляя его (обычно) с префиксом `x:`.  
  
 `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`  
  
 Связь между этими объявлениями состоит в том, что сопоставление префикса `x:` поддерживает встроенные функции, которые являются частью определения языка XAML и [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] является реализацией, которая использует XAML в качестве языка и определяет словарь объектов для XAML. Так как использование словаря WPF более распространено, чем использование встроенных функций XAML, словарь WPF сопоставляется по умолчанию.  
  
 В соглашении о префиксах `x:` для сопоставления встроенных функций языка XAML следуют шаблоны проектов, примеры кода и документация по функциональным возможностям языка в этом пакете SDK. Пространство имен XAML определяет многие часто используемые функциональные возможности, которые необходимы даже для основных приложений WPF. Например, чтобы присоединить какой-либо код программной части к XAML-файлу через разделяемый класс, необходимо именовать класс как атрибут `x:Class` в корневом элементе соответствующего XAML-файла. Или же любой элемент, определенный на странице XAML, к которой необходимо получить доступ в качестве ключевого ресурса, должен иметь заданный атрибут `x:Key`. Дополнительные сведения об этих и других аспектах XAML см. в разделах [Общие сведения о языке XAML (WPF)](../../../desktop-wpf/fundamentals/xaml.md) и [Подробное описание синтаксиса XAML](xaml-syntax-in-detail.md).  
  
<a name="Mapping_To_Custom_Classes_and_Assemblies"></a>   
## <a name="mapping-to-custom-classes-and-assemblies"></a>Сопоставление пользовательских классов и сборок  
 Вы можете сопоставить пространства имен XML со сборками, используя серию токенов в объявлении префиксов `xmlns` аналогично тому, как стандартные пространства имен WPF и встроенных функций XAML сопоставляются с префиксами.  
  
 Синтаксис допускает перечисленные ниже возможные именованные токены и значения.  
  
 `clr-namespace:` Пространство имен CLR объявлено внутри сборки, которая содержит открытые типы, предоставленные как элементы.  
  
 `assembly=` сборку, содержащую некоторые или все связанные пространства имен CLR, на которые имеются ссылки. Это значение обычно содержит только имя сборки, но не путь к ней, и не включает расширение имени файла (например, EXE или DLL). Путь к этой сборке должен быть задан в качестве ссылки проекта в файле проекта, который содержит код XAML, который следует сопоставить. Чтобы включить управление версиями и подписывание строгих имен, значение `assembly` может быть строкой, как определено <xref:System.Reflection.AssemblyName>, а не именем простой строки.  
  
 Обратите внимание, что токен `clr-namespace` отделяется от значения двоеточием (:), тогда как токен `assembly` отделяется от значения знаком равенства (=). Эти два токена разделяются точкой с запятой. Кроме того, не включайте пробел в любом месте объявления.  
  
### <a name="a-basic-custom-mapping-example"></a>Простой пример пользовательского сопоставления  
 В следующем примере кода определяется пользовательский класс.  
  
```csharp  
namespace SDKSample {  
    public class ExampleClass : ContentControl {  
        public ExampleClass() {  
        ...  
        }  
    }  
}  
```  
  
```vb  
Namespace SDKSample  
    Public Class ExampleClass  
        Inherits ContentControl  
         ...  
        Public Sub New()  
        End Sub  
    End Class  
End Namespace  
```  
  
 Затем пользовательский класс компилируется в библиотеку, которая согласно параметрам проекта (не показано) имеет имя `SDKSampleLibrary`.  
  
 Для ссылки на этот пользовательский класс также следует включить его в качестве ссылки на текущий проект, что обычно делается с помощью пользовательского интерфейса обозревателя решений в Visual Studio.  
  
 Теперь, когда есть библиотека с классом и ссылка на него в параметрах проектах, можно добавить следующее сопоставление префикса как часть корневого элемента в XAML.  
  
 `xmlns:custom="clr-namespace:SDKSample;assembly=SDKSampleLibrary"`  
  
 Чтобы собрать это все вместе, используется приведенный ниже код XAML, в который включено пользовательское сопоставление с обычным сопоставлением и сопоставлением x: в корневом теге. Затем используется ссылка с префиксом для создания экземпляра `ExampleClass` в этом пользовательском интерфейсе.  
  
```xaml  
<Page x:Class="WPFApplication1.MainPage"  
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"   
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  
    xmlns:custom="clr-namespace:SDKSample;assembly=SDKSampleLibrary">  
  ...  
  <custom:ExampleClass/>  
...  
</Page>  
```  
  
### <a name="mapping-to-current-assemblies"></a>Сопоставление с текущими сборками  
 Можно опустить `assembly`, если указанное пространство имен `clr-namespace` определяется в этой же сборке в качестве кода приложения, который ссылается на пользовательские классы. Аналогичный способ для этого случая ― указание `assembly=` без строкового токена после знака равенства.  
  
 Пользовательские классы нельзя использовать в качестве корневого элемента страницы, если они определены в той же сборке. Разделяемые классы не требуют сопоставления; должны быть сопоставлены только классы, которые не являются разделяемыми классами страницы в приложении, если планируется ссылка на них как на элементы в XAML.  
  
<a name="Mapping_CLR_Namespaces_to_XML_Namespaces_in_an"></a>   
## <a name="mapping-clr-namespaces-to-xml-namespaces-in-an-assembly"></a>Сопоставление пространств имен CLR с пространствами имен XML в сборке  
 WPF определяет атрибут CLR, который обрабатывается процессорами XAML для сопоставления нескольких пространств имен CLR с одним пространством имен XAML. Этот атрибут, <xref:System.Windows.Markup.XmlnsDefinitionAttribute>, помещается на уровне сборки в исходном коде, который создает сборку. Исходный код сборки WPF использует этот атрибут для соотнесения различных общих пространств имен, таких как <xref:System.Windows> и <xref:System.Windows.Controls>, к пространству имен `http://schemas.microsoft.com/winfx/2006/xaml/presentation`.  
  
 <xref:System.Windows.Markup.XmlnsDefinitionAttribute> принимает два параметра: имя пространства имен XML/XAML и имя пространства имен CLR. Для соответствия нескольких пространств имен CLR одному и тому же пространству имен XML может существовать несколько <xref:System.Windows.Markup.XmlnsDefinitionAttribute>. После сопоставления на члены этих пространств имен при желании можно ссылаться без указания полного имени, предоставляя соответствующий оператор `using` на странице с выделенным кодом разделяемого класса. Дополнительные сведения см. в разделе <xref:System.Windows.Markup.XmlnsDefinitionAttribute>.  
  
## <a name="designer-namespaces-and-other-prefixes-from-xaml-templates"></a>Пространства имен конструктора и другие префиксы из шаблонов XAML  
 При работе в средах разработки или со средствами разработки для WPF XAML можно заметить, что в разметке XAML определены и другие пространства имен и префиксы XAML.  
  
 В [!INCLUDE[wpfdesigner_current_long](../../../../includes/wpfdesigner-current-long-md.md)] используется пространство имен конструктора, обычно сопоставляемое с префиксом `d:`. В новейших шаблонах проектов для WPF это пространство имен XAML иногда заранее сопоставляется так, чтобы обеспечить поддержку переноса кода XAML между [!INCLUDE[wpfdesigner_current_long](../../../../includes/wpfdesigner-current-long-md.md)] и другими средами разработки. Это пространство имен XAML для разработки используется с целью сохранения состояния среды разработки при переносе пользовательского интерфейса, созданного на основе XAML, из одного средства разработки в другое. Оно также используется в таких функциях как `d:IsDataSource`, позволяющих использовать источники данных среды выполнения в конструкторе.  
  
 Среди сопоставленных префиксов также встречается `mc:`. Префикс `mc:` используется для обеспечения совместимости разметки. С его помощью можно обеспечить шаблон совместимости не только для языка XAML. Функции обеспечения совместимости разметки в некоторой степени можно использовать для обмена кодом XAML между различными платформами, взаимодействия между различными схемами XAML, обеспечения совместимости для ограниченных режимов в конструкторах и т. п. Дополнительные сведения о принципах обеспечения совместимости разметки и их применении к WPF см. в разделе [Совместимость разметки (mc:) языковые компоненты](markup-compatibility-mc-language-features.md).  
  
## <a name="wpf-and-assembly-loading"></a>WPF и загрузка сборок  
 Контекст схемы XAML для WPF интегрируется с моделью приложения WPF, которая, в свою очередь, использует концепцию <xref:System.AppDomain>, определяемую CLR. Следующая последовательность описывает, как контекст схемы XAML интерпретирует, как загружать сборки или находить типы во время выполнения или во время разработки, основываясь на использовании <xref:System.AppDomain> и других факторов в WPF.  
  
1. Выполните итерацию по <xref:System.AppDomain>у, чтобы найти уже загруженную сборку, которая соответствует всем аспектам имени, начиная с последней загруженной сборки.  
  
2. Если имя уточнено, вызовите <xref:System.Reflection.Assembly.Load%28System.String%29?displayProperty=nameWithType> для полного имени.  
  
3. Если сборке, из которой была загружена разметка, соответствует сочетание "короткое имя + токен открытого ключа полного имени", возвратите эту сборку.  
  
4. Используйте короткое имя + токен открытого ключа для вызова <xref:System.Reflection.Assembly.Load%28System.String%29?displayProperty=nameWithType>.  
  
5. Если имя не определено, вызовите <xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType>.  
  
 Свободный XAML не использует шаг 3, так как нет сборки, из которой выполнялась загрузка.  
  
 Скомпилированный XAML для WPF (сформированный с помощью Ксамлбуилдтаск) не использует уже загруженные сборки из <xref:System.AppDomain> (шаг 1). Кроме того, имя из выходных данных XamlBuildTask не должно быть неполным, поэтому шаг 5 не применяется.  
  
 Скомпилированный BAML (сформированный с помощью PresentationBuildTask) использует все шаги, хотя BAML также не должен содержать неполные имена сборок.  
  
## <a name="see-also"></a>См. также

- [Основные сведения о пространствах имен XML](https://go.microsoft.com/fwlink/?LinkId=98069)
- [Общие сведения о языке XAML (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
