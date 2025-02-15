---
title: Метод IMetaDataAssemblyImport::GetAssemblyProps
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.GetAssemblyProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::GetAssemblyProps
helpviewer_keywords:
- GetAssemblyProps method [.NET Framework metadata]
- IMetaDataAssemblyImport::GetAssemblyProps method [.NET Framework metadata]
ms.assetid: 0eaa4aa9-9441-444a-920c-e4b2a2db899e
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: ec04588bd1cc21e585d89c734c152a86fb835b15
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67772731"
---
# <a name="imetadataassemblyimportgetassemblyprops-method"></a>Метод IMetaDataAssemblyImport::GetAssemblyProps
Получает набор свойств для сборки с заданной подписью метаданных.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT GetAssemblyProps (  
    [in]  mdAssembly          mda,  
    [out] const void          **ppbPublicKey,   
    [out] ULONG               *pcbPublicKey,  
    [out] ULONG               *pulHashAlgId,  
    [out] LPWSTR              szName,  
    [in] ULONG                cchName,  
    [out] ULONG               *pchName,  
    [out] ASSEMBLYMETADATA    *pMetaData,  
    [out] DWORD               *pdwAssemblyFlags  
);  
```  
  
## <a name="parameters"></a>Параметры  
 `mda`  
 [in]. `mdAssembly` Токен метаданных, представляющий сборку, для которого нужно получить свойства.  
  
 `ppbPublicKey`  
 [out] Указатель на открытый ключ или токен метаданных.  
  
 `pcbPublicKey`  
 [out] Число байтов в возвращаемый открытый ключ.  
  
 `pulHashAlgId`  
 [out] Указатель на алгоритм, используемый для хэширования файлов в сборке.  
  
 `szName`  
 [out] Простое имя сборки.  
  
 `cchName`  
 [in] Размер, в расширенных символах из `szName`.  
  
 `pchName`  
 [out] Число расширенных символов, фактически возвращенных в `szName`.  
  
 `pMetaData`  
 [out] Указатель на структуру ASSEMBLYMETADATA, которая содержит метаданные сборки.  
  
 `pdwAssemblyFlags`  
 [out] Флаги, описывающие метаданные, применяемые к сборке. Это значение представляет собой сочетание одного или нескольких [CorAssemblyFlags](../../../../docs/framework/unmanaged-api/metadata/corassemblyflags-enumeration.md) значения.  
  
## <a name="requirements"></a>Требования  
 **Платформы:** См. раздел [Требования к системе](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок.** Cor.h  
  
 **Библиотека:** Используется как ресурс в MsCorEE.dll  
  
 **Версии платформы .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>См. также

- [Интерфейс IMetaDataAssemblyImport](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-interface.md)
