---
title: Elemento GCLOHThreshold
ms.date: 11/20/2019
helpviewer_keywords:
- GCLOHThreshold element
- <GCLOHThreshold> element
ms.openlocfilehash: d72dc9d27984f60dfb6296217263ce8b176093c6
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2019
ms.locfileid: "74451327"
---
# <a name="gclohthreshold-element"></a>Elemento GCLOHThreshold

Especifica el tamaño del umbral, en bytes, que hace que el recolector de elementos no utilizados coloque objetos en el montón de objetos grandes (montón).

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;[\<en tiempo de ejecución >](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;\<GCLOHThreshold >

## <a name="syntax"></a>Sintaxis

```xml
<GCLOHThreshold
   enabled="nnnn"/>
```

## <a name="attributes"></a>Atributos

|Atributo|Descripción|
|---------------|-----------------|
|`enabled`|Atributo necesario.<br /><br />Especifica el tamaño del umbral que hace que los objetos se dirijan al montón de objetos grandes.|

### <a name="enabled-attribute"></a>atributo Enabled

|Valor|Descripción|
|-----------|-----------------|
|`nnnn`|Tamaño del umbral, en bytes, que hace que los objetos vayan al montón de objetos grandes.|

## <a name="child-elements"></a>Elementos secundarios

Ninguno.

## <a name="parent-elements"></a>Elementos primarios

|Elemento|Descripción|
|-------------|-----------------|
|`configuration`|Elemento raíz de cada archivo de configuración usado por las aplicaciones de Common Language Runtime y .NET Framework.|
|`runtime`|Contiene información del enlace del ensamblado y de la recolección de elementos no utilizados.|

## <a name="remarks"></a>Comentarios

Esta configuración se presentó en .NET Framework 4,8.

## <a name="see-also"></a>Vea también

- [Esquema de configuración de tiempo de ejecución](index.md)
- [Esquema de los archivos de configuración](../index.md)
- [Fundamentos de la recolección de elementos no utilizados](../../../../standard/garbage-collection/fundamentals.md)
- [Opciones de configuración de tiempo de ejecución de .net Core para GC](../../../../core/run-time-config/garbage-collector.md)
