---
title: Descripción de cómo se usan los roles en las entidades basadas en patrones
titleSuffix: Azure Cognitive Services
description: Los roles son subtipos contextuales con nombre de una entidad que solo se usa en patrones. Por ejemplo, en la expresión comprar un billete de Nueva York a Londres, ambas son ciudades, pero cada una tiene un significado diferente en la frase. New York (Nueva York) es la ciudad de origen y London (Londres) es la de destino.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 9bbbb797cd7e7d1cea52f1d5b1b491998b595db7
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638093"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Los roles de entidad en los patrones son subtipos contextuales
Los roles son subtipos contextuales con nombre de una entidad que solo se usa en [patrones](luis-concept-patterns.md).

Por ejemplo, en la expresión `buy a ticket from New York to London`, New York (Nueva York) y London (Londres) son ciudades, pero cada una tiene un significado diferente en la frase. New York (Nueva York) es la ciudad de origen y London (Londres) es la de destino. 

Los roles dan un nombre a esas diferencias:

|Entidad|Rol|Propósito|
|--|--|--|
|Ubicación|origin|De dónde sale el avión|
|Ubicación|de destino|En dónde aterriza el avión|
|Entidad datetimeV2 creada previamente|to|fecha de finalización|
|Entidad datetimeV2 creada previamente|De|fecha de inicio|

## <a name="how-are-roles-used-in-patterns"></a>¿Cómo se usan los roles en los patrones?
En la expresión de plantilla de un patrón, los roles se usan dentro de la expresión: 

|Patrón con roles de entidad|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Sintaxis de los roles en los patrones
La entidad y el rol se incluyen entre paréntesis, `{}`. La entidad y el rol se separan mediante dos puntos. 

## <a name="roles-versus-hierarchical-entities"></a>Roles frente a entidades jerárquicas
Las entidades jerárquicas proporcionan la misma información contextual que los roles, pero solo para las expresiones en **intenciones**. De forma similar, los roles proporcionan la misma información contextual que las entidades jerárquicas, pero solo en los **patrones**.

|Aprendizaje contextual|Se usa en|
|--|--|
|entidades jerárquicas|intenciones|
|roles|patrones|

## <a name="roles-with-prebuilt-entities"></a>Roles con entidades creadas previamente

Utilice roles con las entidades creadas previamente para dar sentido a distintas instancias de la entidad creada previamente dentro de una expresión. 

### <a name="roles-with-datetimev2"></a>Roles con datetimeV2

DatetimeV2, la entidad creada previamente, hace un gran trabajo al comprender una gran variedad de fechas y horas distintas en las expresiones. Puede especificar las fechas y los intervalos de fechas de manera diferente a la descripción predeterminada de la entidad creada previamente. 

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo agregar [roles](luis-how-to-add-entities.md#add-role-to-pattern-based-entity).
