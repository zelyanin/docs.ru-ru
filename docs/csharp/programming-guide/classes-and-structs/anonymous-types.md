---
title: Руководство по программированию на C#. Анонимные типы
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- anonymous types [C#]
- C# Language, anonymous types
ms.assetid: 59c9d7a4-3b0e-475e-b620-0ab86c088e9b
ms.openlocfilehash: 93f02b8a0f828be89c6a1b7bfcdc6ba2a2a93e81
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/19/2019
ms.locfileid: "69597190"
---
# <a name="anonymous-types-c-programming-guide"></a>Анонимные типы (Руководство по программированию в C#)

Анонимные типы позволяют легко инкапсулировать свойства только для чтения в один объект без необходимости предварительного определения типа. Имя типа создается компилятором и недоступно на уровне исходного кода. Тип каждого свойства выводится компилятором.  
  
 Анонимные типы создаются с помощью оператора [new](../../language-reference/operators/new-operator.md) и инициализатора объекта. Дополнительные сведения об инициализаторах объектов см. в статье [Инициализаторы объектов и коллекций](./object-and-collection-initializers.md).  
  
 В следующем примере показан анонимный тип, инициализированный с помощью двух свойств — `Amount` и `Message`.  
  
```csharp  
var v = new { Amount = 108, Message = "Hello" };  
  
// Rest the mouse pointer over v.Amount and v.Message in the following  
// statement to verify that their inferred types are int and string.  
Console.WriteLine(v.Amount + v.Message);  
```  
  
 Обычно анонимные типы используются в предложении [select](../../language-reference/keywords/select-clause.md) выражения запроса для возврата поднабора свойств из каждого объекта в исходной последовательности. Дополнительные сведения о запросах см. в разделе [Выражения запросов LINQ](../linq-query-expressions/index.md).  
  
 Анонимные типы содержат один или несколько публичных свойств только для чтения. Другие члены класса, например методы или события, недопустимы. Выражение, которое используется для инициализации свойства, не может быть `null`, анонимной функцией или типом указателя.  
  
 Наиболее частый сценарий — это инициализация анонимного типа со свойствами из другого типа. В следующем примере предполагается, что существует класс с именем `Product`. Класс `Product` включает свойства `Color` и `Price`, а также другие свойства, в которых вы не заинтересованы. Переменная `products` является коллекцией объектов `Product`. Объявление анонимного типа запускается с помощью ключевого слова `new`. Объявление инициализирует новый тип, который использует только два свойства из `Product`. Это приводит к тому, что для возврата остается меньшее количество данных.  
  
 Если имена членов в анонимном типе не указаны, компилятор присваивает членам анонимного типа такие же имена, как у свойств, используемых для их инициализации. Необходимо указать имя для свойства, инициализируемого с помощью выражения, как показано в предыдущем примере. В следующем примере используются имена свойств анонимного типа `Color` и `Price`.  
  
 [!code-csharp[csRef30Features#81](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csRef30Features/CS/csref30.cs#81)]  
  
 Обычно, если для инициализации переменной используется анонимный тип, необходимо обозначить ее как неявно типизированную переменную с помощью ключевого слова [var](../../language-reference/keywords/var.md). Имя типа не может быть указано в объявлении переменной, так как доступ к базовому имени анонимного типа имеет только компилятор. Дополнительные сведения о `var` см. в разделе [Неявно типизированные локальные переменные](./implicitly-typed-local-variables.md).  
  
 Вы можете создать массив анонимно типизированных элементов, объединив неявно типизированные локальные переменные и неявно типизированный массив, как показано в следующем примере.  
  
```csharp  
var anonArray = new[] { new { name = "apple", diam = 4 }, new { name = "grape", diam = 1 }};  
```  
  
## <a name="remarks"></a>Примечания  
 Анонимные типы являются типами [class](../../language-reference/keywords/class.md), прямыми производными от типа [object](../../language-reference/keywords/object.md), и не могут быть приведены ни к какому иному типу, кроме [object](../../language-reference/keywords/object.md). Компилятор назначает имя для каждого анонимного типа, несмотря на то что для вашего приложения он недоступен. С точки зрения среды CLR анонимный тип не отличается от других ссылочных типов.  
  
 Если два или несколько инициализаторов анонимных объектов в сборке указывают на последовательность свойств, идущих в том же порядке и имеющих те же типы и имена, компилятор обрабатывает объекты как экземпляры одного типа. Они используют одни и те же сведения типа, созданные компилятором.  
  
 Никакое поле, свойство, событие или тип возвращаемого значения метода невозможно объявить, используя анонимный тип. Точно так же нельзя объявить с помощью анонимного типа ни один формальный параметр метода, свойства, конструктора или индексатора. Для передачи анонимного типа или коллекции, содержащей анонимные типы, как аргумента для метода можно использовать этот параметр в качестве объекта типа. Однако подобное решение противоречит цели строгой типизации. Если вам нужно сохранить результаты запроса или передать их за пределы метода, используйте вместо анонимного типа структуру или класс, названные обычным образом.  
  
 Так как методы <xref:System.Object.Equals%2A> и <xref:System.Object.GetHashCode%2A> в анонимных типах определяются через  методы `Equals` и `GetHashCode` свойств, два экземпляра одного и того же анонимного типа равны, только если равны их свойства.  
  
## <a name="see-also"></a>См. также

- [Руководство по программированию на C#](../index.md)
- [Инициализаторы объектов и коллекций](./object-and-collection-initializers.md)
- [Приступая к работе с LINQ в C#](../concepts/linq/getting-started-with-linq.md)
- [Выражения запросов LINQ](../linq-query-expressions/index.md)
