---
title: Использование вариативности для универсальных делегатов Func и Action (Visual Basic)
ms.date: 07/20/2015
ms.assetid: 36c3012f-b39c-493b-b90f-079b5912ac1b
ms.openlocfilehash: a85d6ae2fa32547958e557bbe45b9405e9b660ef
ms.sourcegitcommit: 4f4a32a5c16a75724920fa9627c59985c41e173c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72524240"
---
# <a name="using-variance-for-func-and-action-generic-delegates-visual-basic"></a>Использование вариативности для универсальных делегатов Func и Action (Visual Basic)

Эти примеры показывают, как обеспечить возможность многократного использования методов и сделать код более гибким, используя ковариацию и контравариацию в универсальных методах-делегатах `Func` и `Action`.

Дополнительные сведения о ковариации и контрвариация см. [в разделе вариативность в делегатах (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md).

## <a name="using-delegates-with-covariant-type-parameters"></a>Использование методов-делегатов с параметрами ковариантного типа

Следующий пример иллюстрирует преимущества поддержки ковариации в универсальных методах-делегатах `Func`. Метод `FindByTitle` принимает параметр типа `String` и возвращает объект типа `Employee`. При этом данный метод можно назначить методу-делегату `Func(Of String, Person)`, поскольку `Employee` наследует `Person`.

```vb
' Simple hierarchy of classes.
Public Class Person
End Class

Public Class Employee
    Inherits Person
End Class

Class Finder
    Public Shared Function FindByTitle(
        ByVal title As String) As Employee
        ' This is a stub for a method that returns
        ' an employee that has the specified title.
        Return New Employee
    End Function

    Sub Test()
        ' Create an instance of the delegate without using variance.
        Dim findEmployee As Func(Of String, Employee) =
            AddressOf FindByTitle

        ' The delegate expects a method to return Person,
        ' but you can assign it a method that returns Employee.
        Dim findPerson As Func(Of String, Person) =
            AddressOf FindByTitle

        ' You can also assign a delegate
        ' that returns a more derived type to a delegate
        ' that returns a less derived type.
        findPerson = findEmployee
    End Sub
End Class
```

## <a name="using-delegates-with-contravariant-type-parameters"></a>Использование методов-делегатов с параметрами контравариантного типа

Следующий пример иллюстрирует преимущества поддержки контравариации в универсальных методах-делегатах `Action`. Метод `AddToContacts` принимает параметр типа `Person`. При этом данный метод можно назначить методу-делегату `Action(Of Employee)`, поскольку `Employee` наследует `Person`.

```vb
Public Class Person
End Class

Public Class Employee
    Inherits Person
End Class

Class AddressBook
    Shared Sub AddToContacts(ByVal person As Person)
        ' This method adds a Person object
        ' to a contact list.
    End Sub

    Sub Test()
        ' Create an instance of the delegate without using variance.
        Dim addPersonToContacts As Action(Of Person) =
            AddressOf AddToContacts

        ' The Action delegate expects
        ' a method that has an Employee parameter,
        ' but you can assign it a method that has a Person parameter
        ' because Employee derives from Person.
        Dim addEmployeeToContacts As Action(Of Employee) =
            AddressOf AddToContacts

        ' You can also assign a delegate
        ' that accepts a less derived parameter
        ' to a delegate that accepts a more derived parameter.
        addEmployeeToContacts = addPersonToContacts
    End Sub
End Class
```

## <a name="see-also"></a>См. также

- [Covariance and Contravariance (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/index.md) (Ковариантность и контрвариантность (Visual Basic))
- [Универсальные шаблоны](../../../../standard/generics/index.md)
