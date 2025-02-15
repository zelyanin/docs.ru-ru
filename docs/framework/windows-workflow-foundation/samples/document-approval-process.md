---
title: Процесс утверждения документа
ms.date: 03/30/2017
ms.assetid: 9b240937-76a7-45cd-8823-7f82c34d03bd
ms.openlocfilehash: 20167cd1c06c2ae57dfe48fd07ab3a0e2adf9927
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2019
ms.locfileid: "70038228"
---
# <a name="document-approval-process"></a>Процесс утверждения документа

В этом примере демонстрируется использование многих функций Windows Workflow Foundation (WF) и Windows Communication Foundation (WCF) вместе. Вместе они реализуют сценарий утверждения документов. Клиентское приложение может представлять документы на утверждение и утверждать документы. Диспетчер утверждений облегчает взаимодействие между клиентами и обеспечивает соблюдение порядка утверждения. Утверждение - это рабочий процесс, который может выполняться несколькими способами. Поддерживается единичное утверждение, утверждение кворумом (частью группы утверждающих) и составное утверждение, состоящее из утверждения кворумом и следующего за ним единичного утверждения.

> [!IMPORTANT]
> Образцы уже могут быть установлены на компьютере. Перед продолжением проверьте следующий каталог (по умолчанию).
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Если этот каталог не существует, перейдите к [примерам Windows Communication Foundation (WCF) и Windows Workflow Foundation (WF) для .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) , чтобы скачать все Windows Communication Foundation (WCF) [!INCLUDE[wf1](../../../../includes/wf1-md.md)] и примеры. Этот образец расположен в следующем каталоге.
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Application\DocumentApprovalProcess`

## <a name="sample-details"></a>Подробные сведения об образце

На следующем рисунке показан рабочий процесс утверждения документов.

![Рабочий процесс утверждения документа](./media/document-approval-process/document-approval-process.jpg)

С точки зрения клиента процесс утверждения документа протекает следующим образом.

1. Клиент отправляет заявку на участие в системе утверждения.

2. Клиент WCF отправляет в службу WCF, размещенную в приложении диспетчера утверждений.

3. Клиенту возвращается индивидуальный пользовательский идентификатор. Теперь клиент может участвовать в процессе утверждения.

4. Присоединившись к системе, клиент может отправить документ на единичное утверждение, утверждение кворумом и составное утверждение.

5. По нажатию кнопки в клиентском интерфейсе запускается экземпляр рабочего процесса, размещенного в клиентском узле службы рабочих процессов.

6. Рабочий процесс отправляет запрос на утверждение приложению диспетчера утверждения.

7. Диспетчер рабочего процесса запускает на своем сайте рабочий процесс, представляющий процесс утверждения.

8. По завершении рабочего процесса утверждения диспетчер возвращает клиенту результаты.

9. Результаты отображаются клиентом.

10. Клиент может получить запрос на утверждение и в любой момент ответить на него.

11. Служба WCF, размещенная на клиенте, может получить запрос на утверждение от приложения диспетчера утверждений.

12. Данные о документе представляются на обозрение в клиенте.

13. Пользователь может утвердить или отклонить документ.

14. Клиент WCF используется для отправки ответа об утверждении обратно в приложение диспетчера утверждений.

С точки зрения диспетчера утверждений процесс утверждения протекает следующим образом.

1. Клиент отправляет заявку на участие в системе утверждения.

2. Служба WCF в диспетчере утверждений получает запрос, который будет входить в систему процесса утверждения.

3. Для клиента создается индивидуальный идентификатор. Пользовательские данные сохраняются в базе данных.

4. Пользователю возвращается индивидуальный идентификатор.

5. Получение запроса на утверждение. Диспетчер запускает процесс утверждения.

6. Диспетчер получает запрос на утверждение и начинает новый рабочий процесс.

7. В зависимости от типа запроса (простое утверждение, утверждение кворумом или составное утверждение) запускается определенное действие.

8. Действия по отправке и получению с корреляцией используются для отправки запроса на утверждение клиенту на рассмотрения и для получения ответа.

9. Результат рабочего процесса утверждения отправляется клиенту.

## <a name="using-the-sample"></a>Использование образца

##### <a name="to-set-up-the-database"></a>Настройка базы данных

1. В командной строке Visual Studio 2010, открытой с правами администратора, перейдите в эту папку Документаппровалпроцесс и запустите Setup. cmd.

##### <a name="to-set-up-the-application"></a>Настройка приложения

1. С помощью Visual Studio 2010 откройте файл решения Документаппровалпроцесс. sln.

2. Для построения решения нажмите CTRL+SHIFT+B.

3. Чтобы запустить решение, запустите приложение "Диспетчер утверждений", щелкнув правой кнопкой мыши проект аппровалманажер в **Обозреватель решений** и выбрав->пункт Отладка**запустить** новый экземпляр в контекстном меню.

    Подождите, пока от диспетчера не придет сообщение о готовности.

##### <a name="to-run-the-single-approval-scenario"></a>Выполнение сценария одиночного утверждения

1. Откройте командную строку с разрешениями администратора.

2. Перейдите в каталог, содержащий решение.

3. Перейдите в папку ApprovalClient\Bin\Debug и запустите два экземпляра ApprovalClient.exe.

4. Нажмите кнопку **обнаружить**, дождитесь включения кнопки **Подписка** .

5. Введите любое имя пользователя и щелкнитеподписываться. Для первого клиента используйте `UserType1`, а для второго - `UserType2`.

6. На клиенте `UserType1` выберите один тип подтверждения из раскрывающегося меню и введите имя и содержимое документа. Щелкните **запросить утверждение**.

7. В клиенте `UserType2` появится документ, ожидающий утверждения. Выберите его и нажмите кнопку **утвердить** или отклонить. Результаты должны отобразиться на клиенте `UserType1`.

##### <a name="to-run-the-quorum-approval-scenario"></a>Выполнение сценария утверждения кворумом

1. Откройте командную строку с разрешениями администратора.

2. Перейдите в каталог, содержащий решение.

3. Перейдите в папку ApprovalClient\Bin\Debug и запустите три экземпляра ApprovalClient.exe.

4. Нажмите кнопку **обнаружить**, дождитесь включения кнопки **Подписка** .

5. Введите любое имя пользователя и щелкнитеподписываться. Для одного клиента используйте `UserType1`, а для двух других - `UserType2`.

6. В клиенте `UserType1` выберите в раскрывающемся меню утверждение кворумом и введите имя и содержимое документа. Щелкните **запросить утверждение**. В данном случае требуется, чтобы два клиента `UserType2` утвердили или отклонили документ. Хотя ответить должны оба клиента `UserType2`, для утверждения документа достаточно, чтобы его утвердил один из них.

7. На клиентах `UserType2` появится документ, ожидающий подтверждения. Выберите его и нажмите кнопку **утвердить** или отклонить. Результаты должны отобразиться на клиенте `UserType1`.

##### <a name="to-run-the-complex-approval-scenario"></a>Выполнение сценария составного утверждения

1. Откройте командную строку с разрешениями администратора.

2. Перейдите в каталог, содержащий решение.

3. Перейдите в папку ApprovalClient\Bin\Debug и запустите четыре экземпляра ApprovalClient.exe.

4. Нажмите кнопку **обнаружить**, дождитесь включения кнопки **Подписка** .

5. Введите любое имя пользователя и щелкнитеподписываться. Для одного клиента используйте `UserType1`, для двух других - `UserType2`, а для последнего - `UserType3`.

6. На клиенте `UserType1` выберите один тип подтверждения из раскрывающегося меню и введите имя и содержимое документа. Щелкните **запросить утверждение**.

7. На клиентах `UserType2` появится документ, ожидающий подтверждения. Выберите его и нажмите кнопку **утвердить**. документ передается `UserType3` клиенту.

    Если документ был утвержден первым кворумом `UserType2`, этот документ буден передан клиенту `UserType3`.

8. Утвердите или отклоните документ, полученный от клиента `UserType3`. Результаты должны отобразиться на клиенте `UserType1`.

##### <a name="to-clean-up"></a>Очистка

1. В командной строке Visual Studio 2010 перейдите в папку Документаппровалпроцесс и запустите программу Cleanup. cmd.
