---
title: Creación de su primera función con la CLI de Azure
description: Obtenga información sobre cómo crear su primera función de Azure para su ejecución sin servidor mediante la CLI de Azure y Azure Functions Core Tools.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 09/10/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: 07a079e00963f1f5aff96369649e2e4fb248aae0
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986005"
---
# <a name="create-your-first-function-from-the-command-line"></a>Creación de su primera función desde la línea de comandos

Este tema de inicio rápido lo guía a través de la creación de la primera función desde la línea de comandos o el terminal. Use la CLI de Azure para crear una aplicación de función, que será la infraestructura [sin servidor](https://azure.microsoft.com/solutions/serverless/) que hospedará la función. El proyecto de código de función se genera a partir de una plantilla mediante el uso de [Azure Functions Core Tools](functions-run-local.md), que también se usa para implementar el proyecto de aplicación de función en Azure.

Puede seguir los pasos siguientes con un equipo Mac, Windows o Linux.

## <a name="prerequisites"></a>Requisitos previos

Antes de ejecutar este ejemplo, debe tener lo siguiente:

+ Instale [la versión 2.x de Azure Core Tools](functions-run-local.md#v2).

+ Instale la [CLI de Azure]( /cli/azure/install-azure-cli). En este artículo se necesita la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. También puede usar [Azure Cloud Shell](https://shell.azure.com/bash).

+ Una suscripción de Azure activa.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Creación del proyecto de aplicación de función local

Ejecute el comando siguiente desde la línea de comandos para crear un proyecto de aplicación de función en la carpeta `MyFunctionProj` del directorio local actual. También se crea un repositorio GitHub en `MyFunctionProj`.

```bash
func init MyFunctionProj
```

Cuando se le pida, seleccione un tiempo de ejecución del trabajo de entre los siguientes lenguajes:

+ `dotnet`: crea un proyecto de biblioteca de clases .NET (.csproj).
+ `node`: crea un proyecto JavaScript.

Cuando se ejecute el comando, verá una salida similar a la siguiente:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

Use el siguiente comando para ir a la nueva carpeta del proyecto `MyFunctionProj`.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>Creación de una aplicación de función

Debe tener una Function App para hospedar la ejecución de las funciones. La Function App proporciona un entorno para la ejecución sin servidor de su código de función. Le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos. Cree una Function App con el comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create). 

En el siguiente comando, sustituya un nombre de aplicación de función único donde vea el marcador de posición `<app_name>` y el nombre de la cuenta de almacenamiento para `<storage_name>`. El `<app_name>` se usa como el dominio DNS predeterminado para la Function App y, por ello, el nombre debe ser único en todas las aplicaciones de Azure. El parámetro _deployment-source-url_ es un repositorio de ejemplo de GitHub que contiene una función desencadenada mediante HTTP de tipo "Hola mundo".

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westeurope \
--name <app_name> --storage-account  <storage_name>  
```

Si establece el parámetro _consumption-plan-location_, la aplicación de función se hospedará en un plan de hospedaje de consumo. Los recursos de este plan sin servidor se agregan de forma dinámica según lo requieren las funciones y solo se paga por ellos cuando se están ejecutando dichas funciones. Para obtener más información, vea [Choose the correct hosting plan](functions-scale.md) (Elegir el plan de hospedaje correcto).

Después de que se haya creado la Function App, la CLI de Azure muestra información similar al siguiente ejemplo:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

## <a name="configure-the-function-app"></a>Configuración de la Function App

La versión 2.x de Core Tools crea proyectos mediante plantillas para el entorno de ejecución de Azure Functions 2.x. Es por esto que debe asegurarse de usar el entorno de ejecución de la versión 2.x en Azure. Si establece la configuración de la aplicación `FUNCTIONS_WORKER_RUNTIME` en `~2` anclará la aplicación de función a la versión 2.x más reciente. Establezca la configuración de aplicación con el comando [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set).

En el siguiente comando de la CLI de Azure, <app_name> es el nombre de la aplicación de función.

```azurecli-interactive
az functionapp config appsettings set --name <app_name> \
--resource-group myResourceGroup \
--settings FUNCTIONS_WORKER_RUNTIME=~2
```

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
