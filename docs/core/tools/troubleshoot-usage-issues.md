---
title: Solución de problemas de uso de herramientas de .NET Core
description: Descubra los problemas comunes que se producen al ejecutar herramientas de .NET Core y sus posibles soluciones.
author: kdollard
ms.date: 09/23/2019
ms.openlocfilehash: eb769550493e5a25d4380cd543a3bbec880b38e9
ms.sourcegitcommit: 8b8dd14dde727026fd0b6ead1ec1df2e9d747a48
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71332954"
---
# <a name="troubleshoot-net-core-tool-usage-issues"></a><span data-ttu-id="0af8d-103">Solución de problemas de uso de herramientas de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0af8d-103">Troubleshoot .NET Core tool usage issues</span></span>

<span data-ttu-id="0af8d-104">Es posible que surjan problemas al intentar instalar o ejecutar una herramienta de .NET Core, la cual puede ser global o local.</span><span class="sxs-lookup"><span data-stu-id="0af8d-104">You might come across issues when trying to install or run a .NET Core tool, which can be a global tool or a local tool.</span></span> <span data-ttu-id="0af8d-105">En este artículo se describen las causas principales más comunes y algunas posibles soluciones.</span><span class="sxs-lookup"><span data-stu-id="0af8d-105">This article describes the common root causes and some possible solutions.</span></span>

## <a name="installed-net-core-tool-fails-to-run"></a><span data-ttu-id="0af8d-106">No se puede ejecutar la herramienta de .NET Core instalada</span><span class="sxs-lookup"><span data-stu-id="0af8d-106">Installed .NET Core tool fails to run</span></span>

<span data-ttu-id="0af8d-107">Cuando una herramienta de .NET Core no se ejecuta, lo más probable es que se haya producido uno de los siguientes problemas:</span><span class="sxs-lookup"><span data-stu-id="0af8d-107">When a .NET Core tool fails to run, most likely you ran into one of the following issues:</span></span>

* <span data-ttu-id="0af8d-108">No se encontró el archivo ejecutable de la herramienta.</span><span class="sxs-lookup"><span data-stu-id="0af8d-108">The executable file for the tool wasn't found.</span></span>
* <span data-ttu-id="0af8d-109">No se encontró la versión correcta del runtime de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0af8d-109">The correct version of the .NET Core runtime wasn't found.</span></span> 

### <a name="executable-file-not-found"></a><span data-ttu-id="0af8d-110">No se encontró el archivo ejecutable</span><span class="sxs-lookup"><span data-stu-id="0af8d-110">Executable file not found</span></span>

<span data-ttu-id="0af8d-111">Si no se encuentra el archivo ejecutable, verá un mensaje similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="0af8d-111">If the executable file isn't found, you'll see a message similar to the following:</span></span>

```
Could not execute because the specified command or file was not found.
Possible reasons for this include:
  * You misspelled a built-in dotnet command.
  * You intended to execute a .NET Core program, but dotnet-xyz does not exist.
  * You intended to run a global tool, but a dotnet-prefixed executable with this name could not be found on the PATH.
```

<span data-ttu-id="0af8d-112">El nombre del archivo ejecutable determina cómo se invoca la herramienta.</span><span class="sxs-lookup"><span data-stu-id="0af8d-112">The name of the executable determines how you invoke the tool.</span></span> <span data-ttu-id="0af8d-113">En la siguiente tabla se describe el formato:</span><span class="sxs-lookup"><span data-stu-id="0af8d-113">The following table describes the format:</span></span>

| <span data-ttu-id="0af8d-114">Formato del nombre del archivo ejecutable</span><span class="sxs-lookup"><span data-stu-id="0af8d-114">Executable name format</span></span>  | <span data-ttu-id="0af8d-115">Formato de invocación</span><span class="sxs-lookup"><span data-stu-id="0af8d-115">Invocation format</span></span>   |
|-------------------------|---------------------|
| `dotnet-<toolName>.exe` | `dotnet <toolName>` |
| `<toolName>.exe`        | `<toolName>`        |

* <span data-ttu-id="0af8d-116">Herramientas globales</span><span class="sxs-lookup"><span data-stu-id="0af8d-116">Global tools</span></span>

    <span data-ttu-id="0af8d-117">Las herramientas globales pueden instalarse en el directorio predeterminado o en una ubicación específica.</span><span class="sxs-lookup"><span data-stu-id="0af8d-117">Global tools can be installed in the default directory or in a specific location.</span></span> <span data-ttu-id="0af8d-118">Los directorios predeterminados son:</span><span class="sxs-lookup"><span data-stu-id="0af8d-118">The default directories are:</span></span>

    | <span data-ttu-id="0af8d-119">SO</span><span class="sxs-lookup"><span data-stu-id="0af8d-119">OS</span></span>          | <span data-ttu-id="0af8d-120">Ruta de acceso</span><span class="sxs-lookup"><span data-stu-id="0af8d-120">Path</span></span>                          |
    |-------------|-------------------------------|
    | <span data-ttu-id="0af8d-121">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="0af8d-121">Linux/macOS</span></span> | `$HOME/.dotnet/tools`         |
    | <span data-ttu-id="0af8d-122">Windows</span><span class="sxs-lookup"><span data-stu-id="0af8d-122">Windows</span></span>     | `%USERPROFILE%\.dotnet\tools` |

    <span data-ttu-id="0af8d-123">Si intenta ejecutar una herramienta global, compruebe que la variable del entorno `PATH` de su máquina contiene la ruta de acceso donde instaló la herramienta global y que el archivo ejecutable está en esa ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="0af8d-123">If you're trying to run a global tool, check that the `PATH` environment variable on your machine contains the path where you installed the global tool and that the executable is in that path.</span></span>

    <span data-ttu-id="0af8d-124">La primera vez que se usa, la CLI de .NET Core intenta agregar las ubicaciones predeterminadas a la variable de entorno PATH.</span><span class="sxs-lookup"><span data-stu-id="0af8d-124">The .NET Core CLI tries to add the default locations to the PATH environment variable on its first usage.</span></span> <span data-ttu-id="0af8d-125">Sin embargo, hay un par de escenarios en los que la ubicación podría no agregarse automáticamente a PATH, por lo que deberá editar PATH para configurarlo en los siguientes casos:</span><span class="sxs-lookup"><span data-stu-id="0af8d-125">However, there are a couple of scenarios where the location might not be added to PATH automatically, so you'll have to edit PATH to configure it for the following cases:</span></span>

  * <span data-ttu-id="0af8d-126">Si usa Linux y ha instalado el SDK de .NET Core mediante el uso de archivos *.tar.gz* en lugar de “apt-get” o “rpm”.</span><span class="sxs-lookup"><span data-stu-id="0af8d-126">If you're using Linux and you've installed the .NET Core SDK using *.tar.gz* files and not apt-get or rpm.</span></span>
  * <span data-ttu-id="0af8d-127">Si usa macOS 10.15 “Catalina” o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="0af8d-127">If you're using macOS 10.15 "Catalina" or later versions.</span></span>
  * <span data-ttu-id="0af8d-128">Si usa macOS 10.14 “Mojave” o versiones anteriores y ha instalado el SDK de .NET Core mediante el uso de archivos *.tar.gz* en lugar de *.pkg*.</span><span class="sxs-lookup"><span data-stu-id="0af8d-128">If you're using macOS 10.14 "Mojave" or earlier versions, and you've installed the .NET Core SDK using *.tar.gz* files and not *.pkg*.</span></span>
  * <span data-ttu-id="0af8d-129">Si ha instalado el SDK de .NET Core 3.0 y ha establecido la variable de entorno `DOTNET_ADD_GLOBAL_TOOLS_TO_PATH` en `false`.</span><span class="sxs-lookup"><span data-stu-id="0af8d-129">If you've installed the .NET Core 3.0 SDK and you've set the `DOTNET_ADD_GLOBAL_TOOLS_TO_PATH` environment variable to `false`.</span></span>
  * <span data-ttu-id="0af8d-130">Si ha instalado el SDK de .NET Core 2.2 o versiones anteriores y ha establecido la variable de entorno `DOTNET_SKIP_FIRST_TIME_EXPERIENCE` en `true`.</span><span class="sxs-lookup"><span data-stu-id="0af8d-130">If you've installed .NET Core 2.2 SDK or earlier versions, and you've set the `DOTNET_SKIP_FIRST_TIME_EXPERIENCE` environment variable to `true`.</span></span>
  
  <span data-ttu-id="0af8d-131">Para obtener más información sobre las herramientas globales, vea [Información general sobre las herramientas globales de .NET Core](global-tools.md).</span><span class="sxs-lookup"><span data-stu-id="0af8d-131">For more information about global tools, see [.NET Core Global Tools overview](global-tools.md).</span></span>

* <span data-ttu-id="0af8d-132">Herramientas locales</span><span class="sxs-lookup"><span data-stu-id="0af8d-132">Local tools</span></span>

  <span data-ttu-id="0af8d-133">Si intenta ejecutar una herramienta local, compruebe que hay un archivo de manifiesto denominado *dotnet-tools.json* en el directorio actual o en cualquiera de sus directorios principales.</span><span class="sxs-lookup"><span data-stu-id="0af8d-133">If you're trying to run a local tool, verify that there's a manifest file called *dotnet-tools.json* in the current directory or any of its parent directories.</span></span> <span data-ttu-id="0af8d-134">Este archivo también puede encontrarse en una carpeta denominada *.config* en cualquier lugar de la jerarquía de carpetas del proyecto, en lugar de en la carpeta raíz.</span><span class="sxs-lookup"><span data-stu-id="0af8d-134">This file can also live under a folder named *.config* anywhere in the project folder hierarchy, instead of the root folder.</span></span> <span data-ttu-id="0af8d-135">Si *dotnet-tools.json* existe, ábralo y busque la herramienta que intenta ejecutar.</span><span class="sxs-lookup"><span data-stu-id="0af8d-135">If *dotnet-tools.json* exists, open it and check for the tool you're trying to run.</span></span> <span data-ttu-id="0af8d-136">Si el archivo no contiene una entrada para `"isRoot": true`, busque más arriba en la jerarquía de archivos otros archivos de manifiesto de la herramienta.</span><span class="sxs-lookup"><span data-stu-id="0af8d-136">If the file doesn't contain an entry for `"isRoot": true`, then also check further up the file hierarchy for additional tool manifest files.</span></span>

    <span data-ttu-id="0af8d-137">Si intenta ejecutar una herramienta de .NET Core que se instaló con una ruta de acceso especificada, debe incluir dicha ruta de acceso al usar la herramienta.</span><span class="sxs-lookup"><span data-stu-id="0af8d-137">If you're trying to run a .NET Core tool that was installed with a specified path, you need to include that path when using the tool.</span></span> <span data-ttu-id="0af8d-138">Un ejemplo de uso de una herramienta instalada en una ruta de acceso de herramientas es:</span><span class="sxs-lookup"><span data-stu-id="0af8d-138">An example of using a tool-path installed tool is:</span></span>

   ```console
   ..\<toolDirectory>\dotnet-<toolName>
    ```

### <a name="runtime-not-found"></a><span data-ttu-id="0af8d-139">No se encontró el runtime</span><span class="sxs-lookup"><span data-stu-id="0af8d-139">Runtime not found</span></span>

<span data-ttu-id="0af8d-140">Las herramientas de .NET Core son [aplicaciones dependientes del marco](../deploying/index.md#framework-dependent-deployments-fdd), lo que significa que se basan en un runtime de .NET Core instalado en el equipo.</span><span class="sxs-lookup"><span data-stu-id="0af8d-140">.NET Core tools are [framework-dependent applications](../deploying/index.md#framework-dependent-deployments-fdd), which means they rely on a .NET Core runtime installed on your machine.</span></span> <span data-ttu-id="0af8d-141">Si no se encuentra el runtime esperado, se siguen las reglas normales de puesta al día de runtime de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="0af8d-141">If the expected runtime isn't found, they follow normal .NET Core runtime roll-forward rules such as:</span></span>

* <span data-ttu-id="0af8d-142">Una aplicación avanza a la versión de revisión más alta de las versiones principal y secundaria especificadas.</span><span class="sxs-lookup"><span data-stu-id="0af8d-142">An application rolls forward to the highest patch release of the specified major and minor version.</span></span>
* <span data-ttu-id="0af8d-143">Si no hay ningún runtime que coincida con un número de versión principal y secundaria, se usa la siguiente versión secundaria más alta.</span><span class="sxs-lookup"><span data-stu-id="0af8d-143">If there's no matching runtime with a matching major and minor version number, the next higher minor version is used.</span></span>
* <span data-ttu-id="0af8d-144">Esta puesta al día no se produce entre versiones preliminares del runtime ni entre versiones preliminares y versiones de lanzamiento.</span><span class="sxs-lookup"><span data-stu-id="0af8d-144">Roll forward doesn't occur between preview versions of the runtime or between preview versions and release versions.</span></span> <span data-ttu-id="0af8d-145">Por lo tanto, las herramientas de .NET Core creadas con versiones preliminares deben ser recompiladas, publicadas nuevamente y reinstaladas por el autor.</span><span class="sxs-lookup"><span data-stu-id="0af8d-145">So, .NET Core tools created using preview versions must be rebuilt and republished by the author and reinstalled.</span></span>

<span data-ttu-id="0af8d-146">La puesta al día no se producirá de forma predeterminada en dos escenarios comunes:</span><span class="sxs-lookup"><span data-stu-id="0af8d-146">Roll-forward won't occur by default in two common scenarios:</span></span>

* <span data-ttu-id="0af8d-147">Solo están disponibles las versiones anteriores del runtime.</span><span class="sxs-lookup"><span data-stu-id="0af8d-147">Only lower versions of the runtime are available.</span></span> <span data-ttu-id="0af8d-148">La puesta al día solo selecciona versiones posteriores del runtime.</span><span class="sxs-lookup"><span data-stu-id="0af8d-148">Roll-forward only selects later versions of the runtime.</span></span>
* <span data-ttu-id="0af8d-149">Solo están disponibles las versiones posteriores principales del runtime.</span><span class="sxs-lookup"><span data-stu-id="0af8d-149">Only higher major versions of the runtime are available.</span></span> <span data-ttu-id="0af8d-150">La puesta al día no traspasa los límites de la versión principal.</span><span class="sxs-lookup"><span data-stu-id="0af8d-150">Roll-forward doesn't cross major version boundaries.</span></span>

<span data-ttu-id="0af8d-151">Si una aplicación no encuentra el runtime apropiado, no se puede ejecutar y notifica un error.</span><span class="sxs-lookup"><span data-stu-id="0af8d-151">If an application can't find an appropriate runtime, it fails to run and reports an error.</span></span>

<span data-ttu-id="0af8d-152">Puede averiguar qué runtimes de .NET Core están instalados en su máquina con uno de los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="0af8d-152">You can find out which .NET Core runtimes are installed on your machine using one of the following commands:</span></span>

```dotnetcli
dotnet --list-runtimes
dotnet --info
```

<span data-ttu-id="0af8d-153">Si cree que la herramienta debería ser compatible con la versión de runtime que tiene instalada actualmente, puede ponerse en contacto con el autor de la herramienta para ver si puede actualizar el número de versión o la compatibilidad con varios destinos.</span><span class="sxs-lookup"><span data-stu-id="0af8d-153">If you think the tool should support the runtime version you currently have installed, you can contact the tool author and see if they can update the version number or multi-target.</span></span> <span data-ttu-id="0af8d-154">Una vez que se vuelva a compilar y a publicar el paquete de herramientas en NuGet con un número de versión actualizado, podrá actualizar su copia.</span><span class="sxs-lookup"><span data-stu-id="0af8d-154">Once they've recompiled and republished their tool package to NuGet with an updated version number, you can update your copy.</span></span> <span data-ttu-id="0af8d-155">Mientras tanto, la solución más rápida es instalar una versión del runtime que funcione con la herramienta que intenta ejecutar.</span><span class="sxs-lookup"><span data-stu-id="0af8d-155">While that doesn't happen, the quickest solution for you is to install a version of the runtime that would work with the tool you're trying to run.</span></span> <span data-ttu-id="0af8d-156">Para descargar una versión específica del runtime de .NET Core, visite la [página de descarga de .NET Core](https://dotnet.microsoft.com/download/dotnet-core).</span><span class="sxs-lookup"><span data-stu-id="0af8d-156">To download a specific .NET Core runtime version, visit the [.NET Core download page](https://dotnet.microsoft.com/download/dotnet-core).</span></span>

<span data-ttu-id="0af8d-157">Si instala el SDK de .NET Core en una ubicación que no es la predeterminada, debe establecer la variable de entorno `DOTNET_ROOT` en el directorio que contiene el archivo ejecutable `dotnet`.</span><span class="sxs-lookup"><span data-stu-id="0af8d-157">If you install the .NET Core SDK to a non-default location, you need to set the environment variable `DOTNET_ROOT` to the directory that contains the `dotnet` executable.</span></span>

## <a name="net-core-tool-installation-fails"></a><span data-ttu-id="0af8d-158">Error en la instalación de una herramienta de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0af8d-158">.NET Core tool installation fails</span></span>

<span data-ttu-id="0af8d-159">Hay varias razones por las que se puede producir un error en la instalación de una herramienta local o global de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0af8d-159">There are a number of reasons the installation of a .NET Core global or local tool may fail.</span></span> <span data-ttu-id="0af8d-160">Cuando se produzca un error en la instalación de la herramienta, verá un mensaje similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="0af8d-160">When the tool installation fails, you'll see a message similar to following one:</span></span>

```
Tool '{0}' failed to install. This failure may have been caused by:

* You are attempting to install a preview release and did not use the --version option to specify the version.
* A package by this name was found, but it was not a .NET Core tool.
* The required NuGet feed cannot be accessed, perhaps because of an Internet connection problem.
* You mistyped the name of the tool.

For more reasons, including package naming enforcement, visit https://aka.ms/failure-installing-tool
```

<span data-ttu-id="0af8d-161">Para ayudar a diagnosticar estos errores, los mensajes de NuGet se muestran directamente al usuario, junto con el mensaje anterior.</span><span class="sxs-lookup"><span data-stu-id="0af8d-161">To help diagnose these failures, NuGet messages are shown directly to the user, along with the previous message.</span></span> <span data-ttu-id="0af8d-162">El mensaje de NuGet puede ayudarle a identificar el problema.</span><span class="sxs-lookup"><span data-stu-id="0af8d-162">The NuGet message may help you identify the problem.</span></span>

### <a name="package-naming-enforcement"></a><span data-ttu-id="0af8d-163">Cumplimiento de la nomenclatura de los paquetes</span><span class="sxs-lookup"><span data-stu-id="0af8d-163">Package naming enforcement</span></span>

<span data-ttu-id="0af8d-164">Microsoft ha cambiado sus instrucciones sobre los identificadores de paquetes para las herramientas, lo que ha dado lugar a que no se encuentren diversas herramientas con el nombre previsto.</span><span class="sxs-lookup"><span data-stu-id="0af8d-164">Microsoft has changed its guidance on the Package ID for tools, resulting in a number of tools not being found with the predicted name.</span></span> <span data-ttu-id="0af8d-165">Según las nuevas instrucciones, todas las herramientas de Microsoft deben tener el prefijo “Microsoft”.</span><span class="sxs-lookup"><span data-stu-id="0af8d-165">The new guidance is that all Microsoft tools be prefixed with "Microsoft."</span></span> <span data-ttu-id="0af8d-166">Este prefijo está reservado y solo se puede usar para los paquetes firmados con un certificado autorizado de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0af8d-166">This prefix is reserved and can only be used for packages signed with a Microsoft authorized certificate.</span></span>

<span data-ttu-id="0af8d-167">Durante la transición, algunas herramientas de Microsoft tendrán el formato anterior del identificador de paquete, mientras que otras tendrán el nuevo formato:</span><span class="sxs-lookup"><span data-stu-id="0af8d-167">During the transition, some Microsoft tools will have the old form of the package ID, while others will have the new form:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.<toolName>
dotnet tool install -g <toolName>
```

<span data-ttu-id="0af8d-168">A medida que se actualicen los identificadores de paquetes, deberá usar el nuevo identificador de paquete para obtener las actualizaciones más recientes.</span><span class="sxs-lookup"><span data-stu-id="0af8d-168">As package IDs are updated, you'll need to change to the new package ID to get the latest updates.</span></span> <span data-ttu-id="0af8d-169">Los paquetes con el nombre de herramienta simplificado estarán en desuso.</span><span class="sxs-lookup"><span data-stu-id="0af8d-169">Packages with the simplified tool name will be deprecated.</span></span>

### <a name="preview-releases"></a><span data-ttu-id="0af8d-170">Versiones preliminares</span><span class="sxs-lookup"><span data-stu-id="0af8d-170">Preview releases</span></span>

* <span data-ttu-id="0af8d-171">Intenta instalar una versión preliminar y no ha usado la opción `--version` para especificar la versión.</span><span class="sxs-lookup"><span data-stu-id="0af8d-171">You're attempting to install a preview release and didn't use the `--version` option to specify the version.</span></span>

<span data-ttu-id="0af8d-172">Las herramientas de .NET Core que se encuentran en versión preliminar deben especificarse con una parte del nombre para indicar que están en versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="0af8d-172">.NET Core tools that are in preview must be specified with a portion of the name to indicate that they are in preview.</span></span> <span data-ttu-id="0af8d-173">No es necesario incluir todo el nombre de la versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="0af8d-173">You don't need to include the entire preview.</span></span> <span data-ttu-id="0af8d-174">Si los números de versión tienen el formato esperado, puede usar algo parecido al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0af8d-174">Assuming the version numbers are in the expected format, you can use something like the following example:</span></span>

```dotnetcli
dotnet tool install -g --version 1.1.0-pre <toolName>
```

> [!NOTE]
> <span data-ttu-id="0af8d-175">El equipo de la CLI de .NET Core está planeando agregar un modificador `--preview` en una versión futura para facilitar esta tarea.</span><span class="sxs-lookup"><span data-stu-id="0af8d-175">The .NET Core CLI team is planning to add a `--preview` switch in a future release to make this easier.</span></span>

### <a name="package-isnt-a-net-core-tool"></a><span data-ttu-id="0af8d-176">El paquete no es una herramienta de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0af8d-176">Package isn't a .NET Core tool</span></span>

* <span data-ttu-id="0af8d-177">Se encontró un paquete NuGet con este nombre, pero no era una herramienta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0af8d-177">A NuGet package by this name was found, but it wasn't a .NET Core tool.</span></span>

<span data-ttu-id="0af8d-178">Si intenta instalar un paquete normal de NuGet que no es una herramienta de .NET Core, verá un error similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="0af8d-178">If you try to install a NuGet package that is a regular NuGet package and not a .NET Core tool, you'll see an error similar to the following:</span></span>

`NU1212: Invalid project-package combination for `<ToolName>`. DotnetToolReference project style can only contain references of the DotnetTool type.`

### <a name="nuget-feed-cant-be-accessed"></a><span data-ttu-id="0af8d-179">No se puede acceder a la fuente NuGet</span><span class="sxs-lookup"><span data-stu-id="0af8d-179">NuGet feed can't be accessed</span></span>

* <span data-ttu-id="0af8d-180">No se puede acceder a la fuente NuGet necesaria, quizás debido a un problema con la conexión a Internet.</span><span class="sxs-lookup"><span data-stu-id="0af8d-180">The required NuGet feed can't be accessed, perhaps because of an Internet connection problem.</span></span>

<span data-ttu-id="0af8d-181">La instalación de la herramienta requiere acceso a la fuente NuGet que contiene el paquete de la herramienta.</span><span class="sxs-lookup"><span data-stu-id="0af8d-181">Tool installation requires access to the NuGet feed that contains the tool package.</span></span> <span data-ttu-id="0af8d-182">Se produce un error si la fuente no está disponible.</span><span class="sxs-lookup"><span data-stu-id="0af8d-182">It fails if the feed isn't available.</span></span> <span data-ttu-id="0af8d-183">Puede modificar las fuentes con `nuget.config`, solicitar un archivo `nuget.config` determinado o especificar fuentes adicionales con el modificador `--add-source`.</span><span class="sxs-lookup"><span data-stu-id="0af8d-183">You can alter feeds with `nuget.config`, request a specific `nuget.config` file, or specify additional feeds with the `--add-source` switch.</span></span> <span data-ttu-id="0af8d-184">De forma predeterminada, NuGet genera un error para cualquier fuente con la que no pueda conectar.</span><span class="sxs-lookup"><span data-stu-id="0af8d-184">By default, NuGet throws an error for any feed that can't connect.</span></span> <span data-ttu-id="0af8d-185">La marca `--ignore-failed-sources` puede omitir estos orígenes no accesibles.</span><span class="sxs-lookup"><span data-stu-id="0af8d-185">The flag `--ignore-failed-sources` can skip these non-reachable sources.</span></span>

### <a name="package-id-incorrect"></a><span data-ttu-id="0af8d-186">Identificador de paquete incorrecto</span><span class="sxs-lookup"><span data-stu-id="0af8d-186">Package ID incorrect</span></span>

* <span data-ttu-id="0af8d-187">No ha escrito correctamente el nombre de la herramienta.</span><span class="sxs-lookup"><span data-stu-id="0af8d-187">You mistyped the name of the tool.</span></span>

<span data-ttu-id="0af8d-188">Un motivo habitual de error es que el nombre de la herramienta no es correcto.</span><span class="sxs-lookup"><span data-stu-id="0af8d-188">A common reason for failure is that the tool name isn't correct.</span></span> <span data-ttu-id="0af8d-189">Esto puede ocurrir cuando se produce un error de escritura, o porque la herramienta se ha movido o está en desuso.</span><span class="sxs-lookup"><span data-stu-id="0af8d-189">This can happen because of mistyping, or because the tool has moved or been deprecated.</span></span> <span data-ttu-id="0af8d-190">En el caso de las herramientas de NuGet.org, una manera de asegurarse de que tiene el nombre correcto es buscar la herramienta en NuGet.org y copiar el comando de instalación.</span><span class="sxs-lookup"><span data-stu-id="0af8d-190">For tools on NuGet.org, one way to ensure you have the name correct is to search for the tool at NuGet.org and copy the installation command.</span></span>

## <a name="see-also"></a><span data-ttu-id="0af8d-191">Vea también</span><span class="sxs-lookup"><span data-stu-id="0af8d-191">See also</span></span>
* [<span data-ttu-id="0af8d-192">Información general sobre las herramientas globales de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0af8d-192">.NET Core Global Tools overview</span></span>](global-tools.md)