---
ms.openlocfilehash: 8433899058c6f569e380999800557dbe8ed0a169
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2020
ms.locfileid: "67858575"
---
### <a name="cor_prf_gc_root_handles-are-not-being-enumerated-by-profilers"></a>Los generadores de perfiles no enumeran COR_PRF_GC_ROOT_HANDLE

|   |   |
|---|---|
|Detalles|En .NET Framework v4.5.1, el método <code>RootReferences2()</code> de la API de generación de perfiles de manera incorrecta nunca devuelve <code>COR_PRF_GC_ROOT_HANDLE</code> (en su lugar se devuelven como <code>COR_PRF_GC_ROOT_OTHER</code>). Este problema se corrigió a partir de .NET Framework 4.6.|
|Sugerencia|Este problema se ha corregido en .NET Framework 4.6 y se puede solucionar mediante la actualización a esa versión de .NET Framework.|
|Ámbito|Secundaria|
|Versión|4.5.1|
|Tipo|Tiempo de ejecución|
