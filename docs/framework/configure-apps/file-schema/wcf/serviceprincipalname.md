---
title: <servicePrincipalName>
ms.date: 03/30/2017
ms.assetid: 3f3b85d3-20f2-4cd8-8a6a-ee18befbd165
ms.openlocfilehash: da865a19a91d4af6221a13b53a174637d5fb8139
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70854995"
---
# <a name="serviceprincipalname"></a>\<servicePrincipalName>
Especifica la identidad de un servicio por su Nombre de entidad de seguridad de servicio (SPN).  
  
Para obtener más información acerca de cómo establecer el SPN, consulte [identidad de servicio y autenticación](../../../wcf/feature-details/service-identity-and-authentication.md).  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<> System. serviceModel**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<> de cliente**](client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<> de extremo**](endpoint-of-client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<> de identidad**](identity.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<> servicePrincipalName**  
  
## <a name="syntax"></a>Sintaxis  
  
```xml  
<servicePrincipalName value="String" />
```  
  
## <a name="attributes-and-elements"></a>Atributos y elementos  
 En las siguientes secciones se describen los atributos, los elementos secundarios y los elementos primarios  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|DESCRIPCIÓN|  
|---------------|-----------------|  
|valor|El nombre por el que un cliente identifica de manera unívoca una instancia de un servicio. Si instala varias instancias de un servicio en equipos a lo largo de un bosque, cada instancia debe tener su propio SPN. Una instancia de servicio determinada puede tener varios SPN si hay varios nombres que los clientes pueden usar para la autenticación.|  
  
### <a name="child-elements"></a>Elementos secundarios  
 Ninguno.  
  
### <a name="parent-elements"></a>Elementos primarios  
  
|Elemento|DESCRIPCIÓN|  
|-------------|-----------------|  
|[\<identity>](identity.md)|Especifica la identidad del servicio que va a autenticar el cliente.|  
  
## <a name="remarks"></a>Comentarios  
 Un cliente de Windows Communication Foundation seguro (WCF) que se conecta a un punto de conexión con esta identidad utiliza el SPN al realizar la autenticación SSPI con el punto de conexión.  
  
## <a name="see-also"></a>Vea también

- <xref:System.ServiceModel.Configuration.IdentityElement>
- <xref:System.ServiceModel.EndpointAddress>
- <xref:System.ServiceModel.EndpointAddress.Identity%2A>
- <xref:System.ServiceModel.SpnEndpointIdentity>
- [Identidad del servicio y autenticación](../../../wcf/feature-details/service-identity-and-authentication.md)
- [\<identity>](identity.md)
