---
title: Administración de SSO para el proxy de aplicación de Azure AD | Microsoft Docs
description: Obtenga información acerca de los conceptos básicos del inicio de sesión único con el proxy de aplicación
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: dbdbe8b83af20f66ad2cc99d2a5665262479b4a9
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364155"
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a>¿Cómo permite el proxy de aplicación de Azure AD el inicio de sesión único?

El inicio de sesión único es un elemento clave del proxy de aplicación de Azure AD.  Proporciona la mejor experiencia de usuario porque los usuarios solo deben iniciar sesión en Azure Active Directory en la nube. Una vez que se autentican en Azure Active Directory, el conector del proxy de aplicación controla la autenticación en la aplicación local. La aplicación de back-end no puede apreciar la diferencia entre un usuario remoto que inicia sesión a través del proxy de aplicación y un uso normal en un dispositivo unido al dominio. 

Para usar Azure Active Directory para el inicio de sesión único a las aplicaciones, debe seleccionar **Azure Active Directory** como método de autenticación previa. Si selecciona **Passthrough** (Acceso directo), los usuarios no se autentican en Azure Active Directory, sino que se dirigen directamente a la aplicación. Puede configurar esta opción cuando publica por primera vez una aplicación, o ir a la aplicación en Azure Portal y editar la configuración del proxy de aplicación. 

Para ver las opciones de inicio de sesión único, siga estos pasos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Vaya a **Azure Active Directory** > **Aplicaciones empresariales** > **Todas las aplicaciones**.
3. Seleccione la aplicación cuyas opciones de inicio de sesión único desee administrar.
4. Seleccione **Inicio de sesión único**.

   ![Menú desplegable SSO](./media/application-proxy-single-sign-on/single-sign-on-mode.png)

El menú desplegable muestra cinco opciones para el inicio de sesión único para la aplicación:

* Inicio de sesión único de Azure AD deshabilitado
* Inicio de sesión único con contraseña
* Inicio de sesión vinculado
* Autenticación integrada de Windows
* Inicio de sesión basado en el encabezado

## <a name="azure-ad-single-sign-on-disabled"></a>Inicio de sesión único de Azure AD deshabilitado

Si no desea utilizar la integración de Azure Active Directory para el inicio de sesión único para la aplicación, elija **Se desactivó el inicio de sesión único de Azure AD**. Con esta opción seleccionada, los usuarios pueden autenticarse dos veces. En primer lugar, se autentican en Azure Active Directory y, a continuación, inician sesión en la propia aplicación. 

Esta opción es adecuada si la aplicación local no requiere que los usuarios se autentiquen, pero desea agregar Azure Active Directory como un nivel de seguridad para el acceso remoto. 

## <a name="password-based-sign-on"></a>Inicio de sesión único con contraseña

Si desea usar Azure Active Directory como almacén de contraseñas para las aplicaciones locales, elija **Inicio de sesión con contraseña**. Esta opción es adecuada si la aplicación se autentica con una combinación de nombre de usuario y contraseña en lugar de con tokens de acceso o encabezados. Con el inicio de sesión con contraseña, los usuarios tienen que iniciar sesión en la aplicación la primera vez que acceden a ella. Después, Azure Active Directory proporciona el nombre de usuario y la contraseña en nombre del usuario. 

Para obtener información acerca de cómo configurar el inicio de sesión con contraseña, consulte [Almacén de contraseñas para el inicio de sesión único con el proxy de aplicación](application-proxy-configure-single-sign-on-password-vaulting.md).

## <a name="linked-sign-on"></a>Inicio de sesión vinculado

Si ya tiene una solución de inicio de sesión único configurada para las identidades locales, elija **Inicio de sesión vinculado**. Esta opción permite a Azure Active Directory aprovechar las soluciones existentes de SSO, pero sigue ofreciendo a los usuarios acceso remoto a la aplicación. 

Para más información sobre el inicio de sesión vinculado (conocido anteriormente como inicio de sesión único), consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="integrated-windows-authentication"></a>Autenticación integrada de Windows

Si las aplicaciones locales utilizan la autenticación integrada de Windows (IWA) o si desea usar la delegación limitada de Kerberos (KDC) para el inicio de sesión único, elija **Autenticación integrada de Windows**. Con esta opción, los usuarios solo necesitan autenticarse en Azure Active Directory y, a continuación, el conector del proxy de aplicación suplanta al usuario para obtener un token de Kerberos e iniciar sesión en la aplicación. 

Para más información acerca de cómo configurar la autenticación integrada de Windows, consulte [Delegación limitada de Kerberos para el inicio de sesión único con el proxy de aplicación](application-proxy-configure-single-sign-on-with-kcd.md).

## <a name="header-based-sign-on"></a>Inicio de sesión basado en el encabezado 

Si las aplicaciones utilizan encabezados para la autenticación, elija **Inicio de sesión basado en el encabezado**. Con esta opción, los usuarios solo necesitan la autenticación de Azure Active Directory. Microsoft se ha asociado con un servicio de autenticación de terceros denominado PingAccess, que traduce el token de acceso de Azure Active Directory en un formato de encabezado de la aplicación. 

Para más información acerca de cómo configurar la autenticación basada en el encabezado, consulte [Autenticación basada en el encabezado para el inicio de sesión único con el proxy de aplicación](application-proxy-configure-single-sign-on-with-ping-access.md).

## <a name="next-steps"></a>Pasos siguientes

- [Almacén de contraseñas para el inicio de sesión único con el proxy de aplicación](application-proxy-configure-single-sign-on-password-vaulting.md)
- [Delegación limitada de Kerberos para el inicio de sesión único con el proxy de aplicación](application-proxy-configure-single-sign-on-with-kcd.md)
- [Autenticación basada en el encabezado para el inicio de sesión único con el proxy de aplicación](application-proxy-configure-single-sign-on-with-ping-access.md) 
