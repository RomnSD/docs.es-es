---
title: Orquestación de microservicios y aplicaciones de varios contenedores para una alta escalabilidad y disponibilidad
description: Obtenga información sobre cómo implementar una aplicación con Azure Kubernetes Service.
ms.date: 02/15/2019
ms.openlocfilehash: 88e76b4b0a3686f4227a6aee1b7fbd2bfe55fdcc
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68672632"
---
# <a name="deploy-to-azure-kubernetes-service-aks"></a><span data-ttu-id="973ee-103">Implementación en Azure Kubernetes Service (AKS)</span><span class="sxs-lookup"><span data-stu-id="973ee-103">Deploy to Azure Kubernetes Service (AKS)</span></span>

<span data-ttu-id="973ee-104">Puede interactuar con AKS con su sistema operativo cliente preferido. Aquí le mostraremos cómo hacerlo con Microsoft Windows y una versión insertada de Ubuntu Linux en Windows, mediante el uso de comandos de Bash.</span><span class="sxs-lookup"><span data-stu-id="973ee-104">You can interact with AKS using your preferred client operating system, here we show how to do it with Microsoft Windows and an embedded version of Ubuntu Linux in Windows, using Bash commands.</span></span>

<span data-ttu-id="973ee-105">Estos son los requisitos previos para el uso de AKS:</span><span class="sxs-lookup"><span data-stu-id="973ee-105">Prerequisites to have before using AKS are:</span></span>

- <span data-ttu-id="973ee-106">Máquina de desarrollo Linux o Mac</span><span class="sxs-lookup"><span data-stu-id="973ee-106">Linux or Mac development machine</span></span>
- <span data-ttu-id="973ee-107">Máquina de desarrollo Windows</span><span class="sxs-lookup"><span data-stu-id="973ee-107">Windows development machine</span></span>
  - <span data-ttu-id="973ee-108">Modo de desarrollador habilitado en Windows</span><span class="sxs-lookup"><span data-stu-id="973ee-108">Developer Mode enabled on Windows</span></span>
  - <span data-ttu-id="973ee-109">Subsistema de Windows para Linux</span><span class="sxs-lookup"><span data-stu-id="973ee-109">Windows Subsystem for Linux</span></span>
- <span data-ttu-id="973ee-110">CLI de Azure instalada en [Windows, Mac o Linux](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)</span><span class="sxs-lookup"><span data-stu-id="973ee-110">Azure-CLI installed on [Windows, Mac or Linux](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)</span></span>

> [!NOTE]
> <span data-ttu-id="973ee-111">Aquí encontrará información completa sobre los siguientes temas:</span><span class="sxs-lookup"><span data-stu-id="973ee-111">To find complete information about:</span></span>
>
> <span data-ttu-id="973ee-112">CLI de Azure: <https://docs.microsoft.com/cli/azure/index?view=azure-cli-latest></span><span class="sxs-lookup"><span data-stu-id="973ee-112">Azure-CLI: <https://docs.microsoft.com/cli/azure/index?view=azure-cli-latest></span></span>
>
> <span data-ttu-id="973ee-113">Subsistema Windows para Linux: <https://docs.microsoft.com/windows/wsl/about></span><span class="sxs-lookup"><span data-stu-id="973ee-113">Windows Subsystem for Linux: <https://docs.microsoft.com/windows/wsl/about></span></span>

## <a name="create-the-aks-environment-in-azure"></a><span data-ttu-id="973ee-114">Creación del entorno de AKS en Azure</span><span class="sxs-lookup"><span data-stu-id="973ee-114">Create the AKS environment in Azure</span></span>

<span data-ttu-id="973ee-115">Existen varias formas de crear el entorno de AKS.</span><span class="sxs-lookup"><span data-stu-id="973ee-115">There are several ways to create the AKS Environment.</span></span> <span data-ttu-id="973ee-116">Puede hacerlo mediante comandos de la CLI de Azure o a través de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="973ee-116">It can be done by using Azure-CLI commands or by using the Azure portal.</span></span>

<span data-ttu-id="973ee-117">Aquí puede explorar algunos ejemplos sobre el uso de la CLI de Azure para crear el clúster y de Azure Portal para revisar los resultados.</span><span class="sxs-lookup"><span data-stu-id="973ee-117">Here you can explore some examples using the Azure-CLI to create the cluster and the Azure portal to review the results.</span></span> <span data-ttu-id="973ee-118">También deberá tener Kubectl y Docker en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="973ee-118">You also need to have Kubectl and Docker in your development machine.</span></span>  

## <a name="create-the-aks-cluster"></a><span data-ttu-id="973ee-119">Creación del clúster de AKS</span><span class="sxs-lookup"><span data-stu-id="973ee-119">Create the AKS cluster</span></span>

<span data-ttu-id="973ee-120">Cree el clúster de AKS con este comando:</span><span class="sxs-lookup"><span data-stu-id="973ee-120">Create the AKS cluster using this command:</span></span>

```console
az aks create --resource-group MSSampleResourceGroup --name MSSampleClusterK801 --agent-count 1 --generate-ssh-keys --location westus2
```

<span data-ttu-id="973ee-121">Cuando finalice el trabajo de creación, puede verlo creado en Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="973ee-121">After the creation job finishes, you can see the AKS created in the Azure portal:</span></span>

<span data-ttu-id="973ee-122">Grupo de recursos:</span><span class="sxs-lookup"><span data-stu-id="973ee-122">The resource group:</span></span>

![Vista del explorador del grupo de recursos de Azure AKS.](media/aks-resource-group-view.png)

<span data-ttu-id="973ee-124">**Figura 4-17**.</span><span class="sxs-lookup"><span data-stu-id="973ee-124">**Figure 4-17**.</span></span> <span data-ttu-id="973ee-125">Vista de grupo de recursos de AKS en Azure.</span><span class="sxs-lookup"><span data-stu-id="973ee-125">AKS Resource Group view from Azure.</span></span>

<span data-ttu-id="973ee-126">Clúster de AKS:</span><span class="sxs-lookup"><span data-stu-id="973ee-126">The AKS cluster:</span></span>

![Vista del explorador de un grupo de recursos de AKS.](media/aks-cluster-view.png)

<span data-ttu-id="973ee-128">**Figura 4-18**.</span><span class="sxs-lookup"><span data-stu-id="973ee-128">**Figure 4-18**.</span></span> <span data-ttu-id="973ee-129">Vista de AKS en Azure.</span><span class="sxs-lookup"><span data-stu-id="973ee-129">AKS view from Azure.</span></span>

<span data-ttu-id="973ee-130">También puede ver el nodo creado mediante `Azure-CLI` y `Kubectl`.</span><span class="sxs-lookup"><span data-stu-id="973ee-130">You can also view the node created using `Azure-CLI` and `Kubectl`.</span></span>

<span data-ttu-id="973ee-131">En primer lugar, obtenga las credenciales:</span><span class="sxs-lookup"><span data-stu-id="973ee-131">First, getting the credentials:</span></span>

```console
az aks get-credentials --resource-group MSSampleK8ClusterRG --name MSSampleK8Cluster
```

![Salida de la consola del comando anterior: Merged "MSSampleK8Cluster" as current context in /root/.kube/config](media/get-credentials-command-result.png)

<span data-ttu-id="973ee-133">**Figura 4-19**.</span><span class="sxs-lookup"><span data-stu-id="973ee-133">**Figure 4-19**.</span></span> <span data-ttu-id="973ee-134">Resultado del comando `aks get-credentials`.</span><span class="sxs-lookup"><span data-stu-id="973ee-134">`aks get-credentials` command result.</span></span>

<span data-ttu-id="973ee-135">Después, obtenga los nodos de Kubectl:</span><span class="sxs-lookup"><span data-stu-id="973ee-135">And then, getting nodes from Kubectl:</span></span>

```console
kubectl get nodes
```

![Salida de la consola del comando anterior: Lista de nodos con estado, antigüedad (tiempo de ejecución) y versión](media/kubectl-get-nodes-command-result.png)

<span data-ttu-id="973ee-137">**Figura 4-20**.</span><span class="sxs-lookup"><span data-stu-id="973ee-137">**Figure 4-20**.</span></span> <span data-ttu-id="973ee-138">Resultado del comando `kubectl get nodes`.</span><span class="sxs-lookup"><span data-stu-id="973ee-138">`kubectl get nodes` command result.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="973ee-139">[Anterior](orchestrate-high-scalability-availability.md)
>[Siguiente](docker-apps-development-environment.md)</span><span class="sxs-lookup"><span data-stu-id="973ee-139">[Previous](orchestrate-high-scalability-availability.md)
[Next](docker-apps-development-environment.md)</span></span>