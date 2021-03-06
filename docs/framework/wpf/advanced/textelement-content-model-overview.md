---
title: Información general sobre el modelo de contenido de TextElement
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- documents [WPF], flow documents
- TextElement content model [WPF]
- flow content elements [WPF], TextElement content model
ms.assetid: d0a7791c-b090-438c-812f-b9d009d83ee9
ms.openlocfilehash: ddb2613dc924424b5f4b9d90f06d44b45b7b5e77
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2020
ms.locfileid: "79186901"
---
# <a name="textelement-content-model-overview"></a>Información general sobre el modelo de contenido de TextElement
Esta información general del modelo de <xref:System.Windows.Documents.TextElement>contenido describe el contenido admitido para un archivo . La <xref:System.Windows.Documents.Paragraph> clase es <xref:System.Windows.Documents.TextElement>un tipo de . Un modelo de contenido describe qué objetos o elementos se pueden contener en otros. Esta información general resume el modelo de <xref:System.Windows.Documents.TextElement>contenido utilizado para los objetos derivados de . Para obtener más información, consulte [Información general del documento](flow-document-overview.md)de flujo .  

<a name="text_element_classes"></a>
## <a name="content-model-diagram"></a>Diagrama del modelo de contenido  
 En el diagrama siguiente se resume el <xref:System.Windows.Documents.TextElement> modelo de contenido `TextElement` para las clases derivadas, así como cómo encajan otras clases que no son de clases en este modelo.  
  
 ![Diagrama: Esquema de contención de contenido dinámico](./media/flow-content-schema.png "Flow_Content_Schema")  
  
 Como se puede ver en el diagrama anterior, los elementos secundarios permitidos para un <xref:System.Windows.Documents.Block> elemento no <xref:System.Windows.Documents.Inline> están necesariamente determinados por si una clase se deriva de la clase o una clase. Por ejemplo, <xref:System.Windows.Documents.Span> una <xref:System.Windows.Documents.Inline>(una clase derivada) <xref:System.Windows.Documents.Inline> solo puede <xref:System.Windows.Documents.Figure> tener elementos secundarios, pero una (también una <xref:System.Windows.Documents.Inline>clase derivada) solo puede tener <xref:System.Windows.Documents.Block> elementos secundarios. Por tanto, un diagrama es útil para determinar rápidamente qué elemento puede incluirse en otro. Por ejemplo, vamos a usar el diagrama para determinar cómo <xref:System.Windows.Controls.RichTextBox>construir el contenido de flujo de un archivo .  
  
1. A <xref:System.Windows.Controls.RichTextBox> debe <xref:System.Windows.Documents.FlowDocument> contener un que, <xref:System.Windows.Documents.Block>a su vez, debe contener un objeto derivado. A continuación, se muestra el segmento correspondiente del diagrama anterior.  
  
     ![Diagrama: Reglas de contención de RichTextBox](./media/flow-ovw-schemawalkthrough1.png "Flow_Ovw_SchemaWalkThrough1")  
  
     Llegados a este punto, esta es la apariencia que podría tener el marcado.  
  
     [!code-xaml[FlowOvwSnippets_snip#SchemaWalkThrough1](~/samples/snippets/csharp/VS_Snippets_Wpf/FlowOvwSnippets_snip/CS/MiscSnippets.xaml#schemawalkthrough1)]  
  
2. Según el diagrama, <xref:System.Windows.Documents.Block> hay varios elementos <xref:System.Windows.Documents.Section> <xref:System.Windows.Documents.Table>para <xref:System.Windows.Documents.List>elegir, entre los que se incluyen , <xref:System.Windows.Documents.BlockUIContainer> <xref:System.Windows.Documents.Paragraph>, , y (consulte Clases derivadas de bloques en el diagrama anterior). Digamos que queremos <xref:System.Windows.Documents.Table>un . Según el diagrama anterior, <xref:System.Windows.Documents.Table> <xref:System.Windows.Documents.TableRowGroup> a <xref:System.Windows.Documents.TableRow> contiene un <xref:System.Windows.Documents.TableCell> contenedor de <xref:System.Windows.Documents.Block>elementos, que contienen elementos que contienen un objeto derivado. El siguiente es el <xref:System.Windows.Documents.Table> segmento correspondiente para tomar del diagrama anterior.  
  
     ![Diagrama: esquema secundario&#47;padre para la tabla](./media/flow-ovw-schemawalkthrough2.png "Flow_Ovw_SchemaWalkThrough2")  
  
     A continuación, se muestra el marcado correspondiente.  
  
     [!code-xaml[FlowOvwSnippets_snip#SchemaWalkThrough2](~/samples/snippets/csharp/VS_Snippets_Wpf/FlowOvwSnippets_snip/CS/MiscSnippets.xaml#schemawalkthrough2)]  
  
3. Una vez más, uno o más <xref:System.Windows.Documents.Block> elementos son necesarios debajo de un <xref:System.Windows.Documents.TableCell>archivo . Para facilitar el proceso, vamos a colocar texto dentro de la celda. Podemos hacer esto <xref:System.Windows.Documents.Paragraph> usando <xref:System.Windows.Documents.Run> un con un elemento. A continuación se muestran los segmentos correspondientes del diagrama que muestran que un <xref:System.Windows.Documents.Paragraph> puede tomar un <xref:System.Windows.Documents.Inline> elemento y que un <xref:System.Windows.Documents.Run> (un <xref:System.Windows.Documents.Inline> elemento) sólo puede tomar texto sin formato.  
  
     ![Diagrama: Esquema secundario&#47;padre para párrafo](./media/flow-ovw-schemawalkthrough3.png "Flow_Ovw_SchemaWalkThrough3")  
  
     ![Diagrama: Esquema primario&#47;secundario para Ejecutar](./media/flow-ovw-schemawalkthrough4.png "Flow_Ovw_SchemaWalkThrough4")  
  
 A continuación se muestra el ejemplo completo en el marcado.  
  
 [!code-xaml[FlowOvwSnippets_snip#SchemaExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/FlowOvwSnippets_snip/CS/SchemaExample.xaml#schemaexamplewholepage)]  
  
<a name="Using_the_Content_Property"></a>
## <a name="working-with-textelement-content-programmatically"></a>Trabajar con contenido de TextElement mediante programación  
 El contenido <xref:System.Windows.Documents.TextElement> de a se compone de colecciones y, por lo tanto, la manipulación mediante programación del contenido de <xref:System.Windows.Documents.TextElement> los objetos se realiza trabajando con estas colecciones. Hay tres colecciones <xref:System.Windows.Documents.TextElement> diferentes utilizadas por las clases derivadas:  
  
- <xref:System.Windows.Documents.InlineCollection>: representa una <xref:System.Windows.Documents.Inline> colección de elementos. <xref:System.Windows.Documents.InlineCollection> define el contenido secundario permitido de los elementos <xref:System.Windows.Documents.Paragraph>, <xref:System.Windows.Documents.Span> y <xref:System.Windows.Controls.TextBlock>.  
  
- <xref:System.Windows.Documents.BlockCollection>: representa una <xref:System.Windows.Documents.Block> colección de elementos. <xref:System.Windows.Documents.BlockCollection> define el contenido secundario permitido de los elementos <xref:System.Windows.Documents.FlowDocument>, <xref:System.Windows.Documents.Section>, <xref:System.Windows.Documents.ListItem>, <xref:System.Windows.Documents.TableCell>, <xref:System.Windows.Documents.Floater> y <xref:System.Windows.Documents.Figure>.  
  
- <xref:System.Windows.Documents.ListItemCollection>: un elemento de contenido de flujo que representa <xref:System.Windows.Documents.List>un elemento de contenido determinado en un pedido o desordenado .  
  
 Puede manipular (agregar o quitar elementos) de estas colecciones utilizando las propiedades respectivas de **Inlines**, **Blocks**y **ListItems**. En los ejemplos siguientes se muestra cómo manipular el contenido de un Span mediante el **Inlines** propiedad.  
  
> [!NOTE]
> El objeto Table usa varias colecciones para manipular su contenido, pero no se describen aquí. Para obtener más información, consulte [Información general sobre tablas](table-overview.md).  
  
 En el ejemplo <xref:System.Windows.Documents.Span> siguiente se crea `Add` un nuevo objeto y, a <xref:System.Windows.Documents.Span>continuación, se usa el método para agregar dos ejecuciones de texto como elementos secundarios de contenido del archivo .  
  
 [!code-csharp[SpanSnippets#_SpanInlinesAdd](~/samples/snippets/csharp/VS_Snippets_Wpf/SpanSnippets/CSharp/Window1.xaml.cs#_spaninlinesadd)]
 [!code-vb[SpanSnippets#_SpanInlinesAdd](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SpanSnippets/visualbasic/window1.xaml.vb#_spaninlinesadd)]  
  
 En el ejemplo <xref:System.Windows.Documents.Run> siguiente se crea un nuevo <xref:System.Windows.Documents.Span>elemento y se inserta al principio del archivo .  
  
 [!code-csharp[SpanSnippets#_SpanInlinesInsert](~/samples/snippets/csharp/VS_Snippets_Wpf/SpanSnippets/CSharp/Window1.xaml.cs#_spaninlinesinsert)]
 [!code-vb[SpanSnippets#_SpanInlinesInsert](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SpanSnippets/visualbasic/window1.xaml.vb#_spaninlinesinsert)]  
  
 En el ejemplo siguiente <xref:System.Windows.Documents.Inline> se <xref:System.Windows.Documents.Span>elimina el último elemento del archivo .  
  
 [!code-csharp[SpanSnippets#_SpanInlinesRemoveLast](~/samples/snippets/csharp/VS_Snippets_Wpf/SpanSnippets/CSharp/Window1.xaml.cs#_spaninlinesremovelast)]
 [!code-vb[SpanSnippets#_SpanInlinesRemoveLast](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SpanSnippets/visualbasic/window1.xaml.vb#_spaninlinesremovelast)]  
  
 En el ejemplo siguiente se<xref:System.Windows.Documents.Inline> borra todo <xref:System.Windows.Documents.Span>el contenido ( elementos) del archivo .  
  
 [!code-csharp[SpanSnippets#_SpanInlinesClear](~/samples/snippets/csharp/VS_Snippets_Wpf/SpanSnippets/CSharp/Window1.xaml.cs#_spaninlinesclear)]
 [!code-vb[SpanSnippets#_SpanInlinesClear](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SpanSnippets/visualbasic/window1.xaml.vb#_spaninlinesclear)]  
  
<a name="Types_that_Share_this_Content_Model"></a>
## <a name="types-that-share-this-content-model"></a>Tipos que comparten este modelo de contenido  
 Los siguientes tipos <xref:System.Windows.Documents.TextElement> heredan de la clase y se pueden usar para mostrar el contenido descrito en esta información general.  
  
 <xref:System.Windows.Documents.Bold>, <xref:System.Windows.Documents.Figure>, <xref:System.Windows.Documents.Floater>, <xref:System.Windows.Documents.Hyperlink>, <xref:System.Windows.Documents.InlineUIContainer>, <xref:System.Windows.Documents.Italic>, <xref:System.Windows.Documents.LineBreak>, <xref:System.Windows.Documents.List>, <xref:System.Windows.Documents.ListItem>, <xref:System.Windows.Documents.Paragraph>, <xref:System.Windows.Documents.Run>, <xref:System.Windows.Documents.Section>, <xref:System.Windows.Documents.Span>, <xref:System.Windows.Documents.Table>, <xref:System.Windows.Documents.Underline>.  
  
 Tenga en cuenta que esta lista solo incluye tipos no abstractos distribuidos con el Windows SDK. Puede usar otros tipos <xref:System.Windows.Documents.TextElement>que hereden de .  
  
<a name="Types_that_Can_Contain_ContentControl_Objects"></a>
## <a name="types-that-can-contain-textelement-objects"></a>Tipos que pueden contener objetos TextElement  
 Consulte [Modelo de contenido de WPF](../controls/wpf-content-model.md).  
  
## <a name="see-also"></a>Consulte también

- [Manipular un objeto FlowDocument mediante la propiedad Blocks](how-to-manipulate-a-flowdocument-through-the-blocks-property.md)
- [Manipular elementos de contenido dinámico mediante la propiedad Blocks](how-to-manipulate-flow-content-elements-through-the-blocks-property.md)
- [Manipular un objeto FlowDocument mediante la propiedad Blocks](how-to-manipulate-a-flowdocument-through-the-blocks-property.md)
- [Manipular las columnas de una tabla mediante la propiedad Columns](how-to-manipulate-table-columns-through-the-columns-property.md)
- [Manipular grupos de filas de una tabla mediante la propiedad RowGroups](how-to-manipulate-table-row-groups-through-the-rowgroups-property.md)
