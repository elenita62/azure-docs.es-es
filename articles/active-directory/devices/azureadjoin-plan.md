---
title: Escenarios de uso y consideraciones de implementación de Azure AD Join | Microsoft Docs
description: Se explica cómo los administradores pueden configurar Azure AD Join para sus usuarios finales (empleados, estudiantes, otros usuarios). También describe los distintos escenarios reales de uso de Azure AD Join.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
tags: azure-classic-portal
ms.component: devices
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2018
ms.author: markvi
ms.openlocfilehash: a145b4184fbebd9c4f447face1c47bec20c0ec32
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39415638"
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Escenarios de uso y consideraciones de implementación de Azure AD Join
## <a name="usage-scenarios-for-azure-ad-join"></a>Escenarios de uso de Azure AD Join
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Escenario 1: Empresas principalmente en la nube
Azure Active Directory Join (Azure AD Join) puede resultarle ventajoso si actualmente opera y administra identidades para su negocio en la nube o tiene pensado trasladarse a la nube en un futuro cercano. Puede usar una cuenta que haya creado en Azure AD para iniciar sesión en Windows 10. Los usuarios pueden unir sus equipos a Azure AD ya sea mediante el [proceso de Primera vista de Windows](azuread-joined-devices-frx.md) o desde el [menú de configuración](../user-help/device-management-azuread-joined-devices-setup.md).  Los usuarios también pueden disfrutar de un acceso de inicio de sesión único (SSO) a recursos en la nube como Office 365, tanto en sus exploradores como en las aplicaciones de Office.

### <a name="scenario-2-educational-institutions"></a>Escenario 2: Instituciones educativas
Las instituciones educativas normalmente tienen dos tipos de usuarios: profesores y estudiantes. Los profesores se consideran miembros a largo plazo de la organización. Es conveniente crear cuentas locales para ellos. Sin embargo, los estudiantes son miembros de la organización a más corto plazo y sus cuentas se pueden administrar en Azure AD. Esto significa que la escala de directorio se puede trasladar a la nube en lugar de almacenarse localmente. También significa que los alumnos podrán iniciar sesión en Windows con sus cuentas de Azure AD y obtener acceso a los recursos de Office 365 en las aplicaciones de Office.

### <a name="scenario-3-retail-businesses"></a>Escenario 3: Los negocios minoristas
El sector minorista tiene trabajadores de temporada y empleados a largo plazo. Lo habitual es crear cuentas locales y utilizar equipos unidos a dominio para los empleados a jornada completa cuyos contratos tienen una larga duración. Sin embargo, los trabajadores de temporada son miembros a más corto plazo de la organización y es deseable administrar sus cuentas de manera que las licencias de usuario se puedan mover fácilmente. Si crea sus cuentas de usuario en la nube con licencias de Office 365, estos trabajadores disfrutarán de las ventajas de iniciar sesión en Windows y las aplicaciones de Office con una cuenta de Azure AD y, al mismo tiempo, usted mantendrá una mayor flexibilidad con sus licencias una vez que dejen el puesto de trabajo.

### <a name="scenario-4-additional-scenarios"></a>Escenario 4: Otros escenarios
A las ventajas que se han comentado anteriormente hay que sumar el hecho de que los usuarios unan sus dispositivos a Azure AD gracias a una experiencia de unión simplificada, la administración eficaz de dispositivos, la inscripción de administración automática de dispositivos móviles y el inicio de sesión único en Azure AD y los recursos locales.  

## <a name="deployment-considerations-for-azure-ad-join"></a>Consideraciones de implementación de Azure AD Join
### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Habilitación de los usuarios para unir un dispositivo propiedad de la empresa directamente a Azure AD
Las empresas pueden proporcionar cuentas exclusivamente en la nube a organizaciones y empresas asociadas. Estos asociados pueden acceder así fácilmente a aplicaciones y recursos de la empresa mediante el inicio de sesión único. Este escenario es aplicable a los usuarios que acceden principalmente a los recursos en la nube, como las aplicaciones de Office 365 o de SaaS que basan su autenticación en Azure AD.

### <a name="prerequisites"></a>Requisitos previos
**En el nivel de empresa (administrador)**

* Suscripción de Azure con Azure Active Directory  

**En el nivel de usuario**

* Windows 10 (ediciones Professional y Enterprise)

### <a name="administrator-tasks"></a>Tareas de administrador
* [Configuración del registro de dispositivos](device-management-azure-portal.md)

### <a name="user-tasks"></a>Tareas de usuario
* [Configuración de un nuevo dispositivo de Windows 10 con Azure AD durante la instalación](azuread-joined-devices-frx.md)
* [Configuración de un dispositivo de Windows 10 con Azure AD desde el menú de configuración](../user-help/device-management-azuread-registered-devices-windows10-setup.md)
* [Unión de un dispositivo de Windows 10 personal a su organización](../user-help/device-management-azuread-joined-devices-setup.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a>Habilitación de BYOD en su organización para Windows 10
Puede configurar unos valores que permitan a los usuarios y empleados usar sus dispositivos personales de Windows (BYOD) para acceder a las aplicaciones y los recursos de la empresa. Los usuarios pueden agregar sus cuentas de Azure AD (cuentas profesionales o educativas) a un dispositivo de Windows personal para tener acceso a los recursos de forma segura y conforme a lo establecido.

### <a name="prerequisites"></a>Requisitos previos
**En el nivel de empresa (administrador)**

* Suscripción de Azure AD

**En el nivel de usuario**

* Windows 10 (ediciones Professional y Enterprise)

### <a name="administrator-tasks"></a>Tareas de administrador
* [Configuración del registro de dispositivos](device-management-azure-portal.md)

### <a name="user-tasks"></a>Tareas de usuario
* [Unión de un dispositivo de Windows 10 personal a su organización](../user-help/device-management-azuread-joined-devices-setup.md)

## <a name="next-steps"></a>Pasos siguientes

- [Administración de dispositivos](overview.md)

