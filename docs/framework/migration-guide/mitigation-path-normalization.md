---
title: Устранение рисков. Нормализация путей
ms.date: 03/30/2017
ms.assetid: 158d47b1-ba6d-4fa6-8963-a012666bdc31
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: bc5ea69d80a225adfc2f409e8303ee1c241398db
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2019
ms.locfileid: "70779346"
---
# <a name="mitigation-path-normalization"></a>Устранение рисков. Нормализация путей
Начиная с приложений, ориентированных на .NET Framework 4.6.2, нормализация путей в .NET Framework изменилась.  
  
## <a name="what-is-path-normalization"></a>Что такое нормализация путей?  
 Нормализация пути подразумевает изменение строки, которая идентифицирует путь или файл, чтобы он соответствовал допустимому пути в целевой операционной системе. Нормализация обычно включает в себя:  
  
- канонизацию разделителей компонентов и каталогов;  
  
- применение текущего каталога к относительному пути;  
  
- оценку относительного каталога (`.`) или родительского каталога (`..`) в пути;  
  
- обрезку указанных символов.  
  
## <a name="the-changes"></a>Изменения  
 Начиная с приложений, ориентированных на .NET Framework 4.6.2, нормализация путей изменилась следующим образом:  
  
- Среда выполнения перекладывает нормализацию путей на функцию [GetFullPathName](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea) операционной системы.  
  
- Нормализация больше не предусматривает обрезки окончания сегментов каталогов (например, пробела в конце имени каталога).  
  
- Поддержка синтаксиса пути устройства в режиме полного доверия, включая `\\.\` и (для API-интерфейсов файлового ввода-вывода в mscorlib.dll) `\\?\`.  
  
- Среда выполнения не проверяет пути с синтаксисом устройства.  
  
- Поддерживается использование синтаксиса устройства для доступа к альтернативным потокам данных.  
  
## <a name="impact"></a>Последствия  

Для приложений, предназначенных для .NET Framework 4.6.2 или более поздней версии, эти изменения включены по умолчанию. Они должны улучшить производительность, позволяя методам получать доступ к ранее недоступным путям.  
  
Это изменение не влияет на приложения, предназначенные для .NET Framework 4.6.1 и более ранних версий, но работающие на платформе .NET Framework 4.6.2 или более новой версии.  
  
## <a name="mitigation"></a>Устранение рисков  
 В приложениях, предназначенных для .NET Framework 4.6.2 или более поздней версии, данное изменение можно отключить и использовать устаревшую нормализацию, добавив следующее в раздел [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) файла конфигурации приложения:  
  
```xml  
<runtime>  
    <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=true" />    
</runtime>  
```  
  
В приложениях, предназначенных для .NET Framework 4.6.1 или более ранней версии, но работающих на платформе .NET Framework 4.6.2 или более поздней версии, можно включить изменения в нормализацию пути, добавив следующую строку в раздел [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) файла конфигурации приложения:  
  
```xml  
<runtime>  
    <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=false" />    
</runtime>  
```  
  
## <a name="see-also"></a>См. также

- [Изменение целевой платформы](retargeting-changes-in-the-net-framework-4-6-2.md)
