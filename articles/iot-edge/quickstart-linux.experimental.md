---
title: Simulación de Azure IoT Edge en Linux | Microsoft Docs
description: En esta guía de inicio rápido, aprenda a implementar código creado previamente de manera remota en un dispositivo IoT Edge.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/02/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: dfbe931bbe5887e9c0545558c4d2b2565718dd0a
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578497"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Guía de inicio rápido: Implementación del primer módulo de IoT Edge en un dispositivo Linux x64

Azure IoT Edge le permite realizar análisis y procesamiento de datos en los dispositivos, en lugar de tener que insertar todos los datos en la nube. En los tutoriales de IoT Edge se muestra cómo implementar diferentes tipos de módulos, pero primero se necesita un dispositivo para probarlo. 

En esta guía de inicio rápido, aprenderá a hacer lo siguiente:

1. Cree un centro de IoT Hub.
2. Registre un dispositivo IoT Edge en su instancia de IoT Hub.
3. Iniciar el entorno de ejecución de IoT Edge.
4. Implemente un módulo de manera remota en un dispositivo IoT Edge.

![Arquitectura del tutorial][2]

Esta guía de inicio rápido convierte su equipo o maquina virtual Linux en un dispositivo IoT Edge. Después, puede implementar un módulo desde Azure Portal en el dispositivo. El módulo que se implementa en esta guía de inicio rápido es un sensor simulado que genera datos de temperatura, humedad y presión. Los otros tutoriales de Azure IoT Edge se basan en el trabajo que se realiza aquí mediante la implementación de módulos que analizan los datos simulados para obtener información empresarial. 

Si no tiene una suscripción activa a Azure, cree una [cuenta gratuita][lnk-account] antes de comenzar.

## <a name="prerequisites"></a>Requisitos previos

En esta guía de inicio rápido se usa una máquina Linux como dispositivo IoT Edge. Si no tiene uno disponible para probar, siga las instrucciones de [Creación de una máquina virtual Linux en Azure Portal](../virtual-machines/linux/quick-create-portal.md). 
* No tiene que seguir los pasos para instalar y ejecutar el servidor web. Una vez conectado a la máquina virtual, puede detenerse.  
* Cree la máquina virtual en un nuevo grupo de recursos, que puede usar cuando cree el resto de los recursos de Azure para esta guía de inicio rápido. Asígnele un nombre reconocible, como *IoTEdgeResources*. 
* No necesita una máquina virtual muy grande para probar IoT Edge. Un tamaño como **B1ms** es suficiente. 

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

Para comenzar la guía de inicio rápido, cree su instancia de IoT Hub en Azure Portal.
![Creación de una instancia de IoT Hub][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Registro de un dispositivo de IoT Edge

Registre un dispositivo de IoT Edge con la instancia de IoT Hub recién creada.
![Registro de un dispositivo][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]


## <a name="install-and-start-the-iot-edge-runtime"></a>Instale e inicie el runtime de IoT Edge

Instale e inicie el runtime de Azure IoT Edge en el dispositivo. 
![Registro de un dispositivo][5]

El runtime de IoT Edge se implementa en todos los dispositivos de IoT Edge. Tiene tres componentes. El **demonio de seguridad de IoT Edge** se inicia cada vez que se inicia un dispositivo perimetral y arranca el dispositivo mediante el inicio del agente de IoT Edge. El **agente de IoT Edge** facilita la implementación y supervisión de los módulos en el dispositivo IoT Edge, incluido el centro de IoT Edge. El **centro de IoT Edge** administra las comunicaciones entre los módulos del dispositivo de IoT Edge y entre el dispositivo y la instancia de IoT Hub. 

Complete los siguientes pasos en la máquina Linux o máquina virtual que preparó para esta guía de inicio rápido. 

### <a name="register-your-device-to-use-the-software-repository"></a>Registro del dispositivo para que use el repositorio de software

Los paquetes necesarios para ejecutar el entorno de ejecución de Azure IoT Edge se administran en un repositorio de software. Configure el dispositivo IoT Edge para acceder a este repositorio. 

Los pasos de esta sección son para dispositivos que ejecutan **Ubuntu 16.04**. Para acceder al repositorio de software en otras versiones de Linux, consulte [Install the Azure IoT Edge runtime on Linux (x64)](how-to-install-iot-edge-linux.md) [Instalación del entorno de ejecución de Azure IoT Edge en Linux (x64)] o [Install Azure IoT Edge runtime on Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) [Instalación del entorno de ejecución de Azure IoT Edge en Linux (ARM32v7/armhf)].

1. En la máquina que está utilizando como dispositivo IoT Edge, instale la configuración del repositorio.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

2. Instale una clave pública para acceder al repositorio.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-a-container-runtime"></a>Instalación de un entorno de ejecución del contenedor

El entorno de ejecución de IoT Edge es un conjunto de contenedores y la lógica que implementa en el dispositivo IoT Edge se empaqueta en forma de contenedores. Prepare el dispositivo para estos componentes mediante la instalación de un entorno de ejecución del contenedor.

Actualice **apt-get**.

   ```bash
   sudo apt-get update
   ```

Instale **Moby**, un entorno de ejecución de contenedor.

   ```bash
   sudo apt-get install moby-engine
   ```

Instale los comandos de la CLI para Moby. 

   ```bash
   sudo apt-get install moby-cli
   ```

### <a name="install-and-configure-the-iot-edge-security-daemon"></a>Instalación y configuración del demonio de seguridad de IoT Edge

El demonio de seguridad se instala como un servicio del sistema para que el entorno de ejecución de IoT Edge se inicie cada vez que arranca el dispositivo. La instalación también incluye una versión de **hsmlib** que permite que el demonio de seguridad interactúe con la seguridad del hardware del dispositivo. 

1. Descargue e instale el demonio de seguridad de IoT Edge. 

   ```bash
   sudo apt-get update
   sudo apt-get install iotedge
   ```

2. Abra el archivo de configuración de IoT Edge. Es un archivo protegido por lo que puede que tenga que usar privilegios elevados para acceder a él.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. Agregue la cadena de conexión del dispositivo IoT Edge. Busque la variable **device_connection_string** y actualice su valor con la cadena que ha copiado después de registrar el dispositivo.

4. Guarde y cierre el archivo. 

   `CTRL + X`, `Y`, `Enter`

4. Reinicie el demonio de seguridad de IoT Edge.

   ```bash
   sudo systemctl restart iotedge
   ```

5. Compruebe que el demonio de seguridad de Edge se ejecuta como un servicio del sistema.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Comprobación de cómo el demonio de Edge se ejecuta con un servicio del sistema](./media/quickstart-linux/iotedged-running.png)

   También puede ver los registros del demonio de seguridad de Edge ejecutando el comando siguiente:

   ```bash
   journalctl -u iotedge
   ```

6. Vea los módulos que se ejecutan en el dispositivo. 

   >[!TIP]
   >Deberá usar *sudo* para ejecutar los comandos `iotedge` al principio. Cierre sesión en la máquina y vuelva a iniciarla para actualizar los permisos; después, puede ejecutar los comandos `iotedge` sin privilegios elevados. 

   ```bash
   sudo iotedge list
   ```

   ![Visualización de un módulo en el dispositivo](./media/quickstart-linux/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Implementación de un módulo

Administre el dispositivo Azure IoT Edge desde la nube para implementar un módulo que enviará datos de telemetría a IoT Hub.
![Registro de un dispositivo][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Visualización de datos generados

En esta guía de inicio rápido, ha creado un nuevo dispositivo de IoT Edge y ha instalado el runtime de IoT Edge en él. Luego, ha usado Azure Portal para insertar un módulo de IoT Edge para que se ejecute en el dispositivo sin tener que realizar cambios en el propio dispositivo. En este caso, el módulo que ha insertado crea datos del entorno que se pueden usar para los tutoriales. 

Vuelva a abrir el símbolo del sistema en el equipo que ejecuta el dispositivo simulado. Confirme que el módulo implementado desde la nube se está ejecutando en el dispositivo IoT Edge:

   ```bash
   sudo iotedge list
   ```

   ![Ver tres módulos en el dispositivo](./media/quickstart-linux/iotedge-list-2.png)

Vea los mensajes que se envían desde el módulo tempSensor:

  ```bash
   sudo iotedge logs tempSensor -f 
   ```

Después de un cierre de sesión y el inicio de sesión, *sudo* no es necesario para el comando anterior.

![Ver los datos desde el módulo](./media/quickstart-linux/iotedge-logs.png)

Puede que el módulo del sensor de temperatura esté esperando para conectarse al centro de IoT Edge si la última línea que se ve en el registro es `Using transport Mqtt_Tcp_Only`. Intente terminar el módulo y dejar que el agente de Edge lo reinicie. Puede terminarlo mediante el comando `sudo docker stop tempSensor`.

También puede ver los datos de telemetría que envía el dispositivo con la [extensión Azure IoT Toolkit para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea continuar con los tutoriales de IoT Edge, puede usar el dispositivo que ha registrado y configurado en esta guía de inicio rápido. De lo contrario, puede eliminar los recursos de Azure que ha creado y quitar el entorno de ejecución de Azure IoT Edge del dispositivo. 

### <a name="delete-azure-resources"></a>Eliminación de recursos de Azure

Si ha creado una máquina virtual y un centro de IoT en un nuevo grupo de recursos, puede eliminar dicho grupo y todos los recursos asociados. Si hay algo en ese grupo de recursos que desee mantener, solo hay que eliminar los recursos individuales que se quieran limpiar. 

Para quitar un grupo de recursos, siga estos pasos: 

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **Grupos de recursos**.
2. Escriba el nombre del grupo de recursos que contiene la instancia de IoT Hub en el cuadro de texto **Filtrar por nombre...**. 
3. A la derecha del grupo de recursos de la lista de resultados, haga clic en **...** y, a continuación, en **Eliminar grupo de recursos**.
4. Se le pedirá que confirme la eliminación del grupo de recursos. Escriba otra vez el nombre del grupo de recursos para confirmar y haga clic en **Eliminar**. Transcurridos unos instantes, el grupo de recursos y todos los recursos que contiene se eliminan.

### <a name="remove-the-iot-edge-runtime"></a>Eliminación del entorno de ejecución de Azure IoT Edge

Si desea quitar las instalaciones desde su dispositivo, use los siguientes comandos.  

Quite el entorno de ejecución de Azure IoT Edge.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Cuando se quita el entorno de ejecución de Azure IoT Edge, los contenedores que se han creado se detienen, pero siguen existiendo en el dispositivo. Vea todos los contenedores.

   ```bash
   sudo docker ps -a
   ```

Elimine los contenedores que se crearon en el dispositivo por el entorno de ejecución de Azure IoT Edge. Cambie el nombre del contenedor tempSensor si lo llama de otra manera. 

   ```bash
   sudo docker rm -f tempSensor
   sudo docker rm -f edgeHub
   sudo docker rm -f edgeAgent
   ```

Quite el entorno de ejecución del contenedor.

   ```bash
   sudo apt-get remove --purge moby
   ```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado un nuevo dispositivo IoT Edge y ha usado la interfaz en la nube de Azure IoT Edge para implementar el código en el dispositivo. Ahora tiene un dispositivo simulado que genera datos sin procesar acerca de su entorno. 

Esta guía de inicio rápido es el requisito previo para todos los demás tutoriales de IoT Edge. Puede continuar con cualquiera de los demás tutoriales para obtener información acerca de la forma en que Azure IoT Edge puede ayudarle a transformar estos datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Filtrado de los datos del sensor mediante una función de Azure](tutorial-deploy-function.md)


<!-- Images -->
[1]: ./media/quickstart-linux/view-module.png
[2]: ./media/quickstart-linux/install-edge-full.png
[3]: ./media/quickstart-linux/create-iot-hub.png
[4]: ./media/quickstart-linux/register-device.png
[5]: ./media/quickstart-linux/start-runtime.png
[6]: ./media/quickstart-linux/deploy-module.png
[7]: ./media/quickstart-linux/iotedged-running.png
[8]: ./media/tutorial-simulate-device-linux/running-modules.png
[9]: ./media/tutorial-simulate-device-linux/sensor-data.png

<!-- Links -->
[lnk-account]: https://azure.microsoft.com/free
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
