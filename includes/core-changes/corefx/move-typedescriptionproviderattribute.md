---
ms.openlocfilehash: 4d479636f41095610eaf39f92ad0dad4863ab8b5
ms.sourcegitcommit: 56f1d1203d0075a461a10a301459d3aa452f4f47
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71216346"
---
### <a name="typedescriptionproviderattribute-moved-to-another-assembly"></a><span data-ttu-id="cb955-101">Se ha movido TypeDescriptionProviderAttribute a otro ensamblado</span><span class="sxs-lookup"><span data-stu-id="cb955-101">TypeDescriptionProviderAttribute moved to another assembly</span></span>

<span data-ttu-id="cb955-102">La clase <xref:System.ComponentModel.TypeDescriptionProviderAttribute> se ha movido.</span><span class="sxs-lookup"><span data-stu-id="cb955-102">The <xref:System.ComponentModel.TypeDescriptionProviderAttribute> class has been moved.</span></span>

#### <a name="change-description"></a><span data-ttu-id="cb955-103">Descripción del cambio</span><span class="sxs-lookup"><span data-stu-id="cb955-103">Change description</span></span>

<span data-ttu-id="cb955-104">En .NET Core 2.2 y en versiones anteriores, la clase <xref:System.ComponentModel.TypeDescriptionProviderAttribute> se encuentra en el ensamblado *System.ComponentModel.TypeConverter*.</span><span class="sxs-lookup"><span data-stu-id="cb955-104">In .NET Core 2.2 and earlier versions, The <xref:System.ComponentModel.TypeDescriptionProviderAttribute> class is found in the *System.ComponentModel.TypeConverter* assembly.</span></span>

<span data-ttu-id="cb955-105">A partir de .NET Core 3.0, se encuentra en el ensamblado *System.ObjectModel*.</span><span class="sxs-lookup"><span data-stu-id="cb955-105">Starting with .NET Core 3.0, it is found in the *System.ObjectModel* assembly.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="cb955-106">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="cb955-106">Version introduced</span></span>

<span data-ttu-id="cb955-107">3.0</span><span class="sxs-lookup"><span data-stu-id="cb955-107">3.0</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="cb955-108">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="cb955-108">Recommended action</span></span>

<span data-ttu-id="cb955-109">Este cambio solo afecta a las aplicaciones que usan la reflexión para cargar el tipo <xref:System.ComponentModel.TypeDescriptionProviderAttribute> mediante la llamada a un método como <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType>, o una sobrecarga de <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> que supone que el tipo está en un ensamblado determinado.</span><span class="sxs-lookup"><span data-stu-id="cb955-109">This change only affects applications that use reflection to load the <xref:System.ComponentModel.TypeDescriptionProviderAttribute> type by calling a method such as <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType> or an overload of <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> that assumes the type is in a particular assembly.</span></span> <span data-ttu-id="cb955-110">En ese caso, el ensamblado al que se hace referencia en la llamada al método debe actualizarse para reflejar la nueva ubicación del ensamblado del tipo.</span><span class="sxs-lookup"><span data-stu-id="cb955-110">If that is the case, the assembly referenced in the method call should be updated to reflect the type's new assembly location.</span></span>

#### <a name="category"></a><span data-ttu-id="cb955-111">Categoría</span><span class="sxs-lookup"><span data-stu-id="cb955-111">Category</span></span>

<span data-ttu-id="cb955-112">Windows Forms</span><span class="sxs-lookup"><span data-stu-id="cb955-112">Windows Forms</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="cb955-113">API afectadas</span><span class="sxs-lookup"><span data-stu-id="cb955-113">Affected APIs</span></span>

- <span data-ttu-id="cb955-114">None</span><span class="sxs-lookup"><span data-stu-id="cb955-114">None</span></span>

<!--

### Affected APIs

- Not detectable via API analysis

-->