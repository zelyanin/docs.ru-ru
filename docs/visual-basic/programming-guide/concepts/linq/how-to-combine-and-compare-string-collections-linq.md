---
title: Практическое руководство. Объединение и сравнение коллекций строк (LINQ) (Visual Basic)
ms.date: 07/20/2015
ms.assetid: 243cfafc-9eaa-4354-a9df-d329f1d39913
ms.openlocfilehash: d521e56f9ae311eea53028ba189416db289839cd
ms.sourcegitcommit: da2dd2772fcf32b44eb18b1cbe8affd17b1753c9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2019
ms.locfileid: "71352879"
---
# <a name="how-to-combine-and-compare-string-collections-linq-visual-basic"></a>Практическое руководство. Объединение и сравнение коллекций строк (LINQ) (Visual Basic)

В этом примере показано объединение файлов, содержащих строки текста, и последующая сортировка результатов. В частности, показано выполнение простого сцепления, объединения и пересечения в двух наборах строк текста.

### <a name="to-set-up-the-project-and-the-text-files"></a>Настройка проекта и текстовых файлов

1. Скопируйте эти имена в текстовый файл с именем names1.txt и сохраните его в папке проекта:

    ```text
    Bankov, Peter
    Holm, Michael
    Garcia, Hugo
    Potra, Cristina
    Noriega, Fabricio
    Aw, Kam Foo
    Beebe, Ann
    Toyoshima, Tim
    Guy, Wey Yuan
    Garcia, Debra
    ```

2. Скопируйте эти имена в текстовый файл с именем names2.txt и сохраните его в папке проекта. Обратите внимание, что в этих двух файлах имеются общие имена.

    ```text
    Liu, Jinghao
    Bankov, Peter
    Holm, Michael
    Garcia, Hugo
    Beebe, Ann
    Gilchrist, Beth
    Myrcha, Jacek
    Giakoumakis, Leo
    McLin, Nkenge
    El Yassir, Mehdi
    ```

## <a name="example"></a>Пример

```vb
Class ConcatenateStrings
    Shared Sub Main()

        ' Create the IEnumerable data sources.
        Dim fileA As String() = System.IO.File.ReadAllLines("../../../names1.txt")
        Dim fileB As String() = System.IO.File.ReadAllLines("../../../names2.txt")

        ' Simple concatenation and sort.
        Dim concatQuery = fileA.Concat(fileB).OrderBy(Function(name) name)

        ' Pass the query variable to another function for execution
        OutputQueryResults(concatQuery, "Simple concatenation and sort. Duplicates are preserved:")

        ' New query. Concatenate files and remove duplicates
        Dim uniqueNamesQuery = fileA.Union(fileB).OrderBy(Function(name) name)
        OutputQueryResults(uniqueNamesQuery, "Union removes duplicate names:")

        ' New query. Find the names that occur in both files.
        Dim commonNamesQuery = fileA.Intersect(fileB)
        OutputQueryResults(commonNamesQuery, "Merge based on intersect: ")

        ' New query in three steps for better readability
        ' First filter each list separately

        Dim nameToSearch As String = "Garcia"
        Dim mergeQueryA As IEnumerable(Of String) = From name In fileA
                          Let n = name.Split(New Char() {","})
                          Where n(0) = nameToSearch
                          Select name

        Dim mergeQueryB = From name In fileB
                          Let n = name.Split(New Char() {","})
                          Where n(0) = nameToSearch
                          Select name

        ' Create a new query to concatenate and sort results. Duplicates are removed in Union.
        ' Note that none of the queries actually executed until the call to OutputQueryResults.
        Dim mergeSortQuery = mergeQueryA.Union(mergeQueryB).OrderBy(Function(str) str)

        ' Now execute mergeSortQuery
        OutputQueryResults(mergeSortQuery, "Concat based on partial name match """ & nameToSearch & """ from each list:")

        ' Keep console window open in debug mode.
        Console.WriteLine("Press any key to exit.")
        Console.ReadKey()

    End Sub

    Shared Sub OutputQueryResults(ByVal query As IEnumerable(Of String), ByVal message As String)

        Console.WriteLine(System.Environment.NewLine & message)
        For Each item As String In query
            Console.WriteLine(item)
        Next
        Console.WriteLine(query.Count & " total names in list")

    End Sub
End Class
' Output:

' Simple concatenation and sort. Duplicates are preserved:
' Aw, Kam Foo
' Bankov, Peter
' Bankov, Peter
' Beebe, Ann
' Beebe, Ann
' El Yassir, Mehdi
' Garcia, Debra
' Garcia, Hugo
' Garcia, Hugo
' Giakoumakis, Leo
' Gilchrist, Beth
' Guy, Wey Yuan
' Holm, Michael
' Holm, Michael
' Liu, Jinghao
' McLin, Nkenge
' Myrcha, Jacek
' Noriega, Fabricio
' Potra, Cristina
' Toyoshima, Tim
' 20 total names in list

' Union removes duplicate names:
' Aw, Kam Foo
' Bankov, Peter
' Beebe, Ann
' El Yassir, Mehdi
' Garcia, Debra
' Garcia, Hugo
' Giakoumakis, Leo
' Gilchrist, Beth
' Guy, Wey Yuan
' Holm, Michael
' Liu, Jinghao
' McLin, Nkenge
' Myrcha, Jacek
' Noriega, Fabricio
' Potra, Cristina
' Toyoshima, Tim
' 16 total names in list

' Merge based on intersect:
' Bankov, Peter
' Holm, Michael
' Garcia, Hugo
' Beebe, Ann
' 4 total names in list

' Concat based on partial name match "Garcia" from each list:
' Garcia, Debra
' Garcia, Hugo
' 2 total names in list
```

## <a name="compiling-the-code"></a>Компиляция кода

Создайте проект консольного приложения VB.NET с инструкцией `Imports` для пространства имен System. LINQ.

## <a name="see-also"></a>См. также

- [LINQ и строки (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-and-strings.md)
- [LINQ и каталоги файлов (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-and-file-directories.md)
