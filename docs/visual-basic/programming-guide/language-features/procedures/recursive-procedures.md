---
title: Рекурсивные процедуры (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- procedures [Visual Basic], that call themselves
- procedures [Visual Basic], recursive
- procedures [Visual Basic], calling
- recursive procedures
- functions [Visual Basic], calling recursively
- recursion
ms.assetid: ba1d3962-b4c3-48d3-875e-96fdb4198327
ms.openlocfilehash: b08a06a07f134b7c95251848862d39339e59fe61
ms.sourcegitcommit: 3caa92cb97e9f6c31f21769c7a3f7c4304024b39
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "71274343"
---
# <a name="recursive-procedures-visual-basic"></a>Рекурсивные процедуры (Visual Basic)

*Рекурсивная* процедура вызывает саму себя. Как правило, это не самый эффективный способ написания кода Visual Basic.  
  
 В следующей процедуре используется рекурсия для вычисления факториала исходного аргумента.  
  
 [!code-vb[VbVbcnProcedures#51](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#51)]  
  
## <a name="considerations-with-recursive-procedures"></a>Рекомендации по рекурсивным процедурам

 **Ограничение условий**. Необходимо разработать рекурсивную процедуру для проверки по крайней мере одного условия, которое может завершить рекурсию, и необходимо также обработать случай, когда ни одно из таких условий не будет удовлетворено разумным числом рекурсивных вызовов. При отсутствии хотя бы одного условия, которое может быть выполнено без ошибок, ваша процедура выполняет высокий риск выполнения в бесконечном цикле.

 **Использование памяти**. Приложение имеет ограниченный объем пространства для локальных переменных. Каждый раз, когда процедура вызывает саму себя, она использует больше пространства для дополнительных копий своих локальных переменных. Если этот процесс будет продолжаться неограниченно, он приводит <xref:System.StackOverflowException> к ошибке.

 **Эффективность**. Почти всегда можно заменить цикл для рекурсии. Цикл не имеет дополнительной нагрузки на передачу аргументов, инициализацию дополнительного хранилища и возврат значений. Производительность может быть значительно выше без рекурсивных вызовов.

 **Взаимная рекурсия**. Вы можете столкнуться с очень низкой производительностью или даже с бесконечным циклом, если две процедуры вызывают друг друга. Такой проект представляет те же проблемы, что и одна Рекурсивная процедура, но может быть труднее для обнаружения и отладки.

 **Вызов с круглыми скобками**. При рекурсивном вызове процедуры необходимо следовать имени процедуры с круглыми скобками, даже если нет списка аргументов. `Function` В противном случае имя функции берется, как представляет возвращаемое значение функции.

 **Тестирование**. При написании рекурсивной процедуры следует тщательно протестировать ее, чтобы убедиться, что она всегда соответствует определенному ограничению. Также следует убедиться, что не хватает памяти из-за слишком большого количества рекурсивных вызовов.

## <a name="see-also"></a>См. также

- <xref:System.StackOverflowException>
- [Процедуры](index.md)
- [Подпрограммы](sub-procedures.md)
- [Процедуры функций](function-procedures.md)
- [Процедуры свойств](property-procedures.md)
- [Процедуры операторов](operator-procedures.md)
- [Параметры и аргументы процедуры](procedure-parameters-and-arguments.md)
- [Перегрузка процедур](procedure-overloading.md)
- [Рекомендации по устранению неполадок](troubleshooting-procedures.md)
- [Циклические структуры](../control-flow/loop-structures.md)
