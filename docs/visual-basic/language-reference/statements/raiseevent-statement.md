---
title: Оператор RaiseEvent (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.RaiseEventMethod
- vb.RaiseEvent
- RaiseEvent
helpviewer_keywords:
- raising events [Visual Basic], RaiseEvent statement
- RaiseEvent statement [Visual Basic]
- event handlers, connecting events to
ms.assetid: f82e380a-1e6b-4047-bea8-c853f4d2c742
ms.openlocfilehash: efc7da34a297fd01411302b717cfda83aa9f87d2
ms.sourcegitcommit: 1f12db2d852d05bed8c53845f0b5a57a762979c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2019
ms.locfileid: "72582094"
---
# <a name="raiseevent-statement"></a>Оператор RaiseEvent
Запускает событие, объявленное на уровне модуля в классе, форме или документе.  
  
## <a name="syntax"></a>Синтаксис  
  
```vb  
RaiseEvent eventname[( argumentlist )]  
```  
  
## <a name="parts"></a>Части  
 `eventname`  
 Обязательный. Имя события для активации.  
  
 `argumentlist`  
 Необязательный. Разделенный запятыми список переменных, массивов или выражений. Аргумент `argumentlist` должен быть заключен в круглые скобки. Если аргументы отсутствуют, скобки должны быть опущены.  
  
## <a name="remarks"></a>Заметки  
 Обязательный `eventname` — это имя события, объявленного в модуле. Оно соответствует Visual Basic соглашениям об именовании переменных.  
  
 Если событие не было объявлено в модуле, в котором оно вызывается, возникает ошибка. В следующем фрагменте кода показано объявление события и процедура, в которой возникает событие.  
  
 [!code-vb[VbVbalrEvents#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#37)]  
  
 Нельзя использовать `RaiseEvent` для создания событий, которые не объявляются явным образом в модуле. Например, все формы наследуют событие <xref:System.Windows.Forms.Control.Click> от <xref:System.Windows.Forms.Form?displayProperty=nameWithType>, оно не может быть создано с помощью `RaiseEvent` в производной форме. При объявлении события `Click` в модуле формы он скрывает собственное событие <xref:System.Windows.Forms.Control.Click> формы. Вы по-прежнему можете вызвать событие <xref:System.Windows.Forms.Control.Click> формы, вызвав метод <xref:System.Windows.Forms.Control.OnClick%2A>.  
  
 По умолчанию событие, определенное в Visual Basic, вызывает обработчики событий в том порядке, в котором установлены соединения. Так как события могут иметь `ByRef` параметры, процесс, который подключается позднее, может получить параметры, которые были изменены более ранним обработчиком событий. После выполнения обработчиков событий управление возвращается подподпрограмме, вызвавшей событие.  
  
> [!NOTE]
> Не являющиеся общими события не должны вызываться в конструкторе класса, в котором они объявляются. Хотя такие события не вызывают ошибок во время выполнения, они могут не перехватываться связанными обработчиками событий. Используйте модификатор `Shared` для создания общего события, если необходимо вызвать событие из конструктора.  
  
> [!NOTE]
> Поведение по умолчанию событий можно изменить, определив пользовательское событие. Для пользовательских событий оператор `RaiseEvent` вызывает метод доступа `RaiseEvent` события. Дополнительные сведения о пользовательских событиях см. в разделе [оператор Event](../../../visual-basic/language-reference/statements/event-statement.md).  
  
## <a name="example"></a>Пример  
 В следующем примере события используются для выполнения обратного отсчета от 10 до 0 секунд. Код иллюстрирует несколько методов, свойств и инструкций, связанных с событиями, включая оператор `RaiseEvent`.  
  
 Класс, который вызывает событие, является источником события, а методы, обрабатывающие события, — обработчиками событий. Источник события может иметь несколько обработчиков для создаваемых им событий. Когда класс создает событие, это событие создается во всех классах, выбранных для обработки событий данного экземпляра объекта.  
  
 В примере также используется форма (`Form1`) с кнопкой (`Button1`) и текстовым полем (`TextBox1`). При нажатии кнопки в первом текстовом поле отображается обратный отсчет от 10 до 0 секунд. По истечении всего времени (10 секунд) в первом текстовом поле отображается надпись Done.  
  
 Код для `Form1` указывает начальное и конечное состояния формы. Он также содержит код, выполняемый при создании событий.  
  
 Чтобы использовать этот пример, откройте новый проект приложения Windows, добавьте кнопку с именем `Button1` и текстовое поле с именем `TextBox1` в главную форму с именем `Form1`. Затем щелкните форму правой кнопкой мыши и выберите команду **Просмотреть код** , чтобы открыть редактор кода.  
  
 Добавьте `WithEvents` переменную в раздел Declarations класса `Form1`.  
  
 [!code-vb[VbVbalrEvents#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#14)]  
  
## <a name="example"></a>Пример  
 Добавьте следующий код в код для `Form1`. Замените любые дубликаты процедур, которые могут существовать, например `Form_Load` или `Button_Click`.  
  
 [!code-vb[VbVbalrEvents#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#15)]  
  
 Нажмите клавишу F5, чтобы запустить предыдущий пример, и нажмите кнопку с надписью **Запуск**. Первое текстовое поле начинает обратный отсчет. По истечении всего времени (10 секунд) в первом текстовом поле отображается надпись Done.  
  
> [!NOTE]
> Метод `My.Application.DoEvents` не обрабатывает события точно так же, как и форма. Чтобы форма могла напрямую управлять событиями, можно использовать многопоточность. Дополнительные сведения см. в разделе [управляемые потоки](../../../standard/threading/index.md).  
  
## <a name="see-also"></a>См. также

- [События](../../../visual-basic/programming-guide/language-features/events/index.md)
- [Оператор Event](../../../visual-basic/language-reference/statements/event-statement.md)
- [Оператор AddHandler](../../../visual-basic/language-reference/statements/addhandler-statement.md)
- [Оператор RemoveHandler](../../../visual-basic/language-reference/statements/removehandler-statement.md)
- [Handles](../../../visual-basic/language-reference/statements/handles-clause.md)
