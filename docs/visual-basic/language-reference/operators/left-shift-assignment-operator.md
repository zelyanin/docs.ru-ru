---
title: Оператор <<=
ms.date: 07/20/2015
f1_keywords:
- vb.<<=
helpviewer_keywords:
- operator <<=
- assignment statements [Visual Basic], compound
- <<= operator [Visual Basic]
- statements [Visual Basic], compound assignment
- operator<<=
- compound assignment statements [Visual Basic]
ms.assetid: 8ad26613-faff-4e2f-89ee-63feee33bfda
ms.openlocfilehash: cc89e0dadc7148b21e695a53a2e746a00ed66441
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350958"
---
# <a name="-operator-visual-basic"></a>\<\<= Operator (Visual Basic)
Performs an arithmetic left shift on the value of a variable or property and assigns the result back to the variable or property.  
  
## <a name="syntax"></a>Синтаксис  
  
```vb  
variableorproperty <<= amount  
```  
  
## <a name="parts"></a>Части  
 `variableorproperty`  
 Обязательный. Variable or property of an integral type (`SByte`, `Byte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, or `ULong`).  
  
 `amount`  
 Обязательный. Numeric expression of a data type that widens to `Integer`.  
  
## <a name="remarks"></a>Заметки  
 The element on the left side of the `<<=` operator can be a simple scalar variable, a property, or an element of an array. The variable or property cannot be [ReadOnly](../../../visual-basic/language-reference/modifiers/readonly.md).  
  
 The `<<=` operator first performs an arithmetic left shift on the value of the variable or property. The operator then assigns the result of that operation back to that variable or property.  
  
 Arithmetic shifts are not circular, which means the bits shifted off one end of the result are not reintroduced at the other end. In an arithmetic left shift, the bits shifted beyond the range of the result data type are discarded, and the bit positions vacated on the right are set to zero.  
  
## <a name="overloading"></a>Перегрузка  
 The [<< Operator](../../../visual-basic/language-reference/operators/left-shift-operator.md) can be *overloaded*, which means that a class or structure can redefine its behavior when an operand has the type of that class or structure. Overloading the `<<` operator affects the behavior of the `<<=` operator. If your code uses `<<=` on a class or structure that overloads `<<`, be sure you understand its redefined behavior. Для получения дополнительной информации см. [Operator Procedures](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Пример  
 The following example uses the `<<=` operator to shift the bit pattern of an `Integer` variable left by the specified amount and assign the result to the variable.  
  
 [!code-vb[VbVbalrOperators#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#13)]  
  
## <a name="see-also"></a>См. также

- [Оператор <<](../../../visual-basic/language-reference/operators/left-shift-operator.md)
- [Операторы присваивания](../../../visual-basic/language-reference/operators/assignment-operators.md)
- [Операторы сдвига битов](../../../visual-basic/language-reference/operators/bit-shift-operators.md)
- [Порядок применения операторов в Visual Basic](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [Список операторов, сгруппированных по функциональному назначению](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [Операторы](../../../visual-basic/programming-guide/language-features/statements.md)
