---
title: Pasos del flujo de trabajo de DevOps de bucle externo para una aplicación de Docker
description: Ciclo de vida de aplicaciones de Docker en contenedor con la plataforma y las herramientas de Microsoft
ms.date: 02/15/2019
ms.openlocfilehash: 9fdc5acfd375e4f2266859f061ef1c854286b914
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68673782"
---
# <a name="creating-cicd-pipelines-in-azure-devops-services-for-a-net-core-20-application-on-containers-and-deploying-to-a-kubernetes-cluster"></a><span data-ttu-id="8e6d7-103">Creación de canalizaciones de CI/CD en Azure DevOps Services para una aplicación .NET Core 2.0 en contenedores e implementación en un clúster de Kubernetes</span><span class="sxs-lookup"><span data-stu-id="8e6d7-103">Creating CI/CD pipelines in Azure DevOps Services for a .NET Core 2.0 application on Containers and deploying to a Kubernetes cluster</span></span>

<span data-ttu-id="8e6d7-104">En la figura 5-12 se puede ver el escenario completo de DevOps, que comprende la administración de código, la compilación de código, la creación de imágenes de Docker, la inserción de imágenes de Docker en un registro de Docker y, por último, la implementación en un clúster de Kubernetes en Azure.</span><span class="sxs-lookup"><span data-stu-id="8e6d7-104">In Figure 5-12 you can see the end-to-end DevOps scenario covering the code management, code compilation, Docker images build, Docker images push to a Docker registry and finally the deployment to a Kubernetes cluster in Azure.</span></span>

![Flujo de trabajo: Empieza en la máquina de desarrollo.](media/docker-workflow-ci-cd-aks.png)

<span data-ttu-id="8e6d7-107">**Figura 5-12**.</span><span class="sxs-lookup"><span data-stu-id="8e6d7-107">**Figure 5-12**.</span></span> <span data-ttu-id="8e6d7-108">Escenario CI/CD de creación de imágenes de Docker y su implementación en un clúster de Kubernetes en Azure</span><span class="sxs-lookup"><span data-stu-id="8e6d7-108">CI/CD scenario creating Docker images and deploying to a Kubernetes cluster in Azure</span></span>

<span data-ttu-id="8e6d7-109">Es importante destacar que las dos canalizaciones, la de compilación/CI y la de versión/CD, se conectan a través del registro de Docker (como Docker Hub o Azure Container Registry).</span><span class="sxs-lookup"><span data-stu-id="8e6d7-109">It is important to highlight that the two pipelines, build/CI, and release/CD, are connected through the Docker Registry (such as Docker Hub or Azure Container Registry).</span></span> <span data-ttu-id="8e6d7-110">El registro de Docker es una de las principales diferencias con respecto a un proceso tradicional de CI/CD sin Docker.</span><span class="sxs-lookup"><span data-stu-id="8e6d7-110">The Docker registry is one of the main differences compared to a traditional CI/CD process without Docker.</span></span>

<span data-ttu-id="8e6d7-111">Como se muestra en la figura 5-13, la primera fase es la canalización de compilación/CI.</span><span class="sxs-lookup"><span data-stu-id="8e6d7-111">As shown in Figure 5-13, the first phase is the build/CI pipeline.</span></span> <span data-ttu-id="8e6d7-112">En Azure DevOps Services se pueden crear canalizaciones de compilación/CI que compilen el código, crear las imágenes de Docker e insertarlas en un registro de Docker como Docker Hub o Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="8e6d7-112">In Azure DevOps Services you can create build/CI pipelines that will compile the code, create the Docker images, and push them to a Docker Registry like Docker Hub or Azure Container Registry.</span></span>

![Vista del explorador de Azure DevOps, definición de la tarea de proceso de compilación.](media/build-ci-pipeline-azure-devops-push-to-docker-registry.png)

<span data-ttu-id="8e6d7-114">**Figura 5-13**.</span><span class="sxs-lookup"><span data-stu-id="8e6d7-114">**Figure 5-13**.</span></span> <span data-ttu-id="8e6d7-115">Canalización de compilación/CI de Azure DevOps para crear imágenes de Docker e insertar imágenes en un registro de Docker</span><span class="sxs-lookup"><span data-stu-id="8e6d7-115">Build/CI pipeline in Azure DevOps building Docker images and pushing images to a Docker registry</span></span>

<span data-ttu-id="8e6d7-116">La segunda fase consiste en la creación de una canalización de implementación o publicación.</span><span class="sxs-lookup"><span data-stu-id="8e6d7-116">The second phase is to create a deployment/release pipeline.</span></span> <span data-ttu-id="8e6d7-117">En Azure DevOps Services se puede crear fácilmente una canalización de implementación destinada a un clúster de Kubernetes mediante tareas de Kubernetes para Azure DevOps Services, tal y como se muestra en la figura 5-14.</span><span class="sxs-lookup"><span data-stu-id="8e6d7-117">In Azure DevOps Services, you can easily create a deployment pipeline targeting a Kubernetes cluster by using the Kubernetes tasks for Azure DevOps Services, as shown in Figure 5-14.</span></span>

![Vista del explorador de Azure DevOps, definición de la tarea de implementación en Kubernetes.](media/release-cd-pipeline-azure-devops-deploy-to-kubernetes.png)

<span data-ttu-id="8e6d7-119">**Figura 5-14**.</span><span class="sxs-lookup"><span data-stu-id="8e6d7-119">**Figure 5-14**.</span></span> <span data-ttu-id="8e6d7-120">Canalización de versión/CD de Azure DevOps Services para implementar en un clúster de Kubernetes</span><span class="sxs-lookup"><span data-stu-id="8e6d7-120">Release/CD pipeline in Azure DevOps Services deploying to a Kubernetes cluster</span></span>

> <span data-ttu-id="8e6d7-121">[Tutorial] Implementación de eShopModernized en Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="8e6d7-121">[!Walkthrough] Deploying eShopModernized to Kubernetes:</span></span>
>
> <span data-ttu-id="8e6d7-122">Para obtener un tutorial detallado de la implementación de canalizaciones de CI/CD de Azure DevOps en Kubernetes, vea esta publicación: </span><span class="sxs-lookup"><span data-stu-id="8e6d7-122">For a detailed walkthrough of Azure DevOps CI/CD pipelines deploying to Kubernetes, see this post: </span></span>\
><https://github.com/dotnet-architecture/eShopModernizing/wiki/04.-How-to-deploy-your-Windows-Containers-based-apps-into-Kubernetes-in-Azure-Container-Service-(Including-CI-CD)>

>[!div class="step-by-step"]
><span data-ttu-id="8e6d7-123">[Anterior](docker-application-outer-loop-devops-workflow.md)
>[Siguiente](../run-manage-monitor-docker-environments/index.md)</span><span class="sxs-lookup"><span data-stu-id="8e6d7-123">[Previous](docker-application-outer-loop-devops-workflow.md)
[Next](../run-manage-monitor-docker-environments/index.md)</span></span>