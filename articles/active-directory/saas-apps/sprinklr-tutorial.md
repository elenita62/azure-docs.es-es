---
title: 'Tutorial: Integración de Azure Active Directory con Sprinklr | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Sprinklr.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: e2dc9b7e7cf5964c36b21418a0162c1c2ef92dc8
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430188"
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Tutorial: integración de Azure Active Directory con Sprinklr

En este tutorial, obtendrá información sobre cómo integrar Sprinklr con Azure Active Directory (Azure AD).

La integración de Sprinklr con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Sprinklr.
- Puede habilitar que los usuarios inicien sesión automáticamente en Sprinklr (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Sprinklr, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Sprinklr

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Sprinklr desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-sprinklr-from-the-gallery"></a>Incorporación de Sprinklr desde la galería
Para configurar la integración de Sprinklr en Azure AD, deberá agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Sprinklr desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Sprinklr**.

    ![Creación de un usuario de prueba de Azure AD](./media/sprinklr-tutorial/tutorial_sprinklr_search.png)

1. En el panel de resultados, seleccione **Sprinklr** y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Sprinklr con un usuario de prueba llamado Britta Simon.

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Sprinklr para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario asociado de Sprinklr.

Para establecer la relación de vínculo, en Sprinklr, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Sprinklr, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Sprinklr](#creating-a-sprinklr-test-user)**: para tener un homólogo de Britta Simon en Sprinklr vinculado a la representación del usuario de Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Sprinklr.

**Para configurar el inicio de sesión único de Azure AD con Sprinklr, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Sprinklr**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

1. En la sección **Sprinklr Domain and URLs** (Dominio y direcciones URL de Sprinklr), lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/sprinklr-tutorial/tutorial_sprinklr_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.sprinklr.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.sprinklr.com`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Sprinklr](https://www.sprinklr.com/contact-us/) para obtener estos valores. 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/sprinklr-tutorial/tutorial_general_400.png)

1. En la sección **Sprinklr Configuration** (Configuración de Sprinklr), haga clic en **Configure Sprinklr** (Configurar Sprinklr) para abrir la ventana **Configurar inicio de sesión**. Copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** de la sección **Referencia rápida**.

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de Sprinklr como administrador.

1. Vaya a **Administración \> Configuración**.
   
    ![Administración](./media/sprinklr-tutorial/ic782907.png "Administración")

1. Vaya a **Administrar socios \> Inicio de sesión único** en el panel de la izquierda.
   
    ![Administración de asociado](./media/sprinklr-tutorial/ic782908.png "Administración de asociados")

1. Haga clic en **+Add Single Sign On**(+ Agregar inicios de sesión únicos).
   
    ![Inicios de sesión únicos](./media/sprinklr-tutorial/ic782909.png "Inicios de sesión únicos")

1. Siga estos pasos en la página **Inicio de sesión único** :
   
    ![Inicios de sesión únicos](./media/sprinklr-tutorial/ic782910.png "Inicios de sesión únicos")

    a. En el cuadro de texto **Name** (Nombre), escriba el nombre de la configuración (por ejemplo, *WAADSSOTest*).

    b. Seleccione **Habilitado**.

    c. Seleccione **Use new SSO Certificate**(Usar el nuevo certificado de SSO).
             
    e. Abra el certificado codificado en base 64 en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado de proveedor de identidades**.

    f. Pegue el valor del **SAML Entity ID** (Identificador de entidad de SAML) que ha copiado de Azure Portal en el cuadro de texto **Entity ID** (Id. de entidad).

    g. Pegue el valor de **SAML Single Sign-On Service URL** (Dirección URL del servicio de inicio de sesión único de SAML) que copió de Azure Portal en el cuadro de texto **Identity provider Login URL** (Dirección URL de inicio de sesión del proveedor de identidades).

    h. Pegue el valor de **Sign-Out URL** (Dirección URL de cierre de sesión) que copió de Azure Portal en el cuadro de texto **Identity Provider Logout URL** (Dirección URL de cierre de sesión del proveedor de identidades).
     
    i. Como **Tipo de Id. de usuario de SAML**, seleccione **La aserción contiene el nombre de usuario de sprinklr.com del usuario**.

    j. Para **Ubicación de Id. de usuario de SAML**, seleccione **El Id. de usuario está en el elemento NameIdentifier de la instrucción Subject**.

    k. Haga clic en **Save**(Guardar).
       
    ![SAML](./media/sprinklr-tutorial/ic782911.png "SAML")

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más sobre la característica de documentación insertada aquí: [Vista previa: Administración de inicio de sesión único para aplicaciones empresariales en el nuevo Azure Portal]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/sprinklr-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/sprinklr-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/sprinklr-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/sprinklr-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-sprinklr-test-user"></a>Creación de un usuario de prueba de Sprinklr

1. Inicie sesión en su sitio de la compañía de Sprinklr como administrador.

1. Vaya a **Administración \> Configuración**.
   
    ![Administración](./media/sprinklr-tutorial/ic782907.png "Administración")

1. Vaya a **Administrar clientes \> Usuarios** en el panel de la izquierda.
   
    ![Configuración](./media/sprinklr-tutorial/ic782914.png "Configuración")

1. Haga clic en **Agregar usuario**.
   
    ![Configuración](./media/sprinklr-tutorial/ic782915.png "Configuración")

1. En el cuadro de diálogo **Edit user** (Editar usuario), realice los siguientes pasos:
   
    ![Edición de usuarios](./media/sprinklr-tutorial/ic782916.png "Edición de usuarios") 

    a. En los cuadros de texto **Correo electrónico**, **Nombre** y **Apellido**, escriba la información de una cuenta de usuario de Azure AD que desee aprovisionar.

    b. Seleccione **Password Disabled**(Contraseña deshabilitada).

    c. Seleccione **Language** (Lenguaje).

    d. Seleccione **User Type**(Tipo de usuario).

    e. Haga clic en **Update**(Actualizar).
   
     >[!IMPORTANT]
     >**Password Disabled** (Contraseña deshabilitada) debe estar seleccionada para que los usuarios puedan iniciar sesión a través de un proveedor de identidades. 
     
1. Vaya a **Role**(Rol) y luego lleve a cabo los siguientes pasos:
   
    ![Roles de asociados](./media/sprinklr-tutorial/ic782917.png "Roles de asociados")

    a. En la lista **Global**, seleccione **ALL\_Permissions** (TODOS los permisos).  

    b. Haga clic en **Update**(Actualizar).

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Sprinklr ofrecida por Sprinklr para aprovisionar cuentas de usuario de Azure AD. 

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Sprinklr.

![Asignar usuario][200] 

**Para asignar a Britta Simon a Sprinklr, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Sprinklr**.

    ![Configurar inicio de sesión único](./media/sprinklr-tutorial/tutorial_sprinklr_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Sprinklr del Panel de acceso, se inicia sesión en la aplicación Sprinklr automáticamente. Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/sprinklr-tutorial/tutorial_general_203.png

