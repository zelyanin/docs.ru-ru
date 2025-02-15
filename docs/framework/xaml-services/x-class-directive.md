---
title: Директива x:Class
ms.date: 03/30/2017
f1_keywords:
- x:Class
- xClass
- Class
helpviewer_keywords:
- Class attribute in XAML [XAML Services]
- XAML [XAML Services], x:Class attribute
- x:Class attribute [XAML Services]
ms.assetid: bc4a3d8e-76e2-423e-a5d1-159a023e82ec
ms.openlocfilehash: 6e04085db0fa5a4c4170846dc4ac10d0131032a7
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2019
ms.locfileid: "71053803"
---
# <a name="xclass-directive"></a>Директива x:Class
Настраивает компиляцию разметки XAML для объединения разделяемых классов между разметкой и кодом программной части. Разделяемый класс code определен в отдельном файле кода в языке CLS, а разделяемый класс разметки обычно создается при создании кода во время компиляции XAML.  
  
## <a name="xaml-attribute-usage"></a>Использование атрибута XAML  
  
```xaml  
<object x:Class="namespace.classname"...>  
  ...  
</object>  
```  
  
## <a name="xaml-values"></a>Значения XAML  
  
|||  
|-|-|  
|`namespace`|Необязательный параметр. Указывает пространство имен CLR, содержащее разделяемый класс, `classname`идентифицируемый. Если `namespace` задано значение, точка (.) `namespace` разделяет и `classname`. См. заметки.|  
|`classname`|Обязательный. Указывает CLR-имя разделяемого класса, который подключает загруженный XAML и код программной части для этого XAML.|  
  
## <a name="dependencies"></a>Зависимости  
 `x:Class`может указываться только в корневом элементе рабочего элемента XAML. `x:Class`параметр недопустим для любого объекта, имеющего родительский объект в производстве XAML. Дополнительные сведения см [ \[. в разделе 4.3.1.6 в\] MS-XAML](https://go.microsoft.com/fwlink/?LinkId=114525).  
  
## <a name="remarks"></a>Примечания  
 `namespace` Значение может содержать дополнительные точки для организации связанных пространств имен в иерархии имен, что является распространенным методом в .NET Framework программировании. Только последняя точка в `x:Class` строке значений интерпретируется как отдельный `namespace` , а `classname.` класс, используемый как `x:Class` , не может быть вложенным классом. Вложенные классы не допускаются, так как определение значения точек `x:Class` для строк неоднозначно, если разрешены вложенные классы.  
  
 В существующих моделях программирования, использующих `x:Class`, `x:Class` является необязательным в том смысле, что вполне допустимо наличие страницы XAML без кода программной части. Однако эта возможность взаимодействует с действиями сборки в соответствии с реализацией платформ, использующих XAML. `x:Class`на возможности также влияют роли, которые имеют различные классификации содержимого, указанного в XAML, в модели приложения и в соответствующих действиях сборки. Если XAML объявляет значения атрибутов обработки событий или создает экземпляры пользовательских элементов, в которых определяющие классы находятся в классе кода программной части, необходимо предоставить `x:Class` ссылку на директиву (или [x:Subclass](x-subclass-directive.md)) соответствующему классу для код программной части.  
  
 Значение `x:Class` директивы должно быть строкой, указывающей полное имя класса, но без сведений о сборке (эквивалентно <xref:System.Type.FullName%2A?displayProperty=nameWithType>). Для простых приложений можно опустить сведения о пространстве имен CLR, если код программной части также структурирован таким образом (определение кода начинается на уровне класса).  
  
 Файл кода программной части для определения страницы или приложения должен находиться в файле кода, включенном в состав проекта, который создает скомпилированное приложение и включает компиляцию разметки. Необходимо следовать правилам именования для классов CLR. Дополнительные сведения см. в разделе [рекомендации по проектированию платформы](../../standard/design-guidelines/index.md). По умолчанию класс кода программной части должен иметь `public`значение, однако его можно определить на другом уровне доступа с помощью [директивы КС:классмодифиер](x-classmodifier-directive.md).  
  
 Эта интерпретация `x:Class` атрибута применяется только к реализации XAML на основе среды CLR, в частности .NET Framework службах XAML. Другие реализации XAML, которые не основаны на среде CLR и не используют .NET Framework службы XAML, могут использовать другую формулу разрешения для подключения разметки XAML и резервного кода времени выполнения. Дополнительные сведения о более общих интерпретациях `x:Class`см. в разделе [ \[MS-XAML\]](https://go.microsoft.com/fwlink/?LinkId=114525).  
  
 На определенном уровне архитектуры значение `x:Class` не определено в .NET Framework службах XAML. Это обусловлено тем, что службы .NET Framework XAML не указывают модель программирования, с которой связаны разметка XAML и резервный код. Дополнительные варианты использования `x:Class` директивы могут быть реализованы конкретными платформами, которые используют модели программирования или модели приложений для определения способа подключения разметки XAML и кода программной части на основе среды CLR. Каждая платформа может иметь собственные действия сборки, которые позволяют реализовать некоторые из поведений или отдельные компоненты, которые должны быть включены в среду сборки. В пределах платформы действия сборки также могут различаться в зависимости от конкретного языка среды CLR, используемого для кода программной части.  
  
## <a name="xclass-in-the-wpf-programming-model"></a>x:Class в модели программирования WPF  
 В приложениях WPF и модели `x:Class` приложений WPF можно объявлять как атрибут для любого элемента, который является корнем XAML-файла и компилируется (где XAML включается в проект приложения WPF с `Page` действием построения) или для < > <xref:System.Windows.Application> C4 root в определении приложения скомпилированного приложения WPF. `x:Class` Объявление для элемента, отличного от корня страницы или корня приложения, или в файле XAML WPF, который не компилируется, вызывает ошибку времени компиляции в компиляторе .NET Framework 3,0 и .NET Framework 3,5 WPF XAML. Сведения о других аспектах `x:Class` обработки в WPF см. в разделе [код программной части и XAML в WPF](../wpf/advanced/code-behind-and-xaml-in-wpf.md).  
  
## <a name="xclass-for-windows-workflow-foundation"></a>x:Class для Windows Workflow Foundation  
 Для Windows Workflow Foundation `x:Class` называет класс пользовательского действия, состоящего полностью в XAML, или имя разделяемого класса страницы XAML для конструктора действий с кодом программной части.  
  
## <a name="silverlight-usage-notes"></a>Заметки об использовании Silverlight  
 `x:Class`для Silverlight задокументировано отдельно. Дополнительные сведения см. в [разделе пространство имен XAML (x:). Функции языка (Silverlight)](https://go.microsoft.com/fwlink/?LinkId=199081).  
  
## <a name="see-also"></a>См. также

- [Директива x:Subclass](x-subclass-directive.md)
- [Код XAML и пользовательские классы для WPF](../wpf/advanced/xaml-and-custom-classes-for-wpf.md)
- [Директива x:ClassModifier](x-classmodifier-directive.md)
- [Типы, перенесенные из WPF в System.Xaml](types-migrated-from-wpf-to-system-xaml.md)
