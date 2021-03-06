---
title: Comando dotnet nuget add source
description: El comando dotnet nuget add source agrega un nuevo origen de paquete a los archivos de configuración de NuGet.
ms.date: 03/20/2020
ms.openlocfilehash: c1e398699c7482a69b750cde718e6f9178b5c4bd
ms.sourcegitcommit: 07123a475af89b6da5bb6cc51ea40ab1e8a488f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80148493"
---
# <a name="dotnet-nuget-add-source"></a>dotnet nuget add source

**Este artículo se aplica a:** ✔️ SDK de .NET Core 3.1.200 y versiones posteriores

## <a name="name"></a>NOMBRE

`dotnet nuget add source`: agrega un origen de NuGet.

## <a name="synopsis"></a>Sinopsis

```dotnetcli
dotnet nuget add source <PACKAGE_SOURCE_PATH> [--name] [--username]
    [--password] [--store-password-in-clear-text] [--valid-authentication-types]
    [--configfile]
dotnet nuget add source [-h|--help]
```

## <a name="description"></a>Descripción

El comando `dotnet nuget add source` agrega un nuevo origen de paquete a los archivos de configuración de NuGet.

## <a name="arguments"></a>Argumentos

- **`PACKAGE_SOURCE_PATH`**

  Ruta de acceso al origen del paquete.

## <a name="options"></a>Opciones

- **`--configfile`**

  Archivo de configuración de NuGet. Si se especifica, solo se usará la configuración de este archivo. Si no se especifica, se utilizará la jerarquía de archivos de configuración del directorio actual. Para más información, consulte [Configuraciones comunes de NuGet](https://docs.microsoft.com/nuget/consume-packages/configuring-nuget-behavior).

- **`-n|--name`**

  Nombre del origen.

- **`-p|--password`**

  Contraseña que se debe usar al conectarse a un origen autenticado.

- **`--store-password-in-clear-text`**

  Deshabilita el cifrado de la contraseña para permitir el almacenamiento de las credenciales de origen del paquete portátil.

- **`-u|--username`**

  Nombre de usuario que se usará al conectarse a un origen autenticado.

- **`--valid-authentication-types`**

  Lista separada por comas de tipos de autenticación válidos para este origen. Establézcalo en `basic` si el servidor anuncia NTLM o Negotiate y las credenciales deben enviarse mediante el mecanismo básico, por ejemplo, cuando se usa una instancia de PAT con Azure DevOps Server local. Otros valores válidos son `negotiate`, `kerberos`, `ntlm` y `digest`, pero es poco probable que estos valores sean útiles.

## <a name="examples"></a>Ejemplos

- Agregue `nuget.org` como origen:

  ```dotnetcli
  dotnet nuget add source https://api.nuget.org/v3/index.json -n nuget.org
  ```

- Agregue `c:\packages` como origen local:

  ```dotnetcli
  dotnet nuget add source c:\packages
  ```

- Agregue un origen que necesite autenticación:

  ```dotnetcli
  dotnet nuget add source https://someServer/myTeam -n myTeam -u myUsername -p myPassword --store-password-in-clear-text
  ```

- Agregue un origen que necesite autenticación (luego, pase a la instalación del proveedor de credenciales):

  ```dotnetcli
  dotnet nuget add source https://azureartifacts.microsoft.com/myTeam -n myTeam
  ```

## <a name="see-also"></a>Vea también

- [Secciones de origen del paquete en archivos NuGet.config](/nuget/reference/nuget-config-file#package-source-sections)

- [Comando sources (nuget.exe)](/nuget/reference/cli-reference/cli-ref-sources)
