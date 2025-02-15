---
title: Автоматически реализуемые свойства
ms.date: 07/20/2015
f1_keywords:
- vb.AutoProperty
- vb.AutoImplementedProperty
helpviewer_keywords:
- properties [Visual Basic], auto-implemented
- auto-implemented properties [Visual Basic]
ms.assetid: 5c669f0b-cf95-4b4e-ae84-9cc55212ca87
ms.openlocfilehash: b322bd2215c95298be0a33ace1f3590a63878e24
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350388"
---
# <a name="auto-implemented-properties-visual-basic"></a>Автоматически реализуемые свойства (Visual Basic)
*Auto-implemented properties* enable you to quickly specify a property of a class without having to write code to `Get` and `Set` the property. При написании кода для автоматически реализуемого свойства компилятор Visual Basic автоматически создает закрытое поле для хранения переменной свойства, в дополнение к созданию связанных процедур `Get` и `Set`.  
  
 С помощью автоматически реализуемых свойств вы сможете объявлять свойства, включая значение по умолчанию, в одной строке. В следующем примере показано три объявления свойства.  
  
 [!code-vb[VbVbalrAutoImplementedProperties#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#1)]  
  
 Автоматически реализуемые свойства эквивалентны свойствам, значения которых хранятся в закрытом поле. В следующем примере кода показано автоматически реализуемое свойство.  
  
 [!code-vb[VbVbalrAutoImplementedProperties#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#5)]  
  
 В следующем примере кода показан эквивалентный код для предыдущего примера автоматически реализуемого свойства.  
  
 [!code-vb[VbVbalrAutoImplementedProperties#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#2)]  
  
 В следующем примере кода показана реализация свойств только для чтения:  
  
```vb  
Class Customer  
   Public ReadOnly Property Tags As New List(Of String)  
   Public ReadOnly Property Name As String = ""  
   Public ReadOnly Property File As String  
  
   Sub New(file As String)  
      Me.File = file  
   End Sub  
End Class  
```  
  
 Вы можете назначить свойству выражения инициализации, как показано в примере, или можно присвоить значения свойствам в конструкторе этого типа.  Резервные поля свойств только для чтения можно назначить в любое время.  
  
## <a name="backing-field"></a>Резервное поле  
 When you declare an auto-implemented property, Visual Basic automatically creates a hidden private field called the *backing field* to contain the property value. Имя резервного поля — это имя автоматически реализуемого свойства, с добавленным в начало знаком подчеркивания (_). Например, при объявлении автоматически реализуемого свойства с именем `ID` именем резервного поля будет `_ID`. Если добавить элемент класса также с именем `_ID`, возникнет конфликт имен, и Visual Basic сообщает об ошибке компилятора.  
  
 Резервное поле также имеет следующие характеристики:  
  
- модификатор доступа для резервного поля — всегда `Private`, даже если само свойство имеет другой уровень доступа, например `Public`;  
  
- если свойство помечено как `Shared`, резервное поле также будет общим;  
  
- атрибуты, указанные для свойства, не применяются к резервному полю;  
  
- доступ к резервному полю можно получить из кода внутри класса и из средств отладки, например окна контрольных значений. Однако резервное поле не отображается в списке предложений IntelliSense.  
  
## <a name="initializing-an-auto-implemented-property"></a>Инициализация автоматически реализуемого свойства  
 Любое выражение, которое может использоваться для инициализации поля, можно применять для инициализации автоматически реализуемого свойства. При инициализации автоматически реализуемого свойства выражение анализируется и передается процедуре `Set` для свойства. В следующих примерах кода показаны некоторые автоматически реализуемые свойства с начальными значениями.  
  
 [!code-vb[VbVbalrAutoImplementedProperties#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#3)]  
  
 Невозможно инициализировать автоматически реализуемое свойство, которое является элементом `Interface` или помечено как `MustOverride`.  
  
 При объявлении автоматически реализуемого свойства как элемента `Structure` его можно инициализировать, только если оно помечено как `Shared`.  
  
 При объявлении автоматически реализуемого свойства как массива невозможно указать явные границы массива. Однако можно задать значение с помощью инициализатора массива, как показано в следующих примерах.  
  
 [!code-vb[VbVbalrAutoImplementedProperties#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#4)]  
  
## <a name="property-definitions-that-require-standard-syntax"></a>Определения свойств, для которых требуется стандартный синтаксис  
 Автоматически реализуемые свойства удобны и поддерживают множество сценариев программирования. However, there are situations in which you cannot use an auto-implemented property and must instead use standard, or *expanded*, property syntax.  
  
 Расширенный синтаксис определения свойства необходимо использовать, если вам нужно выполнить следующие действия.  
  
- Добавить код для процедуры `Get` или `Set` свойства, например для проверки входящих значений в процедуре `Set`. Например, вам требуется убедиться, что строка, представляющая номер телефона, содержит необходимое количество цифр перед заданием значения свойства.  
  
- Указать другой уровень доступа для процедуры `Get` и `Set`. Например, вам может потребоваться сделать процедуру `Set``Private`, а процедуру `Get` — `Public`.  
  
- Создать свойства типа `WriteOnly`.  
  
- Использовать параметризованные свойства (включая `Default`). Чтобы задать параметр для свойства или указать дополнительные параметры, требуется объявить развернутое свойство процедуры `Set`.  
  
- Добавить атрибут в резервное поле или изменить уровень доступа резервного поля.  
  
- Добавить XML-комментарии для резервного поля.  
  
## <a name="expanding-an-auto-implemented-property"></a>Расширение автоматически реализуемого свойства  
 Если вам требуется преобразовать автоматически реализуемое свойство в расширенное свойство, которое содержит процедуру `Get` или `Set`, редактор кода Visual Basic может автоматически создать процедуры `Get` и `Set` и оператор `End Property` для свойства. The code is generated if you put the cursor on a blank line following the `Property` statement, type a `G` (for `Get`) or an `S` (for `Set`) and press ENTER. Редактор кода Visual Basic автоматически создает процедуру `Get` или `Set` для свойств только для чтения и только для записи при нажатии клавиши ВВОД после оператора `Property`.  
  
## <a name="see-also"></a>См. также

- [How to: Declare and Call a Default Property in Visual Basic](./how-to-declare-and-call-a-default-property.md)
- [Практическое руководство. Объявление свойства со смешанным уровнем доступа](./how-to-declare-a-property-with-mixed-access-levels.md)
- [Оператор Property](../../../../visual-basic/language-reference/statements/property-statement.md)
- [ReadOnly](../../../../visual-basic/language-reference/modifiers/readonly.md)
- [WriteOnly](../../../../visual-basic/language-reference/modifiers/writeonly.md)
- [Объекты и классы](../../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
