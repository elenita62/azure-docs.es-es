---
title: Aprovisionamiento de dispositivos Windows para la supervisión remota con C - Azure | Microsoft Docs
description: Se describe cómo conectar un dispositivo al acelerador de la solución de supervisión remota mediante una aplicación creada en C y que se ejecuta en Windows.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: dobett
ms.openlocfilehash: 55c8ff799ba3ff7fe9691d46dc90a00d5182d390
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2018
ms.locfileid: "48829417"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-windows"></a>Conectar el dispositivo al acelerador de la solución Supervisión remota (Windows)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

En este tutorial se muestra cómo conectar un dispositivo físico al acelerador de la solución de supervisión remota.

Como sucede con la mayoría de aplicaciones insertadas que se ejecutan en dispositivos restringidos, el código cliente para la aplicación del dispositivo está escrito en C. En este tutorial, se crea la aplicación cliente del dispositivo en una máquina Windows.

## <a name="prerequisites"></a>Requisitos previos

Para completar los pasos descritos en esta guía paso a paso, siga los pasos indicados en [Configuración del entorno de desarrollo de Windows](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#set-up-a-windows-development-environment) para agregar las bibliotecas y las herramientas de desarrollo necesarias a su máquina Windows.

## <a name="view-the-code"></a>Ver el código

El [código de ejemplo](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) que se usa en esta guía está disponible en el repositorio de GitHub de los SDK de C para Azure IoT.

### <a name="download-the-source-code-and-prepare-the-project"></a>Descarga del código fuente y preparación del proyecto

Para preparar el proyecto, clone o descargue el [repositorio de SDK de C para Azure IoT](https://github.com/Azure/azure-iot-sdk-c) en GitHub.

El ejemplo se encuentra en la carpeta **samples/solutions/remote_monitoring_client**.

Abra el archivo **remote_monitoring.c** de la carpeta **samples/solutions/remote_monitoring_client** en un editor de texto.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Edite el archivo **remote_monitoring.c** para reemplazar `<connectionstring>` por la cadena de conexión de dispositivo que ha anotado al principio de esta guía paso a paso al agregar un dispositivo al acelerador de soluciones.

1. Siga los pasos de la sección [Build the C SDK in Windows](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#build-the-c-sdk-in-windows) (Compilar el SDK de C en Windows) para compilar el SDK y la aplicación cliente de supervisión remota.

1. En el símbolo del sistema utilizado para compilar la solución, ejecute:

    ```cmd
    samples\solutions\remote_monitoring_client\Release\remote_monitoring_client.exe
    ```

    La consola muestra mensajes como:

    - La aplicación envía datos de telemetría de ejemplo al acelerador de la solución.
    - Responde a los métodos invocados desde el panel de la solución.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
