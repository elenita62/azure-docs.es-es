---
title: Recuperación de la lista de POP de Verizon actual para Azure CDN| Microsoft Docs
description: Obtenga información acerca de cómo recuperar la lista de POP de Verizon actual mediante el uso de la API REST.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: f703a934b0eaf4bff5be3811adeed8f0287bc658
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50237832"
---
# <a name="retrieve-the-current-verizon-pop-list-for-azure-cdn"></a>Recuperación de la lista de POP de Verizon actual para Azure CDN

Puede utilizar la API REST para recuperar el conjunto de direcciones IP de los servidores de punto de presencia (POP) de Verizon. Estos servidores de POP realizan solicitudes a los servidores de origen que están asociados a los puntos de conexión de Azure Content Delivery Network (CDN) en un perfil de Verizon (**Azure CDN de Verizon estándar** o **Azure CDN de Verizon premium**). Tenga en cuenta que este conjunto de direcciones IP es diferente de las direcciones IP que un cliente vería al realizar solicitudes a los POP. 

Para obtener la sintaxis de la operación de API REST para recuperar la lista de POP, consulte [Edge Nodes - List](https://docs.microsoft.com/rest/api/cdn/edgenodes/list) (Nodos de Edge: lista).

## <a name="typical-use-case"></a>Caso de uso típico

Por motivos de seguridad, puede usar esta lista de direcciones IP para exigir que las solicitudes a su servidor de origen se realicen solo desde un POP de Verizon válido. Por ejemplo, si alguien detectase el nombre de host o la dirección IP de un servidor de origen del punto de conexión de CDN, se podrían realizar las solicitudes directamente al servidor de origen, por lo que se omitirían las funcionalidades de seguridad y escalado proporcionadas por Azure CDN. El establecimiento de las direcciones IP en la lista devuelta como las únicas permitidas en un servidor de origen puede evitar esta situación. Para asegurarse de que tiene la lista de POP más reciente, recupérela al menos una vez al día. 

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de la API REST, consulte [Azure CDN REST API](https://docs.microsoft.com/rest/api/cdn/) (API REST de Azure CDN).
