---
title: Метод IMapToken::Map
ms.date: 03/30/2017
api_name:
- IMapToken.Map
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMapToken::Map
helpviewer_keywords:
- IMapToken::Map method [.NET Framework metadata]
- Map method [.NET Framework metadata]
ms.assetid: b9b4bf2f-1098-43d6-9619-a99b4bda1940
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 8ac12d5b6bc2911e3bd879285a9a12f65c426f0d
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67745858"
---
# <a name="imaptokenmap-method"></a>Метод IMapToken::Map
Сопоставляет связь между сборками, используя подписи метаданных.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT Map (  
    [in]  mdToken tkImp,   
    [in]  mdToken tkEmit  
);  
```  
  
## <a name="parameters"></a>Параметры  
 `tkImp`  
 [in] Токен метаданных, представляющий объект импортированном коде.  
  
 `tkEmit`  
 [in] Токен метаданных, представляющий объект порожденного кода.  
  
## <a name="remarks"></a>Примечания  
 При выполнении повторного сопоставления маркеров во время слияния, исходный маркер действует в области метаданных исходную и в области метаданных целевую относится новый токен.  
  
## <a name="requirements"></a>Требования  
 **Платформы:** См. раздел [Требования к системе](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок.** Cor.h  
  
 **Библиотека:** Используется как ресурс в MsCorEE.dll  
  
 **Версии платформы .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>См. также

- [Интерфейс IMapToken](../../../../docs/framework/unmanaged-api/metadata/imaptoken-interface.md)
