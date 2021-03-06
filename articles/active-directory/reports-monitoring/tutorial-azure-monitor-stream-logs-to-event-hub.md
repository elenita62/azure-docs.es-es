---
title: 'Tutorial: Streaming de los registros de Azure Active Directory a Azure Event Hubs (versión preliminar) | Microsoft Docs'
description: Aprenda a configurar Azure Diagnostics para insertar los registros de Azure Active Directory en Azure Event Hubs (versión preliminar)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: c8157820f7137eb06a0fe10c42645ec91f33edbc
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364092"
---
# <a name="tutorial-stream-azure-ad-logs-to-an-azure-event-hub-preview"></a>Tutorial: Streaming de los registros de Azure AD a Azure Event Hubs (versión preliminar)

En este tutorial, aprenderá a configurar los valores de diagnóstico de Azure Monitor para enviar por streaming los registros de Azure AD a Azure Event Hubs. Use este mecanismo para integrar sus registros con las herramientas de Administración de eventos e información de seguridad (SIEM) de terceros, como Splunk y QRadar.

## <a name="prerequisites"></a>Requisitos previos 

Para usar esta característica, necesita:

* Una suscripción de Azure. Si no tiene ninguna suscripción de Azure, puede [registrarse para obtener una evaluación gratuita](https://azure.microsoft.com/free/).
* Un inquilino de Azure AD.
* Un usuario que sea *administrador global* o *administrador de seguridad* para el inquilino de Azure AD.
* Un espacio de nombres de Event Hubs y un centro de eventos en la suscripción de Azure. Aprenda a [crear un centro de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="archive-logs-to-an-event-hub"></a>Archivo de registros en un centro de eventos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 

2. Seleccione **Azure Active Directory** > **Actividad** > **Registros de auditoría**. 

3. Seleccione **Exportar configuración**.  
    
4. En el panel **Configuración de diagnóstico**, realice una de las siguientes acciones:
    * Para cambiar la configuración existente, seleccione **Editar configuración**.
    * Para agregar la nueva configuración, seleccione **Agregar configuración de diagnóstico**.  
      Puede tener un máximo de tres configuraciones.

      ![Exportar configuración](./media/quickstart-azure-monitor-stream-logs-to-event-hub/ExportSettings.png)

5. Seleccione la casilla **Stream to an event hub** (Hacer streaming a un centro de eventos) y haga clic en **Event Hub/Configure** (Centro de eventos/Configurar).

6. Seleccione la suscripción de Azure y el espacio de nombres de Event Hubs al que desea enrutar los registros.  
    Tanto la suscripción como el espacio de nombres de Event Hubs deben estar asociados al inquilino de Azure AD desde el que se envían los registros por streaming. También se puede especificar un centro de eventos en el espacio de nombres de Event Hubs al que se deben enviar los registros. Si no se especifica ningún centro de eventos, se crea uno en el espacio de nombres con el nombre predeterminado **insights-logs-audit**.

7. Seleccione **Aceptar** para salir de la configuración del centro de eventos.

8. Realice alguna de las siguientes acciones o ambas:
    * Para enviar los registros de auditoría a la cuenta de almacenamiento, seleccione la casilla **AuditLogs**. 
    * Para enviar los registros de inicio de sesión a la cuenta de almacenamiento, seleccione la casilla **SignInLogs**.

9. Seleccione **Guardar** para guardar la configuración.

    ![Configuración de diagnóstico](./media/quickstart-azure-monitor-stream-logs-to-event-hub/DiagnosticSettings.png)

10. Quince minutos después, compruebe que los eventos se muestran en el centro de eventos. Para ello, vaya al centro de eventos desde el portal y compruebe que el número de **mensajes entrantes** es mayor que cero. 

    ![Registros de auditoría](./media/quickstart-azure-monitor-stream-logs-to-event-hub/InsightsLogsAudit.png)

## <a name="access-data-from-your-event-hub"></a>Acceso a datos desde un centro de eventos

Después de mostrar los datos del centro de eventos, puede acceder y leer los datos de dos maneras:

* **Configurar una herramienta SIEM compatible**. Para leer los datos del centro de eventos, la mayoría de las herramientas requieren la cadena de conexión al centro de eventos y determinados permisos para la suscripción de Azure. Herramientas de terceros con la integración de Azure Monitor incluidas, entre otras:
    * **Splunk**: para más información acerca de cómo integrar los registros de Azure AD con Splunk, consulte [Integración de registros de Azure AD con Splunk mediante Azure Monitor](tutorial-integrate-activity-logs-with-splunk.md).
    
    * **IBM QRadar**: DSM y el protocolo de Azure Event Hubs están disponibles para descargarse en [soporte técnico de IBM](http://www.ibm.com/support). Para más información sobre la integración con Azure, vaya al sitio [IBM QRadar Security Intelligence Platform 7.3.0](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
    
    * **Sumo Logic**: para configurar Sumo Logic para que consuma datos de un centro de eventos, consulte [Instalación de la aplicación de Azure AD y visualización de los paneles](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards). 

* **Configuración de herramientas personalizadas**. Si su herramienta SIEM actual aún no se admite en los diagnósticos de Azure Monitor, puede configurar herramientas personalizadas mediante las API de Event Hubs. Para más información, consulte la [Introducción a la recepción de mensajes con el Host del procesador de eventos en .NET Standard](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph).


## <a name="next-steps"></a>Pasos siguientes

* [Integración de registros de Azure AD con Splunk mediante Azure Monitor](tutorial-integrate-activity-logs-with-splunk.md)
* [Integración de registros de Azure AD con SumoLogic mediante Azure Monitor](howto-integrate-activity-logs-with-sumologic.md)
* [Interpretación del esquema de registros de auditoría en Azure Monitor](reference-azure-monitor-audit-log-schema.md)
* [Interpretación del esquema de registros de inicio de sesión en Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md)
