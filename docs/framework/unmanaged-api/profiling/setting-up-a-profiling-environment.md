---
title: Установка профилирующей среды
ms.date: 03/30/2017
helpviewer_keywords:
- environment variables, profiling API
- profiling API [.NET Framework], setting up environment
- COR_PROFILER environment variable
- Windows Service applications, profiling
- profiling API [.NET Framework], Windows Service applications
- COR_ENABLE_PROFILING environment variable
- profiling API [.NET Framework], enabling
ms.assetid: fefca07f-7555-4e77-be86-3c542e928312
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 1e6c87a408b348cb6ecc7a3f6afa7060a1568a37
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2019
ms.locfileid: "69966105"
---
# <a name="setting-up-a-profiling-environment"></a>Установка профилирующей среды
> [!NOTE]
> В .NET Framework 4 были внесены существенные изменения в профилирование.  
  
 При запуске управляемого процесса (приложения или службы) он загружает среду CLR. При инициализации среды CLR она оценивает следующие две переменные среды, чтобы решить, следует ли подключить процесс к профилировщику.  
  
- COR_ENABLE_PROFILING: Среда CLR подключается к профилировщику только в том случае, если эта переменная среды существует и имеет значение 1.  
  
- COR_PROFILER: Если проверка COR_ENABLE_PROFILING пройдена, среда CLR подключается к профилировщику с этим идентификатором CLSID или ProgID, который должен быть ранее сохранен в реестре. Переменная среды COR_PROFILER задается как строка, как показано в следующих двух примерах.  
  
    ```cpp  
    set COR_PROFILER={32E2F4DA-1BEA-47ea-88F9-C5DAF691C94A}  
    set COR_PROFILER="MyProfiler"  
    ```  
  
 Чтобы выполнить профилирование приложения среды CLR, необходимо задать переменные среды COR_ENABLE_PROFILING и COR_PROFILER перед запуском приложения. Кроме того, необходимо убедиться, что DLL профилировщика зарегистрирована.  
  
> [!NOTE]
> Начиная с .NET Framework 4, профилировщики не обязательно должны быть зарегистрированы.  
  
> [!NOTE]
> Для использования профилировщиков .NET Framework версий 2,0, 3,0 и 3,5 в .NET Framework 4 и более поздних версиях необходимо задать переменную среды COMPLUS_ProfAPI_ProfilerCompatibilitySetting.  
  
## <a name="environment-variable-scope"></a>Область действия переменных среды.  
 Способ установки переменных среды COR_ENABLE_PROFILING и COR_PROFILER будет определять их области действия. Вы можете установить эти переменные одним из следующих способов.  
  
- Если задать переменные в вызове [ICorDebug:: CreateProcess](../../../../docs/framework/unmanaged-api/debugging/icordebug-createprocess-method.md) , они будут применяться только к приложению, которое выполняется в данный момент. (Они также будут применяться к другим приложениям, запущенным этим приложением, которые наследуют среду.)  
  
- Если переменные задаются в окне командной строки, то они будут применяться ко всем приложениям, которые запускаются из этого окна.  
  
- Если задать переменные на уровне пользователя, они будут применяться ко всем приложениям, запускаемым из проводника. Окно командной строки, открытое после установки этих переменных, будет использовать эти параметры среды, как и любое приложение, запущенное из этого окна. Чтобы задать переменные среды на уровне пользователя, щелкните правой кнопкой мыши **Мой компьютер**, выберите пункт **свойства**, перейдите на вкладку **Дополнительно** , щелкните **переменные среды**и добавьте переменные в список **переменные пользователя** .  
  
- Если задать переменные на уровне компьютера, они будут применяться ко всем приложениям, запускаемым на этом компьютере. Окно командной строки, открытое на этом компьютере, будет использовать эти параметры среды, как и любое приложение, запущенное из этого окна. Это означает, что все управляемые процессы на этом компьютере будут запускаться с вашим профилировщиком. Чтобы задать переменные среды на уровне компьютера, щелкните правой кнопкой мыши **Мой компьютер**, выберите пункт **свойства**, перейдите на вкладку **Дополнительно** , щелкните **переменные среды**, добавьте переменные в список **системные переменные** , а затем Перезагрузите компьютер. После перезагрузки эти переменные будут доступны во всей системе.  
  
 При профилировании службы Windows необходимо перезагрузить компьютер после установки переменных среды и регистрации DLL профилировщика. Дополнительные сведения об этих вопросах см. в разделе [Профилирование службы Windows](#windows_service).  
  
## <a name="additional-considerations"></a>Дополнительные сведения  
  
- Класс профилировщика реализует интерфейсы [ICorProfilerCallback](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-interface.md) и [ICorProfilerCallback2](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback2-interface.md) . На платформе .NET Framework версии 2.0 профилировщик должен реализовывать интерфейс `ICorProfilerCallback2`. Если это не так, то `ICorProfilerCallback2` не будет загружен.  
  
- Только один профилировщик может профилировать процесс в каждый момент времени в конкретной среде. Можно зарегистрировать два различных профилировщика в разных средах, но каждый из них должен профилировать разные процессы. Профилировщик должен быть реализован как внутрипроцессная DLL COM-сервера, назначенная в то же адресное пространство, что и профилируемый процесс. Это означает, что профилировщик функционирует внутрипроцессно. Платформа .NET Framework не поддерживает другие типы COM-сервера. Например, если профилировщику необходимо выполнить мониторинг приложений с удаленного компьютера, он должен реализовать агенты сборщика на каждом компьютере. Эти агенты будут выполнять пакетную обработку результатов и сообщать о них центральному компьютеру коллекции данных.  
  
- Так как профилировщик является COM-объектом, создаваемым в процессе, каждое профилируемое приложение будет иметь собственную копию профилировщика. Таким образом, одиночный экземпляр профилировщика не должен обрабатывать данные из нескольких приложений. Тем не менее необходимо добавить логику в код ведения журнала профилировщика для предотвращения перезаписи файла журнала другими профилируемыми приложениями.  
  
## <a name="initializing-the-profiler"></a>Инициализация профилировщика  
 Когда обе переменные среды проходят проверку, среда CLR  создает экземпляр профилировщика аналогично функции COM `CoCreateInstance`. Профилировщик не загружается посредством прямого вызова `CoCreateInstance`. Таким образом, исключается вызов `CoInitialize`, требующий установки потоковой модели. Затем среда CLR вызывает метод [ICorProfilerCallback:: Initialize](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-initialize-method.md) в профилировщике. Сигнатура этого метода выглядит следующим образом.  
  
```cpp  
HRESULT Initialize(IUnknown *pICorProfilerInfoUnk)  
```  
  
 Профилировщик должен запросить `pICorProfilerInfoUnk` указатель интерфейса [ICorProfilerInfo](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo-interface.md) или [ICorProfilerInfo2](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo2-interface.md) и сохранить его, чтобы он мог запрашивать дополнительные сведения позже во время профилирования.  
  
## <a name="setting-event-notifications"></a>Настройка уведомлений о событиях  
 Затем профилировщик вызывает метод [ICorProfilerInfo:: SetEventMask](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo-seteventmask-method.md) , чтобы указать, в каких категориях уведомлений он заинтересован. Например, если профилировщик интересуют только уведомления вызова-возврата в функциях и уведомления о сборке мусора, то указывается следующее.  
  
```cpp  
ICorProfilerInfo* pInfo;  
pICorProfilerInfoUnk->QueryInterface(IID_ICorProfilerInfo, (void**)&pInfo);  
pInfo->SetEventMask(COR_PRF_MONITOR_ENTERLEAVE | COR_PRF_MONITOR_GC)  
```  
  
 Путем такого задания маски уведомлений профилировщик может ограничить принимаемые уведомления. Такой подход помогает пользователю строить простой или специальный профилировщик. Он также сокращает время ЦП, которое тратится на отправку уведомлений, которые профилировщик должен игнорировать.  
  
 Некоторые события профилировщика являются неизменяемыми. Это означает, что сразу после установки этих событий в обратном вызове `ICorProfilerCallback::Initialize` они не могут быть отключены и новые события не могут быть включены. Попытки изменить неизменяемое событие приведут к возврату методом `ICorProfilerInfo::SetEventMask` HRESULT с ошибкой.  
  
<a name="windows_service"></a>   
## <a name="profiling-a-windows-service"></a>Профилирование службы Windows  
 Профилирование службы Windows аналогично профилированию приложения среды CLR. Обе операции профилирования включаются посредством переменных среды. Поскольку служба Windows запускается при запуске операционной системы, переменные среды, которые рассматривались выше в этом разделе, должны присутствовать и иметь необходимые значения до запуска системы. Кроме того, библиотека DLL профилирования должна уже быть зарегистрирована в системе.  
  
 После установки переменных среды COR_ENABLE_PROFILING и COR_PROFILER и регистрации библиотеки DLL профилировщика вы должны перезагрузить компьютер, чтобы служба Windows могла обнаружить эти изменения.  
  
 Обратите внимание, что эти изменения будут включать профилирование в рамках всей системы. Во избежание профилирования всех последовательно запускаемых управляемых приложений следует удалить переменные среды после перезагрузки целевого компьютера.  
  
 Этот метод также приводит к профилированию каждого процесса CLR. Профилировщик должен добавить логику в свой обратный вызов [ICorProfilerCallback:: Initialize](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-initialize-method.md) , чтобы определить, является ли текущий процесс интересным. Если это не так, профилировщик может завершить обратный вызов с ошибкой без выполнения инициализации.  
  
## <a name="see-also"></a>См. также

- [Общие сведения о профилировании](../../../../docs/framework/unmanaged-api/profiling/profiling-overview.md)
