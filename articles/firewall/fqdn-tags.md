---
title: Información general de las etiquetas FQDN para Azure Firewall
description: Obtenga información acerca de las etiquetas FQDN en Azure Firewall
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 6dc7d20d31d9399355b2b3de90ea90f2f3e07af5
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224654"
---
# <a name="fqdn-tags-overview"></a>Información general de las etiquetas FQDN

Una etiqueta FQDN representa un grupo de nombres de dominio completo (FQDN) asociados con los servicios de Microsoft conocidos. Puede usar una etiqueta FQDN en las reglas de una aplicación para permitir que pase el tráfico de red saliente necesario a través del firewall.

Por ejemplo, para permitir manualmente el tráfico de red de Windows Update a través del firewall, deberá crear varias reglas de aplicación de acuerdo con la documentación de Microsoft. Si usa etiquetas FQDN, puede crear una regla de aplicación e incluir la etiqueta **Windows Update** para que el tráfico de red que apunta a los puntos de conexión de Windows Update pueda fluir a través del firewall.

No puede crear sus propias etiquetas FQDN ni especificar qué nombres de dominio completo se incluyen en una etiqueta. Microsoft administra los nombres de dominio completo que abarca la etiqueta FQDN y la actualiza a medida que estos cambian. 

<!--- screenshot of application rule with a FQDN tag.-->

En la siguiente tabla se muestran las etiquetas FQDN actuales que puede usar. Microsoft mantiene estas etiquetas y agregará otras adicionales periódicamente.

|Etiqueta FQDN  |DESCRIPCIÓN  |
|---------|---------|
|Windows Update     |Permite el acceso saliente a Microsoft Update como se describe en [How to Configure a Firewall for Software Updates](https://technet.microsoft.com/library/bb693717.aspx) (Cómo configurar el firewall para las actualizaciones de software).|
|Diagnósticos de Windows|Permite el acceso saliente a todos los [puntos de conexión de Diagnósticos de Windows](https://docs.microsoft.com/windows/privacy/configure-windows-diagnostic-data-in-your-organization#endpoints).|
|Microsoft Active Protection Service (MAPS)|Permite el acceso saliente a [MAPS](https://cloudblogs.microsoft.com/enterprisemobility/2016/05/31/important-changes-to-microsoft-active-protection-service-maps-endpoint/).|
|App Service Environment (ASE)|Permite el acceso saliente hacia el tráfico de la plataforma de ASE. Esta etiqueta no incluye los puntos de conexión de SQL y Storage específicos del cliente creados por ASE. Dichos puntos de conexión deben habilitarse a través de los [puntos de conexión de servicio](../virtual-network/tutorial-restrict-network-access-to-resources.md) o se deben agregar manualmente.|
|Azure Backup|Permite el acceso saliente a los servicios de Azure Backup.

> [!NOTE]
> Al seleccionar Etiqueta FQDN en una regla de aplicación, en el campo protocolo: puerto debe estar seleccionada la opción **https**.

## <a name="next-steps"></a>Pasos siguientes

Para aprender a implementar una instancia de Azure Firewall, consulte el [Tutorial: Implementación y configuración de Azure Firewall mediante Azure Portal](tutorial-firewall-deploy-portal.md).