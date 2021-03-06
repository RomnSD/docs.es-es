---
title: 'Cómo: Animar un objeto a lo largo de un trazado (animación en matriz con acumulación de desplazamiento)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- offset accumulation [WPF]
- animation [WPF], objects along paths (matrix animation with offset accumulation)
- matrix animation with offset accumulation [WPF]
ms.assetid: 1bca90ef-9832-4128-8ed6-62908e7ec146
ms.openlocfilehash: 8b3975ef5031b50381dfa9baf7f34a58fc05ab4e
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453109"
---
# <a name="how-to-animate-an-object-along-a-path-matrix-animation-with-offset-accumulation"></a>Cómo: Animar un objeto a lo largo de un trazado (animación en matriz con acumulación de desplazamiento)
En este ejemplo se muestra cómo usar la clase <xref:System.Windows.Media.Animation.MatrixAnimationUsingPath> para animar un objeto a lo largo de un trazado y hacer que la animación acumule sus valores de desplazamiento a medida que se repite.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se usa el objeto <xref:System.Windows.Media.Animation.MatrixAnimationUsingPath> para animar la propiedad <xref:System.Windows.Media.MatrixTransform.Matrix%2A> de un <xref:System.Windows.Media.MatrixTransform> aplicado a un botón. Como resultado, un botón se mueve a lo largo de un trazado curvo.  
  
 Además, en el ejemplo se establece la propiedad <xref:System.Windows.Media.Animation.MatrixAnimationUsingPath.IsOffsetCumulative%2A> en `true`, lo que hace que el desplazamiento de la matriz animada se acumule a medida que se repite la animación. Dado que el desplazamiento se acumula, el botón se aleja por la pantalla cuando se repite la animación, en lugar de restablecerse a la posición inicial.  
  
 [!code-xaml[PathAnimationGallery_snippet#MatrixAnimationUsingPathOffsetCumulativeWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_snippet/CS/matrixanimationusingpathexampleoffsetcumulative.xaml#matrixanimationusingpathoffsetcumulativewholepage)]  
  
 [!code-csharp[PathAnimationGallery_procedural_snip#MatrixAnimationUsingPathOffsetCumulativeWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/CSharp/MatrixAnimationUsingPathExampleOffsetCumulative.cs#matrixanimationusingpathoffsetcumulativewholepage)]
 [!code-vb[PathAnimationGallery_procedural_snip#MatrixAnimationUsingPathOffsetCumulativeWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/VisualBasic/MatrixAnimationUsingPathExampleOffsetCumulative.vb#matrixanimationusingpathoffsetcumulativewholepage)]  
  
 Tenga en cuenta que, aunque la propiedad <xref:System.Windows.Media.Animation.MatrixAnimationUsingPath.IsOffsetCumulative%2A> hace que los valores de desplazamiento se acumulen en las repeticiones, no hace que se acumulen los valores de rotación. Para que se acumulen los valores de rotación, establezca las propiedades <xref:System.Windows.Media.Animation.MatrixAnimationUsingPath.DoesRotateWithTangent%2A> y <xref:System.Windows.Media.Animation.MatrixAnimationUsingPath.IsAngleCumulative%2A> de la animación en `true`.  
  
 Para obtener el ejemplo completo, consulte [ejemplo de animación de trazado](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/PathAnimations). Para ver un ejemplo en el que se muestra cómo animar un valor <xref:System.Windows.Media.Matrix> a lo largo de un trazado sin acumulación de desplazamiento, vea [animar un objeto a lo largo de un trazado (animación en matriz)](how-to-animate-an-object-along-a-path-matrix-animation.md).  
  
## <a name="see-also"></a>Consulte también

- [Información general sobre animaciones](animation-overview.md)
- [Temas de procedimientos de animación de trazado](path-animation-how-to-topics.md)
