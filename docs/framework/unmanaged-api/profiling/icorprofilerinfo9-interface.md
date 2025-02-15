---
title: Интерфейс ICorProfilerInfo9
ms.date: 08/06/2019
author: davmason
ms.author: davmason
ms.openlocfilehash: af6bd02c6d4e88c72dca20d2520d1ecc8cf1c421
ms.sourcegitcommit: 33c8d6f7342a4bb2c577842b7f075b0e20a2fa40
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2019
ms.locfileid: "70928788"
---
# <a name="icorprofilerinfo9-interface"></a>Интерфейс ICorProfilerInfo9

Подкласс [ICorProfilerInfo8](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo8-interface.md) , предоставляющий методы для запроса сведений о функциях с несколькими версиями машинного кода.  

## <a name="methods"></a>Методы  

| Метод|Описание|  
| ------------|-----------------|  
|[Метод Жетнативекодестартаддрессес](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo9-getnativecodestartaddresses-method.md)| При наличии кода functionId и Режитид перечисляются начальные адреса всех откомпилированных версий этого кода, которые в настоящее время существуют. |
|[Метод GetILToNativeMapping3](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo9-getiltonativemapping3-method.md)| С учетом начального адреса машинного кода возвращает сведения о сопоставлении IL для этой откомпилированной версии кода. |
|[Метод GetCodeInfo4](icorprofilerinfo9-getcodeinfo4-method.md)| При наличии начального адреса машинного кода возвращает блоки виртуальной памяти, в которых хранится этот код. |

## <a name="requirements"></a>Требования  
**Платформ** См. раздел [Поддерживаемые операционные системы .NET Core](../../../core/windows-prerequisites.md#net-core-supported-operating-systems).  
**Заголовок.** CorProf. idl, CorProf. h  
**Версии .NET:** [!INCLUDE[net_core](../../../../includes/net-core-22-md.md)]  

## <a name="see-also"></a>См. также

- [Интерфейсы профилирования](../../../../docs/framework/unmanaged-api/profiling/profiling-interfaces.md)
