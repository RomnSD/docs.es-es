---
title: 'Instalación de .NET Core en el administrador de paquetes de Linux RHEL 8: .NET Core'
description: Use un administrador de paquetes para instalar el SDK y el entorno de ejecución de .NET Core en RHEL 8.
author: thraka
ms.author: adegeo
ms.date: 03/17/2020
ms.openlocfilehash: b564a386eb67b6e414a832ad3bca10d3d09022bd
ms.sourcegitcommit: 07123a475af89b6da5bb6cc51ea40ab1e8a488f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80134192"
---
# <a name="rhel-8-package-manager---install-net-core"></a>Administrador de paquetes de RHEL 8: instalación de .NET Core

[!INCLUDE [package-manager-switcher](includes/package-manager-switcher.md)]

En este artículo se describe cómo usar un administrador de paquetes para instalar .NET Core en RHEL 8.

[!INCLUDE [package-manager-intro-sdk-vs-runtime](includes/package-manager-intro-sdk-vs-runtime.md)]

## <a name="register-your-red-hat-subscription"></a>Registro de la suscripción de Red Hat

Para instalar .NET Core desde Red Hat en RHEL, primero debe registrarse con el administrador de suscripciones de Red Hat. Si esto no se ha realizado en el sistema, o si no tiene certeza de ello, vea la [documentación del producto de Red Hat para .NET Core](https://access.redhat.com/documentation/net_core/).

## <a name="install-the-net-core-sdk"></a>Instalación del SDK de .NET Core

Después de registrarse con el administrador de suscripciones, está a punto para instalar y habilitar el SDK de .NET Core. En el terminal, ejecute los comandos siguientes.

```bash
sudo dnf update
sudo dnf install dotnet-sdk-3.1
```

## <a name="install-the-aspnet-core-runtime"></a>Instalación del entorno de ejecución de ASP.NET Core

Después de registrarse con el administrador de suscripciones, está a punto para instalar y habilitar el entorno de ejecución de ASP.NET Core. En el terminal, ejecute los comandos siguientes.

```bash
sudo dnf update
sudo dnf install aspnetcore-runtime-3.1
```

## <a name="install-the-net-core-runtime"></a>Instalación del entorno de ejecución de .NET Core

Después de registrarse con el administrador de suscripciones, está a punto para instalar y habilitar el entorno de ejecución de .NET Core. En el terminal, ejecute los comandos siguientes.

```bash
sudo dnf update
sudo dnf install dotnet-runtime-3.1
```

## <a name="see-also"></a>Vea también

- [Uso de .NET Core 3.1 en Red Hat Enterprise Linux 8](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/developing_.net_applications_in_rhel_8/index)
