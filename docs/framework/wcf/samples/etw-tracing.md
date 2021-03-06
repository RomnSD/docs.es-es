---
title: Seguimiento ETW
ms.date: 03/30/2017
ms.assetid: ac99a063-e2d2-40cc-b659-d23c2f783f92
ms.openlocfilehash: 07379a464e6635a3de10c08647dbc769a5885e4e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2020
ms.locfileid: "79183706"
---
# <a name="etw-tracing"></a>Seguimiento ETW
Este ejemplo muestra cómo implementar el seguimiento de un extremo a otro (E2E) mediante el Seguimiento de eventos para Windows (ETW) y `ETWTraceListener` que se proporciona este ejemplo. El ejemplo se basa en la [Introducción](../../../../docs/framework/wcf/samples/getting-started-sample.md) e incluye el seguimiento ETW.  
  
> [!NOTE]
> El procedimiento de instalación y las instrucciones de compilación de este ejemplo se encuentran al final de este tema.  
  
 En este ejemplo se supone que está familiarizado con [El seguimiento y](../../../../docs/framework/wcf/samples/tracing-and-message-logging.md)el registro de mensajes .  
  
 Cada origen de seguimiento en el modelo de seguimiento<xref:System.Diagnostics> puede tener varios agentes de escucha de seguimiento que determinan donde y cómo se siguen paso a paso los datos. El tipo de agente de escucha define el formato en el que la información de seguimiento está registrada. El ejemplo de código siguiente muestra cómo agregar el agente de escucha a la configuración.  
  
```xml  
<system.diagnostics>  
    <sources>  
        <source name="System.ServiceModel"
             switchValue="Verbose,ActivityTracing"  
             propagateActivity="true">  
            <listeners>  
                <add type=  
                   "System.Diagnostics.DefaultTraceListener"  
                   name="Default">  
                   <filter type="" />  
                </add>  
                <add name="ETW">  
                    <filter type="" />  
                </add>  
            </listeners>  
        </source>  
    </sources>  
    <sharedListeners>  
        <add type=  
            "Microsoft.ServiceModel.Samples.EtwTraceListener, ETWTraceListener"  
            name="ETW" traceOutputOptions="Timestamp">  
            <filter type="" />  
       </add>  
    </sharedListeners>  
</system.diagnostics>  
```  
  
 Antes de utilizar este agente de escucha, se debe iniciar una Sesión de seguimiento de ETW. Esta sesión se puede iniciar utilizando Logman.exe o Tracelog.exe. Un archivo SetupETW.bat está incluido con este ejemplo para que pueda preparar la Sesión de seguimiento ETW junto con un archivo CleanupETW.bat para cerrar la sesión y completar el archivo de registro.  
  
> [!NOTE]
> El procedimiento de instalación y las instrucciones de compilación de este ejemplo se encuentran al final de este tema. Para obtener más información acerca de estas herramientas, consulte<https://go.microsoft.com/fwlink/?LinkId=56580>  
  
 Al utilizar ETWTraceListener, los rastros están registrados en archivos .etl binarios. Con el seguimiento ServiceModel activado, todos los rastros generados aparecen en el mismo archivo. Utilice la herramienta [Service Trace Viewer (SvcTraceViewer.exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md) para ver los archivos de registro .etl y .svclog. El visor crea una vista de un extremo a otro del sistema que permite hacer el seguimiento un mensaje de su origen a su destino y punto de consumo.  
  
 El Agente de escucha de seguimiento de ETW admite el registro circular. Para habilitar esta característica, vaya a `cmd` **Inicio**, **Ejecutar** y escriba para iniciar una consola de comandos. En el comando siguiente, reemplace el parámetro `<logfilename>` con el nombre de su archivo de registro.  
  
```console  
logman create trace Wcf -o <logfilename> -p "{411a0819-c24b-428c-83e2-26b41091702e}" -f bincirc -max 1000  
```  
  
 Los modificadores `-f` y `-max` son opcionales. Especifican respectivamente el formato circular binario y un tamaño de registro de máximo de 1000MB. El modificador `-p` se utiliza para especificar el proveedor de seguimiento. En nuestro ejemplo, `"{411a0819-c24b-428c-83e2-26b41091702e}"` es el GUID para "Proveedor del Ejemplo de "XML ETW.  
  
 Para iniciar la sesión, escriba el siguiente comando.  
  
```console  
logman start Wcf  
```  
  
 Después de haber terminado de registrarse, puede detener la sesión con el comando siguiente.  
  
```console  
logman stop Wcf  
```  
  
 Este proceso genera registros circulares binarios que puede procesar con la herramienta de su elección, incluida la herramienta [Service Trace Viewer (SvcTraceViewer.exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md) o Tracerpt.  
  
 También puede revisar el ejemplo [de seguimiento circular](../../../../docs/framework/wcf/samples/circular-tracing.md) para obtener más información sobre un agente de escucha alternativo para realizar el registro circular.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Configurar, compilar y ejecutar el ejemplo  
  
1. Asegúrese de que ha realizado el procedimiento de instalación única [para los ejemplos](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)de Windows Communication Foundation .  
  
2. Para compilar la solución, siga las instrucciones de Creación de [ejemplos](../../../../docs/framework/wcf/samples/building-the-samples.md)de Windows Communication Foundation .  
  
    > [!NOTE]
    > Para utilizar los comandos RegisterProvider.bat, SetupETW.bat y CleanupETW.bat, debe ejecutarlos en una cuenta de administrador local. Si utiliza Windows Vista o posterior, también debe ejecutar el símbolo del sistema con privilegios elevados. Para ello, haga clic con el botón derecho en el icono del símbolo del sistema y, a continuación, haga clic en **Ejecutar como administrador**.  
  
3. Antes de ejecutar el ejemplo, ejecute RegisterProvider.bat en el cliente y servidor. Esto prepara el archivo ETWTracingSampleLog.etl resultante para generar rastros que puedan ser leídos por el visor de seguimiento de servicios. Este archivo se puede buscar en la carpeta C:\logs. Si esta carpeta no existe, se debe crear; de lo contrario, no se generará ningún seguimiento. A continuación, ejecute SetupETW.bat en los equipos servidor y cliente para comenzar la sesión de seguimiento de ETW. El archivo SetupETW.bat se encuentra en la carpeta CS\Client.  
  
4. Para ejecutar el ejemplo en una configuración de un equipo o entre equipos, siga las instrucciones de Ejecución de [los ejemplos](../../../../docs/framework/wcf/samples/running-the-samples.md)de Windows Communication Foundation .  
  
5. Cuando complete el ejemplo, ejecute CleanupETW.bat para completar la creación del archivo ETWTracingSampleLog.etl.  
  
6. Abra el archivo ETWTracingSampleLog.etl desde dentro del Visor de seguimiento de servicios. Le solicitarán que guarde el archivo con formato binario como un archivo .svclog.  
  
7. Abra el archivo .svclog creado recientemente desde el Visor de seguimiento de servicios para ver los seguimientos de ServiceModel y ETW.  
  
> [!IMPORTANT]
> Puede que los ejemplos ya estén instalados en su equipo. Compruebe el siguiente directorio (predeterminado) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Si este directorio no existe, vaya a Ejemplos de [Windows Communication Foundation (WCF) y Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para descargar todos los ejemplos y [!INCLUDE[wf1](../../../../includes/wf1-md.md)] Windows Communication Foundation (WCF). Este ejemplo se encuentra en el siguiente directorio.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\AnalyticTrace`  
  
## <a name="see-also"></a>Consulte también

- [Ejemplos de supervisión de AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ff383407(v=azure.10))
