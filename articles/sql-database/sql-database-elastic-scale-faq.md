---
title: Preguntas más frecuentes sobre la escala elástica de SQL Azure | Microsoft Docs
description: Preguntas más frecuentes sobre el Escalado elástico de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 10/05/2018
ms.openlocfilehash: 31501cb0cc66c9d8db646e3cf0c75cc3d8daf7e0
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242541"
---
# <a name="elastic-database-tools-frequently-asked-questions-faq"></a>Preguntas frecuentes (P+F) sobre las herramientas de base de datos elástica

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Si tengo un solo inquilino por partición y ninguna clave de particionamiento, ¿cómo relleno la clave de particionamiento para la información de esquema?

El objeto de información del esquema solo se usa en escenarios de combinación o división. Si una aplicación es inherentemente de un solo inquilino, no requiere la herramienta de combinación o división y, por lo tanto, no es necesario rellenar el objeto de información del esquema.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>He aprovisionado una base de datos y ya tengo un Administrador de asignación de particiones, ¿cómo registro esta nueva base de datos como una partición?

Consulte **[Incorporación de una partición a una aplicación mediante la biblioteca de cliente de Elastic Database](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>¿Cuánto cuestan las herramientas de Base de datos elástica?

El uso de la biblioteca cliente de Base de datos elástica no incurre en costos. Los costos se acumulan solo para las bases de datos SQL de Azure que usa para particiones y el Administrador de asignación de particiones, así como los roles web/de trabajo aprovisionados para la herramienta de combinación o división.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>¿Por qué mis credenciales no funcionan cuando agrego una partición de un servidor diferente?

No use credenciales con el formato "Id. de usuario username@servername"; en su lugar, simplemente use "Id. de Usuario=nombre de usuario".  Además, asegúrese de que el inicio de sesión de "nombre de usuario" tiene permisos en la partición.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>¿Necesito crear un Administrador de asignación de particiones y rellenar las particiones cada vez que inicie las aplicaciones?

No, la creación del Administrador de asignación de particiones (por ejemplo, **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) es una operación única.  La aplicación debe usar la llamada **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** en el momento que se inicie la aplicación.  Solo debería haber una de estas llamadas por dominio de aplicación.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Tengo preguntas acerca del uso de las herramientas de Base de datos elástica, ¿cómo puedo obtener ayuda?

Póngase en contacto con nosotros a través del [foro de Azure SQL Database](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Al obtener una conexión de base de datos mediante una clave de particionamiento, puedo consultar los datos de otras claves de particionamiento en la misma partición.  ¿Esto es así por defecto?

Las API de Escalado elástico ofrecen una conexión a la base de datos correcta para su clave de particionamiento, pero no proporcionan filtrado de claves de particionamiento.  Agregue cláusulas **WHERE** a su consulta para restringir el ámbito a la clave de particionamiento proporcionada, si es necesario.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>¿Puedo usar una edición diferente de Base de datos de Azure para cada partición en mi conjunto de particiones?

Sí, una partición es una base de datos individual y, por lo tanto, una partición podría ser una edición Premium y otra una edición Standard. Además, la edición de una partición puede escalar verticalmente o reducirse verticalmente varias veces durante la duración de la partición.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>¿La herramienta de combinación o división aprovisiona (o elimina) una base de datos durante una operación de combinación o división?

No. En el caso de las operaciones de **división** , la base de datos de destino debe existir con el esquema apropiado y registrarse con el Administrador de asignación de particiones.  En el caso de las operaciones de **combinación** , debe eliminar la partición desde el Administrador de asignación de particiones y, luego, eliminar la base de datos.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]