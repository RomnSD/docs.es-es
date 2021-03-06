---
title: 'Cómo: Implementar la validación con el control DataGrid'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGrid [WPF], validation
- validation [WPF], DataGrid
ms.assetid: ec6078a8-1e42-4648-b414-f4348e81bda1
ms.openlocfilehash: 401949aa4bea924b458a91dc00c454c97aff4895
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458458"
---
# <a name="how-to-implement-validation-with-the-datagrid-control"></a>Cómo: Implementar la validación con el control DataGrid
El control <xref:System.Windows.Controls.DataGrid> permite realizar la validación en el nivel de celda y de fila. Con la validación de nivel de celda, se validan las propiedades individuales de un objeto de datos enlazado cuando un usuario actualiza un valor. Con la validación de nivel de fila, se validan los objetos de datos completos cuando un usuario confirma los cambios en una fila. También puede proporcionar comentarios visuales personalizados para los errores de validación o usar los comentarios visuales predeterminados que proporciona el control de <xref:System.Windows.Controls.DataGrid>.  
  
 Los procedimientos siguientes describen cómo aplicar reglas de validación a enlaces de <xref:System.Windows.Controls.DataGrid> y personalizar los comentarios visuales.  
  
### <a name="to-validate-individual-cell-values"></a>Para validar valores de celda individuales  
  
- Especifique una o varias reglas de validación en el enlace utilizado con una columna. Esto es similar a la validación de datos en controles simples, como se describe en [información general sobre el enlace de datos](../data/data-binding-overview.md).  
  
     En el ejemplo siguiente se muestra un control de <xref:System.Windows.Controls.DataGrid> con cuatro columnas enlazadas a diferentes propiedades de un objeto comercial. Tres de las columnas especifican el <xref:System.Windows.Controls.ExceptionValidationRule> estableciendo la propiedad <xref:System.Windows.Data.Binding.ValidatesOnExceptions%2A> en `true`.  
  
     [!code-xaml[DataGrid_Validation#BasicXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/window1.xaml#basicxaml)]  
  
     Cuando un usuario escribe un valor no válido (como un entero en la columna ID. de curso), aparece un borde rojo alrededor de la celda. Puede cambiar estos comentarios de validación predeterminados tal y como se describe en el procedimiento siguiente.  
  
### <a name="to-customize-cell-validation-feedback"></a>Para personalizar los comentarios de validación de celda  
  
- Establezca la propiedad <xref:System.Windows.Controls.DataGridBoundColumn.EditingElementStyle%2A> de la columna en un estilo apropiado para el control de edición de la columna. Dado que los controles de edición se crean en tiempo de ejecución, no puede usar la propiedad adjunta <xref:System.Windows.Controls.Validation.ErrorTemplate%2A?displayProperty=nameWithType> como lo haría con los controles simples.  
  
     En el ejemplo siguiente se actualiza el ejemplo anterior agregando un estilo de error compartido por las tres columnas con reglas de validación. Cuando un usuario escribe un valor no válido, el estilo cambia el color de fondo de la celda y agrega una información sobre herramientas. Tenga en cuenta el uso de un desencadenador para determinar si hay un error de validación. Esto es necesario porque actualmente no hay ninguna plantilla de error dedicada para las celdas.  
  
     [!code-xaml[DataGrid_Validation#CellValidationXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml#cellvalidationxaml)]  
  
     Puede implementar una personalización más amplia si reemplaza la <xref:System.Windows.Controls.DataGridColumn.CellStyle%2A> utilizada por la columna.  
  
### <a name="to-validate-multiple-values-in-a-single-row"></a>Para validar varios valores en una sola fila  
  
1. Implemente una subclase <xref:System.Windows.Controls.ValidationRule> que compruebe varias propiedades del objeto de datos enlazado. En la implementación del método de <xref:System.Windows.Controls.ValidationRule.Validate%2A>, convierta el valor del parámetro `value` en una instancia de <xref:System.Windows.Data.BindingGroup>. A continuación, puede tener acceso al objeto de datos a través de la propiedad <xref:System.Windows.Data.BindingGroup.Items%2A>.  
  
     En el ejemplo siguiente se muestra este proceso para validar si el valor de propiedad `StartDate` para un objeto `Course` es anterior a su valor de propiedad `EndDate`.  
  
     [!code-csharp[DataGrid_Validation#CourseValidationRule](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml.cs#coursevalidationrule)]
     [!code-vb[DataGrid_Validation#CourseValidationRule](~/samples/snippets/visualbasic/VS_Snippets_Wpf/datagrid_validation/vb/mainwindow.xaml.vb#coursevalidationrule)]  
  
2. Agregue la regla de validación a la colección de <xref:System.Windows.Controls.DataGrid.RowValidationRules%2A?displayProperty=nameWithType>. La propiedad <xref:System.Windows.Controls.DataGrid.RowValidationRules%2A> proporciona acceso directo a la propiedad <xref:System.Windows.Data.BindingGroup.ValidationRules%2A> de una instancia de <xref:System.Windows.Data.BindingGroup> que agrupa todos los enlaces utilizados por el control.  
  
     En el ejemplo siguiente se establece la propiedad <xref:System.Windows.Controls.DataGrid.RowValidationRules%2A> en XAML. La propiedad <xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> se establece en <xref:System.Windows.Controls.ValidationStep.UpdatedValue> para que la validación se produzca solo después de que se actualice el objeto de datos enlazado.  
  
     [!code-xaml[DataGrid_Validation#RowValidationRulesXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml#rowvalidationrulesxaml)]  
  
     Cuando un usuario especifica una fecha de finalización anterior a la fecha de inicio, aparece un signo de exclamación rojo (!) en el encabezado de fila. Puede cambiar estos comentarios de validación predeterminados tal y como se describe en el procedimiento siguiente.  
  
### <a name="to-customize-row-validation-feedback"></a>Para personalizar los comentarios de validación de filas  
  
- Establecer la propiedad <xref:System.Windows.Controls.DataGrid.RowValidationErrorTemplate%2A?displayProperty=nameWithType>. Esta propiedad le permite personalizar los comentarios de la validación de filas para los controles de <xref:System.Windows.Controls.DataGrid> individuales. También puede afectar a varios controles mediante el uso de un estilo de fila implícito para establecer la propiedad <xref:System.Windows.Controls.DataGridRow.ValidationErrorTemplate%2A?displayProperty=nameWithType>.  
  
     En el ejemplo siguiente se reemplazan los comentarios de validación de filas predeterminados por un indicador más visible. Cuando un usuario escribe un valor no válido, aparece un círculo rojo con un signo de exclamación blanco en el encabezado de fila. Esto sucede en los errores de validación de filas y celdas. El mensaje de error asociado se muestra en una información sobre herramientas.  
  
     [!code-xaml[DataGrid_Validation#RowValidationFeedbackXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml#rowvalidationfeedbackxaml)]  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se proporciona una demostración completa de la validación de celdas y filas. La clase `Course` proporciona un objeto de datos de ejemplo que implementa <xref:System.ComponentModel.IEditableObject> para admitir transacciones. El control <xref:System.Windows.Controls.DataGrid> interactúa con <xref:System.ComponentModel.IEditableObject> para permitir que los usuarios reviertan los cambios presionando ESC.  
  
> [!NOTE]
> Si usa Visual Basic, en la primera línea de MainWindow. XAML, reemplace `x:Class="DataGridValidation.MainWindow"` por `x:Class="MainWindow"`.  
  
 Para probar la validación, intente lo siguiente:  
  
- En la columna ID. de curso, escriba un valor que no sea entero.  
  
- En la columna fecha de finalización, escriba una fecha anterior a la fecha de inicio.  
  
- Elimine el valor de ID. de curso, fecha de inicio o fecha de finalización.  
  
- Para deshacer un valor de celda no válido, vuelva a colocar el cursor en la celda y presione la tecla ESC.  
  
- Para deshacer los cambios de una fila completa cuando la celda actual está en modo de edición, presione la tecla ESC dos veces.  
  
- Cuando se produzca un error de validación, mueva el puntero del mouse sobre el indicador del encabezado de fila para ver el mensaje de error asociado.  
  
 [!code-csharp[DataGrid_Validation#FullCode](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml.cs#fullcode)]
 [!code-vb[DataGrid_Validation#FullCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/datagrid_validation/vb/mainwindow.xaml.vb#fullcode)]  
  
 [!code-xaml[DataGrid_Validation#FullXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml#fullxaml)]  
  
## <a name="see-also"></a>Vea también

- <xref:System.Windows.Controls.DataGrid>
- [DataGrid](datagrid.md)
- [Enlace de datos](../../../desktop-wpf/data/data-binding-overview.md)
- [Implementar la validación de enlaces](../data/how-to-implement-binding-validation.md)
- [Implementar lógica de validación en objetos personalizados](../data/how-to-implement-validation-logic-on-custom-objects.md)
