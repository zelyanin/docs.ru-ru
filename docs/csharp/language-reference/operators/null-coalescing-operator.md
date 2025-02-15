---
title: ?? и ??= — справочник по C#
ms.custom: seodec18
ms.date: 09/10/2019
f1_keywords:
- ??_CSharpKeyword
- ??=_CSharpKeyword
helpviewer_keywords:
- null-coalescing operator [C#]
- ?? operator [C#]
- null-coalescing assignment [C#]
- ??= operator [C#]
ms.assetid: 088b1f0d-c1af-4fe1-b4b8-196fd5ea9132
ms.openlocfilehash: 1e94038a41a6a6cc19be6c67bff2891397793fb3
ms.sourcegitcommit: 33c8d6f7342a4bb2c577842b7f075b0e20a2fa40
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2019
ms.locfileid: "70924680"
---
# <a name="-and--operators-c-reference"></a>?? и ??= (справочник по C#)

Оператор объединения с NULL `??` возвращает значение своего операнда слева, если его значение не равно `null`. В противном случае он вычисляет операнд справа и возвращает его результат. Оператор `??` не выполняет оценку своего операнда справа, если его операнд слева имеет значение, отличное от NULL.

Начиная с C# 8.0 можно использовать оператор присваивания объединения со значением NULL `??=` для присваивания значения правого операнда левому операнду только в том случае, если левый операнд принимает значение `null`. Оператор `??=` не выполняет оценку своего операнда справа, если его операнд слева имеет значение, отличное от NULL.

[!code-csharp[null-coalescing assignment](~/samples/csharp/language-reference/operators/NullCoalescingOperator.cs#Assignment)]

Левый операнд оператора `??=` должен быть переменной, [свойством](../../programming-guide/classes-and-structs/properties.md) или элементом [индексатора](../../programming-guide/indexers/index.md). Дополнительные сведения о присваивании объединения со значением NULL см. в [примечании к предлагаемой функции](~/_csharplang/proposals/csharp-8.0/null-coalescing-assignment.md).

В C# 7.3 и более ранних версий левый операнд оператора `??` должен иметь либо ссылочный тип, либо [тип значения, допускающий значение NULL](../../programming-guide/nullable-types/index.md). Начиная с C# 8.0 это требование заменяется следующим: тип левого операнда операторов `??` и `??=` не может быть типом значения, не допускающим значение NULL. В частности, можно использовать операторы объединения со значением NULL с неограниченными параметрами типа в C# 8.0 и более поздних версиях:

[!code-csharp[unconstrained type parameter](~/samples/csharp/language-reference/operators/NullCoalescingOperator.cs#UnconstrainedType)]

Операторы объединения со значением NULL являются правоассоциативными. То есть выражения в форме

```csharp
a ?? b ?? c
d ??= e ??= f
```

вычисляются как

```csharp
a ?? (b ?? c)
d ??= (e ??= f)
```

## <a name="examples"></a>Примеры

Операторы `??` и `??=` могут быть полезны в таких случаях:

- В выражениях с [NULL-условными операторами ?. и ?[]](member-access-operators.md#null-conditional-operators--and-) можно использовать оператор объединения с NULL, чтобы задать альтернативное выражение для оценки на случай, если результат выражения с NULL-условной операцией будет равен `null`.

  [!code-csharp-interactive[with null-conditional](~/samples/csharp/language-reference/operators/NullCoalescingOperator.cs#WithNullConditional)]

- При работе с [типами, допускающими значение NULL](../../programming-guide/nullable-types/index.md), и если нужно указать значение базового типа значения, используйте оператор объединения с NULL для указания значения, возвращаемого в том случае, если значение типа, допускающего значение NULL, равно `null`.

  [!code-csharp-interactive[with nullable types](~/samples/csharp/language-reference/operators/NullCoalescingOperator.cs#WithNullableTypes)]

  Используйте метод <xref:System.Nullable%601.GetValueOrDefault?displayProperty=nameWithType>, если значение, которое будет использоваться, когда значение типа, допускающего значение NULL, равно `null`, должно быть значением по умолчанию базового типа.

- Начиная с C# 7.0 можно сократить код проверки аргументов, используя [выражение `throw`](../keywords/throw.md#the-throw-expression) в качестве правого операнда оператора объединения с NULL:

  [!code-csharp[with throw expression](~/samples/csharp/language-reference/operators/NullCoalescingOperator.cs#WithThrowExpression)]

  В предыдущем примере также демонстрируются способы определения свойства с помощью [членов, заданных выражениями](../../programming-guide/statements-expressions-operators/expression-bodied-members.md).

- Начиная с C# 8.0 можно использовать оператор `??=` для замены кода формы

  ```csharp
  if (variable is null)
  {
      variable = expression;
  }
  ```

  на новый код:

  ```csharp
  variable ??= expression;
  ```

## <a name="operator-overloadability"></a>Возможность перегрузки оператора

Операторы `??` и `??=` не могут быть перегружены.

## <a name="c-language-specification"></a>Спецификация языка C#

Дополнительные сведения об операторе `??` см. в разделе об [операторе объединения со значением NULL](~/_csharplang/spec/expressions.md#the-null-coalescing-operator) в [спецификации языка C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>См. также

- [справочник по C#](../index.md)
- [Операторы в C#](index.md)
- [Операторы ?. и ?[]](member-access-operators.md#null-conditional-operators--and-)
- [Оператор ?](conditional-operator.md)
