---
title: Administración de una aplicación de Azure IoT Central | Microsoft Docs
description: Como administrador, ¿cómo administra su aplicación de Azure IoT Central?
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 25b4777be4257933b84d58d0f10cf12571de9590
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155327"
---
# <a name="administer-your-iot-central-application"></a>Administración de la aplicación de IoT Central

Después de crear una aplicación Microsoft Azure IoT Central, puede utilizar la sección **Administración** de la interfaz de usuario de Azure IoT Central para administrarla. Para ir a la sección **Administración**, seleccione **Administración** en el menú de navegación de la izquierda.

La sección **Administración** le permite:

- Administrar usuarios

- Administrar roles

- Ver la información de facturación

- Administrar configuración de la aplicación

- Ofrecer una prueba gratuita

En la sección **Administración**, hay un menú de navegación secundario con vínculos a las distintas tareas de administración.

Para tener acceso a la sección **Administración** y poder usarla, debe disponer del rol **Administrador** para la aplicación Azure IoT Central. Si crea una aplicación Azure IoT Central, se le asigna automáticamente el rol **Administrator** para esa aplicación. La sección [Administración de usuarios](#manage-users) de este artículo explica más acerca de cómo asignar el rol **Administrador** a otros usuarios.

## <a name="change-application-name"></a>Cambiar el nombre de la aplicación

Para cambiar el nombre de la aplicación, use el menú de navegación secundario para ir a la página **Configuración de la aplicación** de la sección **Administración**.

En la página **Configuración de la aplicación**, escriba un nombre de su elección en el campo **Nombre de aplicación**. Después, seleccione **Guardar**.

## <a name="change-the-application-url"></a>Cambiar la URL de la aplicación

Para cambiar la dirección URL de la aplicación, utilice el menú de navegación secundario para acceder a la página **Configuración de la aplicación** de la sección **Administración**.

![Página Configuración de la aplicación](media\howto-administer\image0-a.png)

En la página **Configuración de la aplicación**, escriba la dirección URL de su elección en el campo **URL** y, después, seleccione **Guardar**. La dirección URL debe tener como máximo 200 caracteres de longitud. Si la dirección URL no está disponible, verá un error de validación.

> [!Note]
> Si cambia la dirección URL, la dirección URL anterior la puede tomar otro cliente de Azure IoT Central. En ese caso, usted ya no podrá usarla. Cuando cambie la dirección URL, la dirección antigua ya no funcionará y tendrá que notificar a los usuarios la nueva URL que deben usar.

## <a name="change-the-application-image"></a>Cambiar la imagen de la aplicación

Para más información sobre el uso de imágenes en una aplicación de Azure IoT Central, consulte [Preparación y carga de imágenes a una aplicación de Azure IoT Central](howto-prepare-images.md).

## <a name="copy-an-application"></a>Copia de una aplicación

Puede crear una copia de cualquier aplicación, menos las instancias de dispositivo, el historial de datos del dispositivo y los datos de usuario. La copia será una aplicación de pago por la que se le cobrará. No puede crear una aplicación de prueba copiando otra.

Para copiar una aplicación, vaya a la página **Configuración de la aplicación**. Después, haga clic en el botón **Copiar**.

![Página Configuración de la aplicación](media\howto-administer\appCopy1.png)

Al hacer clic en el botón **Copiar**, se abre un cuadro de diálogo en el que puede seleccionar un nombre, una dirección URL, un directorio de Azure AD, una suscripción y una región de Azure para la nueva aplicación que se creará al copiar la otra. Seleccione los valores para cada uno de estos campos. Después, haga clic en el botón **Copiar** para confirmar que quiere continuar. Puede obtener más información sobre qué escribir para esos valores en el artículo sobre [cómo crear una aplicación](howto-create-application.md).

![Página Configuración de la aplicación](media\howto-administer\appCopy2.png)

Una vez finalizada correctamente la operación de copia de la aplicación, puede ir a la nueva aplicación que ha creado al copiar la aplicación. Para ir a la aplicación, seleccione el vínculo que aparece en la página **Configuración de la aplicación**.

![Página Configuración de la aplicación](media\howto-administer\appCopy3.png)

> [!Note]
> Al copiar una aplicación, también se copia la definición de reglas o acciones. Aun así, puesto que los usuarios que tienen acceso a la aplicación original no se copian en la aplicación copiada, tendrá que agregar manualmente los usuarios a acciones como el correo electrónico, para las que los usuarios son un requisito previo.

## <a name="delete-an-application"></a>Eliminar una aplicación

Para eliminar la aplicación, utilice el menú de navegación secundario para acceder a la página **Configuración de la aplicación** de la sección **Administración**.

Elija **Eliminar**.

> [!Note]
> Al eliminar una aplicación de manera permanente, se eliminan todos los datos asociados con la aplicación.  Para eliminar una aplicación, también debe tener permisos para eliminar recursos en la suscripción de Azure que haya elegido al crear la aplicación. Para obtener más información, vea el artículo sobre el [uso del control de acceso basado en rol para administrar el acceso a los recursos de la suscripción de Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="roles-in-azure-iot-central"></a>Roles en Azure IoT Central

Los roles le permiten controlar qué usuarios de la organización pueden realizar varias tareas de Azure IoT Central. Azure IoT Central tiene tres roles que puede asignar a los usuarios de la aplicación. Los roles se asignan por cada aplicación. El mismo usuario puede tener diferentes roles en diferentes aplicaciones. Puede asignar el mismo usuario a varios roles dentro de una aplicación.

### <a name="administrator"></a>Administrador

Los usuarios del rol **Administrador** tienen acceso a toda la funcionalidad de una aplicación de Azure IoT Central.

El usuario que crea una aplicación se asigna automáticamente al rol **Administrador**. Siempre debe haber al menos un usuario en el rol **Administrador**.

### <a name="application-builder"></a>Generador de aplicaciones

Los usuarios con el rol **Application Builder** (Generador de aplicaciones) pueden hacer todo en una aplicación Azure IoT Central excepto administrar la aplicación.

### <a name="application-operator"></a>Operador de aplicaciones

Los usuarios con el rol **Application Operator** (Operador de aplicaciones) no tienen acceso a la página **Application Builder** (Generador de aplicaciones). No pueden administrar la aplicación.

## <a name="manage-users"></a>Administrar usuarios

Los administradores de aplicaciones pueden asignar usuarios a los roles de la aplicación.

### <a name="add-users"></a>Agregar usuarios

Cada usuario debe tener una cuenta de usuario antes de poder iniciar sesión y acceder a la aplicación Azure IoT Central. Se admiten las cuentas de Microsoft (MSA) y las de Azure Active Directory (Azure AD) en Azure IoT Central. Actualmente no se admiten los grupos de Azure Active Directory en Azure IoT Central.

Para más información, vea la [ayuda para cuentas de Microsoft](https://support.microsoft.com/products/microsoft-account?category=manage-account) e [Inicio rápido: Agregar un nuevo usuario a Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Para agregar una cuenta de usuario a una aplicación de Azure IoT Central, use el menú de navegación secundario para ir a la página **Usuarios** de la sección **Administración**.

    ![Lista de usuarios](media\howto-administer\image1.png)

1. Para agregar un usuario, en la página **Usuarios**, seleccione **+ Agregar usuario**.

    ![Agregar usuario](media\howto-administer\image2.png)

1. Seleccione un rol para el usuario en el menú desplegable **Rol**. Aprenda más sobre los roles en la sección *Roles en Azure IoT Central* de este artículo.

    ![Selección de roles](media\howto-administer\image3.png)

    > [!NOTE]
    >  Para agregar usuarios de forma masiva, indique los identificadores de usuario de todos los usuarios que quiera agregar, separados por punto y coma. Seleccione un rol en el menú desplegable **Rol**. Después, seleccione **Guardar**.

1. Después de agregar un usuario, aparece una entrada para ese usuario en la página **Usuarios**.

    ![Lista de usuarios](media\howto-administer\image4.png)

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Modificar los roles asignados a usuarios

Los roles no se pueden cambiar una vez asignados. Para cambiar el rol asignado a un usuario, elimine el usuario y agréguelo de nuevo con un rol diferente.

### <a name="delete-users"></a>Eliminación de usuarios

Para eliminar usuarios, seleccione una o más casillas en la página **Usuarios**. A continuación, seleccione **Eliminar**.

## <a name="view-your-bill"></a>Ver la factura

Para ver la factura, vaya a la página **Facturación** en la sección **Administración**. Después, seleccione **Facturación**. La página de facturación de Azure se abre en una nueva pestaña, en la que puede ver la factura de cada una de las aplicaciones de Azure IoT Central.

## <a name="convert-your-trial-to-a-paid-application"></a>Convertir la evaluación gratuita en una aplicación de pagada

Cuando haya evaluado IoT Central, puede convertir la evaluación gratuita en una aplicación de pago. Para completar el proceso de autoservicio, siga estos pasos:

1. Use el menú de navegación secundario para ir a la página **Facturación** en la sección **Administración**. Si no ha extendido la evaluación gratuita, la página tiene el aspecto que se muestra en la captura de pantalla siguiente:

    ![Estado de la evaluación gratuita](media/howto-administer/freetrial.png)

2. Seleccione **Convert to Paid** (Convertir en versión de pago). Si no ha extendido la evaluación gratuita, la ventana emergente tiene el aspecto que se muestra en la captura de pantalla siguiente:

    ![Extender la evaluación gratuita](media/howto-administer/extend.png)

3. En la ventana emergente, seleccione el inquilino apropiado de Azure Active Directory y, después, la suscripción a Azure que se va a usar en la aplicación IoT Central.

3. Después de seleccionar **Convertir**, la evaluación gratuita se convierte en una aplicación de pago y comienza a facturarse.

## <a name="extend-your-free-trial"></a>Ampliar la evaluación gratuita

De forma predeterminada, todas las evaluaciones gratuitas están disponibles durante siete días. Si quiere aumentar la evaluación gratuita a 30 días, siga estos pasos:

1. Use el menú de navegación secundario para ir a la página **Facturación** en la sección **Administración**.

1. Seleccione **Ampliar prueba**. En la ventana emergente, seleccione el inquilino apropiado de Azure Active Directory y, después, la suscripción a Azure que se va a usar en la aplicación IoT Central.

1. Luego, seleccione **Ampliar**. La versión de prueba ahora es válida durante 30 días.

## <a name="use-the-azure-sdks-for-control-plane-operations"></a>Usar Azure SDK para operaciones de plano de control

Hay paquetes de SDK de Azure Resource Manager SDK de IoT Central disponibles para Node, Python, C#, Ruby, Java y Go. Estas bibliotecas admiten operaciones de plano de control para IoT Central, que permiten crear, enumerar, actualizar o eliminar aplicaciones de IoT Central. También proporcionan aplicaciones auxiliares para tratar con la autenticación y el control de errores específico de cada idioma. 

Encontrará ejemplos de cómo usar los SDK de Azure Resource Manager en [https://github.com/emgarten/iotcentral-arm-sdk-examples](https://github.com/emgarten/iotcentral-arm-sdk-examples).

Para obtener más información, eche un vistazo a estos paquetes en GitHub.

| Idioma | Repositorio | Paquete |
| ---------| ---------- | ------- |
| Nodo | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Go | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar una aplicación de Azure IoT Central, este es el paso siguiente sugerido:

> [!div class="nextstepaction"]
> [Configuración de la plantilla de dispositivo](howto-set-up-template.md)
