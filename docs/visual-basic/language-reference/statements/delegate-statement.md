---
title: Оператор Delegate
ms.date: 07/20/2015
f1_keywords:
- vb.Delegate
helpviewer_keywords:
- delegate keyword [Visual Basic]
- Delegate statement [Visual Basic]
ms.assetid: f799c518-0817-40cc-ad0b-4da846fdba57
ms.openlocfilehash: 662d2c3c0767adfe406e0a6f1b1e6dccd704e795
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354075"
---
# <a name="delegate-statement"></a>Оператор Delegate
Used to declare a delegate. A delegate is a reference type that refers to a `Shared` method of a type or to an instance method of an object. Any procedure with matching parameter and return types can be used to create an instance of this delegate class. The procedure can then later be invoked by means of the delegate instance.  
  
## <a name="syntax"></a>Синтаксис  
  
```vb  
[ <attrlist> ] [ accessmodifier ] _  
[ Shadows ] Delegate [ Sub | Function ] name [( Of typeparamlist )] [([ parameterlist ])] [ As type ]  
```  
  
## <a name="parts"></a>Части  
  
|Термин|Определение|  
|---|---|  
|`attrlist`|Необязательный. List of attributes that apply to this delegate. Несколько атрибутов разделяются запятыми. You must enclose the [Attribute List](../../../visual-basic/language-reference/statements/attribute-list.md) in angle brackets ("`<`" and "`>`").|  
|`accessmodifier`|Необязательный. Specifies what code can access the delegate. Ниже указаны доступные значения.<br /><br /> - [Public](../../../visual-basic/language-reference/modifiers/public.md). Any code that can access the element that declares the delegate can access it.<br />-   [Protected](../../../visual-basic/language-reference/modifiers/protected.md). Only code within the delegate's class or a derived class can access it.<br />-   [Friend](../../../visual-basic/language-reference/modifiers/friend.md). Only code within the same assembly can access the delegate.<br />- [Private](../../../visual-basic/language-reference/modifiers/private.md). Only code within the element that declares the delegate can access it.<br /><br /> - [Protected Friend](../../language-reference/modifiers/protected-friend.md) Only code within the delegate's class, a derived class, or the same assembly can access the delegate. <br />- [Private Protected](../../language-reference/modifiers/private-protected.md) Only code within the delegate's class or in a derived class in the same assembly can access the delegate. |  
|`Shadows`|Необязательный. Indicates that this delegate redeclares and hides an identically named programming element, or set of overloaded elements, in a base class. Можно скрыть любой тип объявленного элемента, используя любой другой тип.<br /><br /> Скрытый элемент недоступен из производного класса, который его скрывает, за исключением тех классов, из которых недоступен скрывающий элемент. For example, if a `Private` element shadows a base class element, code that does not have permission to access the `Private` element accesses the base class element instead.|  
|`Sub`|Optional, but either `Sub` or `Function` must appear. Declares this procedure as a delegate `Sub` procedure that does not return a value.|  
|`Function`|Optional, but either `Sub` or `Function` must appear. Declares this procedure as a delegate `Function` procedure that returns a value.|  
|`name`|Обязательный. Name of the delegate type; follows standard variable naming conventions.|  
|`typeparamlist`|Необязательный. List of type parameters for this delegate. Multiple type parameters are separated by commas. Optionally, each type parameter can be declared variant by using `In` and `Out` generic modifiers. You must enclose the [Type List](../../../visual-basic/language-reference/statements/type-list.md) in parentheses and introduce it with the `Of` keyword.|  
|`parameterlist`|Необязательный. List of parameters that are passed to the procedure when it is called. You must enclose the [Parameter List](../../../visual-basic/language-reference/statements/parameter-list.md) in parentheses.|  
|`type`|Required if you specify a `Function` procedure. Data type of the return value.|  
  
## <a name="remarks"></a>Заметки  
 The `Delegate` statement defines the parameter and return types of a delegate class. Any procedure with matching parameters and return types can be used to create an instance of this delegate class. The procedure can then later be invoked by means of the delegate instance, by calling the delegate's `Invoke` method.  
  
 Delegates can be declared at the namespace, module, class, or structure level, but not within a procedure.  
  
 Каждый класс делегата определяет конструктор, которому передается спецификация метода объекта. Аргумент конструктора делегата должен быть ссылкой на метод или лямбда-выражение.  
  
 Чтобы указать ссылку на метод, используйте следующий синтаксис:  
  
 `AddressOf` [`expression`.]`methodname`  
  
 Тип `expression` во время компиляции должен представлять собой имя класса или интерфейса, который содержит метод с указанным именем, сигнатура которого соответствует сигнатуре класса делегата. `methodname` должен быть общим методом или методом экземпляра. `methodname` всегда является обязательным, даже если делегат создается для метода по умолчанию в классе.  
  
 Чтобы указать лямбда-выражение, используйте следующий синтаксис:  
  
 `Function` ([`parm` As `type`, `parm2` As `type2`, ...]) `expression`  
  
 Сигнатура функции должна соответствовать сигнатуре типа делегата. Дополнительные сведения о лямбда-выражениях см. в разделе [Лямбда-выражения](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md).  
  
 и [делегатах](../../../visual-basic/programming-guide/language-features/delegates/index.md).  
  
## <a name="example"></a>Пример  
 The following example uses the `Delegate` statement to declare a delegate for operating on two numbers and returning a number. The `DelegateTest` method takes an instance of a delegate of this type and uses it to operate on pairs of numbers.  
  
 [!code-vb[VbVbalrDelegates#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#14)]  
  
## <a name="see-also"></a>См. также

- [Оператор AddressOf](../../../visual-basic/language-reference/operators/addressof-operator.md)
- [Of](../../../visual-basic/language-reference/statements/of-clause.md)
- [Делегаты](../../../visual-basic/programming-guide/language-features/delegates/index.md)
- [Практическое руководство. Использование универсального класса](../../../visual-basic/programming-guide/language-features/data-types/how-to-use-a-generic-class.md)
- [Generic Types in Visual Basic](../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)
- [Ковариация и контрвариантность](../../programming-guide/concepts/covariance-contravariance/index.md)
- [In](../../../visual-basic/language-reference/modifiers/in-generic-modifier.md)
- [Out](../../../visual-basic/language-reference/modifiers/out-generic-modifier.md)
