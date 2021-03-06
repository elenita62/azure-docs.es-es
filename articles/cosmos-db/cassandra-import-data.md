---
title: Migración de los datos a una cuenta de Cassandra API de Azure Cosmos DB
description: Obtenga información sobre cómo usar el comando Copy de CQL y Spark para copiar datos de Apache Cassandra a la Cassandra API de Azure Cosmos DB.
services: cosmos-db
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.author: govindk
ms.topic: tutorial
ms.date: 09/24/2018
ms.reviewer: sngun
ms.openlocfilehash: f73a201a25bb2f975e8a261a6c21aa7b066c3a7c
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48247857"
---
# <a name="migrate-your-data-to-azure-cosmos-db-cassandra-api-account"></a>Migración de los datos a una cuenta de Cassandra API de Azure Cosmos DB

Este tutorial proporciona instrucciones sobre cómo migrar datos de Apache Cassandra a la Cassandra API de Azure Cosmos DB. 

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Planear la migración
> * Requisitos previos para la migración
> * Migrar datos mediante el comando COPY de cqlsh
> * Migrar datos con Spark 

## <a name="plan-for-migration"></a>Planear la migración

Antes de migrar datos a la Cassandra API de Azure Cosmos DB, debería calcular las necesidades de rendimiento de la carga de trabajo. En general, se recomienda comenzar con el rendimiento medio requerido por las operaciones CRUD y después incluir el rendimiento adicional necesario para las operaciones de picos o Extraer carga de transformación (ETL). Necesita los siguientes detalles para planear la migración: 

* **Tamaño de los datos existentes o tamaño de datos estimado:** define el requisito de tamaño y el rendimiento de base de datos mínimo. Si realiza la estimación de tamaño de los datos para una nueva aplicación, puede asumir que los datos se distribuyen uniformemente entre las filas y calcular el valor multiplicando con el tamaño de los datos. 

* **Rendimiento necesario:** tasa de rendimiento de lectura (consultar/obtener) y escritura (actualizar/eliminar/insertar) aproximada. Este valor es necesario para calcular las unidades de solicitud necesarios junto con el tamaño de datos de estado estable.  

* **Obtener el esquema:** conéctese a su clúster de Cassandra existente mediante cqlsh y exporte el esquema de Cassandra: 

  ```bash
  cqlsh [IP] "-e DESC SCHEMA" > orig_schema.cql
  ```

Después de identificar los requisitos de carga de trabajo existentes, debe crear una cuenta de Azure Cosmos DB, base de datos y contenedores según los requisitos de rendimiento recopilados.  

* **Determinar la carga de unidad de solicitud para una operación:** puede determinar las unidades de solicitud utilizando el SDK de Cassandra API de Azure Cosmos DB que prefiera. En este ejemplo se muestra la versión de .NET de la obtención de cargos de la unidad de solicitud.

  ```csharp
  var tableInsertStatement = table.Insert(sampleEntity);
  var insertResult = await tableInsertStatement.ExecuteAsync();

  foreach (string key in insertResult.Info.IncomingPayload)
    {
       byte[] valueInBytes = customPayload[key];
       string value = Encoding.UTF8.GetString(valueInBytes);
       Console.WriteLine($"CustomPayload:  {key}: {value}");
    }
  ```

* **Asignar el rendimiento necesario:** Azure Cosmos DB puede escalar automáticamente el almacenamiento y rendimiento a medida que crecen sus necesidades. Puede estimar sus necesidades de rendimiento mediante el uso de la [Calculadora de unidades de solicitud de Azure Cosmos DB](https://www.documentdb.com/capacityplanner). 

## <a name="prerequisites-for-migration"></a>Requisitos previos para la migración

* **Crear tablas en la cuenta de Cassandra API de Azure Cosmos DB:** antes de comenzar la migración de datos, cree previamente todas las tablas de Azure Portal o cqlsh. Si va a migrar a una cuenta de Azure Cosmos DB que tiene un rendimiento de nivel de base de datos, asegúrese de proporcionar una clave de partición al crear los contenedores de Azure Cosmos DB.

* **Aumentar el rendimiento:** la duración de la migración de datos depende de la cantidad de rendimiento aprovisionado para las tablas de Azure Cosmos DB. Aumente el rendimiento de la duración de la migración. Gracias al mayor rendimiento, puede evitar la limitación de velocidad y realizar la migración en menos tiempo. Después de haber completado la migración, reduzca el rendimiento para ahorrar costos. Para obtener más información sobre cómo aumentar el rendimiento, vea [configurar rendimiento](set-throughput.md) para contenedores de Azure Cosmos DB. También se recomienda tener la cuenta de Azure Cosmos DB en la misma región que la base de datos de origen. 

* **Habilitar SSL:** Azure Cosmos DB tiene estándares y requisitos de seguridad estrictos. No olvide habilitar SSL al interactuar con la cuenta. Al usar CQL con SSH, tiene una opción para proporcionar información de SSL.

## <a name="options-to-migrate-data"></a>Opciones para migrar datos

Puede mover los datos de cargas de trabajo existentes de Cassandra para Azure Cosmos DB mediante el uso de las siguientes opciones:

* [Usar el comando COPY de cqlsh](#using-cqlsh-copy-command)  
* [Usar Spark](#using-spark) 

## <a name="migrate-data-using-cqlsh-copy-command"></a>Migrar datos mediante el comando COPY de cqlsh

[El comando COPY de CQL](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) se usa para copiar datos locales a la cuenta de Cassandra API de Azure Cosmos DB. Use los siguientes pasos para copiar datos:

1. Obtenga la información de cadena de conexión de su cuenta de Cassandra API:

   * Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a la cuenta de Azure Cosmos DB.

   * Abra el panel **Cadena de conexión** que contenga toda la información que necesite para conectarse a su cuenta de API de Cassandra desde cqlsh.

2. Inicie sesión en cqlsh mediante la información de conexión desde el portal.

3. Use el comando COPY de CQL para copiar datos locales a la cuenta de API de Cassandra.

   ```bash
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="migrate-data-using-spark"></a>Migrar datos con Spark 

Siga estos pasos para migrar datos a la API de Cassandra de Azure Cosmos DB con Spark:

- Aprovisione un [Azure Databricks](cassandra-spark-databricks.md) o un [Clúster de HDInsight](cassandra-spark-hdinsight.md) 

- Mover datos al punto de conexión de Cassandra API de destino mediante la [operación de copia de tabla](cassandra-spark-table-copy-ops.md) 

La migración de datos mediante el uso de trabajos de spark es una opción recomendada si tiene datos que residen en un clúster existente en máquinas virtuales de Azure o cualquier otra nube. Esto requiere que spark se configure como intermediario para la ingesta única o regular. Puede acelerar la migración mediante el uso de conectividad de expressroute entre local y Azure. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo migrar los datos a la cuenta de Cassandra API de Azure Cosmos DB. Ahora puede continuar con la sección de conceptos para obtener más información sobre Azure Cosmos DB. 

> [!div class="nextstepaction"]
> [Niveles de coherencia de datos optimizables en Azure Cosmos DB](../cosmos-db/consistency-levels.md)


