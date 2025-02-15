---
title: Практическое руководство. Как заменить URL-резервирование WCF на ограниченное резервирование
ms.date: 03/30/2017
ms.assetid: 2754d223-79fc-4e2b-a6ce-989889f2abfa
ms.openlocfilehash: 981c4890b11130b937e176da78f378340c0d3894
ms.sourcegitcommit: 005980b14629dfc193ff6cdc040800bc75e0a5a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2019
ms.locfileid: "70991664"
---
# <a name="how-to-replace-the-wcf-url-reservation-with-a-restricted-reservation"></a>Практическое руководство. Как заменить URL-резервирование WCF на ограниченное резервирование
Резервирование URL-адреса позволяет ограничить список тех, кто может получать сообщения с URL-адреса или набора URL-адресов. Резервирование включает шаблон URL-адреса, список управления доступом (ACL) и набор флагов. Шаблон URL-адреса определяет, какие URL-адреса будут затронуты резервированием. Дополнительные сведения об обработке шаблонов URL-адресов см. в разделе [Маршрутизация входящих запросов](https://go.microsoft.com/fwlink/?LinkId=136764). Список управления доступом (ACL) управляет тем, какой пользователь или группа пользователей может получать сообщения с указанных URL-адресов. Флаги указывают, разрешает ли резервирование пользователю или группе отслеживать URL-адрес напрямую или делегирует это разрешение какому-либо другому процессу.  
  
 В рамках стандартной конфигурации операционной системы Windows Communication Foundation (WCF) создает глобально доступное резервирование для порта 80, чтобы позволить всем пользователям запускать приложения, использующие двойную привязку HTTP для дуплексной связи. Поскольку список управления доступом (ACL) для данного резервирования предназначен для каждого пользователя, администраторы не могут явным образом позволить или запретить отслеживать URL-адрес или набор URL-адресов. В данном разделе объясняется, как удалить это резервирование и создать его повторно с ограниченным списком управления доступом (ACL).  
  
 В [!INCLUDE[wv](../../../../includes/wv-md.md)] или [!INCLUDE[lserver](../../../../includes/lserver-md.md)] можно просмотреть все резервирования HTTP URL-адресов, выполнив в командной строке с правами администратора команду `netsh http show urlacl`.  В следующем примере показано, как должно выглядеть резервирование URL-адреса WCF.  

```
Reserved URL : http://+:80/Temporary_Listen_Addresses/  
        User: \Everyone  
            Listen: Yes  
            Delegate: No  
            SDDL: D:(A;;GX;;;WD)  
```

 Резервирование состоит из шаблона URL-адреса, используемого, когда приложение WCF использует двойную привязку HTTP для дуплексной связи. URL-адреса этой формы используются, чтобы служба WCF отправляла сообщения обратно клиенту WCF при взаимодействии с двойной привязкой HTTP. Каждый пользователь получает разрешение отслеживать URL-адрес, но не может делегировать отслеживание другому процессу. Наконец, список управления доступом (ACL) описывается на языке Security Descriptor Definition Language (SSDL). Дополнительные сведения о языке SSDL см. в разделе [SSDL](https://go.microsoft.com/fwlink/?LinkId=136789) .  
  
## <a name="to-delete-the-wcf-url-reservation"></a>Удаление резервирования URL-адреса WCF  
  
1. Нажмите **кнопку Пуск**, укажите **пункт Все программы**, затем **стандартные**, щелкните правой кнопкой мыши пункт **Командная строка** и выберите команду **Запуск от имени администратора** в контекстном меню, которое появляется. Нажмите кнопку **продолжить** в окне контроля учетных записей (UAC), которое может запрашивать разрешения на продолжение.  
  
2. Введите в окне командной строки команду **netsh http http://+:80/Temporary_Listen_Addresses/ Delete urlacl URL =** .  
  
3. Если резервирование удалено успешно, появится следующее сообщение. **Резервирование URL-адресов успешно удалено**  
  
## <a name="creating-a-new-security-group-and-new-restricted-url-reservation"></a>Создание новой группы безопасности и нового ограниченного резервирования URL-адреса  
 Чтобы заменить резервирование URL-адреса WCF с ограниченным резервированием, сначала необходимо создать новую группу безопасности. Это можно сделать двумя способами: из командной строки или из консоли управления компьютером. Требуется выполнить только одно действие.  
  
### <a name="to-create-a-new-security-group-from-a-command-prompt"></a>Создание новой группы безопасности из командной строки  
  
1. Нажмите **кнопку Пуск**, укажите **пункт Все программы**, затем **стандартные**, щелкните правой кнопкой мыши пункт **Командная строка** и выберите команду **Запуск от имени администратора** в контекстном меню, которое появляется. Нажмите кнопку **продолжить** в окне контроля учетных записей (UAC), которое может запрашивать разрешения на продолжение.  
  
2. Введите в командной строке **net\<localgroup "имя группы безопасности >"/Comment\<: "Описание группы безопасности >"/Add "** . **Замените\<имя группы безопасности >** именем группы безопасности, которую вы хотите создать, и  **\<описание группы безопасности >** с подходящим описанием для группы безопасности.  
  
3. Если группа безопасности создана успешно, появится следующее сообщение. **Команда выполнена успешно.**  
  
### <a name="to-create-a-new-security-group-from-the-computer-management-console"></a>Создание новой группы безопасности из консоли управления компьютером  
  
1. Нажмите кнопку **Пуск**, выберите **Панель управления**, щелкните **Администрирование**, а затем — **Управление компьютером** , чтобы открыть консоль управления компьютером. Нажмите кнопку **продолжить** в окне контроля учетных записей (UAC), которое может запрашивать разрешения на продолжение.  
  
2. Щелкните " **Служебные программы**", щелкните " **Локальные пользователи и группы**", щелкните правой кнопкой мыши папку " **группы** " и выберите пункт " **создать группу** " в контекстном меню. Введите имя, **Описание** и другие сведения о нужной **группе**безопасности и нажмите кнопку **создать** , чтобы создать группу безопасности.  
  
### <a name="to-create-the-restricted-url-reservation"></a>Создание ограниченного резервирования URL-адреса  
  
1. Нажмите **кнопку Пуск**, укажите **пункт Все программы**, затем **стандартные**, щелкните правой кнопкой мыши пункт **Командная строка** и выберите команду **Запуск от имени администратора** в контекстном меню, которое появляется. Нажмите кнопку **продолжить** в окне контроля учетных записей (UAC), которое может запрашивать разрешения на продолжение.  
  
2. Введите в командной строке команду **netsh http Add http://+:80/Temporary_Listen_Addresses/ urlacl URL =\< User = "\\ имя компьютера > <\> имя группы безопасности** . **Замените\<имя компьютера >** именем компьютера, на котором должна быть создана группа, и  **\<именем группы безопасности >** именем группы безопасности, созданной ранее.  
  
3. Если резервирование создано успешно, появится следующее сообщение. **Резервирование URL-адресов успешно добавлено**.
