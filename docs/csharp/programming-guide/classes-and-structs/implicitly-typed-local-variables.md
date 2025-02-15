---
title: Руководство по программированию на C#. Неявно типизированные локальные переменные
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- implicitly-typed local variables [C#]
- var [C#]
ms.assetid: b9218fb2-ef5d-4814-8a8e-2bc29b0bbc9b
ms.openlocfilehash: 7010c38797ab64e5106c96c06cd814c143ca9c24
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2019
ms.locfileid: "73419379"
---
# <a name="implicitly-typed-local-variables-c-programming-guide"></a>Неявно типизированные локальные переменные (руководство по программированию на C#)

Локальные переменные можно объявлять без указания конкретного типа. Ключевое слово `var` указывает, что компилятор должен вывести тип переменной из выражения справа от оператора инициализации. Выведенный тип может быть встроенным, анонимным, определяемым пользователем либо типом, определяемым в библиотеке классов .NET Framework. Дополнительные сведения об инициализации массивов с `var` см. в разделе [Неявно типизированные массивы](../arrays/implicitly-typed-arrays.md).

В приведенных ниже примерах показаны различные способы объявления локальных переменных с помощью `var`:

[!code-csharp[csProgGuideLINQ#43](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#43)]

Важно понимать, что ключевое слово `var` не означает "variant" и не означает, что переменная является слабо типизированной или имеет позднее связывание. Он указывает только на то, что компилятор определяет и назначает наиболее подходящий тип.

Ключевое слово `var` можно использовать в следующих контекстах:

- С локальными переменными (переменными, объявленными в области метода), как показано в предыдущем примере.

- В операторе инициализации [for](../../language-reference/keywords/for.md).

    ```csharp
    for(var x = 1; x < 10; x++)
    ```

- В операторе инициализации [foreach](../../language-reference/keywords/foreach-in.md).

    ```csharp
    foreach(var item in list){...}
    ```

- В операторе [using](../../language-reference/keywords/using-statement.md).

    ```csharp
    using (var file = new StreamReader("C:\\myfile.txt")) {...}
    ```

Дополнительные сведения см. в разделе [Практическое руководство. Руководство по программированию на C#. Использование явно введенных локальных переменных и массивов в выражении запроса](how-to-use-implicitly-typed-local-variables-and-arrays-in-a-query-expression.md).

## <a name="var-and-anonymous-types"></a>Переменная var и анонимные типы

Во многих случаях переменная `var` не является обязательной и предназначена только для синтаксического удобства. Тем не менее если переменная инициализируется с помощью анонимного типа и вам потребуется доступ к свойствам объекта на более позднем этапе, ее необходимо объявить как `var`. Это — распространенный сценарий в выражениях запросов [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)]. Дополнительные сведения см. в разделе [Анонимные типы](anonymous-types.md).

С точки зрения исходного кода анонимный тип безымянен. Таким образом, если переменная запроса инициализирована с помощью `var`, то единственный способ получить доступ к свойствам в возвращаемой последовательности объектов — это использовать `var` как тип переменной итерации в операторе `foreach`.

[!code-csharp[csProgGuideLINQ#44](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#44)]

## <a name="remarks"></a>Примечания

В объявлениям неявно типизированных переменных применяются следующие ограничения:

- `var` можно использовать только в том случае, если локальная переменная объявляется и инициализируется в одном и том же операторе; переменная не может инициализироваться в нулевое значение, в группу методов или в анонимную функцию.

- `var` нельзя применять к полям в области видимости класса.

- Переменные, объявленные с помощью `var`, нельзя использовать в выражении инициализации. Другими словами, это выражение является допустимым выражением `: int i = (i = 20);`, но вызывает ошибку времени компиляции: `var i = (i = 20);`

- Инициализировать сразу несколько неявно типизированных переменных в одном и том же операторе нельзя.

- Если тип с именем `var` входит в область видимости, то ключевое слово `var` разрешится в это имя типа и не будет обрабатываться как часть объявления неявно типизированной локальной переменной.

Неявное типизирование с ключевым словом `var` может применяться только к переменным в области локального метода. Неявное типизирование недоступно для полей класса C#, так как при обработке кода компилятор столкнется с логическим парадоксом: компилятор должен знать тип поля, но он не может определить тип, пока не проанализирует выражение присваивания, и не может вычислить выражение, не зная тип. Рассмотрим следующий код.

```csharp
private var bookTitles;
```

`bookTitles` — это поле класса, которому присваивается тип `var`. Так как поле не имеет выражения для оценки, то компилятор не сможет вывести тип `bookTitles`. Кроме того, добавления выражения в поле (так же, как для локальной переменной) тоже недостаточно:

```csharp
private var bookTitles = new List<string>();
```

Когда компилятор обнаруживает поля во время компиляции кода, он записывает тип каждого поля перед обработкой любого выражения, связанного с ним. Компилятор обнаруживает тот же парадокс при попытке анализа `bookTitles`: он должен знать тип поля, но обычно он определяет тип `var` путем анализа выражения, который невозможно определить, если заранее не знать тип.

Переменную `var` можно также использовать в выражениях запросов, где точный сконструированный тип переменной запроса определить непросто. Подобная ситуация может возникнуть в операциях группировки и сортировки.

Кроме того, ключевое слово `var` может пригодиться, если конкретный тип переменной сложно набрать с клавиатуры, а также если он очевиден либо затрудняет чтение кода. Использовать `var` таким образом можно, например, с вложенными универсальными типами — подобные типы применяются в групповых операциях. В следующем запросе переменная запроса имеет тип `IEnumerable<IGrouping<string, Student>>`. Если вы и другие пользователи, которые работают с кодом, это понимаете, использовать неявный ввод для удобства и краткости кода можно без проблем.

[!code-csharp[cscsrefQueryKeywords#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#13)]

При этом использование ключевого слова `var` может усложнить понимание вашего кода для других разработчиков. В связи с этим в документации по C# `var` применяется только в тех случаях, когда это необходимо.

## <a name="see-also"></a>См. также

- [Справочник по C#](../../language-reference/index.md)
- [Неявно типизированные массивы](../arrays/implicitly-typed-arrays.md)
- [Практическое руководство. Использование явно введенных локальных переменных и массивов в выражении запроса](how-to-use-implicitly-typed-local-variables-and-arrays-in-a-query-expression.md)
- [Анонимные типы](anonymous-types.md)
- [Инициализаторы объектов и коллекций](object-and-collection-initializers.md)
- [var](../../language-reference/keywords/var.md)
- [LINQ в C#](../../linq/index.md)
- [Встроенный язык запросов LINQ](../../linq/index.md)
- [for](../../language-reference/keywords/for.md)
- [foreach, in](../../language-reference/keywords/foreach-in.md)
- [Оператор using](../../language-reference/keywords/using-statement.md)
