---
title: Configuración del agente de Azure Log Analytics para el equipo Windows híbrido | Microsoft Docs
description: Aprenda a implementar el agente de Log Analytics para Windows en equipos fuera de Azure y a habilitar la colección de datos con Log Analytics.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/02/2018
ms.author: magoedte
ms.custom: mvc
ms.component: ''
ms.openlocfilehash: 87d1aef0ffbf1a6b371947d804aa48503b507284
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50417062"
---
# <a name="configure-log-analytics-agent-for-windows-computers-in-a-hybrid-environment"></a>Configuración del agente de Log Analytics para los equipos Windows en un entorno híbrido
[Azure Log Analytics](log-analytics-overview.md) puede recopilar datos directamente de los equipos Windows físicos o virtuales en un centro de datos o en otro entorno en la nube dentro de un único repositorio para llevar a cabo una correlación y un análisis detallados.  En esta guía de inicio rápido se muestra cómo configurar y recopilar datos de equipos Windows con unos pasos sencillos.  Para máquinas virtuales Windows de Azure, vea el tema [Recopilación de datos acerca de máquinas virtuales de Azure](log-analytics-quick-collect-azurevm.md).  

Para comprender la configuración compatible, revise los [sistemas operativos Windows admitidos](log-analytics-concept-hybrid.md#supported-windows-operating-systems) y la [configuración del firewall de red](log-analytics-concept-hybrid.md#network-firewall-requirements).
 
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="log-in-to-azure-portal"></a>Iniciar sesión en Azure Portal
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Crear un área de trabajo
1. En Azure Portal, haga clic en **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.<br><br> ![Azure Portal](media/log-analytics-quick-collect-windows-computer/azure-portal-01.png)<br><br>  
2. Haga clic en **Crear** y, a continuación, seleccione opciones para los elementos siguientes:

  * Escriba el nombre del nuevo **área de trabajo de Log Analytics**, como por ejemplo *DefaultLAWorkspace*. 
  * Seleccione una **suscripción** a la que vincularlo en la lista desplegable si la opción predeterminada seleccionada no es adecuada.
  * Para **Grupo de recursos**, seleccione un grupo de recursos existente que contenga una o más máquinas virtuales de Azure.  
  * Seleccione la **Ubicación** en que están implementadas las VM.  Para obtener más información, consulte en qué [regiones está disponible Log Analytics](https://azure.microsoft.com/regions/services/).  
  * Si va a crear un área de trabajo en una nueva suscripción creada después del 2 de abril de 2018, esta utilizará automáticamente el plan de precios *Por GB* y la opción para seleccionar un plan de tarifas no estará disponible.  Si va a crear un área de trabajo para una suscripción existente creada antes del 2 de abril o para una suscripción asociada a una inscripción de EA existente, seleccione el plan de tarifa que prefiera.  Para obtener más información sobre planes concretos, consulte los [detalles de precios de Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-windows-computer/create-loganalytics-workspace-02.png)<br>  

3. Después de proporcionar la información necesaria en el panel **Área de trabajo de Log Analytics**, haga clic en **Aceptar**.  

Mientras se comprueba la información y se crea el espacio de trabajo, puede realizar un seguimiento de su progreso en **Notificaciones** en el menú. 

## <a name="obtain-workspace-id-and-key"></a>Obtención de la clave y el identificador de área de trabajo
Antes de instalar Microsoft Monitoring Agent para Windows, necesita la clave y el identificador de área de trabajo para el área de trabajo de Log Analytics.  El asistente de configuración necesita esta información para configurar el agente de la forma adecuada y asegurarse de que puede comunicarse correctamente con Log Analytics.  

1. En Azure Portal, haga clic en **Todos los servicios**, en la esquina superior izquierda. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.
2. En la lista de áreas de trabajo de Log Analytics, seleccione *DefaultLAWorkspace* (creada antes).
3. Seleccione **Configuración avanzada**.<br><br> ![Configuración avanzada de Log Analytics](media/log-analytics-quick-collect-windows-computer/log-analytics-advanced-settings-01.png)<br><br>  
4. Seleccione **Orígenes conectados** y **Servidores Windows**.   
5. Encontrará los valores a la derecha de **Id. del área de trabajo** y **Clave principal**. Copie y pegue ambos valores en el editor que prefiera.   

## <a name="install-the-agent-for-windows"></a>Instalación del agente para Windows
Los siguientes pasos permiten la instalación y configuración de Log Analytics en Azure y Azure Government Cloud mediante la configuración para Microsoft Monitoring Agent en su equipo.  

1. En la página **Servidores Windows**, seleccione la versión de **Descargar el agente de Windows** que descargar según la arquitectura del procesador del sistema operativo Windows.
2. Ejecute el programa de instalación para instalar al agente en el equipo.
2. En la página **principal**, haga clic en **Siguiente**.
3. En la página **Términos de licencia**, lea la licencia y haga clic en **Acepto**.
4. En la página e **Carpeta de destino**, cambie o mantenga la carpeta de instalación predeterminada y haga clic en **Siguiente**.
5. En la página **Opciones de instalación del agente**, elija la opción para conectar el agente a Azure Log Analytics y luego haga clic en **Siguiente**.   
6. En la página **Azure Log Analytics**, realice lo siguiente:
   1. Pegue el **Id. del área de trabajo** y la **clave del área de trabajo (clave principal)** que copió anteriormente.  Si el equipo tiene que notificar a un área de trabajo de Log Analytics en Azure Government Cloud, seleccione **Azure Gobierno de EE.UU.** desde la lista desplegable **Azure Cloud**.  
   2. Si el equipo necesita comunicarse a través de un servidor proxy con el servicio de Log Analytics, haga clic en **Avanzado** y proporcione la dirección URL y el número de puerto del servidor proxy.  Si el servidor proxy requiere autenticación, escriba el nombre de usuario y la contraseña para autenticar con el servidor proxy y, luego, haga clic en **Siguiente**.  
7. Haga clic en **Siguiente** cuando haya terminado de proporcionar las opciones de configuración necesarias.<br><br> ![pegar identificador del área de trabajo y clave principal](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-setup-laworkspace.png)<br><br>
8. En la página **Preparado para instalar**, revise las opciones seleccionadas y haga clic en **Instalar**.
9. En la página **La configuración finalizó correctamente**, haga clic en **Finalizar**.

Una vez completado el proceso, el **Agente de administración de Microsoft** aparece en el **Panel de control**. Puede revisar la configuración y comprobar que el agente esté conectado a Log Analytics. Cuando se conecta, en la pestaña **Azure Log Analytics**, el agente muestra un mensaje similar al siguiente: **Microsoft Monitoring Agent se conectó correctamente al servicio Microsoft Log Analytics.**<br><br> ![Estado de la conexión de MMA en Log Analytics](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-laworkspace-status.png)

## <a name="collect-event-and-performance-data"></a>Recopilación de datos de eventos y rendimiento
Log Analytics puede recopilar eventos de los registros de eventos de Windows, así como de los contadores de rendimiento que especifique para los informes y análisis a largo plazo, y actuar cuando se detecte una condición determinada.  Siga estos pasos para configurar la colección de eventos desde el Registro de eventos de Windows, así como desde varios contadores de rendimiento comunes, para empezar.  

1. En Azure Portal, haga clic en **Más servicios**, en la esquina inferior izquierda. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.
2. Seleccione **Configuración avanzada**.<br><br> ![Configuración avanzada de Log Analytics](media/log-analytics-quick-collect-windows-computer/log-analytics-advanced-settings-01.png)<br><br> 
3. Seleccione **Datos** y, a continuación, **Registros de eventos de Windows**.  
4. Para agregar un registro de eventos, escriba el nombre del registro.  Escriba **System** y, a continuación, haga clic en el signo más **+**.  
5. En la tabla, compruebe los niveles de gravedad **Error** y **Advertencia**.   
6. Haga clic en **Guardar** en la parte superior de la página para guardar la configuración.
7. Seleccione **Windows Performance Data** (Datos de rendimiento de Windows) para habilitar la recopilación de contadores de rendimiento en un equipo Windows. 
8. La primera vez que se configuran los contadores de rendimiento Windows para un área de trabajo de Log Analytics nueva, se ofrece la opción de crear rápidamente varios contadores comunes. Se muestran todos con una casilla junto a cada uno.<br> ![Contadores de rendimiento Windows predeterminados seleccionados](media/log-analytics-quick-collect-windows-computer/windows-perfcounters-default.png).<br> Haga clic en **Agregar los contadores de rendimiento seleccionados**.  Se agregan con el valor preestablecido de un intervalo de ejemplo de recopilación de diez segundos.  
9. Haga clic en **Guardar** en la parte superior de la página para guardar la configuración.

## <a name="view-data-collected"></a>Ver datos recopilados
Ahora que ya ha habilitado la recopilación de datos, vamos a ver un sencillo ejemplo de búsqueda de registros para consultar algunos datos del equipo de destino.  

1. En Azure portal, en el área de trabajo seleccionada, haga clic en el icono **Búsqueda de registros**.  
2. En el panel Búsqueda de registros, en el campo de consulta, escriba `Perf` y presione Entrar o haga clic en el botón de búsqueda situado a la derecha del campo de consulta.<br><br> ![Ejemplo de consulta de búsqueda de registros de Log Analytics](media/log-analytics-quick-collect-windows-computer/log-analytics-portal-queryexample.png)<br><br> Por ejemplo, la consulta de la imagen siguiente ha devuelto 735 registros de rendimiento.<br><br> ![Resultado de búsqueda de registros de Log Analytics](media/log-analytics-quick-collect-windows-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no lo necesite, puede quitar el agente del equipo Windows y eliminar el área de trabajo de Log Analytics.  

Para eliminar el agente, realice los pasos siguientes.

1. Abra el **Panel de control**.
2. Abra **Programas y características**.
3. En **Programas y características**, seleccione **Microsoft Monitoring Agent** y haga clic en **Desinstalar**.

Para eliminar el área de trabajo, seleccione el área de trabajo de Log Analytics que ha creado anteriormente y, en la página de recursos, haga clic en **Eliminar**.<br><br> ![Eliminar recurso de Log Analytics](media/log-analytics-quick-collect-windows-computer/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Pasos siguientes
Ahora que ya recopila datos sobre el funcionamiento y el rendimiento del equipo Linux local, le resultará muy fácil empezar a explorar y analizar los datos que se recopilan, además de tomar las medidas correspondientes a partir de estos. Todo ello, *de forma gratuita*.  

Para obtener información sobre cómo ver y analizar los datos, continúe con el tutorial.   

> [!div class="nextstepaction"]
> [Ver o analizar datos en Log Analytics](log-analytics-tutorial-viewdata.md)
