---
title: Практическое руководство. Руководство по программированию на C#. Возвращение подмножества свойств элементов в запросе
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- anonymous types [C#], for subsets of element properties
ms.assetid: fabdf349-f443-4e3f-8368-6c471be1dd7b
ms.openlocfilehash: 196383731507137bf4309d38d27b36f29b23a06c
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2019
ms.locfileid: "73419302"
---
# <a name="how-to-return-subsets-of-element-properties-in-a-query-c-programming-guide"></a>Практическое руководство. Руководство по программированию на C#. Возвращение подмножества свойств элементов в запросе
Используйте анонимный тип в выражении запроса, если выполняются оба следующих условия:  
  
- требуется возвращать только некоторые свойства каждого исходного элемента;  
  
- не требуется хранить результаты запроса за пределами области метода, в котором выполняется запрос.  
  
 Если требуется возвращать только одно свойство или поле из каждого исходного элемента, вы можете использовать оператор "точка" в предложении `select`. Например, чтобы вернуть только `ID` для каждого элемента `student`, напишите предложение `select` следующим образом:  
  
```csharp  
select student.ID;  
```  
  
## <a name="example"></a>Пример  
 В следующем примере показано, как использовать анонимный тип для возвращения только конкретного набора свойств каждого исходного элемента, соответствующего указанному условию.  
  
 [!code-csharp[csProgGuideLINQ#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#31)]  
  
 Обратите внимание, что анонимный тип использует имена исходных элементов для соответствующих свойств, если имена не заданы. Чтобы присваивать новые имена свойствам в анонимном типе, напишите инструкцию `select` следующим образом:  
  
```csharp  
select new { First = student.FirstName, Last = student.LastName };  
```  
  
 Если вы попробуете сделать это в предыдущем примере, также необходимо изменить инструкцию `Console.WriteLine`:  
  
```csharp  
Console.WriteLine(student.First + " " + student.Last);  
```  
  
## <a name="compiling-the-code"></a>Компиляция кода  
  
Чтобы выполнить этот код, скопируйте и вставьте класс в консольное приложение C# с директивой `using` для пространства имен System.Linq.
  
## <a name="see-also"></a>См. также

- [Руководство по программированию на C#](../index.md)
- [Анонимные типы](./anonymous-types.md)
- [LINQ в C#](../../linq/index.md)
