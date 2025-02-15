---
title: Как выполнить запрос к ArrayList с помощью LINQ (Visual Basic)
ms.date: 07/20/2015
ms.assetid: 176358a9-d765-4b57-9557-7feb4428138d
ms.openlocfilehash: 5b05fa2ed5c9b3b701571ef4760600caac7193d5
ms.sourcegitcommit: 4f4a32a5c16a75724920fa9627c59985c41e173c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72524150"
---
# <a name="how-to-query-an-arraylist-with-linq-visual-basic"></a>Как выполнить запрос к ArrayList с помощью LINQ (Visual Basic)

При использовании LINQ для запросов к неуниверсальным коллекциям <xref:System.Collections.IEnumerable>, таким как <xref:System.Collections.ArrayList>, необходимо явно объявить тип переменной диапазона, чтобы отразить конкретный тип объектов в коллекции. Например, если имеется <xref:System.Collections.ArrayList> объектов `Student`, [предложение from](../../../../visual-basic/language-reference/queries/from-clause.md) должно выглядеть следующим образом:

```vb
Dim query = From student As Student In arrList
'...
```

Указав тип переменной диапазона, вы приводите каждый элемент в <xref:System.Collections.ArrayList> к `Student`.

Использование явным образом типизированной переменной диапазона в выражении запроса эквивалентно вызову метода <xref:System.Linq.Enumerable.Cast%2A>. Если выполнить приведение не удается, <xref:System.Linq.Enumerable.Cast%2A> создает исключение. Методы <xref:System.Linq.Enumerable.Cast%2A> и <xref:System.Linq.Enumerable.OfType%2A> стандартного оператора запроса используются для работы с неуниверсальными типами <xref:System.Collections.IEnumerable>. В Visual Basic необходимо явно вызвать метод <xref:System.Linq.Enumerable.Cast%2A> в источнике данных, чтобы обеспечить конкретный тип переменной диапазона. Дополнительные сведения см. [в разделе связи типов в операциях запроса (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/type-relationships-in-query-operations.md).

## <a name="example"></a>Пример

В следующем примере показан простой запрос к объекту <xref:System.Collections.ArrayList>. Обратите внимание на то, что в этом примере инициализаторы объектов используются, когда код вызывает метод <xref:System.Collections.ArrayList.Add%2A>, но это не обязательно.

```vb
Imports System.Collections
Imports System.Linq

Module Module1

    Public Class Student
        Public Property FirstName As String
        Public Property LastName As String
        Public Property Scores As Integer()
    End Class

    Sub Main()

        Dim student1 As New Student With {.FirstName = "Svetlana",
                                     .LastName = "Omelchenko",
                                     .Scores = New Integer() {98, 92, 81, 60}}
        Dim student2 As New Student With {.FirstName = "Claire",
                                    .LastName = "O'Donnell",
                                    .Scores = New Integer() {75, 84, 91, 39}}
        Dim student3 As New Student With {.FirstName = "Cesar",
                                    .LastName = "Garcia",
                                    .Scores = New Integer() {97, 89, 85, 82}}
        Dim student4 As New Student With {.FirstName = "Sven",
                                    .LastName = "Mortensen",
                                    .Scores = New Integer() {88, 94, 65, 91}}

        Dim arrList As New ArrayList()
        arrList.Add(student1)
        arrList.Add(student2)
        arrList.Add(student3)
        arrList.Add(student4)

        ' Use an explicit type for non-generic collections
        Dim query = From student As Student In arrList
                    Where student.Scores(0) > 95
                    Select student

        For Each student As Student In query
            Console.WriteLine(student.LastName & ": " & student.Scores(0))
        Next
        ' Keep the console window open in debug mode.
        Console.WriteLine("Press any key to exit.")
        Console.ReadKey()
    End Sub

End Module
' Output:
'   Omelchenko: 98
'   Garcia: 97
```

## <a name="see-also"></a>См. также

- [LINQ to Objects (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)
