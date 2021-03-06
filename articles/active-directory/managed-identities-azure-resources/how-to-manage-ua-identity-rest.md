---
title: Cómo administrar identidades administradas asignadas por el usuario mediante REST
description: Instrucciones paso a paso sobre cómo crear, enumerar y eliminar una identidad administrada asignada por el usuario para hacer llamadas a la API REST.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/26/2018
ms.author: daveba
ms.openlocfilehash: dc7abd4bdec30ae870ff6add33d4b9b1c08b5bbd
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159646"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-rest-api-calls"></a>Crear, enumerar o eliminar una identidad administrada asignada por el usuario mediante llamadas a la API REST

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Las identidades administradas para los recursos de Azure proporcionan a los servicios de Azure la capacidad de autenticar servicios que admitan la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, aprenderá a crear, enumerar y eliminar una identidad administrada asignada por el usuario mediante CURL para hacer llamadas a la API REST.

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md). **No olvide revisar la [diferencia entre una identidad administrada que haya asignado el sistema de otra que haya administrado el usuario](overview.md#how-does-it-work)**.
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Si usa Windows, instale el [subsistema de Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about) o [Azure Cloud Shell](../../cloud-shell/overview.md) en Azure Portal.
- Si utiliza el [subsistema de Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about) o un [sistema operativo de distribución de Linux](/cli/azure/install-azure-cli-apt?view=azure-cli-latest), [instale la consola local de la CLI de Azure](/cli/azure/install-azure-cli).
- Si utiliza la consola local de la CLI de Azure, inicie sesión en Azure mediante `az login` con una cuenta asociada a la suscripción de Azure de la que desea implementar o recuperar la información de identidad administrada asignada por el usuario.
- Para llevar a cabo las operaciones de administración de este artículo, su cuenta debe tener las siguientes asignaciones de roles:
    - Rol [Colaborador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para crear, leer (enumerar), actualizar y eliminar una identidad administrada que haya asignado el usuario.
    - Rol [Operador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para leer (enumerar) las propiedades de una identidad administrada que haya asignado el usuario.
- Recupere un token de acceso de portador mediante `az account get-access-token` para realizar las siguientes operaciones de identidad administrada asignada por el usuario.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Crear una identidad administrada asignada por el usuario 

Para crear una identidad administrada asignada por el usuario, use la siguiente solicitud CURL a la API de Azure Resource Manager. Reemplace los valores `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<USER ASSIGNED IDENTITY NAME>`, `<LOCATION>` y `<ACCESS TOKEN>` por sus propios valores:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="list-user-assigned-managed-identities"></a>Enumerar identidades administradas asignadas por el usuario

Para enumerar identidades administradas asignadas por el usuario, use la siguiente solicitud CURL a la API de Azure Resource Manager. Reemplace los valores `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` y `<ACCESS TOKEN>` por sus propios valores:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```
## <a name="delete-a-user-assigned-managed-identity"></a>Eliminar una identidad administrada asignada por el usuario

Para eliminar una identidad administrada asignada por el usuario, use la siguiente solicitud CURL a la API de Azure Resource Manager. Reemplace los parámetros `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` y `<ACCESS TOKEN>` por sus propios valores:

> [!NOTE]
> La eliminación de una identidad administrada asignada por el usuario no quitará la referencia de ningún recurso al que se haya asignado. Para quitar una identidad administrada asignada por el usuario desde una máquina virtual mediante CURL, consulte [Remove a user-assigned identity from an Azure VM](qs-configure-rest-vm.md#remove-a-user-assigned identity-from-an-azure-vm) (Eliminar una identidad asignada por el usuario de una máquina virtual de Azure).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo asignar una identidad asignada por el usuario a una máquina virtual o a un conjunto de escalado de máquinas virtuales de Azure mediante CURL, consulte cómo [configurar las identidades administradas para los recursos de Azure en una máquina virtual de Azure mediante las llamadas a API REST](qs-configure-rest-vm.md#user-assigned-managed-identity) y cómo [configurar las identidades administradas para los recursos de Azure en un conjunto de escalado de máquinas virtuales mediante las llamadas a API REST](qs-configure-rest-vmss.md#user-assigned-managed-identity).


