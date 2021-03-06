---
title: Planeamiento de la capacidad y el escalado para la replicación de VMware en Azure con Azure Site Recovery | Microsoft Docs
description: Use este artículo para planear la capacidad y escala cuando replique VM de VMware en Azure con Azure Site Recovery.
services: site-recovery
author: nsoneji
manager: garavd
ms.service: site-recovery
ms.date: 10/10/2018
ms.topic: conceptual
ms.author: rayne
ms.openlocfilehash: 833000cb09d315a996ebc9e2d66210c805160f2a
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067561"
---
# <a name="plan-capacity-and-scaling-for-vmware-replication-with-azure-site-recovery"></a>Planeamiento de la capacidad y el escalado para la replicación de VMware con Azure Site Recovery

Use este artículo para averiguar cómo planear la capacidad y el escalado cuando se replican VM de VMware locales y servidores físicos en Azure con [Azure Site Recovery](site-recovery-overview.md).

## <a name="how-do-i-start-capacity-planning"></a>¿Cómo se puede iniciar el planeamiento de la capacidad?

Recopile información sobre el entorno de replicación ejecutando [Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner-doc) para la replicación de VMware. [Más información](site-recovery-deployment-planner.md) sobre esta herramienta. Podrá recopilar información sobre máquinas virtuales compatibles e incompatibles, discos por máquina virtual y la actividad de datos por disco. Esta herramienta también se encarga de los requisitos de ancho de banda de red y de la infraestructura de Azure necesaria para una replicación y conmutación por error de prueba correctas.

## <a name="capacity-considerations"></a>Consideraciones de capacidad

**Componente** | **Detalles** |
--- | --- | ---
**Replicación** | **Tasa máxima de cambios diaria**: una máquina protegida solo puede usar un servidor de procesos, y un servidor de procesos puede gestionar una tasa de cambios diaria de hasta 2 TB. Por lo tanto, la tasa máxima de cambios diaria que admite una máquina protegida es de 2 TB.<br/><br/> **Rendimiento máximo**: una máquina replicada puede pertenecer a una cuenta de almacenamiento en Azure. Una cuenta de almacenamiento estándar puede controlar un máximo de 20 000 solicitudes por segundo y se recomienda mantener la cantidad de operaciones de entrada/salida por segundo (IOPS) en una máquina de origen en 20 000. Por ejemplo, si tiene una máquina de origen con 5 discos y cada disco genera 120 E/S por segundo (8 K de tamaño) en la máquina de origen, se encontrará dentro del límite de 500 de Azure por E/S por segundo por disco. (El número de cuentas de almacenamiento necesario es igual al IOPS de máquina de origen total dividido por 20 000).
**Servidor de configuración** | El servidor de configuración debe poder controlar la capacidad de tasa de cambios diaria en todas las cargas de trabajo que se ejecutan en máquinas protegidas, y necesita el ancho de banda suficiente para realizar una replicación continua de los datos en el Azure Storage.<br/><br/> Como procedimiento recomendado, localice el servidor de configuración en la misma red y mismo segmento de LAN que las máquinas que desea proteger. De todos modos, puede estar en una red distinta, pero las máquinas que desea proteger deben contar con la visibilidad de red de nivel L3 en ella.<br/><br/> En la tabla de la siguiente sección se resumen las recomendaciones de tamaño para el servidor de configuración.
**Servidor de proceso** | El primer servidor de procesos está instalado en el servidor de configuración de forma predeterminada. Puede implementar servidores de procesos adicionales para escalar el entorno. <br/><br/> El servidor de procesos recibe datos de replicación provenientes de las máquinas protegidas y los optimiza con almacenamiento en caché, compresión y cifrado. A continuación, envía los datos a Azure. La máquina del servidor de procesos debe tener los recursos suficientes para realizar estas tareas.<br/><br/> El servidor de procesos utiliza una caché basada en disco. Utilice un disco de caché independiente con 600 GB o más de capacidad para controlar los cambios en los datos almacenados ante la eventualidad de una interrupción o un cuello de botella en la red.

## <a name="size-recommendations-for-the-configuration-server"></a>Recomendaciones de tamaño del servidor de configuración

**CPU** | **Memoria** | **Tamaño del disco de caché** | **Frecuencia de cambio de datos** | **Máquinas protegidas**
--- | --- | --- | --- | ---
8 vCPU (2 sockets * 4 núcleos \@ 2,5 gigahercios [GHz]) | 16 GB | < 300 GB | 500 GB o menos | Replicar menos de 100 máquinas.
12 vCPU (2 sockets * 6 núcleos \@ 2,5 GHz) | 18 GB | 600 GB | 500 GB a 1 TB | Replicar entre 100 y 150 máquinas.
16 vCPU (2 sockets * 8 núcleos \@ 2,5 GHz) | 32 GB | 1 TB | 1 TB a 2 TB | Replicar entre 150 y 200 máquinas.
Implementar otro servidor de procesos | | | 2 TB | Implemente servidores de procesos adicionales si replica más de 200 máquinas o si la tasa de cambios de datos diaria supera los 2 TB.

Donde:

* Cada máquina de origen está configurada con 3 discos de 100 GB cada una.
* Usamos almacenamiento de pruebas comparativas de 8 unidades SAS de 10 K RPM con RAID 10 para las mediciones de disco de caché.

## <a name="size-recommendations-for-the-process-server"></a>Recomendaciones de tamaño para el servidor de procesos

Si necesita proteger más de 200 máquinas, o si la tasa de cambios diaria es mayor que 2 TB, puede agregar servidores de procesos para controlar la carga de replicación. Para escalar horizontalmente, puede:

* Aumentar el número de servidores de configuración. Por ejemplo, puede proteger hasta 400 máquinas con dos servidores de configuración.
* Agregar más servidores de procesos y utilizarlos para controlar el tráfico en lugar (o además) del servidor de configuración.

La tabla siguiente describe un escenario en el cual:

* No pretende utilizar el servidor de configuración como un servidor de procesos.
* Ha configurado un servidor de procesos adicional.
* Ha configurado máquinas virtuales protegidas para utilizar el servidor de procesos adicional.
* Cada máquina de origen protegida está configurada con tres discos de 100 GB cada uno.

**Servidor de configuración** | **Servidores de procesos adicionales** | **Tamaño del disco de caché** | **Frecuencia de cambio de datos** | **Máquinas protegidas**
--- | --- | --- | --- | ---
8 vCPU (2 sockets * 4 núcleos \@ 2,5 GHz), 16 GB de memoria | 4 vCPU (2 sockets * 2 núcleos \@ 2,5 GHz), 8 GB de memoria | < 300 GB | 250 GB o menos | Replicar 85 máquinas o menos.
8 vCPU (2 sockets * 4 núcleos \@ 2,5 GHz), 16 GB de memoria | 8 vCPU (2 sockets * 4 núcleos \@ 2,5 GHz), 12 GB de memoria | 600 GB | 250 GB a 1 TB | Replicar entre 85 y 150 máquinas.
12 vCPU (2 sockets * 6 núcleos \@ 2,5 GHz), 18 GB de memoria | 12 vCPU (2 sockets * 6 núcleos \@ 2,5 GHz), 24 GB de memoria | 1 TB | 1 TB a 2 TB | Replicar entre 150 y 225 máquinas.

La manera en la que escalará los servidores depende de su preferencia con respecto a un modelo de escalado vertical u horizontal.  Puede escalar verticalmente con la implementación de algunos servidores de procesos y de configuración de alto nivel, mientras que, para escalar horizontalmente, debe implementar más servidores con menos recursos. Por ejemplo, si necesita proteger 220 máquinas, podría elegir una de las siguientes opciones:

* Configurar el servidor de configuración con 12 vCPU, 18 GB de memoria y un servidor de procesos adicional con 12 vCPU, 24 GB de memoria. Configurar máquinas protegidas para que utilicen solo el servidor de procesos adicional.
* Configurar dos servidores de configuración (2 x 8 vCPU, 16 GB de RAM) y dos servidores de procesos adicionales (1 x 8 vCPU y 4 vCPU x 1 para controlar 135 + 85 [220] máquinas). Configurar máquinas protegidas para que utilicen solo servidores de procesos adicionales.


## <a name="control-network-bandwidth"></a>Ancho de banda de red de control

Después de utilizar la [herramienta de planeamiento de implementación](site-recovery-deployment-planner.md) para calcular el ancho de banda necesario para la replicación (la replicación inicial y la delta), puede controlar la cantidad de ancho de banda utilizado para la replicación mediante un par de opciones:

* **Limitar ancho de banda**: el tráfico de VMware que se replica en Azure pasa a través de un servidor de procesos específico. También puede limitar el ancho de banda en las máquinas que se ejecutan como servidores de procesos.
* **Influir en el ancho de banda**: puede influir en el ancho de banda utilizado para la replicación mediante un par de claves del Registro:
  * El valor del Registro **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM** especifica el número de subprocesos que se utilizan para la transferencia de datos (replicación inicial o diferencial) de un disco. Un valor mayor aumenta el ancho de banda de red utilizado para la replicación.
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\DownloadThreadsPerVM** especifica el número de subprocesos usados para la transferencia de datos durante la conmutación por recuperación.

### <a name="throttle-bandwidth"></a>Limitar el ancho de banda

1. Abra el complemento MMC de Azure Backup en la máquina que actúa como el servidor de procesos. De manera predeterminada, hay disponible un acceso directo para Backup en el escritorio o en la siguiente carpeta: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. En el complemento, haga clic en **Cambiar propiedades**.

    ![Captura de pantalla de la opción de complemento MMC de Azure Backup para cambiar las propiedades](./media/site-recovery-vmware-to-azure/throttle1.png)
3. En la pestaña **Limitación**, seleccione la opción **Habilitar el límite de uso del ancho de banda de Internet para operaciones de copia de seguridad**. Establezca los límites para las horas laborables y no laborables. Los intervalos válidos van de 512 Kbps a 1023 Mbps por segundo.

    ![Captura de pantalla del cuadro de diálogo de propiedades de Azure Backup](./media/site-recovery-vmware-to-azure/throttle2.png)

También puede utilizar el cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) para establecer la limitación. Aquí tiene un ejemplo:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** indica que no se requiere ninguna limitación.

### <a name="influence-network-bandwidth-for-a-vm"></a>Control del uso de ancho de banda de red para una VM

1. En el Registro de la VM, vaya a **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Para influir en el tráfico de ancho de banda en un disco de replicación, modifique el valor de **UploadThreadsPerVM** o cree la clave en caso de que no exista.
   * Para influir en el ancho de banda para el tráfico de conmutación por recuperación de Azure, modifique el valor de **DownloadThreadsPerVM**.
2. El valor predeterminado es 4. En una red "sobreaprovisionada", se deben cambiar los valores predeterminados de estas claves de registro. El valor máximo es 32. Supervise el tráfico para optimizar el valor.


## <a name="deploy-additional-process-servers"></a>Implementar servidores de procesos adicionales

Si debe escalar horizontalmente la implementación a más de 200 máquinas de origen o si la tasa de renovación diaria total supera los 2 TB, necesitará servidores de procesos adicionales para controlar el volumen del tráfico. Siga las instrucciones de [este artículo](vmware-azure-set-up-process-server-scale.md) para configurar el servidor de procesos. Después de configurar el servidor, puede migrar las máquinas de origen para que lo usen.

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrar máquinas para utilizar el nuevo servidor de procesos

1. En **Configuración** > **Site Recovery servers** (Servidores de Site Recovery), haga clic en el servidor de configuración y expanda **Servidores de procesos**.

    ![Captura de pantalla del cuadro de diálogo Servidor de procesos](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. Haga clic con el botón derecho en el servidor de procesos que se utiliza actualmente y haga clic en **Cambiar**.

    ![Captura de pantalla del cuadro de diálogo Servidor de configuración](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. En **Seleccionar servidor de procesos de destino**, elija el nuevo servidor de procesos que desee usar y elija las máquinas virtuales que controlará el servidor. Haga clic en el icono de información para obtener detalles sobre el servidor. Aparece el espacio promedio que se necesita para replicar cada máquina virtual seleccionada en el nuevo servidor de procesos para ayudarlo a tomar decisiones relacionadas con la carga. Haga clic en la marca de verificación para comenzar a replicar en el nuevo servidor de procesos.

## <a name="deploy-additional-master-target-servers"></a>Implementar servidores de destino maestros adicionales

Necesitará un servidor de destino maestro adicional en los escenarios siguientes:

1. Si está intentando proteger una máquina virtual basada en Linux.
2. Si el servidor de destino maestro disponible en el servidor de configuración no tiene acceso al almacén de datos de la máquina virtual.
3. Si el número total de discos en el servidor de destino maestro (el número de discos locales en el servidor + los discos que se van a proteger) supera los 60 discos.

Para agregar un servidor de destino maestro nuevo a una **máquina virtual basada en Linux**, [haga clic aquí](vmware-azure-install-linux-master-target.md).

Para una **máquina virtual basada en Windows**, siga las instrucciones siguientes.

1. Vaya a **Almacén de Recovery Services** > **Infraestructura de Site Recovery** > **Servidores de configuración**.
2. Haga clic en el servidor de configuración necesario > **+Servidor de destino maestro**.![add-master-target-server.png](media/site-recovery-plan-capacity-vmware/add-master-target-server.png)
3. Descargue la instalación unificada y ejecútela en la máquina virtual para configurar el servidor de destino maestro.
4. Haga clic en **Instalación del destino principal** > **Siguiente**. ![choose-MT.PNG](media/site-recovery-plan-capacity-vmware/choose-MT.PNG)
5. Elija la ubicación de instalación predeterminada > haga clic en **Instalar**. ![MT-installation](media/site-recovery-plan-capacity-vmware/MT-installation.PNG)
6. Haga clic en **Continuar con la configuración** para registrar el destino maestro con el servidor de configuración. ![MT-proceed-configuration.PNG](media/site-recovery-plan-capacity-vmware/MT-proceed-configuration.PNG)
7. Escriba la dirección IP del servidor de configuración y la frase de contraseña. [Haga clic aquí](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) para obtener información sobre cómo generar la frase de contraseña.![cs-ip-passphrase](media/site-recovery-plan-capacity-vmware/cs-ip-passphrase.PNG)
8. Haga clic en **Registrar** y, después del registro, haga clic en **Finalizar**.
9. Después de registrarse correctamente, este servidor aparece en el portal en **Almacén de Recovery Services** > **Infraestructura de Site Recovery** > **Servidores de configuración** > servidores de destino maestros del servidor de configuración relevante.

 >[!NOTE]
 >También puede descargar la versión más reciente de la instalación unificada del servidor de destino maestro para Windows [aquí](https://aka.ms/latestmobsvc).

## <a name="next-steps"></a>Pasos siguientes

Descargue y ejecute [Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner).
