---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: include
ms.custom: include file
ms.date: 09/27/2018
ms.author: diberry
ms.openlocfilehash: 9ad721be84263969a91b8c27c349676be35ec663
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48019107"
---
Agregue el siguiente bloque de código dentro de la clase Program:

```csharp
public struct Response
{
    public HttpResponseHeaders headers;
    public string response;

    public Response(HttpResponseHeaders headers, string response)
    {
        this.headers = headers;
        this.response = response;
    }
}

static string PrettyPrint(string s)
{
    return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
}
```