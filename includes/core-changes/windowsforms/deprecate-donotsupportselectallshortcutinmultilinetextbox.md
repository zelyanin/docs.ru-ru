---
ms.openlocfilehash: a9d0fe3516ade04bc38b804268a9d989f6891d80
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71181752"
---
### <a name="switchsystemwindowsformsdonotsupportselectallshortcutinmultilinetextbox-compatibility-switch-not-supported"></a>Параметр совместимости Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox не поддерживается

Параметр совместимости `Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox`, появившийся в .NET Framework 4.6.1, не поддерживается в Windows Forms в .NET Core 3.0.

#### <a name="change-description"></a>Описание изменений

Начиная с .NET Framework 4.6.1, при нажатии клавиш <kbd>CTRL</kbd> + <kbd>A</kbd> в элементе управления <xref:System.Windows.Forms.TextBox> выделяется весь текст. В .NET Framework 4.6 и более ранних версиях при нажатии клавиш <kbd>CTRL</kbd> + <kbd>A</kbd> выделение всего текста не происходило, если свойства [Textbox.ShortcutsEnabled](xref:System.Windows.Forms.TextBoxBase.ShortcutsEnabled) и <xref:System.Windows.Forms.TextBox.Multiline?displayProperty=nameWithType> имели значение `true`. Параметр совместимости `Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox` появился в .NET Framework 4.6.1 для восстановления прежнего поведения. Дополнительные сведения см. в разделе <xref:System.Windows.Forms.TextBox.ProcessCmdKey%2A?displayProperty=nameWithType>.

В .NET Core параметр `Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox` не поддерживается.

#### <a name="version-introduced"></a>Представленная версия

3.0, предварительная версия 9

#### <a name="recommended-action"></a>Рекомендуемое действие

Удалите параметр. Он не поддерживается, и альтернативного варианта нет.

#### <a name="category"></a>Категория

Windows Forms

#### <a name="affected-apis"></a>Затронутые API

- Нет

<!-- 

### Affected APIs

- Not detectable via API analysis

-->
