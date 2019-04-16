---
title: Comando dotnet list package
description: El comando "dotnet list package" ofrece una opción práctica para mostrar las referencias de paquete de un proyecto o una solución.
ms.date: 04/09/2019
ms.openlocfilehash: bc38b94201f85ed4b22e11722ef5cabcb6fbf040
ms.sourcegitcommit: 859b2ba0c74a1a5a4ad0d59a3c3af23450995981
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59481577"
---
# <a name="dotnet-list-package"></a><span data-ttu-id="2b5ef-103">dotnet list package</span><span class="sxs-lookup"><span data-stu-id="2b5ef-103">dotnet list package</span></span>

[!INCLUDE [topic-appliesto-net-core-22plus](../../../includes/topic-appliesto-net-core-22plus.md)]

## <a name="name"></a><span data-ttu-id="2b5ef-104">nombre</span><span class="sxs-lookup"><span data-stu-id="2b5ef-104">Name</span></span>

`dotnet list package` <span data-ttu-id="2b5ef-105">: muestra las referencias de paquete de un proyecto o una solución.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-105">- Lists the package references for a project or solution.</span></span>

## <a name="synopsis"></a><span data-ttu-id="2b5ef-106">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="2b5ef-106">Synopsis</span></span>

```
dotnet list [<PROJECT | SOLUTION>] package [--config] [--framework] [--highest-minor] [--highest-patch] 
   [--include-prerelease] [--include-transitive] [--outdated] [--source]
dotnet list package [-h|--help]
```

## <a name="description"></a><span data-ttu-id="2b5ef-107">Descripción</span><span class="sxs-lookup"><span data-stu-id="2b5ef-107">Description</span></span>

<span data-ttu-id="2b5ef-108">El comando `dotnet list package` ofrece una opción práctica para mostrar todas las referencias de paquete de NuGet de una solución o un proyecto específico.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-108">The `dotnet list package` command provides a convenient option to list all NuGet package references for a specific project or a solution.</span></span> <span data-ttu-id="2b5ef-109">Primero deberá crear el proyecto para tener los recursos necesarios para que este comando se procese.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-109">You first need to build the project in order to have the assets needed for this command to process.</span></span> <span data-ttu-id="2b5ef-110">En el ejemplo siguiente se muestra la salida del comando `dotnet list package` para el proyecto [SentimentAnalysis](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/SentimentAnalysis):</span><span class="sxs-lookup"><span data-stu-id="2b5ef-110">The following example shows the output of the `dotnet list package` command for the [SentimentAnalysis](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/SentimentAnalysis) project:</span></span>

```output
Project 'SentimentAnalysis' has the following package references
   [netcoreapp2.1]:
   Top-level Package               Requested   Resolved
   > Microsoft.ML                  0.11.0      0.11.0
   > Microsoft.NETCore.App   (A)   [2.1.0, )   2.1.0

(A) : Auto-referenced package.
```

<span data-ttu-id="2b5ef-111">La columna **Requested** hace referencia a la versión de paquete especificada en el archivo del proyecto y puede ser un intervalo.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-111">The **Requested** column refers to the package version specified in the project file and can be a range.</span></span> <span data-ttu-id="2b5ef-112">La columna **Resolved** muestra la versión que el proyecto usa actualmente y siempre se trata de un valor único.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-112">The **Resolved** column lists the version that the project is currently using and is always a single value.</span></span> <span data-ttu-id="2b5ef-113">Los paquetes que tienen `(A)` junto al nombre representan [referencias implícitas de paquete](csproj.md#implicit-package-references) que se deducen de la configuración del proyecto (tipo de `Sdk`, propiedad `<TargetFramework>` o `<TargetFrameworks>`, etc.).</span><span class="sxs-lookup"><span data-stu-id="2b5ef-113">The packages displaying an `(A)` right next to their names represent [implicit package references](csproj.md#implicit-package-references) that are inferred from your project settings (`Sdk` type, `<TargetFramework>` or `<TargetFrameworks>` property, etc.)</span></span>

<span data-ttu-id="2b5ef-114">Use la opción `--outdated` para averiguar si hay disponibles más recientes de los paquetes que usa en los proyectos.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-114">Use the `--outdated` option to find out if there are newer versions available of the packages you're using in your projects.</span></span> <span data-ttu-id="2b5ef-115">De manera predeterminada, `--outdated` muestra los paquetes estables más recientes, a menos que la versión resuelta también sea una versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-115">By default, `--outdated` lists the latest stable packages unless the resolved version is also a prerelease version.</span></span> <span data-ttu-id="2b5ef-116">Para incluir versiones preliminares cuando se muestren versiones más recientes, especifique también la opción `--include-prerelease`.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-116">To include prerelease versions when listing newer versions, also specify the `--include-prerelease` option.</span></span> <span data-ttu-id="2b5ef-117">En los ejemplos siguientes se muestra la salida del comando `dotnet list package --outdated --include-prerelease` para el mismo proyecto, tal como en el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="2b5ef-117">The following examples shows the output of the `dotnet list package --outdated --include-prerelease` command for the same project as the previous example:</span></span>

```output
The following sources were used:
   https://api.nuget.org/v3/index.json

Project `SentimentAnalysis` has the following updates to its packages
   [netcoreapp2.1]:
   Top-level Package      Requested   Resolved   Latest
   > Microsoft.ML         0.11.0      0.11.0     1.0.0-preview
```

<span data-ttu-id="2b5ef-118">Si tiene que averiguar si el proyecto tiene dependencias transitivas, use la opción `--include-transitive`.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-118">If you need to find out whether your project has transitive dependencies, use the `--include-transitive` option.</span></span> <span data-ttu-id="2b5ef-119">Las dependencias transitivas se producen cuando se agrega un paquete al proyecto que, a su vez, se basa en otro paquete.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-119">Transitive dependencies occur when you add a package to your project that in turn relies on another package.</span></span> <span data-ttu-id="2b5ef-120">En el ejemplo siguiente se muestra la salida de la ejecución del comando `dotnet list package --include-transitive` en el proyecto [HelloPlugin](https://github.com/dotnet/samples/tree/master/core/extensions/AppWithPlugin/HelloPlugin), que muestra paquetes de nivel superior y los paquetes de los que dependen:</span><span class="sxs-lookup"><span data-stu-id="2b5ef-120">The following example shows the output from running the `dotnet list package --include-transitive` command for the [HelloPlugin](https://github.com/dotnet/samples/tree/master/core/extensions/AppWithPlugin/HelloPlugin) project, which displays top-level packages and the packages they depend on:</span></span>

```output
Project 'HelloPlugin' has the following package references
   [netcoreapp3.0]:
   Top-level Package                      Requested                    Resolved
   > Microsoft.NETCore.Platforms    (A)   [3.0.0-preview3.19128.7, )   3.0.0-preview3.19128.7
   > Microsoft.WindowsDesktop.App   (A)   [3.0.0-preview3-27504-2, )   3.0.0-preview3-27504-2

   Transitive Package               Resolved
   > Microsoft.NETCore.Targets      2.0.0
   > PluginBase                     1.0.0

(A) : Auto-referenced package.
```

## <a name="arguments"></a><span data-ttu-id="2b5ef-121">Argumentos</span><span class="sxs-lookup"><span data-stu-id="2b5ef-121">Arguments</span></span>

`PROJECT | SOLUTION`

<span data-ttu-id="2b5ef-122">El archivo de proyecto o solución donde se operará.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-122">The project or solution file to operate on.</span></span> <span data-ttu-id="2b5ef-123">Si no se especifica, el comando busca uno en el directorio actual.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-123">If not specified, the command searches the current directory for one.</span></span> <span data-ttu-id="2b5ef-124">Si se encuentra más de una solución o proyecto, se genera un error.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-124">If more than one solution or project is found, an error is thrown.</span></span>

## <a name="options"></a><span data-ttu-id="2b5ef-125">Opciones</span><span class="sxs-lookup"><span data-stu-id="2b5ef-125">Options</span></span>

* **`--config <SOURCE>`**

  <span data-ttu-id="2b5ef-126">Los orígenes de NuGet que se usarán al buscar paquetes más recientes.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-126">The NuGet sources to use when searching for newer packages.</span></span> <span data-ttu-id="2b5ef-127">Requiere la opción `--outdated`.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-127">Requires the `--outdated` option.</span></span>

* **`--framework <FRAMEWORK>`**

  <span data-ttu-id="2b5ef-128">Muestra solo los paquetes aplicables al [marco de destino](../../standard/frameworks.md) especificado.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-128">Displays only the packages applicable for the specified [target framework](../../standard/frameworks.md).</span></span> <span data-ttu-id="2b5ef-129">Para especificar varios marcos, repita la opción varias veces.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-129">To specify multiple frameworks, repeat the option multiple times.</span></span> <span data-ttu-id="2b5ef-130">Por ejemplo: `--framework netcoreapp2.2 --framework netstandard2.0`.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-130">For example: `--framework netcoreapp2.2 --framework netstandard2.0`.</span></span>

* **`-h|--help`**

  <span data-ttu-id="2b5ef-131">Imprime una corta ayuda para el comando.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-131">Prints out a short help for the command.</span></span>

* **`--highest-minor`**

  <span data-ttu-id="2b5ef-132">Considere solo los paquetes con un número de versión principal coincidente al buscar paquetes más recientes.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-132">Considers only the packages with a matching major version number when searching for newer packages.</span></span> <span data-ttu-id="2b5ef-133">Requiere la opción `--outdated`.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-133">Requires the `--outdated` option.</span></span>

* **`--highest-patch`**

  <span data-ttu-id="2b5ef-134">Considere solo los paquetes con número de versión principal y secundario coincidente al buscar paquetes más recientes.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-134">Considers only the packages with a matching major and minor version numbers when searching for newer packages.</span></span> <span data-ttu-id="2b5ef-135">Requiere la opción `--outdated`.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-135">Requires the `--outdated` option.</span></span>

* **`--include-prerelease`**

  <span data-ttu-id="2b5ef-136">Considere los paquetes con versiones preliminares al buscar paquetes más recientes.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-136">Considers packages with prerelease versions when searching for newer packages.</span></span> <span data-ttu-id="2b5ef-137">Requiere la opción `--outdated`.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-137">Requires the `--outdated` option.</span></span>

* **`--include-transitive`**

  <span data-ttu-id="2b5ef-138">Enumera los paquetes transitivos, además de los paquetes de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-138">Lists transitive packages, in addition to the top-level packages.</span></span> <span data-ttu-id="2b5ef-139">Al especificar esta opción, recibe una lista de paquetes de los que dependen los paquetes de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-139">When specifying this option, you get a list of packages that the top-level packages depend on.</span></span>

* **`--outdated`**

  <span data-ttu-id="2b5ef-140">Enumera los paquetes con versiones más recientes disponibles.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-140">Lists packages that have newer versions available.</span></span>

* **`-s|--source <SOURCE>`**

  <span data-ttu-id="2b5ef-141">Los orígenes de NuGet que se usarán al buscar paquetes más recientes.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-141">The NuGet sources to use when searching for newer packages.</span></span> <span data-ttu-id="2b5ef-142">Requiere la opción `--outdated`.</span><span class="sxs-lookup"><span data-stu-id="2b5ef-142">Requires the `--outdated` option.</span></span>

## <a name="examples"></a><span data-ttu-id="2b5ef-143">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="2b5ef-143">Examples</span></span>

* <span data-ttu-id="2b5ef-144">Muestre las referencias de paquete de un proyecto específico:</span><span class="sxs-lookup"><span data-stu-id="2b5ef-144">List package references of a specific project:</span></span>

  ```console
  dotnet list SentimentAnalysis.csproj package
  ```

* <span data-ttu-id="2b5ef-145">Muestre las referencias de paquete que tienen versiones más recientes disponibles, incluidas versiones preliminares:</span><span class="sxs-lookup"><span data-stu-id="2b5ef-145">List package references that have newer versions available, including prerelease versions:</span></span>

  ```console
  dotnet list package --outdated --include-prerelease
  ```

* <span data-ttu-id="2b5ef-146">Muestre las referencias de paquete para un marco de destino específico:</span><span class="sxs-lookup"><span data-stu-id="2b5ef-146">List package references for a specific target framework:</span></span>

  ```console
  dotnet list package --framework netcoreapp3.0
  ```