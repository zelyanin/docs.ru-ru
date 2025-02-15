---
title: Операторы true и false. Справочник по C#
ms.custom: seodec18
ms.date: 12/10/2018
helpviewer_keywords:
- false operator [C#]
- true operator [C#]
ms.assetid: 81a888fd-011e-4589-b242-6c261fea505e
ms.openlocfilehash: 7cbfca932b5f9f8a6f658e84204da5005da5ffb8
ms.sourcegitcommit: eaa6d5cd0f4e7189dbe0bd756e9f53508b01989e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67609837"
---
# <a name="true-and-false-operators-c-reference"></a>Операторы true и false (справочник по C#)

Оператор `true` возвращает [логическое](../keywords/bool.md) значение `true`, указывая, что операнд имеет значение true. Оператор `false` возвращает значение `bool` `true`, указывая, что операнд имеет значение false. Операторы `true` и `false` не обязательно дополняют друг друга. То есть оба оператора `true` и `false` могут вернуть значение `bool` `false` для одного операнда. Если тип определяет один из двух операторов, он должен также определять другой оператор.

> [!TIP]
> Используйте тип `bool?`, если вам нужно использовать трехзначную логику, например при работе с базами данных, которые поддерживают трехзначный логический тип. В C# предоставляются операторы `&` и `|`, которые поддерживают троичную логику с операндами `bool?`. См. подробнее о [логических операторах, поддерживающих значение NULL](boolean-logical-operators.md#nullable-boolean-logical-operators) в описании [логических операторов](boolean-logical-operators.md).

## <a name="boolean-expressions"></a>Логические выражения

Тип с определенным оператором `true` может быть типом результата управляющего условного выражения в операторах [if](../keywords/if-else.md), [do](../keywords/do.md), [while](../keywords/while.md) и [for](../keywords/for.md) и [условном операторе`?:`](conditional-operator.md). Дополнительные сведения см. в описании [логических выражений](~/_csharplang/spec/expressions.md#boolean-expressions) в [спецификации языка C#](~/_csharplang/spec/introduction.md).

## <a name="user-defined-conditional-logical-operators"></a>Пользовательские условные логические операторы

Если тип с определенными операторами `true` и `false` [перегружает](operator-overloading.md) логические операторы [OR](boolean-logical-operators.md#logical-or-operator-) `|` или [AND](boolean-logical-operators.md#logical-and-operator-) `&` определенным образом, [условные логические операторы OR](boolean-logical-operators.md#conditional-logical-or-operator-) `||` или [AND](boolean-logical-operators.md#conditional-logical-and-operator-) `&&` соответственно могут вычисляться для операндов этого типа. Дополнительные сведения см. в разделе [Пользовательские условные логические операторы](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators) в [Спецификации языка C#](~/_csharplang/spec/introduction.md).

## <a name="example"></a>Пример

В следующем примере представлен тип, который определяет операторы `true` и `false`. Этот тип также перегружает логический оператор И `&` таким образом, что оператор `&&` может быть вычислен для операндов этого типа.

[!code-csharp[true and false operators example](~/samples/csharp/language-reference/operators/TrueFalseOperators.cs)]

Обратите внимание на поведение сокращенного вычисления оператора `&&`. Когда метод `GetFuelLaunchStatus` возвращает `LaunchStatus.Red`, правый операнд оператора `&&` не вычисляется. Это обусловлено тем, что `LaunchStatus.Red` имеет значение false. Результат логического оператора И не зависит от значения правого операнда. Выходные данные примера могут быть следующими:

```console
Getting fuel launch status...
Wait!
```

## <a name="see-also"></a>См. также

- [справочник по C#](../index.md)
- [Операторы в C#](index.md)
- [Литерал true](../keywords/true-literal.md)
- [Литерал false](../keywords/false-literal.md)
