---
title: Configuración de entornos de ensayo para aplicaciones web en Azure App Service | Microsoft Docs
description: Aprenda a utilizar la publicación de ensayo para aplicaciones web en Azure App Service.
services: app-service
documentationcenter: ''
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: 35027b4eadba72776fd27826a389763cc0832eee
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48044321"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Configuración de entornos de ensayo en Azure App Service
<a name="Overview"></a>

Al implementar la aplicación web, la aplicación web en Linux, el back-end móvil y la aplicación API en [App Service](http://go.microsoft.com/fwlink/?LinkId=529714), puede implementarlas en una ranura de implementación independiente en lugar de en la ranura de producción predeterminada si realiza la ejecución en el nivel de plan **Estándar** o **Premium** de App Service. Las ranuras de implementación son realmente aplicaciones activas con sus propios nombres de host. Los elementos de contenido y configuraciones de aplicaciones se pueden intercambiar entre dos ranuras de implementación, incluida la ranura de producción. La implementación de la aplicación en un espacio de implementación ofrece las ventajas siguientes:

* Puede validar los cambios en la aplicación en una ranura de implementación de ensayo antes de intercambiarla con la ranura de producción.
* La implementación de una aplicación en una ranura en primer lugar y su intercambio con la de la producción garantiza que todas las instancias de la ranura estén activas antes de colocarse en producción. Esto elimina tiempos de inactividad a la hora de implementar la aplicación. El redireccionamiento del tráfico es impecable y no se anulan las solicitudes como consecuencia de las operaciones de intercambio. Este flujo de trabajo completo puede automatizarse mediante la configuración de [Intercambio automático](#Auto-Swap) .
* Después del intercambio, la ranura con la aplicación de ensayo anterior ahora ocupa la aplicación de producción anterior. Si los cambios intercambiados en el espacio de producción no son los esperados, puede realizar el mismo intercambio inmediatamente para volver a obtener el "último sitio en buen estado".

Cada nivel del plan de App Service admite un número distinto de ranuras de implementación. Para averiguar el número de ranuras que admite el nivel de la aplicación, consulte [Límites de App Service](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). Para escalar la aplicación a un nivel diferente, el nivel de destino debe admitir el número de ranuras que la aplicación ya usa. Por ejemplo, si la aplicación tiene más de 5 ranuras, no se puede reducir verticalmente al nivel **Estándar**, porque dicho nivel solo admite 5 ranuras de implementación.

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a>Incorporación de una ranura de implementación
La aplicación se debe estar ejecutando en el nivel **Estándar** o **Premium** para permitir varias ranuras de implementación.

1. En [Azure Portal](https://portal.azure.com/), abra la [hoja de recursos](../azure-resource-manager/resource-group-portal.md#manage-resources) de la aplicación.
2. Elija la opción **Ranuras de implementación** y, a continuación, haga clic en **Agregar ranura**.
   
    ![Agregar una nueva ranura de implementación][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > Si la aplicación ya no está en el nivel **Estándar** o **Premium**, recibirá un mensaje que indica los niveles compatibles para habilitar la publicación de ensayo. Llegados a este punto, tiene la opción de seleccionar **Actualizar** e ir a la pestaña **Escala** de la aplicación antes de continuar.
   > 
   > 
3. En la hoja **Agregar una ranura**, asigne un nombre a la ranura y seleccione si la configuración de la aplicación se debe clonar de otra ranura de implementación existente. Haga clic en la marca de verificación para continuar.
   
    ![Origen de configuración][ConfigurationSource1]
   
    La primera vez que agrega una ranura, solo tendrá dos opciones: clonar o no la configuración de la ranura predeterminada en producción.
    Después de haber creado varios espacios, podrá clonar la configuración de un espacio que no sea el de producción:
   
    ![Orígenes de configuración][MultipleConfigurationSources]
4. En la hoja de recursos de la aplicación, haga clic en **Ranuras de implementación** y, a continuación, haga clic en una ranura de implementación para abrir la hoja de recursos de esa ranura, con un conjunto de métricas y una configuración como cualquier otra aplicación. El nombre de la ranura se muestra en la parte superior de la hoja para recordarle que está viendo la ranura de implementación.
   
    ![Título de la ranura de implementación][StagingTitle]
5. Haga clic en la dirección URL de la aplicación en la hoja del espacio. Observe que el espacio de implementación tiene su propio nombre de host y que se trata también de una aplicación activa. Para limitar el acceso público a la ranura de implementación, consulte [Aplicación web de App Service: bloquear acceso web a ranuras de implementación que no son de producción](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

No hay ningún contenido después de la creación de un espacio de implementación. Puede implementar el espacio desde una rama de repositorio diferente o desde un repositorio completamente diferente. También puede cambiar la configuración del espacio. Utilice el perfil de publicación o las credenciales de implementación asociadas al espacio de implementación para ver las actualizaciones del contenido.  Por ejemplo, [puede publicar en esta ranura mediante Git](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>

## <a name="which-settings-are-swapped"></a>¿Qué configuración se intercambia?
Cuando crea un clon de la configuración de otro espacio de implementación, la configuración clonada se puede editar. Además, algunos elementos de configuración seguirán el contenido a través de un intercambio (no son específicos del espacio) mientras que otros elementos de configuración permanecerán en el mismo espacio después de un intercambio (son específicos del espacio). Las listas siguientes muestran la configuración que cambia cuando se intercambian las ranuras.

**Configuraciones que se intercambian**:

* Configuración general: por ejemplo, versión de framework, 32 o 64 bits, Web Sockets
* Configuración de la aplicación (puede configurarse para ajustarse a un espacio)
* Cadenas de conexión (puede configurarse para ajustarse a un espacio)
* Asignaciones de controlador
* Configuración de supervisión y diagnóstico
* Contenido de WebJobs

**Configuraciones que no se intercambian**:

* Extremos de publicación
* Nombres de dominio personalizados
* Certificados SSL y enlaces
* Configuración de escala
* Programadores de WebJobs

Para establecer una configuración de aplicación o una cadena de conexión que se ajuste a una ranura (no intercambiada), obtenga acceso a la hoja **Configuración de la aplicación** para una ranura específica y seleccione el cuadro **Configuración de ranuras** para los elementos de configuración que se deben ajustar a la ranura. Marcar un elemento de configuración como ranura específica tiene el efecto de establecer ese elemento como no intercambiable en todas las ranuras de implementación asociadas con la aplicación.

![Configuración de ranura][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a>Intercambio de ranuras de implementación 
Puede intercambiar ranuras de implementación en la vista **Información general** o **Ranuras de implementación** de la hoja de recursos de la aplicación.

> [!IMPORTANT]
> Antes de colocar una aplicación de una ranura de implementación en producción, asegúrese de que todos los valores específicos que no son de ranura están configurados exactamente como desea que estén en el destino de intercambio.
> 
> 

1. Para intercambiar las ranuras de implementación, haga clic en el botón **Intercambiar** en la barra de comandos de la aplicación o en la barra de comandos de una ranura de implementación.
   
    ![Botón de cambio][SwapButtonBar]

2. Asegúrese de que el origen y el destino del intercambio estén correctamente establecidos. Normalmente, el destino de intercambio es la ranura de producción. Haga clic en **Aceptar** para completar la operación. Cuando termine la operación, los espacios de implementación se habrán intercambiado.

    ![Intercambio completo](./media/web-sites-staged-publishing/SwapImmediately.png)

    Para el tipo de intercambio **Intercambio con vista previa**, consulte [Intercambio con vista previa (intercambio en varias fases)](#Multi-Phase).  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a>Intercambio con vista previa (intercambio en varias fases)

Intercambio con vista previa o en varias fases simplifican la validación de elementos de configuración específicos de ranura, como cadenas de conexión.
Para las cargas de trabajo críticas, desea validar que la aplicación se comporte según lo esperado al aplicarse la configuración de la ranura de producción, debiendo llevar a cabo dicha validación *antes* de que la aplicación se coloque en producción. Intercambio con vista previa es lo que necesita.

> [!NOTE]
> El intercambio con vista previa no se admite en las aplicaciones web en Linux.

Al usar la opción **Intercambio con vista previa** (consulte [Intercambiar ranuras de implementación](#Swap)), App Service hace lo siguiente:

- Mantiene la ranura de destino sin cambios para que la carga de trabajo existente en esa ranura (por ejemplo, producción) no se vea afectada.
- Aplica los elementos de configuración de la ranura de destino a la ranura de origen, incluidas las cadenas de conexión específicas de ranura y la configuración de aplicación.
- Reinicia los procesos de trabajo en la ranura de origen con estos elementos de configuración ya mencionados.
- Al completar el intercambio: se mueve la ranura de origen previamente activada en la ranura de destino. La ranura de destino se mueve en la ranura de origen como en un intercambio manual.
- Al cancelar el intercambio: se vuelven a aplicar los elementos de configuración de la ranura de origen a la ranura de origen.

Puede previsualizar exactamente cómo se comportará la aplicación con la configuración de la ranura de destino. Una vez completada la validación, completará el intercambio en un paso independiente. Este paso tiene la ventaja añadida de que la ranura de origen ya se ha activado con la configuración deseada y los clientes no experimentarán tiempos de inactividad.  

En los cmdlets de Azure PowerShell se incluyen ejemplos para los cmdlets de Azure PowerShell disponibles para el intercambio de varias fases se incluyen para la sección de ranuras de implementación.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Configuración de intercambio automático
El intercambio automático optimiza los escenarios de DevOps donde desee implementar continuamente la aplicación sin arranques en frío ni tiempos de inactividad para los clientes finales de la aplicación. Cuando se configura una ranura de implementación para el intercambio automático en producción, cada vez que inserte una actualización de código para esa ranura, App Service coloca automáticamente la aplicación en producción, una vez que ya esté activa en la ranura.

> [!IMPORTANT]
> Al habilitar el intercambio automático para una ranura, asegúrese de que la configuración de ranura sea exactamente la configuración prevista para la ranura de destino (normalmente la ranura de producción).
> 
> 

> [!NOTE]
> El intercambio automático no se admite en aplicaciones web en Linux.

Es fácil configurar el intercambio automático para un espacio. Siga estos pasos:

1. En **Ranuras de implementación**, seleccione una ranura que no sea de producción y elija **Configuración de la aplicación** en la hoja de recursos de esa ranura.  
   
    ![][Autoswap1]
2. Seleccione **Activado** para **Intercambio automático**, elija la ranura de destino deseada en **Ranura de intercambio automático** y haga clic en **Guardar** en la barra de comandos. Asegúrese de que la configuración del espacio sea exactamente la configuración prevista para el espacio de destino.
   
    La pestaña **Notificaciones** parpadea en color verde indicando que se ha completado con **éxito** una vez finalizada la operación.
   
    ![][Autoswap2]
   
   > [!NOTE]
   > Para probar el intercambio automático para la aplicación, puede seleccionar primero una ranura de destino que no sea de producción en **Ranura de intercambio automático** para familiarizarse con la característica.  
   > 
   > 
3. Ejecute una inserción de código para este espacio de implementación. El intercambio automático se realiza después de un breve tiempo y la actualización se refleja en la dirección URL de la ranura de destino.

<a name="Rollback"></a>

## <a name="roll-back-a-production-app-after-swap"></a>Reversión de una aplicación de producción después de un intercambio
Si se identifican errores en producción después del intercambio de espacios, revierta los espacios a sus estados anteriores. Para ello, intercambie los mismos dos espacios inmediatamente.

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a>Preparación personalizada antes del intercambio
Es posible que algunas aplicaciones necesiten acciones de preparación personalizadas. El elemento de configuración `applicationInitialization` en el archivo web.config permite especificar acciones de inicialización personalizadas antes de recibir una solicitud. La operación de intercambio espera a que se complete la preparación personalizada. He aquí un fragmento de ejemplo del archivo web.config.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostname="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

## <a name="monitor-swap-progress"></a>Supervisión del progreso de intercambio

En ocasiones, la operación de intercambio tarda algún tiempo en completarse, por ejemplo, cuando la aplicación que se intercambia tiene un período de preparación largo. Puede obtener más información sobre las operaciones de intercambio en el [registro de actividad](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) en [Azure Portal](https://portal.azure.com).

En la página de aplicación del portal, en el panel de navegación izquierdo, seleccione **Registro de actividad**.

Una operación de intercambio aparece en la consulta de registro como `Slotsswap`. Puede expandirla y seleccionar una de los suboperaciones o errores para ver los detalles.

![Registro de actividad para el intercambio de ranuras](media/web-sites-staged-publishing/activity-log.png)

<a name="Delete"></a>

## <a name="delete-a-deployment-slot"></a>Eliminación de una ranura de implementación
En la hoja para una ranura de implementación, abra la hoja de la ranura de implementación, haga clic en **Información general** (la página predeterminada) y haga clic en **Eliminar** en la barra de comandos.  

![Eliminación de una ranura de implementación][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-azure-powershell"></a>Automatización con Azure PowerShell

Azure PowerShell es un módulo que proporciona cmdlets para administrar Azure mediante Windows PowerShell, incluida la compatibilidad para administrar ranuras de implementación de Azure App Service.

* Para obtener información acerca de cómo instalar y configurar Azure PowerShell y cómo autenticar Azure PowerShell con su suscripción de Azure, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/overview).  

- - -
### <a name="create-a-web-app"></a>Creación de una aplicación web
```PowerShell
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a>Creación de una ranura de implementación
```PowerShell
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a>Inicio de un intercambio con vista previa (intercambio en varias fases) y aplicación de configuración de la ranura de destino a la ranura de origen
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Cancelación de un intercambio pendiente (intercambio con vista previa) y restauración de configuración de la ranura de origen
```PowerShell
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Intercambio de ranuras de implementación
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Supervisión de los eventos de intercambio en el registro de actividad
```PowerShell
Get-AzureRmLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

- - -
### <a name="delete-deployment-slot"></a>Eliminación de una ranura de implementación
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-azure-cli"></a>Automatización con la CLI de Azure

Para información sobre los comandos de la [CLI de Azure](https://github.com/Azure/azure-cli) que se usan con las ranuras de implementación, consulte [az webapp deployment slot](/cli/azure/webapp/deployment/slot).

## <a name="next-steps"></a>Pasos siguientes
[Aplicación web de Azure App Service: bloquear acceso web a ranuras de implementación que no son de producción](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)  
[Introducción a App Service en Linux](../app-service/containers/app-service-linux-intro.md)  
[Evaluación gratuita de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png

