---
title: Выражение Function (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- Function expression [Visual Basic]
- functions [Visual Basic], function expressions
- lambda expressions [Visual Basic], function expression
ms.assetid: e8a47a45-4b8a-4f45-a623-7653625dffbc
ms.openlocfilehash: 0ab4a77395b478df06f34240212438f3e6e18f6e
ms.sourcegitcommit: 35da8fb45b4cca4e59cc99a5c56262c356977159
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2019
ms.locfileid: "71592206"
---
# <a name="function-expression-visual-basic"></a>Выражение Function (Visual Basic)
Объявляет параметры и код, определяющие лямбда-выражение функции.  
  
## <a name="syntax"></a>Синтаксис  
  
```vb  
Function ( [ parameterlist ] ) expression  
- or -  
Function ( [ parameterlist ] )  
  [ statements ]  
End Function  
```  
  
## <a name="parts"></a>Части  
  
|Термин|Определение|  
|---|---|  
|`parameterlist`|Необязательный параметр. Список имен локальных переменных, представляющих параметры этой процедуры. Круглые скобки должны присутствовать, даже если список пуст. См. [список параметров](../../../visual-basic/language-reference/statements/parameter-list.md).|  
|`expression`|Обязательный. Одно выражение. Тип выражения является типом возвращаемого значения функции.|  
|`statements`|Обязательный. Список инструкций, которые возвращают значение с помощью оператора `Return`. (См. раздел [оператор return](../../../visual-basic/language-reference/statements/return-statement.md).) Тип возвращаемого значения — это возвращаемый тип функции.|  
  
## <a name="remarks"></a>Примечания  
 *Лямбда-выражение* — это функция без имени, которая вычисляет и возвращает значение. Лямбда-выражение можно использовать в любом месте, где можно использовать тип делегата, кроме аргумента для `RemoveHandler`. Дополнительные сведения о делегатах и использовании лямбда-выражений с делегатами см. в разделе [оператор Delegate](../../../visual-basic/language-reference/statements/delegate-statement.md) и [Преобразование неявного делегата](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md).  
  
## <a name="lambda-expression-syntax"></a>Синтаксис лямбда-выражений  
 Синтаксис лямбда-выражения напоминает стандартную функцию. Ниже приведены различия.  
  
- Лямбда-выражение не имеет имени.  
  
- Лямбда-выражения не могут иметь модификаторы, такие как `Overloads` или `Overrides`.  
  
- Лямбда-выражения не используют предложение `As` для обозначения возвращаемого типа функции. Вместо этого тип выводится из значения, которое принимает текст однострочного лямбда-выражения, или возвращаемое значение многострочного лямбда-выражения. Например, если текст однострочного лямбда-выражения имеет `Where cust.City = "London"`, его возвращаемым типом является `Boolean`.  
  
- Тело однострочного лямбда-выражения должно быть выражением, а не оператором. Тело может состоять из вызова процедуры функции, но не вызова процедуры подпрограммы.  
  
- Либо все параметры должны иметь указанные типы данных, либо все должны быть выведены.  
  
- Параметры Optional и ParamArray не разрешены.  
  
- Универсальные параметры не допускаются.  
  
## <a name="example"></a>Пример  
 В следующих примерах показано два способа создания простых лямбда-выражений. Первый использует `Dim` для предоставления имени функции. Для вызова функции вы отправляете значение для параметра.  
  
 [!code-vb[VbVbalrLambdas#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#1)]  
  
 [!code-vb[VbVbalrLambdas#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#2)]  
  
## <a name="example"></a>Пример  
 Кроме того, можно одновременно объявить и запустить функцию.  
  
 [!code-vb[VbVbalrLambdas#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#3)]  
  
## <a name="example"></a>Пример  
 Ниже приведен пример лямбда-выражения, которое увеличивает свой аргумент и возвращает значение. В примере показан синтаксис однострочного и многострочного лямбда-выражения для функции. Дополнительные примеры см. в разделе [лямбда-выражения](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md).  
  
 [!code-vb[VbVbalrLambdas#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#14)]  
  
## <a name="example"></a>Пример  
 Лямбда-выражения лежат в основе многих операторов запроса в [!INCLUDE[vbteclinqext](~/includes/vbteclinqext-md.md)], и их можно использовать явно в запросах, основанных на методах. В следующем примере показан типичный запрос [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)], а затем перевод запроса в формат метода.  
  
```vb  
Dim londonCusts = From cust In db.Customers  
                       Where cust.City = "London"  
                       Select cust  
  
' This query is compiled to the following code:  
Dim londonCusts = db.Customers.  
                  Where(Function(cust) cust.City = "London").  
                  Select(Function(cust) cust)  
```  
  
 Дополнительные сведения о методах запросов см. в разделе [запросы](../../../visual-basic/language-reference/queries/index.md). Дополнительные сведения о стандартных операторах запросов см. в разделе [Общие сведения о стандартных операторах запросов](../../programming-guide/concepts/linq/standard-query-operators-overview.md).  
  
## <a name="see-also"></a>См. также

- [Оператор Function](../../../visual-basic/language-reference/statements/function-statement.md)
- [Лямбда-выражения](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)
- [Операторы и выражения](../../../visual-basic/programming-guide/language-features/operators-and-expressions/index.md)
- [Операторы](../../../visual-basic/programming-guide/language-features/statements.md)
- [Сравнения значений](../../../visual-basic/programming-guide/language-features/operators-and-expressions/value-comparisons.md)
- [Логические выражения](../../../visual-basic/programming-guide/language-features/operators-and-expressions/boolean-expressions.md)
- [Оператор If](../../../visual-basic/language-reference/operators/if-operator.md)
- [Неявное преобразование делегата](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
