---
title: Crear aplicaciones de ASP.NET Core 2.1 implementadas como contenedores de Linux en clústeres de AKS y Kubernetes
description: Ciclo de vida de aplicaciones de Docker en contenedor con la plataforma y las herramientas de Microsoft
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 02/25/2019
ms.openlocfilehash: a00a5c42facb105a23cd85fce79f9fd16a96ccfa
ms.sourcegitcommit: bd28ff1e312eaba9718c4f7ea272c2d4781a7cac
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56835517"
---
# <a name="build-aspnet-core-21-applications-deployed-as-linux-containers-into-an-akskubernetes-orchestrator"></a><span data-ttu-id="26b3c-103">Crear aplicaciones de ASP.NET Core 2.1 implementadas como contenedores de Linux en un orquestador de Kubernetes/AKS</span><span class="sxs-lookup"><span data-stu-id="26b3c-103">Build ASP.NET Core 2.1 applications deployed as Linux containers into an AKS/Kubernetes orchestrator</span></span>

<span data-ttu-id="26b3c-104">Azure Kubernetes Service (AKS) es Kubernetes orquestaciones servicios administrados de Azure que simplifican la administración e implementación de contenedor.</span><span class="sxs-lookup"><span data-stu-id="26b3c-104">Azure Kubernetes Services (AKS) is Azure's managed Kubernetes orchestrations services that simplify container deployment and management.</span></span>

<span data-ttu-id="26b3c-105">Características principales de AKS son:</span><span class="sxs-lookup"><span data-stu-id="26b3c-105">AKS main features are:</span></span>

- <span data-ttu-id="26b3c-106">Un plano de control hospedado de Azure</span><span class="sxs-lookup"><span data-stu-id="26b3c-106">An Azure-hosted control plane</span></span>
- <span data-ttu-id="26b3c-107">Actualizaciones automatizadas</span><span class="sxs-lookup"><span data-stu-id="26b3c-107">Automated upgrades</span></span>
- <span data-ttu-id="26b3c-108">Autorrecuperación</span><span class="sxs-lookup"><span data-stu-id="26b3c-108">Self-healing</span></span>
- <span data-ttu-id="26b3c-109">Escalado configurables de usuario</span><span class="sxs-lookup"><span data-stu-id="26b3c-109">User configurable scaling</span></span>
- <span data-ttu-id="26b3c-110">Una experiencia de usuario más sencilla para desarrolladores y operadores del clúster.</span><span class="sxs-lookup"><span data-stu-id="26b3c-110">A simpler user experience for both developers and cluster operators.</span></span>

<span data-ttu-id="26b3c-111">Los ejemplos siguientes exploración la creación de una aplicación de ASP.NET Core 2.1 se ejecuta en Linux y se implementa en un clúster de AKS en Azure, mientras se realiza el desarrollo con Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="26b3c-111">The following examples explore the creation of an ASP.NET Core 2.1 application that runs on Linux and deploys to an AKS Cluster in Azure, while development is done using Visual Studio 2017.</span></span>

## <a name="creating-the-aspnet-core-21-project-using-visual-studio-2017"></a><span data-ttu-id="26b3c-112">Crear el proyecto de ASP.NET Core 2.1 con Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="26b3c-112">Creating the ASP.NET Core 2.1 Project using Visual Studio 2017</span></span>

<span data-ttu-id="26b3c-113">ASP.NET Core es una plataforma de desarrollo de propósito general mantenida por Microsoft y la Comunidad de .NET en GitHub.</span><span class="sxs-lookup"><span data-stu-id="26b3c-113">ASP.NET Core is a general-purpose development platform maintained by Microsoft and the .NET community on GitHub.</span></span> <span data-ttu-id="26b3c-114">Es multiplataforma, admite Windows, macOS y Linux y puede usarse en escenarios de dispositivo, nube, IoT e incrustados.</span><span class="sxs-lookup"><span data-stu-id="26b3c-114">It is cross-platform, supporting Windows, macOS and Linux, and can be used in device, cloud, and embedded/IoT scenarios.</span></span>

<span data-ttu-id="26b3c-115">Este ejemplo usa un proyecto simple que se basa basado en una plantilla de API Web de Visual Studio, por lo que no necesita conocimientos adicionales para crear el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="26b3c-115">This example uses a simple project that's based based on a Visual Studio Web API template, so you don't need any additional knowledge to create the sample.</span></span> <span data-ttu-id="26b3c-116">Solo debe crear el proyecto con una plantilla estándar que incluye todos los elementos para ejecutar un proyecto pequeño con una API de REST, mediante la tecnología de ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="26b3c-116">You only have to create the project using a standard template that includes all the elements to run a small project with a REST API, using ASP.NET Core 2.1 technology.</span></span>

![Agregar ventana nuevo proyecto en Visual Studio, seleccione la aplicación Web ASP.NET Core.](media/create-aspnet-core-application.png)

<span data-ttu-id="26b3c-118">**Figura 4-36**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-118">**Figure 4-36**.</span></span> <span data-ttu-id="26b3c-119">Crear aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26b3c-119">Creating ASP.NET Core Application</span></span>

<span data-ttu-id="26b3c-120">Para crear el proyecto de ejemplo en Visual Studio, seleccione **archivo** > **New** > **proyecto**, seleccione el **Web**tipos en el panel izquierdo, seguido de proyecto **aplicación Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-120">To create the sample project in Visual Studio, select **File** > **New** > **Project**, select the **Web** project types in the left pane, followed by **ASP.NET Core Web Application**.</span></span>

<span data-ttu-id="26b3c-121">Visual Studio le mostrará las plantillas para proyectos web.</span><span class="sxs-lookup"><span data-stu-id="26b3c-121">Visual Studio lists templates for web projects.</span></span> <span data-ttu-id="26b3c-122">En nuestro ejemplo, seleccione **API** para crear una aplicación de API Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="26b3c-122">For our example, select **API** to create an ASP.NET Web API Application.</span></span>

<span data-ttu-id="26b3c-123">Compruebe que ha seleccionado como el marco de ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="26b3c-123">Verify that you have selected ASP.NET Core 2.1 as the framework.</span></span> <span data-ttu-id="26b3c-124">.NET core 2.1 se incluye en la última versión de Visual Studio 2017 y automáticamente se instala y configurará automáticamente al instalar Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="26b3c-124">.NET Core 2.1 is included in the last version of Visual Studio 2017 and is automatically installed and configured for you when you install Visual Studio 2017.</span></span>

![Diálogo Visual Studio para seleccionar el tipo de una aplicación Web de ASP.NET Core con la opción de API seleccionada.](media/create-web-api-application.png)

<span data-ttu-id="26b3c-126">**Figura 4-37**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-126">**Figure 4-37**.</span></span> <span data-ttu-id="26b3c-127">Tipo de proyecto Web API y seleccione ASP.NET CORE 2.1</span><span class="sxs-lookup"><span data-stu-id="26b3c-127">Selecting ASP.NET CORE 2.1 and Web API project type</span></span>

<span data-ttu-id="26b3c-128">Si tiene cualquier versión anterior de .NET Core, puede descargar e instalar la versión 2.1 de <https://www.microsoft.com/net/download/core#/sdk>.</span><span class="sxs-lookup"><span data-stu-id="26b3c-128">If you have any previous version of .NET Core, you can download and install the 2.1 version from <https://www.microsoft.com/net/download/core#/sdk>.</span></span>

<span data-ttu-id="26b3c-129">Puede agregar compatibilidad con Docker al crear el proyecto o posteriormente, por lo que se puede "aplicar docker a" el proyecto en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="26b3c-129">You can add Docker support when creating the project or afterwards, so you can "Dockerize" your project at any time.</span></span> <span data-ttu-id="26b3c-130">Para agregar compatibilidad con Docker después de la creación del proyecto, haga doble clic en el nodo del proyecto en el Explorador de soluciones y seleccione **agregar** > **compatibilidad con Docker** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="26b3c-130">To add Docker support after project creation, right-click on the project node in Solution Explorer and select **Add** > **Docker support** on the context menu.</span></span>

![Opción del menú contextual para agregar compatibilidad con Docker a un proyecto existente: Haga clic con el botón derecho (en el proyecto) > Agregar > compatibilidad con Docker.](media/add-docker-support-to-project.png)

<span data-ttu-id="26b3c-132">**Figura 4-38**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-132">**Figure 4-38**.</span></span> <span data-ttu-id="26b3c-133">Agregar compatibilidad con Docker al proyecto existente</span><span class="sxs-lookup"><span data-stu-id="26b3c-133">Adding Docker support to existing project</span></span>

<span data-ttu-id="26b3c-134">Para completar la adición de compatibilidad con Docker, puede elegir Windows o Linux.</span><span class="sxs-lookup"><span data-stu-id="26b3c-134">To complete adding Docker support, you can choose Windows or Linux.</span></span> <span data-ttu-id="26b3c-135">En este caso, seleccione **Linux**, ya que AKS no es compatible con contenedores de Windows (como los finales de 2018).</span><span class="sxs-lookup"><span data-stu-id="26b3c-135">In this case, select **Linux**, because AKS doesn’t support Windows Containers (as of late 2018).</span></span>

![Cuadro de diálogo para seleccionar el SO de destino para el archivo Dockerfile.](media/select-linux-docker-support.png)

<span data-ttu-id="26b3c-137">**Figura 4-39**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-137">**Figure 4-39**.</span></span> <span data-ttu-id="26b3c-138">Seleccionar contenedores de Linux.</span><span class="sxs-lookup"><span data-stu-id="26b3c-138">Selecting Linux containers.</span></span>

<span data-ttu-id="26b3c-139">Con estos sencillos pasos, tiene la aplicación de ASP.NET Core 2.1, que se ejecuta en un contenedor de Linux.</span><span class="sxs-lookup"><span data-stu-id="26b3c-139">With these simple steps, you have your ASP.NET Core 2.1 application running on a Linux container.</span></span>

<span data-ttu-id="26b3c-140">Como puede ver, la integración entre Visual Studio 2017 y Docker está totalmente orientada a la productividad del desarrollador.</span><span class="sxs-lookup"><span data-stu-id="26b3c-140">As you can see, the integration between Visual Studio 2017 and Docker is totally oriented to the developer’s productivity.</span></span>

<span data-ttu-id="26b3c-141">Ahora puede ejecutar la aplicación con el **F5** clave o mediante el **reproducir** botón.</span><span class="sxs-lookup"><span data-stu-id="26b3c-141">Now you can run your application with the **F5** key or by using the **Play** button.</span></span>

<span data-ttu-id="26b3c-142">Después de ejecutar el proyecto, puede enumerar las imágenes mediante la `docker images` comando.</span><span class="sxs-lookup"><span data-stu-id="26b3c-142">After running the project, you can list the images using the `docker images` command.</span></span> <span data-ttu-id="26b3c-143">Debería ver el `mssampleapplication` imagen creada por la implementación automática de nuestro proyecto de Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="26b3c-143">You should see the `mssampleapplication` image created by the automatic deployment of our project with Visual Studio 2017.</span></span>

```console
docker images
```

![Salida del comando de imágenes de docker, se muestra una lista con la consola: Repositorio, etiqueta, identificador de la imagen, Created (fecha) y tamaño.](media/docker-images-command.png)

<span data-ttu-id="26b3c-145">**Figura 4-40**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-145">**Figure 4-40**.</span></span> <span data-ttu-id="26b3c-146">Vista de imágenes de Docker</span><span class="sxs-lookup"><span data-stu-id="26b3c-146">View of Docker images</span></span>

## <a name="register-the-solution-in-the-azure-container-registry"></a><span data-ttu-id="26b3c-147">Registre la solución en Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="26b3c-147">Register the Solution in the Azure Container Registry</span></span>

<span data-ttu-id="26b3c-148">Cargar la imagen en cualquier registro de Docker, como [Azure Container Registry (ACR)](https://azure.microsoft.com/services/container-registry/) o Docker Hub, por lo que las imágenes pueden implementarse en el clúster de AKS desde ese registro.</span><span class="sxs-lookup"><span data-stu-id="26b3c-148">Upload the image to any Docker registry, like [Azure Container Registry (ACR)](https://azure.microsoft.com/services/container-registry/) or Docker Hub, so the images can be deployed to the AKS cluster from that registry.</span></span> <span data-ttu-id="26b3c-149">En este caso, nos estamos cargando la imagen en Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="26b3c-149">In this case, we’re uploading the image to Azure Container Registry.</span></span>

### <a name="create-the-image-in-release-mode"></a><span data-ttu-id="26b3c-150">Crear la imagen en modo de lanzamiento</span><span class="sxs-lookup"><span data-stu-id="26b3c-150">Create the image in Release mode</span></span>

<span data-ttu-id="26b3c-151">Ahora vamos a crear la imagen en **versión** modo (listo para producción), cambiando a **versión**, como se muestra en la figura 4-41 y ejecutar la aplicación que vimos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="26b3c-151">We'll now create the image in **Release** mode (ready for production) by changing to **Release**, as shown in Figure 4-41, and running the application as we did before.</span></span>

![Opción de barra de herramientas en Visual Studio para compilar en modo de lanzamiento.](media/select-release-mode.png)

<span data-ttu-id="26b3c-153">**Figura 4-41**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-153">**Figure 4-41**.</span></span> <span data-ttu-id="26b3c-154">Seleccionar modo de versión</span><span class="sxs-lookup"><span data-stu-id="26b3c-154">Selecting Release Mode</span></span>

<span data-ttu-id="26b3c-155">Si ejecuta el `docker image` comando, verá ambas imágenes creadas, uno para `debug` y otro para `release` modo.</span><span class="sxs-lookup"><span data-stu-id="26b3c-155">If you execute the `docker image` command, you'll see both images created, one for `debug` and the other for `release` mode.</span></span>

### <a name="create-a-new-tag-for-the-image"></a><span data-ttu-id="26b3c-156">Crear una nueva etiqueta para la imagen</span><span class="sxs-lookup"><span data-stu-id="26b3c-156">Create a new Tag for the Image</span></span>

<span data-ttu-id="26b3c-157">Cada imagen de contenedor debe estar etiquetada con el `loginServer` nombre del registro.</span><span class="sxs-lookup"><span data-stu-id="26b3c-157">Each container image needs to be tagged with the `loginServer` name of the registry.</span></span> <span data-ttu-id="26b3c-158">Esta etiqueta se utiliza para el enrutamiento al insertar imágenes de contenedor en un registro de imágenes.</span><span class="sxs-lookup"><span data-stu-id="26b3c-158">This tag is used for routing when pushing container images to an image registry.</span></span>

<span data-ttu-id="26b3c-159">Puede ver el `loginServer` nombre desde el portal de Azure, teniendo la información de Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="26b3c-159">You can view the `loginServer` name from the Azure portal, taking the information from the Azure Container Registry</span></span>

![Vista de explorador del nombre del registro de contenedor de Azure, en la esquina superior derecha.](media/loginServer-name.png)

<span data-ttu-id="26b3c-161">**Figura 4-42**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-161">**Figure 4-42**.</span></span> <span data-ttu-id="26b3c-162">Vista del nombre del registro</span><span class="sxs-lookup"><span data-stu-id="26b3c-162">View of the name of the Registry</span></span>

<span data-ttu-id="26b3c-163">O bien ejecutando el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="26b3c-163">Or by running the following command:</span></span>

```console
az acr list --resource-group MSSampleResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

![Salida del comando anterior en la consola.](media/az-cli-loginServer-name.png)

<span data-ttu-id="26b3c-165">**Figura 4-43**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-165">**Figure 4-43**.</span></span> <span data-ttu-id="26b3c-166">Obtener el nombre del registro con PowerShell</span><span class="sxs-lookup"><span data-stu-id="26b3c-166">Get the name of the registry using PowerShell</span></span>

<span data-ttu-id="26b3c-167">En ambos casos, deberá obtener el nombre.</span><span class="sxs-lookup"><span data-stu-id="26b3c-167">In both cases, you'll obtain the name.</span></span> <span data-ttu-id="26b3c-168">En nuestro ejemplo, `mssampleacr.azurecr.io`.</span><span class="sxs-lookup"><span data-stu-id="26b3c-168">In our example, `mssampleacr.azurecr.io`.</span></span>

<span data-ttu-id="26b3c-169">Ahora puede etiquetar la imagen, teniendo la imagen más reciente (la imagen de lanzamiento), con el comando:</span><span class="sxs-lookup"><span data-stu-id="26b3c-169">Now you can tag the image, taking the latest image (the Release image), with the command:</span></span>

```console
docker tag mssampleaksapplication:latest mssampleacr.azurecr.io/mssampleaksapplication:v1
```

<span data-ttu-id="26b3c-170">Después de ejecutar el `docker tag` de comandos, enumerar las imágenes con el `docker images` comando, donde debería ver la imagen con la nueva etiqueta.</span><span class="sxs-lookup"><span data-stu-id="26b3c-170">After running the `docker tag` command, list the images with the `docker images` command, and you should see the image with the new tag.</span></span>

![Salida del comando de imágenes de docker en la consola.](media/tagged-docker-images-list.png)

<span data-ttu-id="26b3c-172">**Figura 4-44**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-172">**Figure 4-44**.</span></span> <span data-ttu-id="26b3c-173">Vista de las imágenes etiquetadas</span><span class="sxs-lookup"><span data-stu-id="26b3c-173">View of tagged images</span></span>

### <a name="push-the-image-into-the-azure-acr"></a><span data-ttu-id="26b3c-174">Insertar la imagen en el ACR de Azure</span><span class="sxs-lookup"><span data-stu-id="26b3c-174">Push the image into the Azure ACR</span></span>

<span data-ttu-id="26b3c-175">Inserte la imagen en el ACR de Azure, mediante el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="26b3c-175">Push the image into the Azure ACR, using the following command:</span></span>

```console
docker push mssampleacr.azurecr.io/mssampleaksapplication:v1
```

<span data-ttu-id="26b3c-176">Este comando tarda unos minutos, pero proporciona comentarios en el proceso de carga de imágenes.</span><span class="sxs-lookup"><span data-stu-id="26b3c-176">This command takes a while uploading the images but gives you feedback in the process.</span></span>

![Salida del comando de inserción de docker de la consola: muestra una barra de progreso basada en caracteres para cada capa.](media/uploading-image-to-acr.png)

<span data-ttu-id="26b3c-178">**Figura 4-45**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-178">**Figure 4-45**.</span></span> <span data-ttu-id="26b3c-179">Cargar la imagen en ACR</span><span class="sxs-lookup"><span data-stu-id="26b3c-179">Uploading the image to the ACR</span></span>

<span data-ttu-id="26b3c-180">Puede ver a continuación el resultado que debería obtener cuando se complete el proceso:</span><span class="sxs-lookup"><span data-stu-id="26b3c-180">You can see below the result you should get when the process completes:</span></span>

![Salida del comando de inserción de docker ha terminado, que muestra todas las capas o nodos en la consola.](media/uploading-docker-images-complete.png)

<span data-ttu-id="26b3c-182">**Figura 4-46**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-182">**Figure 4-46**.</span></span> <span data-ttu-id="26b3c-183">Vista de nodos</span><span class="sxs-lookup"><span data-stu-id="26b3c-183">View of nodes</span></span>

<span data-ttu-id="26b3c-184">El siguiente paso es implementar el contenedor en el clúster de Kubernetes de AKS.</span><span class="sxs-lookup"><span data-stu-id="26b3c-184">The next step is to deploy your container into your AKS Kubernetes cluster.</span></span> <span data-ttu-id="26b3c-185">Para ello, necesita un archivo (**.yml implementar el archivo**) que contenga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="26b3c-185">For that, you need a file (**.yml deploy file**) that contains the following:</span></span>

```yml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: mssamplesbook
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: mssample-kub-app
    spec:
      containers:
        - mane: mssample-services-app
          image: mssampleacr.azurecr.io/mssampleaksapplication:v1
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
    name: mssample-kub-app
spec:
  ports:
    - name: http-port
      port: 80
      targetPort: 80
  selector:
    app: mssample-kub-app
  type: LoadBalancer
```

> [!NOTE]
> <span data-ttu-id="26b3c-186">Para obtener más información sobre la implementación con Kubernetes, consulte: <https://kubernetes.io/docs/reference/kubectl/cheatsheet/></span><span class="sxs-lookup"><span data-stu-id="26b3c-186">For more information on deployment with Kubernetes see: <https://kubernetes.io/docs/reference/kubectl/cheatsheet/></span></span>

<span data-ttu-id="26b3c-187">Ahora ya está casi listo para implementar mediante **Kubectl**, pero primero debe obtener las credenciales para el clúster de AKS con este comando:</span><span class="sxs-lookup"><span data-stu-id="26b3c-187">Now you're almost ready to deploy using **Kubectl**, but first you must get the credentials to the AKS Cluster with this command:</span></span>

```console
az aks get-credentials --resource-group MSSampleResourceGroupAKS --name mssampleclusterk801
```

![Salida del comando anterior en la consola: Combinado MSSampleK8Cluster"como el contexto actual en /root/.kube/config](media/getting-aks-credentials.png)

<span data-ttu-id="26b3c-189">**Figura 4-47**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-189">**Figure 4-47**.</span></span> <span data-ttu-id="26b3c-190">obtención de credenciales</span><span class="sxs-lookup"><span data-stu-id="26b3c-190">getting credentials</span></span>

<span data-ttu-id="26b3c-191">A continuación, utilice el `kubectl create` comando para iniciar la implementación.</span><span class="sxs-lookup"><span data-stu-id="26b3c-191">Then, use the `kubectl create` command to launch the deployment.</span></span>

```console
kubectl create -f mssample-deploy.yml
```

![Salida del comando anterior de la consola: implementación "mssamplesbook" creado.](media/kubectl-create-command.png)

<span data-ttu-id="26b3c-194">**Figura 4-48**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-194">**Figure 4-48**.</span></span> <span data-ttu-id="26b3c-195">Implementar en Kubernetes</span><span class="sxs-lookup"><span data-stu-id="26b3c-195">Deploy to Kubernetes</span></span>

<span data-ttu-id="26b3c-196">Cuando se completa la implementación, puede tener acceso a la consola de Kubernetes con un proxy local que puede tener acceso temporalmente con este comando:</span><span class="sxs-lookup"><span data-stu-id="26b3c-196">When the deployment completes, you can access the Kubernetes console with a local proxy that you can temporally access with this command:</span></span>

```console
az aks browse --resource-group MSSampleResourceGroupAKS --name mssampleclusterk801
```

<span data-ttu-id="26b3c-197">Y el acceso a la dirección url `http://127.0.0.1:8001`.</span><span class="sxs-lookup"><span data-stu-id="26b3c-197">And accessing the url `http://127.0.0.1:8001`.</span></span>

![Vista de explorador del panel de Kubernetes, que muestra las implementaciones, Pods, conjuntos de réplicas y los servicios.](media/kubernetes-cluster-information.png)

<span data-ttu-id="26b3c-199">**Figura 4-49**.</span><span class="sxs-lookup"><span data-stu-id="26b3c-199">**Figure 4-49**.</span></span> <span data-ttu-id="26b3c-200">Ver información de clúster de Kubernetes</span><span class="sxs-lookup"><span data-stu-id="26b3c-200">View Kubernetes cluster information</span></span>

<span data-ttu-id="26b3c-201">Ahora tiene la aplicación implementada en Azure mediante un contenedor de Linux y un clúster de AKS de Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="26b3c-201">Now you have your application deployed on Azure, using a Linux Container, and an AKS Kubernetes Cluster.</span></span> <span data-ttu-id="26b3c-202">Puede tener acceso a la aplicación que vaya a la dirección IP pública del servicio, que se puede obtener desde el portal de Azure.</span><span class="sxs-lookup"><span data-stu-id="26b3c-202">You can access your app browsing to the public IP of your service, which you can get from the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="26b3c-203">Puede ver cómo crear el clúster de AKS para este ejemplo en la sección [ **implementar a Azure Kubernetes Service (AKS)** ](deploy-azure-kubernetes-service.md) en esta guía.</span><span class="sxs-lookup"><span data-stu-id="26b3c-203">You can see how to create the AKS Cluster for this sample in section [**Deploy to Azure Kubernetes Service (AKS)**](deploy-azure-kubernetes-service.md) on this guide.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="26b3c-204">[Anterior](set-up-windows-containers-with-powershell.md)
>[Siguiente](../docker-devops-workflow/index.md)</span><span class="sxs-lookup"><span data-stu-id="26b3c-204">[Previous](set-up-windows-containers-with-powershell.md)
[Next](../docker-devops-workflow/index.md)</span></span>