---
title: Realizar pruebas de posicionamiento en la capa visual
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- hit testing functionality [WPF]
- visual layer [WPF], hit testing functionality
ms.assetid: b1a64b61-14be-4d75-b89a-5c67bebb2c7b
ms.openlocfilehash: d4d304353e91147c57297dcc4525759ff1474b4f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2020
ms.locfileid: "79186403"
---
# <a name="hit-testing-in-the-visual-layer"></a>Realizar pruebas de posicionamiento en la capa visual
En este tema se proporciona información general sobre la funcionalidad de prueba de posicionamiento que proporciona la capa visual. La compatibilidad con pruebas de posicionación le permite determinar si <xref:System.Windows.Media.Visual>una geometría o un valor de punto se encuentra dentro del contenido representado de un , lo que le permite implementar el comportamiento de la interfaz de usuario, como un rectángulo de selección, para seleccionar varios objetos.  

<a name="hit_testing_scenarios"></a>
## <a name="hit-testing-scenarios"></a>Escenarios de pruebas de posicionamiento  
 La <xref:System.Windows.UIElement> clase <xref:System.Windows.UIElement.InputHitTest%2A> proporciona el método, que le permite golpear la prueba contra un elemento mediante un valor de coordenadas determinado. En muchos casos, el <xref:System.Windows.UIElement.InputHitTest%2A> método proporciona la funcionalidad deseada para implementar pruebas de posicionación de elementos. Sin embargo, hay varios escenarios en los que puede necesitar implementar pruebas de posicionamiento en la capa visual.  
  
- Pruebas de posicionación contra<xref:System.Windows.UIElement> objetos que no<xref:System.Windows.UIElement> son objetos: esto se aplica si se realizan pruebas de posicionación que no son objetos, como <xref:System.Windows.Media.DrawingVisual> objetos gráficos.  
  
- Pruebas de posicionamiento con la utilización de una geometría: esto se aplica si necesita realizar una prueba de posicionamiento con un objeto de geometría en lugar usar el valor de coordenadas de un punto.  
  
- Pruebas de posicionamiento frente a varios objetos: esto se aplica cuando es necesario realizar pruebas de posicionamiento frente a varios objetos, tales como objetos superpuestos. Puede obtener resultados para todos los elementos visuales que corten una geometría o un punto, no solamente para el primero.  
  
- Ignorar <xref:System.Windows.UIElement> la directiva de prueba de posicionamiento: esto se aplica cuando necesita ignorar la <xref:System.Windows.UIElement> directiva de prueba de posicionamiento, que tiene en cuenta factores como si un elemento está deshabilitado o es invisible.  
  
> [!NOTE]
> Para obtener un ejemplo de código completo que muestra la prueba de posicionamiento en la capa visual, vea [Ejemplo de prueba de posicionamiento con DrawingVisuals](https://github.com/Microsoft/WPF-Samples/tree/master/Visual%20Layer/DrawingVisual) y [Ejemplo de prueba de posicionamiento de interoperabilidad con Win32](https://github.com/microsoft/WPF-Samples/tree/master/Visual%20Layer/VisualsHitTesting).  
  
<a name="hit_testing_support"></a>
## <a name="hit-testing-support"></a>Compatibilidad con la prueba de posicionamiento  
 El propósito <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> de los <xref:System.Windows.Media.VisualTreeHelper> métodos de la clase es determinar si un valor de geometría o de coordenadas de punto está dentro del contenido representado de un objeto determinado, como un control o un elemento gráfico. Por ejemplo, podría utilizar la prueba de posicionamiento para determinar si un clic del mouse dentro del rectángulo delimitador de un objeto pertenece a la geometría de un círculo. También puede optar por invalidar la implementación predeterminada de la prueba de posicionamiento, para realizar cálculos propios en las pruebas de posicionamiento.  
  
 La ilustración siguiente muestra la relación entre la región de un objeto no rectangular y su rectángulo delimitador.  
  
 ![Diagrama de región de prueba de posicionamiento válida](./media/wcpsdk-mmgraphics-visuals-hittest-1.png "wcpsdk_mmgraphics_visuals_hittest_1")  
Diagrama de región de prueba de posicionamiento válida  
  
<a name="hit_testing_and_z-order"></a>
## <a name="hit-testing-and-z-order"></a>Prueba de posicionamiento y orden z  
 La capa visual de [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] admite la prueba de posicionamiento frente a todos los objetos situados bajo un punto o una geometría, no solamente para el objeto de nivel superior. Los resultados se devuelven en un orden z. Sin embargo, el objeto visual que <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> se pasa como parámetro al método determina qué parte del árbol visual que se realizará la prueba de posicionación. Puede realizar la prueba de posicionamiento frente al árbol visual completo o frente a cualquier parte de él.  
  
 En la ilustración siguiente, el objeto de círculo está encima del objeto cuadrado y del triangular. Si solo está interesado en probar el objeto visual cuyo valor de orden z es superior, puede establecer la enumeración de prueba de posicionación visual para que vuelva <xref:System.Windows.Media.HitTestResultBehavior.Stop> desde el <xref:System.Windows.Media.HitTestResultCallback> para detener el recorrido de prueba de posicionación después del primer elemento.  
  
 ![Diagrama del orden z de un árbol visual](./media/wcpsdk-mmgraphics-visuals-hittest-2.png "wcpsdk_mmgraphics_visuals_hittest_2")  
Diagrama del orden z de un árbol visual  
  
 Si desea enumerar todos los objetos visuales bajo <xref:System.Windows.Media.HitTestResultBehavior.Continue> un <xref:System.Windows.Media.HitTestResultCallback>punto o geometría específicos, vuelva desde el archivo . Esto significa que puede realizar pruebas de posicionamiento para objetos visuales que estén bajo otros objetos, aunque estén completamente ocultos. Vea el ejemplo de código en la sección "Utilizar una devolución de llamada de resultados de pruebas de posicionamiento" para obtener más información.  
  
> [!NOTE]
> Un objeto visual transparente también se puede someter a una prueba de posicionamiento.  
  
<a name="using_default_hit_testing"></a>
## <a name="using-default-hit-testing"></a>Uso de la prueba de posicionamiento predeterminada  
 Puede identificar si un punto está dentro de la <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> geometría de un objeto visual, utilizando el método para especificar un objeto visual y un valor de coordenada de punto para probar. El parámetro de objeto visual identifica el punto inicial en el árbol visual para la búsqueda de la prueba de posicionamiento. Si se encuentra un objeto visual en el árbol visual cuya <xref:System.Windows.Media.HitTestResult.VisualHit%2A> geometría <xref:System.Windows.Media.HitTestResult> contiene la coordenada, se establece en la propiedad de un objeto. A <xref:System.Windows.Media.HitTestResult> continuación, se <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> devuelve desde el método. Si el punto no está contenido con el subárbol <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> `null`visual que está realizando pruebas, devuelve .  
  
> [!NOTE]
> La prueba de posicionamiento predeterminada devuelve el objeto de nivel superior en el orden z. Para identificar todos los objetos visuales, incluso aquellos que puedan estar ocultos, de forma total o parcial, utilice una devolución de llamada de resultado de prueba de posicionamiento.  
  
 El valor de coordenadas que <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> se pasa como parámetro de punto para el método tiene que ser relativo al espacio de coordenadas del objeto visual con el que se está realizando la prueba. Por ejemplo, si ha anidado objetos visuales definidos en (100, 100) en el espacio de coordenadas del elemento primario, la prueba de posicionamiento de un elemento secundario visual en (0, 0) es equivalente a la prueba de posicionamiento en (100, 100) en el espacio de coordenadas del elemento primario.  
  
 En el código siguiente se muestra cómo <xref:System.Windows.UIElement> configurar controladores de eventos del mouse para un objeto que se usa para capturar eventos utilizados para las pruebas de posicionación.  
  
 [!code-csharp[HitTestingOverview#100](~/samples/snippets/csharp/VS_Snippets_Wpf/HitTestingOverview/CSharp/Window1.xaml.cs#100)]
 [!code-vb[HitTestingOverview#100](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HitTestingOverview/visualbasic/window1.xaml.vb#100)]  
  
### <a name="how-the-visual-tree-affects-hit-testing"></a>Cómo afecta el árbol visual a la prueba de posicionamiento  
 El punto inicial en el árbol visual determina qué objetos se devuelven durante la enumeración de objetos de la prueba de posicionamiento. Si tiene varios objetos que desea someter a la prueba de posicionamiento, el objeto visual utilizado como punto inicial en el árbol visual debe ser el antecesor común de todos los objetos de interés. Por ejemplo, si estuviera interesado en la prueba de posicionamiento tanto del elemento de botón como del elemento visual de dibujo del diagrama siguiente, tendría que establecer el punto inicial del árbol visual en el antecesor común de ambos. En este caso, el elemento de lienzo es el antecesor común del elemento de botón y del elemento visual de dibujo.  
  
 ![Diagrama de una jerarquía de árbol visual](./media/wcpsdk-mmgraphics-visuals-overview-01.gif "wcpsdk_mmgraphics_visuals_overview_01")  
Diagrama de una jerarquía de árbol visual  
  
> [!NOTE]
> La <xref:System.Windows.UIElement.IsHitTestVisible%2A> propiedad obtiene o establece un valor <xref:System.Windows.UIElement>que declara si un objeto derivado posiblemente se puede devolver como resultado de la prueba de posicionamiento de alguna parte de su contenido representado. Esto permite modificar de manera selectiva el árbol visual para determinar qué objetos visuales están implicados en una prueba de posicionamiento.  
  
<a name="using_a_hit_test_result_callback"></a>
## <a name="using-a-hit-test-result-callback"></a>Uso de una devolución de llamada de resultados de prueba de posicionamiento  
 Puede enumerar todos los objetos visuales de un árbol visual cuya geometría contenga un valor de coordenadas especificado. Esto permite identificar todos los objetos visuales, incluso aquellos que puedan estar ocultos, de forma parcial o total, por otros objetos visuales. Para enumerar objetos visuales en <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> un árbol visual, utilice el método con una función de devolución de llamada de prueba de posicionación. El sistema llama a la función de devolución de llamada de la prueba de posicionamiento cuando el valor de coordenadas especificado esté contenido en un objeto visual.  
  
 Durante la enumeración de resultados de pruebas de posicionamiento no se debe realizar ninguna operación que modifique el árbol visual. Agregar o quitar un objeto del árbol visual mientras se recorre puede producir un comportamiento imprevisible. Puede modificar de forma segura <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> el árbol visual después de que se devuelva el método. Es posible que desee proporcionar una <xref:System.Collections.ArrayList>estructura de datos, como un , para almacenar valores durante la enumeración de resultados de la prueba de posicionación.  
  
 [!code-csharp[HitTestingOverview#101](~/samples/snippets/csharp/VS_Snippets_Wpf/HitTestingOverview/CSharp/Window1.xaml.cs#101)]
 [!code-vb[HitTestingOverview#101](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HitTestingOverview/visualbasic/window1.xaml.vb#101)]  
  
 El método de devolución de llamada de la prueba de posicionamiento define las acciones que se realizan cuando se identifica una prueba de posicionamiento en un objeto visual determinado del árbol visual. Después de realizar las <xref:System.Windows.Media.HitTestResultBehavior> acciones, devuelve un valor que determina si se debe continuar la enumeración de cualquier otro objeto visual o no.  
  
 [!code-csharp[HitTestingOverview#102](~/samples/snippets/csharp/VS_Snippets_Wpf/HitTestingOverview/CSharp/Window1.xaml.cs#102)]
 [!code-vb[HitTestingOverview#102](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HitTestingOverview/visualbasic/window1.xaml.vb#102)]  
  
> [!NOTE]
> El orden de enumeración de los objetos visuales de la posición es el orden z. El objeto visual de orden z de nivel superior es el primer objeto enumerado. Los demás objetos visuales enumerados están en orden z decreciente. Este orden de enumeración corresponde al orden de representación de elementos visuales.  
  
 Puede detener la enumeración de objetos visuales en cualquier <xref:System.Windows.Media.HitTestResultBehavior.Stop>momento en la función de devolución de llamada de prueba de posicionamiento devolviendo .  
  
 [!code-csharp[HitTestingOverview#103](~/samples/snippets/csharp/VS_Snippets_Wpf/HitTestingOverview/CSharp/Window1.xaml.cs#103)]
 [!code-vb[HitTestingOverview#103](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HitTestingOverview/visualbasic/window1.xaml.vb#103)]  
  
<a name="using_a_hit_test_filter_callback"></a>
## <a name="using-a-hit-test-filter-callback"></a>Uso de una devolución de llamada de filtro de prueba de posicionamiento  
 Puede utilizar un filtro opcional de la prueba de posicionamiento para restringir los objetos que se pasan en los resultados de pruebas de posicionamiento. Esto permite omitir, en los resultados de pruebas de posicionamiento, las partes del árbol visual que no desee procesar. Para implementar un filtro de prueba de posicionamiento, defina una función de <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> devolución de llamada del filtro de prueba de posicionamiento y pásela como un valor de parámetro cuando llame al método.  
  
 [!code-csharp[HitTestingOverview#104](~/samples/snippets/csharp/VS_Snippets_Wpf/HitTestingOverview/CSharp/Window1.xaml.cs#104)]
 [!code-vb[HitTestingOverview#104](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HitTestingOverview/visualbasic/window1.xaml.vb#104)]  
  
 Si no desea proporcionar la función de devolución `null` de llamada del <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> filtro de prueba de posicionación opcional, pase un valor como parámetro para el método.  
  
 [!code-csharp[HitTestingOverview#105](~/samples/snippets/csharp/VS_Snippets_Wpf/HitTestingOverview/CSharp/Window1.xaml.cs#105)]
 [!code-vb[HitTestingOverview#105](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HitTestingOverview/visualbasic/window1.xaml.vb#105)]  
  
 ![Eliminación de un árbol visual con un filtro de prueba de posicionamiento](./media/filteredvisualtree-01.png "FilteredVisualTree_01")  
Eliminar un árbol visual  
  
 La función de devolución de llamada de filtro de prueba de posicionamiento permite enumerar todos los objetos visuales cuyo contenido representado contenga las coordenadas que especifique. No obstante, es posible que desee omitir determinadas ramas del árbol visual, porque no le interese procesarlas en la función de devolución de llamada de los resultados de pruebas de posicionamiento. El valor devuelto de la función de devolución de llamada del filtro de la prueba de posicionamiento determina el tipo de acción que debe realizar la enumeración de los objetos visuales. Por ejemplo, si devuelve <xref:System.Windows.Media.HitTestFilterBehavior.ContinueSkipSelfAndChildren>el valor, , puede quitar el objeto visual actual y sus elementos secundarios de la enumeración de resultados de la prueba de posicionamiento. Esto significa que la función de devolución de llamada de los resultados de pruebas de posicionamiento no verá estos objetos en su enumeración. Cuando se eliminan objetos del árbol visual, se reduce el número de procesos durante el paso de enumeración de resultados de pruebas de posicionamiento. En el ejemplo de código siguiente, el filtro omite las etiquetas y sus descendientes y realiza pruebas de posicionamiento con todos los demás objetos.  
  
 [!code-csharp[HitTestingOverview#106](~/samples/snippets/csharp/VS_Snippets_Wpf/HitTestingOverview/CSharp/Window1.xaml.cs#106)]
 [!code-vb[HitTestingOverview#106](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HitTestingOverview/visualbasic/window1.xaml.vb#106)]  
  
> [!NOTE]
> Se llamará a la devolución de llamada de filtro de la prueba de posicionamiento en aquellos casos en que no se llame a la devolución de llamada de resultados de pruebas de posicionamiento.  
  
<a name="overriding_default_hit_testing"></a>
## <a name="overriding-default-hit-testing"></a>Invalidación de la prueba de posicionamiento predeterminada  
 Puede invalidar la compatibilidad predeterminada de pruebas de <xref:System.Windows.Media.Visual.HitTestCore%2A> posicionación de un objeto visual reemplazando el método. Esto significa que cuando <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> se invoca el <xref:System.Windows.Media.Visual.HitTestCore%2A> método, se llama a la implementación invalidada de. Se llama al método de invalidación cuando una prueba de posicionamiento está dentro del rectángulo delimitador del objeto visual, aunque las coordenadas estén fuera del contenido representado de dicho objeto.  
  
 [!code-csharp[HitTestingOverview#107](~/samples/snippets/csharp/VS_Snippets_Wpf/HitTestingOverview/CSharp/Window1.xaml.cs#107)]
 [!code-vb[HitTestingOverview#107](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HitTestingOverview/visualbasic/window1.xaml.vb#107)]  
  
 Pueden darse ocasiones en que desee realizar la prueba de posicionamiento tanto respecto al rectángulo delimitador como respecto al contenido representado de un objeto visual. Mediante el `PointHitTestParameters` uso del valor <xref:System.Windows.Media.Visual.HitTestCore%2A> de parámetro en el <xref:System.Windows.Media.Visual.HitTestCore%2A>método invalidado como parámetro para el método base , puede realizar acciones basadas en un hit del rectángulo delimitador de un objeto visual y, a continuación, realizar una segunda prueba de posicionamiento en el contenido representado del objeto visual.  
  
 [!code-csharp[HitTestingOverview#108](~/samples/snippets/csharp/VS_Snippets_Wpf/HitTestingOverview/CSharp/Window1.xaml.cs#108)]
 [!code-vb[HitTestingOverview#108](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HitTestingOverview/visualbasic/window1.xaml.vb#108)]  
  
## <a name="see-also"></a>Consulte también

- <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A>
- <xref:System.Windows.Media.HitTestResult>
- <xref:System.Windows.Media.HitTestResultCallback>
- <xref:System.Windows.Media.HitTestFilterCallback>
- <xref:System.Windows.UIElement.IsHitTestVisible%2A>
- [Ejemplo de prueba de posicionamiento con DrawingVisuals](https://github.com/Microsoft/WPF-Samples/tree/master/Visual%20Layer/DrawingVisual)
- [Ejemplo de prueba de posicionamiento de interoperabilidad con Win32](https://github.com/microsoft/WPF-Samples/tree/master/Visual%20Layer/VisualsHitTesting)
- [Geometría de una prueba de posicionamiento en un objeto Visual](how-to-hit-test-geometry-in-a-visual.md)
- [Realizar pruebas de posicionamiento mediante un contenedor host Win32](how-to-hit-test-using-a-win32-host-container.md)
