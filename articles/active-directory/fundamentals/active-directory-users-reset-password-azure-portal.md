---
title: Restablecimiento de la contraseña de un usuario en Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo restablecer una contraseña de usuario con Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 3803808ab589c3d880111421f650141f0d829f0b
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736108"
---
# <a name="how-to-reset-a-users-password-using-azure-active-directory"></a>Restablecimiento de la contraseña de un usuario con Azure Active Directory
Puede restablecer la contraseña de un usuario si se olvida la contraseña, si el usuario bloquea su dispositivo o si nunca ha recibido una contraseña.

>[!Note]
>Si el inquilino de Azure AD no es el directorio principal de un usuario, no podrá restablecer su contraseña. Esto significa que si el usuario inicia sesión en su organización mediante una cuenta de otra organización, una cuenta de Microsoft o una cuenta de Google, no podrá restablecer su contraseña.<br><br>Si el usuario tiene una fuente de autoridad como Windows Server Active Directory, solo podrá restablecer la contraseña si ha activado la escritura diferida de contraseñas.<br><br>Si el usuario tiene una fuente de autoridad como Azure AD externo, no podrá restablecer la contraseña. Solo el usuario, o un administrador en Azure AD externo, puede restablecer la contraseña.

## <a name="to-reset-a-password"></a>Para restablecer una contraseña

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global, administrador de usuarios o administrador de contraseñas. Para más información acerca de los roles disponibles, consulte [Asignación de roles de administrador en Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md#available-roles)

2. Seleccione **Azure Active Directory**, seleccione **Usuarios** y, a continuación, busque y seleccione los usuarios que necesitan del restablecimiento y haga clic en **Restablecer contraseña**.

    Se muestra la página **Alain Charon - Perfil** con la opción **Restablecer contraseña**.

    ![Página de perfil del usuario, con la opción Restablecer contraseña resaltada](media/active-directory-users-reset-password-azure-portal/user-profile-reset-password-link.png)

3. En la página **Restablecer contraseña**, seleccione **Restablecer contraseña**.

    Una contraseña temporal se genera automáticamente para el usuario.

4. Copie la contraseña y proporciónela al usuario. El usuario deberá cambiar la contraseña durante el siguiente proceso de inicio de sesión.

    >[!Note]
    >La contraseña temporal nunca expira. La próxima vez que el usuario inicie sesión, la contraseña seguirá funcionando, sin importar cuánto tiempo haya transcurrido desde que se generó la contraseña temporal.

## <a name="next-steps"></a>Pasos siguientes
Después de restablecer la contraseña del usuario, puede realizar los siguientes procesos básicos:

- [Adición o eliminación de usuarios](add-users-azure-active-directory.md)

- [Asignación de roles a usuarios](active-directory-users-assign-role-azure-portal.md)

- [Adiciones o cambios a la información del perfil](active-directory-users-profile-azure-portal.md)

- [Creación de un grupo básico e incorporación de miembros](active-directory-groups-create-azure-portal.md)

O bien, puede realizar otros escenarios de usuario complejos, como asignar delegados, usar directivas y compartir cuentas de usuario. Para obtener más información acerca de otras acciones disponibles, consulte la [documentación de administración de usuarios en Azure Active Directory](../users-groups-roles/index.yml).
