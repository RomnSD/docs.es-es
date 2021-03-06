---
title: COINITIEE (Enumeración)
ms.date: 03/30/2017
api_name:
- COINITIEE
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- COINITIEE
helpviewer_keywords:
- COINITIEE enumeration [.NET Framework metadata]
ms.assetid: 64264238-3b68-4bac-a887-36b552426a6c
topic_type:
- apiref
ms.openlocfilehash: 2ccc038b4420040779dae70f15e3a8827ba94180
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2019
ms.locfileid: "74444107"
---
# <a name="coinitiee-enumeration"></a>COINITIEE (Enumeración)
Especifica las constantes utilizadas por el [Coinicializador](../../../../docs/framework/unmanaged-api/hosting/coinitializeee-function.md) al inicializar el Common Language Runtime.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
typedef enum tagCOINITEE {  
   COINITEE_DEFAULT = 0x0,  
   COINITEE_DLL     = 0x1,  
   COINITEE_MAIN    = 0x2  
} COINITIEE;  
```  
  
## <a name="members"></a>Miembros  
  
|Miembro|Descripción|  
|------------|-----------------|  
|`COINITEE_DEFAULT`|Modo de inicialización predeterminado. Esto inicializa el tiempo de ejecución y crea el <xref:System.AppDomain>predeterminado.|  
|`COINITEE_DLL`|Inicializa para ejecutar un archivo DLL administrado.|  
|`COINITEE_MAIN`|Inicializa para ejecutar un ejecutable administrado. Esto inicializa el tiempo de ejecución, pero no crea el <xref:System.AppDomain>predeterminado, que se crea después de escribir la rutina principal del archivo EXE.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Vea [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Encabezado:** Cor. h  
  
 **Biblioteca:** Se incluye como recurso en MsCorEE. dll  
  
 **Versiones de .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Vea también

- [Enumeraciones para metadatos](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
