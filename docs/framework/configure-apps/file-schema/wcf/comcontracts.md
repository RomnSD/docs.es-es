---
title: <comContracts>
ms.date: 03/30/2017
ms.assetid: 42e74148-223d-4888-a8ed-1d928527eb09
ms.openlocfilehash: d061d48374a8745dc61e1ca156e4fcbbccee5ef7
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69919475"
---
# <a name="comcontracts"></a>\<comContracts>
La sección de configuración `comContracts` contiene elementos que le permiten especificar varias propiedades de un contrato de servicios de la integración de COM+.  
  
## <a name="specifying-namespace-and-contract"></a>Especificar espacio de nombres y contrato  
 Los contratos de servicio de integración de com+ están `http://tempuri.org` restringidos actualmente al espacio de nombres y el nombre del contrato se deriva de la interfaz com compatible. Puede, sin embargo, especificar alternativas mediante la sección `comContracts` en el archivo de configuración.  
  
 Por ejemplo, puede utilizar la configuración siguiente para especificar el espacio de nombres y nombre del contrato del contrato de servicios, así como una opción para exigir el uso en enlaces con sesión.  
  
```xml  
<comContracts>
  <comContract contract="{5163B1E7-F0CF-4B6A-9A02-4AB654F34284}"
               namespace="http://tempuri.org/5163B1E7-F0CF-4B6A-9A02-4AB654F34284"
               name="_Broker"
               requireSession="true">
  </comContract>
</comContracts>
```  
  
 Cuando se inicializa el servicio, los espacios de nombres y nombres del contrato especificados se aplican a las descripciones de servicio generadas.  
  
 Cuando esta sección está vacía, la inicialización del servicio aplica un espacio de nombres y un nombre de contrato predeterminados tomados del identificador de la interfaz COM de apoyo.  
  
 Además, puede utilizar el [ \<elemento > de exposedMethod](exposedmethod.md) para especificar métodos com+ que se exponen cuando la interfaz en un componente com+ se expone como un servicio Web. También puede usar el [ \<> persistableTypes](persistabletypes.md) para especificar los tipos que se pueden conservar que se usan en la integración de. Por último, puede usar el [ \<elemento > userDefinedType](userdefinedtype.md) para incluir tipos definidos por el usuario (UDT) que se van a incluir en el contrato de servicio.  
  
## <a name="see-also"></a>Vea también

- <xref:System.ServiceModel.Configuration.ComContractElementCollection>
- <xref:System.ServiceModel.Configuration.ComContractElement>
- [\<> exposedMethod](exposedmethod.md)
- [\<persistableTypes>](persistabletypes.md)
- [\<userDefinedType>](userdefinedtype.md)
- [\<comContract>](comcontract.md)
- [Integración en aplicaciones COM+](../../../wcf/feature-details/integrating-with-com-plus-applications.md)
- [Cómo: Configurar el servicio COM+](../../../wcf/feature-details/how-to-configure-com-service-settings.md)
