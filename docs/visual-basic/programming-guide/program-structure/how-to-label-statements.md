---
title: Практическое руководство. Операторы меток (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- colons (:)
- statements [Visual Basic], labels
- ': separator character'
- Visual Basic code, labeling statements
ms.assetid: 38f1ff43-2054-42cb-963b-1998e60c6ed4
ms.openlocfilehash: 9a5f2039716a18011cac3dfd9b011d5b3868c294
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2019
ms.locfileid: "71054055"
---
# <a name="how-to-label-statements-visual-basic"></a>Практическое руководство. Операторы меток (Visual Basic)

Блоки инструкций состоят из строк кода, разделенных двоеточиями. Строки кода, которым предшествует идентифицирующая строка или целое число, говорят о *метке*. Метки операторов используются для пометки строки кода, чтобы она определялась для использования с такими операторами `On Error Goto`, как.

Метки могут быть либо допустимыми Visual Basic идентификаторами, например, для идентификации программных элементов, либо целочисленными литералами. Метка должна располагаться в начале строки исходного кода, после которой должно следовать двоеточие, независимо от того, за чем следует оператор в той же строке.

Компилятор определяет метки, проверяя, соответствует ли начало строки любому уже определенному идентификатору. Если это не так, компилятор предполагает, что это метка.

Метки имеют собственную область объявления и не мешают другим идентификаторам. Областью действия метки является тело метода. Объявление метки имеет приоритет при любой неоднозначной ситуации.

> [!NOTE]
> Метки могут использоваться только в исполняемых инструкциях внутри методов.

## <a name="to-label-a-line-of-code"></a>Добавление метки к строке кода

Поместите идентификатор, за которым следует двоеточие, в начало строки исходного кода.

Например, следующие строки кода помечены как `Jump` и `120`, соответственно:

[!code-vb[VbVbalrStatements#708](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#708)]

## <a name="see-also"></a>См. также

- [Операторы](../../../visual-basic/programming-guide/language-features/statements.md)
- [Имена объявленных элементов](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)
- [Соглашения о структуре программы и коде](../../../visual-basic/programming-guide/program-structure/program-structure-and-code-conventions.md)
