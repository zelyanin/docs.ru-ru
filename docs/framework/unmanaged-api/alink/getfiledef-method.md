---
title: Метод GetFileDef
ms.date: 03/30/2017
api_name:
- IALink2.GetFileDef
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- GetFileDef
helpviewer_keywords:
- GetFileDef method
ms.assetid: 4e3fbe6c-b82a-4181-ab17-7faa1263f5b3
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 5db205993bc1a0665dc0003948ce805813251f48
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2019
ms.locfileid: "70787448"
---
# <a name="getfiledef-method"></a>Метод GetFileDef
Извлекает фактический токен Филедеф, используемый в метаданных (в отличие от маркера, назначенного ALink).  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT GetFileDef(  
    mdAssembly AssemblyID,  
    mdFile TargetFile,  
    mdFile* pScope  
) PURE;  
```  
  
## <a name="parameters"></a>Параметры  
 `AssemblyID`  
 Идентификатор сборки.  
  
 `TargetFile`  
 Токен добавленного файла, полученный из метода AddFile или метода AddImport.  
  
 `pScope`  
 Получает токен Филедеф.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Если метод завершается с ошибкой, возвращает значение S_OK.  
  
## <a name="requirements"></a>Требования  
 Требуется ALink. h  
  
## <a name="see-also"></a>См. также

- [Интерфейс IALink2](ialink2-interface.md)
- [Интерфейс IALink](ialink-interface.md)
- [API ALink](index.md)
