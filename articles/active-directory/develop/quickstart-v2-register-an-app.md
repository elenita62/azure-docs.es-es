---
title: Registro de una aplicación con el punto de conexión de Azure AD v2.0 | Microsoft Docs
description: Obtenga información acerca de cómo registrar una aplicación con Microsoft para habilitar el inicio de sesión y obtener acceso a servicios de Microsoft con el punto de conexión Azure AD v2.0.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: lenalepa
ms.custom: aaddev
ms.openlocfilehash: b2dea11b6573be8f574bd18fa69ee76658d6d698
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975817"
---
# <a name="quickstart-register-an-app-with-the-azure-active-directory-v20-endpoint"></a>Inicio rápido: Registro de una aplicación con el punto de conexión de Azure Active Directory v2.0

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Para compilar una aplicación que acepte tanto el inicio de sesión con una cuenta de Microsoft (MSA) personal como una cuenta profesional o educativa (Azure AD), tendrá que registrar primero una aplicación con el punto de conexión de Azure Active Directory (Azure AD) v2.0. En este momento, no podrá usar ninguna aplicación existente que tenga con Azure AD o MSA. Tendrá que crear una completamente nueva.

> [!NOTE]
> No todas las características ni escenarios de Azure AD son compatibles con el punto de conexión v2.0. Para determinar si debe utilizar la versión 2.0 del punto de conexión, obtenga información sobre las [limitaciones de esta versión](active-directory-v2-limitations.md).

## <a name="step-1-sign-in-to-the-microsoft-application-registration-portal"></a>Paso 1: Inicio de sesión en el Portal de registro de aplicaciones de Microsoft

1. Vaya al Portal de registro de aplicaciones de Microsoft en [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).
1. Inicie sesión con una cuenta personal o una cuenta profesional o educativa de Microsoft. Si no tiene ninguna de ellas, suscríbase para una nueva cuenta personal.
1. ¿Ha acabado? Ahora debería estar viendo su lista de aplicaciones de Microsoft, que probablemente esté vacía. Cambiemos eso.

## <a name="step-2-register-an-app"></a>Paso 2: Registro de una aplicación

1. Seleccione **Agregar una aplicación** y asígnele un nombre.
    El portal le asignará a su aplicación un identificador de aplicación único global que usará más adelante en su código. Si su aplicación incluye un componente del lado del servidor que necesita tokens de acceso para llamar a las API (como Office, Azure o su propia API web), aquí también le interesará crear un **Secreto de la aplicación**.
1. A continuación, agregue las **plataformas** que usará la aplicación.
    * Para las aplicaciones basadas en web, proporcione un **URI de redirección** al que se puedan enviar los mensajes de inicio de sesión.
    * Para las aplicaciones móviles, copie el URI de redirección predeterminado creado automáticamente para usted.
    * Para las API web, se crea automáticamente un ámbito predeterminado para acceder a ellas.
        Puede agregar ámbitos adicionales mediante el botón **Agregar ámbito**. También puede agregar todas las aplicaciones que estén previamente autorizadas para usar la API web mediante el formulario **Aplicaciones preautorizadas**.
1. Si lo desea, personalice la apariencia de la página de inicio de sesión en la sección **Perfil**. 
1. Debe **guardar** los cambios antes de continuar.

> [!NOTE]
> Cuando cree una aplicación con [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), la aplicación se registrará en el inquilino principal de la cuenta que use para iniciar sesión en el portal. Esto significa que no se puede registrar una aplicación en el inquilino de Azure AD con una cuenta personal de Microsoft. Si desea registrar explícitamente una aplicación en un inquilino determinado, inicie sesión con una cuenta creada originalmente en ese inquilino.

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene una aplicación de Microsoft, puede completar uno de las guías de inicio rápido de v2.0. Estas son algunas recomendaciones:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]
