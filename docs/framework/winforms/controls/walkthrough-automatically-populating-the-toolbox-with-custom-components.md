---
title: 'Tutorial: Rellenar automáticamente el cuadro de herramientas con componentes personalizados'
ms.date: 03/30/2017
helpviewer_keywords:
- IToolboxService interface
- Toolbox [Windows Forms], populating
- custom components [Windows Forms], adding to Toolbox
ms.assetid: 2fa1e3e8-6b9f-42b2-97c0-2be57444dba4
ms.openlocfilehash: 876de650f9c182c0f82a02d1c5b356faa4f7f118
ms.sourcegitcommit: 0d0a6e96737dfe24d3257b7c94f25d9500f383ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65211153"
---
# <a name="walkthrough-automatically-populating-the-toolbox-with-custom-components"></a>Tutorial: Rellenar automáticamente el cuadro de herramientas con componentes personalizados

Si los componentes están definidos por un proyecto en la solución actualmente abierta, estas aparecerán automáticamente en el **cuadro de herramientas**, con ninguna acción requerida por el usuario. Puede rellenar manualmente el **cuadro de herramientas** con componentes personalizados mediante el uso de la [elegir elementos de cuadro de diálogo) (Visual Studio)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/dyca0t6t(v=vs.100)), pero la **cuadro de herramientas** tiene en cuenta de elementos de la solución de resultados de la compilación con las siguientes características:

- Implementa <xref:System.ComponentModel.IComponent>;

- No tiene <xref:System.ComponentModel.ToolboxItemAttribute> establecido en `false`;

- No tiene <xref:System.ComponentModel.DesignTimeVisibleAttribute> establecido en `false`.

> [!NOTE]
> El **cuadro de herramientas** no sigue las cadenas de referencia, por lo que no muestra los elementos que no se compilan en un proyecto de la solución.

Este tutorial muestra cómo un componente personalizado aparece automáticamente en el **cuadro de herramientas** una vez que se basa el componente. Las tareas ilustradas en este tutorial incluyen:

- Crear un proyecto de Windows Forms.

- Creación de un componente personalizado.

- Creación de una instancia de un componente personalizado.

- Descarga y carga un componente personalizado.

Cuando haya terminado, verá que el **cuadro de herramientas** se rellena con un componente que ha creado.

## <a name="create-the-project"></a>Crear el proyecto

1. En Visual Studio, cree un proyecto de aplicación basada en Windows llamado `ToolboxExample` (**archivo** > **New** > **proyecto**  >  **Visual C#**  o **Visual Basic** > **escritorio clásico de** > **Windows Forms Aplicación**).

2. Agregue un nuevo componente al proyecto. Asígnele el nombre `DemoComponent`.

     Para obtener más información, vea [Cómo: Agregar nuevos elementos de proyecto](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/w0572c5b(v=vs.100)).

3. Compile el proyecto.

4. Desde el **herramientas** menú, haga clic en el **opciones** elemento. Haga clic en **General** bajo el **Diseñador de Windows Forms** de elementos y asegúrese de que el **AutoToolboxPopulate** opción está establecida en **True**.

## <a name="create-an-instance-of-a-custom-component"></a>Cree una instancia de un componente personalizado

El siguiente paso es crear una instancia del componente personalizado en el formulario. Dado que el **cuadro de herramientas** automáticamente cuentas para el componente nuevo, esto es tan fácil como crear cualquier otro componente o control.

1. Abra el formulario del proyecto en el **Diseñador de formularios**.

2. En el **cuadro de herramientas**, haga clic en la nueva ficha denominada **Componentes ToolboxExample**.

     Al hacer clic en la pestaña, verá **DemoComponent**.

    > [!NOTE]
    > Por motivos de rendimiento, los componentes en el área rellena automáticamente de la **cuadro de herramientas** no mostrar mapas de bits personalizados y el <xref:System.Drawing.ToolboxBitmapAttribute> no se admite. Para mostrar un icono para un componente personalizado en el **cuadro de herramientas**, utilice el **elegir elementos del cuadro de herramientas** cuadro de diálogo para cargar el componente.

3. Arrastre el componente al formulario.

     Se crea una instancia del componente y se agrega a la **Bandeja de componentes**.

## <a name="unload-and-reload-a-custom-component"></a>Descargar y recargar un componente personalizado

El **cuadro de herramientas** tiene en cuenta los componentes en cada carga de proyecto y, cuando se descarga un proyecto, se quita las referencias a componentes del proyecto.

1. Descargue el proyecto de la solución.

     Para obtener más información sobre cómo descargar los proyectos, vea [Cómo: Descargar y recargar proyectos](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/tt479x1t(v=vs.100)). Si se le solicite guardar, elija **Sí**.

2. Agregue un nuevo **aplicación Windows** proyecto a la solución. Abra el formulario en el **diseñador**.

     El **Componentes ToolboxExample** ficha desde el proyecto anterior es ahora han desaparecido.

3. Volver a cargar el `ToolboxExample` proyecto.

     El **Componentes ToolboxExample** pestaña ahora vuelve a aparecer.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se muestra que el **cuadro de herramientas** tiene en cuenta los componentes de un proyecto, pero la **cuadro de herramientas** es también tiene en cuenta los controles. Experimentar con sus propios controles personalizados mediante la adición y eliminación de proyectos de control de la solución.

## <a name="see-also"></a>Vea también

- [General, Diseñador de formularios de Windows, cuadro de diálogo Opciones](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/5aazxs78(v=vs.100))
- [Cómo: Manipular las fichas del cuadro de herramientas](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/66kwe227(v=vs.100))
- [Elegir elementos del cuadro de herramientas (Cuadro de diálogo): Visual Studio](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/dyca0t6t(v=vs.100))
- [Insertar controles en Windows Forms](putting-controls-on-windows-forms.md)
