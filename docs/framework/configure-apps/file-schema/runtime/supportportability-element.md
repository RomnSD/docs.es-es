---
title: <supportPortability> (Elemento)
ms.date: 03/30/2017
helpviewer_keywords:
- supportPortability element
- <supportPortability> element
ms.assetid: 6453ef66-19b4-41f3-b712-52d0c2abc9ca
ms.openlocfilehash: 63c309a8a93c1d31ed8f73a495cf5154c3590d56
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73115654"
---
# <a name="supportportability-element"></a>\<elemento > supportPortability
Especifica que una aplicación puede hacer referencia al mismo ensamblado en dos implementaciones diferentes de .NET Framework, deshabilitando el comportamiento predeterminado que trata los ensamblados como equivalentes para los propósitos de portabilidad de aplicación.  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<en tiempo de ejecución >** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<assemblyBinding >** ](assemblybinding-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<supportPortability >**  
  
## <a name="syntax"></a>Sintaxis  
  
```xml  
<supportPortability PKT="public_key_token" enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos y elementos  

En las siguientes secciones se describen los atributos, los elementos secundarios y los elementos primarios.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descripción|  
|---------------|-----------------|  
|PKT|Atributo necesario.<br /><br /> Especifica el token de clave pública del ensamblado afectado, como una cadena.|  
|enabled|Atributo opcional.<br /><br /> Especifica si se debería habilitar el soporte para la portabilidad entre las implementaciones del ensamblado de .NET Framework especificado.|  
  
## <a name="enabled-attribute"></a>Atributo enabled  
  
|Valor|Descripción|  
|-----------|-----------------|  
|true|Habilita el soporte para la portabilidad entre las implementaciones del ensamblado de .NET Framework especificado. Este es el valor predeterminado.|  
|False|Deshabilita el soporte para la portabilidad entre las implementaciones del ensamblado de .NET Framework especificado. Esto permite a la aplicación tener referencias a varias implementaciones del ensamblado especificado.|  
  
### <a name="child-elements"></a>Elementos secundarios  

Ninguno.  
  
### <a name="parent-elements"></a>Elementos primarios  
  
|Elemento|Descripción|  
|-------------|-----------------|  
|`configuration`|Elemento raíz de cada archivo de configuración usado por las aplicaciones de Common Language Runtime y .NET Framework.|  
|`runtime`|Contiene información del enlace del ensamblado y de la recolección de elementos no utilizados.|  
|`assemblyBinding`|Contiene información sobre la redirección de versiones de ensamblado y las ubicaciones de ensamblados.|  
  
## <a name="remarks"></a>Comentarios  

A partir de la .NET Framework 4, se proporciona automáticamente compatibilidad para las aplicaciones que pueden utilizar cualquiera de las dos implementaciones de la .NET Framework, por ejemplo, la implementación de .NET Framework o la .NET Framework para la implementación de Silverlight. El enlazador del ensamblado considera equivalentes las dos implementaciones de un ensamblado de .NET Framework determinado. En algunos escenarios, esta característica de portabilidad de aplicación produce problemas. En esos escenarios, se puede usar el elemento `<supportPortability>` para deshabilitar la característica.  
  
Uno de estos escenarios es un ensamblado que tiene que hacer referencia al mismo tiempo a la implementación de .NET Framework y a la implementación de .NET Framework para Silverlight de un ensamblado de referencia determinado. Por ejemplo, un diseñador de XAML escrito en Windows Presentation Foundation (WPF) podría tener que hacer referencia a la implementación del escritorio de WPF, para la interfaz de usuario del diseñador, y al subconjunto de WPF que se incluye en la implementación de Silverlight. De forma predeterminada, las referencias independientes producen un error del compilador, ya que el enlace del ensamblado considera los dos ensamblados equivalentes. Este elemento deshabilita el comportamiento predeterminado y permite que la compilación se realice correctamente.  
  
> [!IMPORTANT]
> Para que el compilador pase la información a la lógica de enlace del ensamblado de Common Language Runtime, debe usar la opción de compilador `/appconfig` para especificar la ubicación del archivo app.config que contiene este elemento.  
  
## <a name="example"></a>Ejemplo  

El siguiente ejemplo permite a una aplicación tener referencias a la implementación de .NET Framework y de .NET Framework para Silverlight de cualquier ensamblado de .NET Framework que exista en ambas implementaciones. Se debe usar la opción de compilador `/appconfig` para especificar la ubicación de este archivo app.config.  
  
```xml  
<configuration>  
   <runtime>  
      <assemblyBinding>  
         <supportPortability PKT="7cec85d7bea7798e" enable="false"/>  
         <supportPortability PKT="31bf3856ad364e35" enable="false"/>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Vea también

- [-appconfig (Opciones del compilador de C#)](../../../../csharp/language-reference/compiler-options/appconfig-compiler-option.md)
- [Información general sobre la unificación de ensamblados de .NET Framework](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/db7849ey(v=vs.100))
