---
title: 'Ejemplo de script de la CLI de Azure: creación de una suscripción a Azure Event Grid | Microsoft Docs'
description: El script de la CLI de Azure de este tema muestra cómo crear una suscripción de Event Grid a nivel de cuenta para cambios en el estado de trabajo.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: eae9947b7dcbd6f2c52fb0d4abc65375aed7766e
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378795"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>Ejemplo de CLI: creación de una suscripción a Azure Event Grid 

El script de la CLI de Azure de este artículo muestra cómo crear una suscripción de Event Grid a nivel de cuenta para cambios en el estado de trabajo.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0.20 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los [ejemplos de la CLI de Azure](../cli-samples.md).
