---
title: Espacio de nombres jerárquico de Azure Data Lake Storage Gen2 (versión preliminar)
description: Describe el concepto del espacio de nombres jerárquico de Azure Data Lake Storage Gen2 (versión preliminar).
services: storage
author: jamesbak
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 44eec21f4687d2df64c59d41cdb02c6ef2268f82
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39528704"
---
# <a name="azure-data-lake-storage-gen2-preview-hierarchical-namespace"></a>Espacio de nombres jerárquico de Azure Data Lake Storage Gen2 (versión preliminar)

La adición de un **espacio de nombres jerárquico** es un mecanismo clave que permite a Azure Data Lake Storage Gen2 (versión preliminar) proporcionar rendimiento para el sistema de archivos a precios y escala de almacenamiento de objetos. Esto permite que la colección de objetos o archivos dentro de una cuenta esté organizada en una jerarquía de directorios y subdirectorios anidados de la misma manera en que se organiza el sistema de archivos en el equipo. Con el espacio de nombres jerárquico habilitado, Data Lake Storage Gen2 proporciona la escalabilidad y la rentabilidad del almacenamiento de objetos, con una semántica de sistema de archivos con la que los marcos y motores de análisis están familiarizados.

## <a name="the-benefits-of-the-hierarchical-namespace"></a>Ventajas del espacio de nombres jerárquico

> [!NOTE]
> Durante la versión preliminar pública de Azure Data Lake Storage Gen2, puede variar la disponibilidad de algunas de las características enumeradas a continuación. Como las regiones y características nuevas se lanzan durante el programa de versión preliminar, se comunicará esta información a través de nuestro grupo dedicado de Yammer.  

Las siguientes ventajas están asociadas a los sistemas de archivos que implementan un espacio de nombres jerárquico sobre los datos de blob:

- **Manipulación atómica de directorios:** los almacenamientos de objetos se aproximan a una jerarquía de directorios mediante la adopción de una convención de inserción de barras diagonales (/) en el nombre del objeto para indicar los segmentos de trazado. Aunque esta convención funciona para la organización de objetos, no proporciona asistencia para acciones como mover o eliminar directorios, ni para cambiarles el nombre. Sin directorios reales, las aplicaciones podrían tener que procesar millones de blobs individuales para lograr realizar las tareas de nivel de directorio. Por el contrario, el espacio de nombres jerárquico procesa estas tareas mediante la actualización de una sola entrada (el directorio principal). 

    Esta notable optimización es especialmente significativa para muchos marcos de análisis de macrodatos. Con frecuencia, herramientas como Hive, Spark, etc. escriben la salida en ubicaciones temporales y, a continuación, cambian el nombre de la ubicación al finalizar el trabajo. Sin el espacio de nombres jerárquico, muchas veces este cambio de nombre puede tardar más tiempo que el propio proceso de análisis. La disminución de la latencia de trabajo equivale a la reducción del costo total de propiedad (TCO) de las cargas de trabajo de análisis.

- **Estilo de interfaz familiar:** tanto los desarrolladores como los usuarios comprenden correctamente los sistemas de archivos. Al moverse a la nube, no es necesario aprender un nuevo paradigma de almacenamiento, ya que la interfaz del sistema de archivo que presenta Data Lake Storage Gen2 es el mismo paradigma que usan los equipos, grandes y pequeños.

Uno de los motivos por los que históricamente los almacenamientos de objetos no admitían espacios de nombres jerárquicos es porque estos limitaban la escala. Sin embargo, el espacio de nombres jerárquico de Data Lake Storage Gen2 se escala linealmente y no degrada la capacidad de datos ni el rendimiento.

## <a name="when-to-enable-the-hierarchical-namespace"></a>Cuándo habilitar el espacio de nombres jerárquico

Se recomienda activar el espacio de nombres jerárquico para las cargas de trabajo de almacenamiento diseñadas para sistemas de archivos que manipulan directorios. Esto incluye todas las cargas de trabajo que son principalmente para el procesamiento de análisis. Los conjuntos de datos que requieren un alto grado de organización también se beneficiarán de la habilitación del espacio de nombres jerárquico.

Las razones para habilitar el espacio de nombres jerárquico las determina un análisis de TCO. Por lo general, las mejoras en la latencia de carga de trabajo debido a la aceleración de almacenamiento requerirán recursos de proceso durante menos tiempo. La latencia de muchas cargas de trabajo se puede mejorar gracias a la manipulación atómica de directorios que habilita el espacio de nombres jerárquico. En muchas cargas de trabajo, el recurso de proceso representa más del 85 % del costo total e incluso una reducción modesta de latencia de carga de trabajo equivale a una cantidad significativa de los ahorros de TCO. Incluso en los casos en los que al habilitar el espacio de nombres jerárquico aumentan los costos de almacenamiento, el TCO aún disminuye debido a los costos de proceso reducidos.

## <a name="when-to-disable-the-hierarchical-namespace"></a>Cuándo deshabilitar el espacio de nombres jerárquico

Algunas cargas de trabajo de almacenamiento de objetos puede que no obtengan ninguna ventaja al habilitar el espacio de nombres jerárquico. Algunos ejemplos de estas cargas de trabajo incluyen las copias de seguridad, el almacenamiento de imágenes y otras aplicaciones donde la organización de objetos y los propios objetos se almacenan por separado (*p. ej.*, en una base de datos independiente).

> [!NOTE]
> Con la versión preliminar, si habilita el espacio de nombres jerárquico, no hay ninguna interoperabilidad de datos u operaciones entre las API REST de blob y Data Lake Storage Gen2. Esta funcionalidad se agregará durante la versión preliminar.

## <a name="next-steps"></a>Pasos siguientes

- [Crear una cuenta de almacenamiento](./quickstart-create-account.md)