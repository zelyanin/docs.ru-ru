---
title: Устранение рисков. Кадры PNG в объектах Icon
ms.date: 03/30/2017
ms.assetid: ca87fefb-7144-4b4e-8832-5a939adbb4b2
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 85a76681cf6efd649fe366a68d956246334975fe
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2019
ms.locfileid: "70789972"
---
# <a name="mitigation-png-frames-in-icon-objects"></a>Устранение рисков. Кадры PNG в объектах Icon
начиная с версии .NET Framework 4.6 метод <xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> успешно преобразует значки с кадрами PNG в объекты <xref:System.Drawing.Bitmap> .  
  
 В приложениях, предназначенных для .NET Framework 4.5.2 и более ранних версий, метод <xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> создает исключение <xref:System.ArgumentOutOfRangeException> , если объект <xref:System.Drawing.Icon> содержит кадры PNG.  
  
## <a name="impact"></a>Последствия  
 Это изменение затрагивает приложения, которые компилируются повторно для платформы .NET Framework 4.6 и в которых реализуется специальная обработка исключения <xref:System.ArgumentOutOfRangeException> , создаваемого при наличии кадров PNG в объекте <xref:System.Drawing.Icon> . При выполнении в .NET Framework 4.6 преобразование проходит успешно, исключение <xref:System.ArgumentOutOfRangeException> больше не создается, и поэтому обработчик исключений больше не вызывается.  
  
### <a name="mitigation"></a>Устранение рисков  
 Если такое поведение нежелательно, можно сохранить прежнее поведение, добавив в раздел [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) файла app.config следующий элемент:  
  
```xml  
<AppContextSwitchOverrides   
      value="Switch.System.Drawing.DontSupportPngFramesInIcons=true" />  
```  
  
 Если файл app.config уже содержит элемент `AppContextSwitchOverrides`, новое значение следует объединить с атрибутом `value` следующим образом:  
  
```xml  
<AppContextSwitchOverrides   
      value="Switch.System.Drawing.DontSupportPngFramesInIcons=true;<previous key>=<previous value>" />  
```  
  
## <a name="see-also"></a>См. также

- [Изменение целевой платформы](retargeting-changes-in-the-net-framework-4-6.md)
