---
title: Procedimiento para recortar y ajustar la escala de las imágenes
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- images [Windows Forms], cropping
- images [Windows Forms], scaling
ms.assetid: 053e3360-bca0-4b25-9afa-0e77a6f17b03
ms.openlocfilehash: 4257431881565f9160f45795111d374cc680dedd
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61937583"
---
# <a name="how-to-crop-and-scale-images"></a>Procedimiento para recortar y ajustar la escala de las imágenes
El <xref:System.Drawing.Graphics> clase proporciona varios <xref:System.Drawing.Graphics.DrawImage%2A> métodos, algunos de los cuales tienen parámetros de rectángulo de origen y destino que pueden usar para recortar y escalar imágenes.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se crea un <xref:System.Drawing.Image> objeto desde el archivo de disco Apple.gif. El código dibuja la imagen completa en su tamaño original. El código, a continuación, llama a la <xref:System.Drawing.Graphics.DrawImage%2A> método de un <xref:System.Drawing.Graphics> objeto que se va a dibujar una parte de la imagen de apple en un rectángulo de destino es mayor que la imagen original.  
  
 El <xref:System.Drawing.Graphics.DrawImage%2A> método determina qué parte de la manzana que se va a dibujar observando el rectángulo de origen, que se especifica mediante el tercero, cuarto, quinto y sexto argumentos. En este caso, se recorta el apple al 75 por ciento de su ancho y el 75 por ciento de su alto.  
  
 El <xref:System.Drawing.Graphics.DrawImage%2A> método determina dónde se va a dibujar la manzana recortada y qué tamaño observando el rectángulo de destino, que es especificado por el segundo argumento. En este caso, el rectángulo de destino es el 30 por ciento más ancho y el 30 por ciento más alta de la imagen original.  
  
 La ilustración siguiente muestra la manzana original y la escala y recorta apple.  
  
 ![Captura de pantalla de una imagen original y la misma imagen recortada.](./media/how-to-crop-and-scale-images/original-image-cropped-image.png)  
  
 [!code-csharp[System.Drawing.WorkingWithImages#11](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.WorkingWithImages/CS/Class1.cs#11)]
 [!code-vb[System.Drawing.WorkingWithImages#11](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.WorkingWithImages/VB/Class1.vb#11)]  
  
## <a name="compiling-the-code"></a>Compilar el código  
 El ejemplo anterior está diseñado para su uso con Windows Forms y requiere <xref:System.Windows.Forms.PaintEventArgs> `e`, que es un parámetro de la <xref:System.Windows.Forms.Control.Paint> controlador de eventos. No olvide reemplazar `Apple.gif` con un nombre de archivo de imagen y la ruta de acceso que son válidos en el sistema.  
  
## <a name="see-also"></a>Vea también

- [Imágenes, mapas de bits y metarchivos](images-bitmaps-and-metafiles.md)
- [Trabajar con imágenes, mapas de bits, iconos y metarchivos](working-with-images-bitmaps-icons-and-metafiles.md)
