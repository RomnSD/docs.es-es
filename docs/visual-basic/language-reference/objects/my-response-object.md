---
title: My.Response (Objeto)
ms.date: 07/20/2015
f1_keywords:
- My.MyWebExtension.Response
- My.Response
helpviewer_keywords:
- My.Response object
ms.assetid: 626359bc-3165-40b4-bfaf-2c610e26eb5b
ms.openlocfilehash: 522814ad48fb7548032b8a37779bb3ff6ca62413
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350656"
---
# <a name="myresponse-object"></a>My.Response (Objeto)
Obtiene el objeto de <xref:System.Web.HttpResponse> asociado al <xref:System.Web.UI.Page>. Este objeto permite enviar datos de respuesta HTTP a un cliente y contiene información sobre esa respuesta.  
  
## <a name="remarks"></a>Comentarios  
 El objeto `My.Response` contiene el objeto <xref:System.Web.HttpResponse> actual asociado a la página.  
  
 El objeto `My.Response` solo está disponible para las aplicaciones ASP.NET.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se obtiene la colección de encabezados del objeto `My.Request` y se usa el objeto `My.Response` para escribirlo en la página ASP.NET.  
  
 [!code-aspx-vb[VbVbalrMyWeb#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyWeb/VB/Default.aspx#1)]  
  
## <a name="see-also"></a>Vea también

- <xref:System.Web.HttpResponse>
- [My.Request (objeto)](../../../visual-basic/language-reference/objects/my-request-object.md)
