---
title: Создание исходного документа Office Open XML (Visual Basic)
ms.date: 07/20/2015
ms.assetid: 61ccd6fb-0c47-4075-afdf-5b5021330f21
ms.openlocfilehash: 75030f3d1c2940cc84f81b85dca921497137439f
ms.sourcegitcommit: da2dd2772fcf32b44eb18b1cbe8affd17b1753c9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2019
ms.locfileid: "71352944"
---
# <a name="creating-the-source-office-open-xml-document-visual-basic"></a>Создание исходного документа Office Open XML (Visual Basic)
В этом разделе показано создание документа Office Open XML WordprocessingML, который используется в примерах этого учебника. Если следовать приведенным ниже инструкциям, выходные данные будут соответствовать выходным данным каждого примера.  
  
 Тем не менее примеры в этом учебнике работают с любым допустимым документом WordprocessingML.  
  
 Чтобы создать документ, который используется в этом учебнике, необходимо иметь установленный выпуск 2007 системы Microsoft Office или более поздней версии либо Microsoft Office 2003 с пакетом обеспечения совместимости Microsoft Office для форматов файлов Word, Excel и PowerPoint 2007.  
  
## <a name="creating-the-wordprocessingml-document"></a>Создание документа WordprocessingML  
  
#### <a name="to-create-the-wordprocessingml-document"></a>Создание документа WordprocessingML  
  
1. Создайте документ Microsoft Word.  
  
2. Вставьте в новый документ следующий текст.  
  
    ```text  
    Parsing WordprocessingML with LINQ to XML  
  
    The following example prints to the console.  
  
    Imports System  
  
    Class Program  
        Public Shared  Sub Main(ByVal args() As String)  
            Console.WriteLine("Hello World")  
        End Sub  
    End Class  
  
    This example produces the following output:  
  
    Hello World  
    ```  
  
3. Отформатируйте первую строку стилем «Заголовок 1».  
  
4. Выберите строки, которые содержат код Visual Basic. Первая строка начинается с ключевого слова `Imports`. Последняя строка — "конечный класс". Отформатируйте эти строки шрифтом courier. Создайте из них новый стиль и присвойте ему имя «Code».  
  
5. Наконец, выделите всю строку, содержащую выходные данные, и отформатируйте ее стилем `Code`.  
  
6. Сохраните документ с именем SampleDoc.docx.  
  
    > [!NOTE]
    > Если используется Microsoft Word 2003, в раскрывающемся списке **Тип файла** выберите **Документ Word 2007**.  
  
## <a name="see-also"></a>См. также

- [Учебник. Обработка содержимого в документе WordprocessingML (Visual Basic) ](../../../../visual-basic/programming-guide/concepts/linq/tutorial-manipulating-content-in-a-wordprocessingml-document.md)
