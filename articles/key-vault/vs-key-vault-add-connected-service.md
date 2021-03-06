---
title: Adición de compatibilidad con Key Vault en su proyecto de ASP.NET con Visual Studio | Microsoft Docs
description: Use este tutorial para obtener información sobre cómo agregar compatibilidad con Key Vault a una aplicación web de ASP.NET o ASP.NET Core.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: c90ef26c0170db67b1d422701b6969ca3f9c9e38
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958523"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Adición de Key Vault a una aplicación web mediante los servicios conectados de Visual Studio

En este tutorial, aprenderá cómo agregar con facilidad todo lo que necesita para empezar a usar Azure Key Vault para administrar sus secretos para proyectos web en Visual Studio, independientemente de que use ASP.NET Core o cualquier tipo de proyecto de ASP.NET. Mediante la característica de servicios conectados de Visual Studio 2017, Visual Studio puede agregar automáticamente todas las opciones de configuración y los paquetes de NuGet que necesita para conectarse a Key Vault en Azure. 

Para obtener información sobre los cambios que los servicios conectados realizan en el proyecto para habilitar Key Vault, vea [Servicio conectado de Key Vault : ¿Qué ha sucedido con mi proyecto de ASP.NET 4.7.1?](vs-key-vault-aspnet-what-happened.md) o [Servicio conectado de Key Vault: ¿Qué ha sucedido con mi proyecto de ASP.NET Core?](vs-key-vault-aspnet-core-what-happened.md)

## <a name="prerequisites"></a>Requisitos previos

- **Una suscripción de Azure**. Si no tiene, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2017 15.7** con la carga de trabajo de **Desarrollo web** instalado. [Descárguelo ahora](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- Para ASP.NET (no Core),necesita las herramientas de desarrollo de .NET Framework 4.7.1, que no se instalan de forma predeterminada. Para instalarlas, inicie el Instalador de Visual Studio, elija **Modificar** y luego seleccione **Componentes individuales**; después, en el lado derecho, expanda **ASP.NET y desarrollo web** y seleccione **Herramientas de desarrollo de .NET Framework 4.7.1**.
- Abra el proyecto web de ASP.NET 4.7.1 o ASP.NET Core 2.0.

## <a name="add-key-vault-support-to-your-project"></a>Adición de la compatibilidad con Key Vault a un proyecto

1. En el **Explorador de soluciones**, elija **Agregar** > **Servicio conectado**.
   Aparece la página del servicio conectado con los servicios que puede agregar al proyecto.
1. En el menú de servicios disponibles, elija **Proteja los secretos con Azure Key Vault**.

   ![Selección de "Proteja los secretos con Azure Key Vault"](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

   Si ha iniciado sesión en Visual Studio y tiene una suscripción de Azure asociada a su cuenta, aparece una página con una lista desplegable con las suscripciones. Asegúrese de que ha iniciado sesión en Visual Studio con la misma cuenta que usó para la suscripción a Azure.

1. Seleccione la suscripción que desea usar y luego elija una instancia de Key Vault nueva o existente o seleccione el vínculo Editar para modificar el nombre generado automáticamente.

   ![Seleccione su suscripción.](media/vs-key-vault-add-connected-service/KeyVaultConnectedService3.PNG)

1. Escriba el nombre que desea usar para la instancia de Key Vault.

   ![Cambiar el nombre de la instancia de Key Vault y elegir un grupo de recursos](media/vs-key-vault-add-connected-service/KeyVaultConnectedService-Edit.PNG)

1. Seleccione un grupo de recursos existente o elija crear uno con un nombre único generado automáticamente.  Si desea crear un grupo con un nombre diferente, puede usar [Azure Portal](https://portal.azure.com) y luego cerrar la página y reiniciar la recarga de la lista de grupos de recursos.
1. Elija la región en que va a crear la instancia de Key Vault. Si la aplicación web se hospeda en Azure, elija la región que hospeda la aplicación web para lograr un rendimiento óptimo.
1. Elija un modelo de precios. Para más información, vea los [precios de Key Vault ](https://azure.microsoft.com/pricing/details/key-vault/).
1. Haga clic en Aceptar para aceptar las opciones de configuración.
1. Seleccione **Agregar** para crear la instancia de Key Vault. El proceso de creación puede producir un error si elige un nombre que ya se ha utilizado.  Si esto ocurre, use el vínculo **Editar** para cambiar el nombre de la instancia de Key Vault y vuelva a intentarlo.

   ![Adición del servicio conectado al proyecto](media/vs-key-vault-add-connected-service/KeyVaultConnectedService4.PNG)

1. Ahora, agregue un secreto a Key Vault en Azure. Para localizar la ubicación correcta en Azure Portal, haga clic en el vínculo para administrar los secretos almacenados en esta instancia de Key Vault. Si cerró la página o el proyecto, puede acceder a ellos en [Azure Portal](https://portal.azure.com); para ello, seleccione **Todos los servicios** en **Seguridad**, elija **Key Vault** y luego seleccione la instancia de Key Vault que acaba de crear.

   ![Navegación hasta Azure Portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. En la sección Key Vault del almacén de claves creado, seleccione **Secretos** y luego **Generar/Importar**.

   ![Generar/Importar un secreto](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Escriba un secreto, como "MiSecreto", asígnele cualquier valor de cadena como prueba y luego seleccione el botón **Crear**.

   ![Crear un secreto](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (Opcional) Escriba otro secreto, pero, en esta ocasión, asígnele una categoría asignándole el nombre "Secretos--MiSecreto". Esta sintaxis especifica una categoría "Secretos" que contiene un secreto "MiSecreto".
 
Ahora, puede acceder a los secretos en el código. Los pasos siguientes son distintos en función de si usa ASP.NET 4.7.1 o ASP.NET Core.

## <a name="access-your-secrets-in-code-aspnet-core-projects"></a>Acceso a los secretos en el código (proyectos de ASP.NET Core)

La conexión a Key Vault se establece en el inicio mediante una clase que implementa [Microsoft.AspNetCore.Hosting.IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1) utilizando una manera de extender el comportamiento de inicio que se describe en [Mejorar una aplicación desde un ensamblado externo en ASP.NET Core con IHostingStartup](/aspnet/core/fundamentals/host/platform-specific-configuration). La clase de inicio utiliza dos variables de entorno que contienen la información de conexión de Key Vault: ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED, establecida en true, y ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT, establecida en la dirección URL de Key Vault. Se agregan al archivo launchsettings.json cuando ejecuta a través del proceso **Agregar servicio conectado**.

Para acceder a los secretos:

1. En Visual Studio, en el proyecto de ASP.NET Core, ahora puede hacer referencia a estos secretos mediante el uso de las siguientes expresiones en el código:
 
   ```csharp
      config["MySecret"] // Access a secret without a section
      config["Secrets:MySecret"] // Access a secret in a section
      config.GetSection("Secrets")["MySecret"] // Get the configuration section and access a secret in it.
   ```

1. En una página .cshtml, como About.cshtml, agregue la directiva @inject cerca de la parte superior del archivo para configurar una variable que pueda usar para acceder a la configuración de Key Vault.

   ```cshtml
      @inject Microsoft.Extensions.Configuration.IConfiguration config
   ```

1. Como prueba, puede confirmar que el valor del secreto está disponible mostrándolo en una de las páginas. Use @config para hacer referencia a la variable de configuración.
 
   ```cshtml
      <p> @config["MySecret"] </p>
      <p> @config.GetSection("Secrets")["MySecret"] </p>
      <p> @config["Secrets:MySecret"] </p>
   ```

1. Compile y ejecute la aplicación web, vaya a la página Acerca de y consulte el valor "secreto".

## <a name="access-your-secrets-in-code-aspnet-471-projects"></a>Acceso a los secretos en el código (proyectos de ASP.NET 4.7.1)

La conexión con Key Vault se configura mediante la clase ConfigurationBuilder utilizando la información que se ha agregado al archivo web.config cuando ejecutó a través del proceso **Agregar servicio conectado**.

Para acceder a los secretos:

1. Cambie el archivo web.config de la manera siguiente. Las claves son marcadores de posición que AzureKeyVault ConfigurationBuilder reemplazará por los valores de los secretos en Key Vault.

   ```xml
     <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="MySecret" value="dummy1"/>
       <add key="Secrets--MySecret" value="dummy2"/>
     </appSettings>
   ```

1. En HomeController, en el método de controlador Acerca de, agregue las líneas siguientes para recuperar el secreto y almacenarlo en el elemento ViewBag.
 
   ```csharp
            var secret = ConfigurationManager.AppSettings["MySecret"];
            var secret2 = ConfigurationManager.AppSettings["Secrets--MySecret"];
            ViewBag.Secret = $"Secret: {secret}";
            ViewBag.Secret2 = $"Secret2: {secret2}";
   ```

1. En la vista About.cshtml, agregue lo siguiente para mostrar el valor del secreto (solo para pruebas).

   ```csharp
      <h3>@ViewBag.Secret</h3>
      <h3>@ViewBag.Secret2</h3>
   ```

1. Ejecute la aplicación localmente para comprobar que puede leer el valor del secreto que escribió en Azure Portal, no el valor ficticio del archivo de configuración.

A continuación, publique la aplicación en Azure.

## <a name="publish-to-azure-app-service"></a>Publicar en Azure App Service

1. Haga clic con el botón derecho en el nodo de proyecto y elija **Publicar**. Aparece una pantalla que dice **Elegir un destino de publicación**. A la izquierda, elija **App Service** y luego **Crear nuevo**.

   ![Publicación en App Service](media/vs-key-vault-add-connected-service/AppServicePublish1.PNG)

1. En la pantalla **Crear servicio de aplicaciones**, asegúrese de que la suscripción y el grupo de recursos son los mismos que creó en Key Vault y elija **Crear**.

   ![Creación de un elemento App Service](media/vs-key-vault-add-connected-service/AppServicePublish2.PNG)

1. Una vez creada la aplicación web, aparecerá la pantalla **Publicar**. Observe la dirección URL de la aplicación web publicada que se hospeda en Azure. Si ve **Ninguno** junto a **Key Vault**, tendrá que indicar a App Service a qué instancia de Key Vault debe conectarse. Elija el vínculo **Agregar Key Vault** y elija la instancia de Key Vault que creó.

   ![Agregar Key Vault](media/vs-key-vault-add-connected-service/AppServicePublish3.PNG)

   Si ve **Administrar almacén de claves**, puede hacer clic para ver la configuración actual, editar permisos o realizar cambios en los secretos en Azure Portal.

1. Ahora, elija el vínculo URL del sitio para ir a la aplicación web en el explorador. Compruebe que ve el valor correcto en la instancia de Key Vault.

Enhorabuena, se ha confirmado que la aplicación web puede usar Key Vault para tener acceso de forma segura a los secretos almacenados cuando se ejecutan en Azure.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite el grupo de recursos, elimínelo. De esta forma, se eliminan la instancia de Key Vault y los recursos relacionados. Para eliminar el grupo de recursos mediante el portal:

1. Escriba el nombre del grupo de recursos en el cuadro de búsqueda de la parte superior del portal. Cuando vea el grupo de recursos que se utiliza en esta guía de inicio rápido en los resultados de búsqueda, selecciónelo.
2. Seleccione **Eliminar grupo de recursos**.
3. En el cuadro **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS:** escriba el nombre del grupo de recursos y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Puede obtener más información sobre el desarrollo con Key Vault en la [Guía para desarrolladores de Key Vault](key-vault-developers-guide.md).