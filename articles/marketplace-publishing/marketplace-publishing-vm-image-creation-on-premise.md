---
title: Creación de una imagen de máquina virtual de forma local para Azure Marketplace | Microsoft Docs
description: Entienda y ejecute los pasos para crear una imagen de máquina virtual de forma local e impleméntela en Azure Marketplace para que otros usuarios la compren.
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: f68dadab96e27cc7b90f44681d87ffa7cce8126b
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49390064"
---
# <a name="develop-an-on-premises-virtual-machine-image-for-the-azure-marketplace"></a>Desarrollo de una imagen de máquina virtual de forma local para Azure Marketplace
Se recomienda encarecidamente que desarrolle discos duros virtuales de Azure (VHD) directamente en la nube mediante el uso del Protocolo de escritorio remoto. Sin embargo, si es necesario, es posible descargar un disco duro virtual y desarrollarlo mediante infraestructura local.  

Para el desarrollo local, debe descargar el disco duro virtual del sistema operativo de la máquina virtual creada. Estos pasos tendrán lugar como parte del paso 3.3, arriba.  

## <a name="download-a-vhd-image"></a>Descargar una imagen de disco duro virtual
### <a name="locate-a-blob-url"></a>Búsqueda de la dirección URL de blob
Para descargar el disco duro virtual, busque primero la dirección URL de blob del disco del sistema operativo.

Busque la dirección URL de blob en el nuevo [Portal de Microsoft Azure](https://portal.azure.com):

1. Vaya a **Examinar** > **Máquinas virtuales**, y seleccione la máquina virtual implementada.
2. En **Configurar**, seleccione el icono **Discos**, lo cual abre la hoja Discos.
   
   ![dibujo](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. Seleccione el **Disco del sistema operativo**, que abre otra hoja que muestra las propiedades del disco, incluida la ubicación del disco duro virtual.
4. Copie esta dirección URL de blob.
   
   ![dibujo](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. Ahora, elimine la máquina virtual implementada sin eliminar los discos de copia de seguridad. También puede detener la máquina virtual en lugar de eliminarla. No descargue el disco duro virtual del sistema operativo cuando se esté ejecutando la máquina virtual.
   
   ![dibujo](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a>Descarga de un disco duro virtual
Una vez que conozca la dirección URL de blob, podrá descargar el disco duro virtual mediante el [Portal de Azure](http://manage.windowsazure.com/) o PowerShell.  

> [!NOTE]
> En el momento de la creación de esta guía, la funcionalidad para descargar un disco duro virtual no está todavía presente en el nuevo Portal de Microsoft Azure.  
> 
> 

**Descarga del disco duro virtual del sistema operativo a través de la versión actual de [Azure Portal](http://manage.windowsazure.com/)**

1. Si aún no lo ha hecho, inicie sesión en el Portal de Azure.
2. Haga clic en la pestaña **Almacenamiento** .
3. Seleccione la cuenta de almacenamiento donde se almacena el disco duro virtual.
   
   ![dibujo](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. De este modo se muestran las propiedades de la cuenta de almacenamiento. Seleccione la pestaña **Contenedores** .
   
   ![dibujo](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. Seleccione el contenedor donde se almacena el disco duro virtual. De forma predeterminada, al crearse en el Portal, el disco duro virtual se almacena en un contenedor de discos duros virtuales.
   
   ![dibujo](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. Seleccione el disco duro virtual del sistema operativo correcto comparando la dirección URL con la que ha guardado.
7. Haga clic en **Descargar**.
   
   ![dibujo](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a>Descarga de un disco duro virtual mediante PowerShell
Además de utilizar el Portal de Azure, puede usar el cmdlet [Save-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) para descargar el disco duro virtual del sistema operativo.

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
Por ejemplo, Save-AzureVhd -Source “https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\baseimagevm.vhd” -StorageKey <String>

> [!NOTE]
> **Save-AzureVhd** también tiene la opción **NumberOfThreads**, que puede utilizarse para aumentar el paralelismo para aprovechar al máximo el ancho de banda disponible para la descarga.
> 
> 

## <a name="upload-vhds-to-an-azure-storage-account"></a>Cargar discos duros virtuales a una cuenta de almacenamiento de Azure
Si ha preparado sus discos duros virtuales de forma local, debe cargarlos en una cuenta de almacenamiento de Azure. Este paso tendrá lugar después de crear su disco duro virtual de forma local, pero antes de obtener la certificación para la imagen de máquina virtual.

### <a name="create-a-storage-account-and-container"></a>Creación de una cuenta de almacenamiento y un contenedor
Se recomienda que los discos duros virtuales se carguen en una cuenta de almacenamiento en una región de Estados Unidos. Todos los discos duros virtuales de un SKU único deben colocarse en un contenedor único dentro de una cuenta de almacenamiento única.

Para crear una cuenta de almacenamiento, puede utilizar el [Portal de Microsoft Azure](https://portal.azure.com/), PowerShell o la herramienta de línea de comandos de Linux.  

**Creación de una cuenta de almacenamiento en el Portal de Microsoft Azure**

1. Haga clic en **Crear un recurso**.
2. Seleccione **Storage**.
3. Indique un nombre para la cuenta de almacenamiento y luego seleccione una ubicación.
   
   ![dibujo](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. Haga clic en **Create**(Crear).
5. La hoja de la cuenta de almacenamiento creada debe estar abierta. Si no es así, seleccione **Examinar** > **Cuentas de almacenamiento**. En la hoja Cuenta de almacenamiento, seleccione la cuenta de almacenamiento creada.
6. Seleccione **Contenedores**.
   
   ![dibujo](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. En la hoja Contenedores, seleccione **Agregar**y escriba un nombre de contenedor y los permisos de este. Seleccione **Privado** para los permisos del contenedor.

> [!TIP]
> Se recomienda crear un contenedor por SKU que pretende publicar.
> 
> 

  ![dibujo](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a>Creación de una cuenta de almacenamiento mediante PowerShell
Con PowerShell, cree una cuenta de almacenamiento mediante el cmdlet [New-AzureStorageAccount](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azurestorageaccount) .

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

A continuación, puede crear un contenedor dentro de esa cuenta de almacenamiento, con el cmdlet [NewAzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer).

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> Esos comandos asumen que ya se ha establecido el contexto de la cuenta de almacenamiento actual en PowerShell.   Consulte [Configuración de Azure PowerShell](marketplace-publishing-powershell-setup.md) para obtener más detalles sobre la configuración de PowerShell.  
> 
> ### <a name="create-a-storage-account-by-using-the-command-line-tool-for-mac-and-linux"></a>Creación de una cuenta de almacenamiento con la herramienta de línea de comandos para Mac y Linux
> En la [Herramienta de línea de comandos de Linux](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), cree una cuenta de almacenamiento de la manera siguiente.
> 
> 

        azure storage account create mystorageaccount --location "West US"

Cree un contenedor de la manera siguiente.

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a>Carga de un disco duro virtual
Una vez creados la cuenta de almacenamiento y el contenedor, puede cargar sus discos duros virtuales preparados. Es posible usar PowerShell, la herramienta de línea de comandos de Linux u otras herramientas de administración de Azure Storage.

### <a name="upload-a-vhd-via-powershell"></a>Cargar un disco duro virtual a través de PowerShell
Use el cmdlet [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) .

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-the-command-line-tool-for-mac-and-linux"></a>Carga de un disco duro virtual con la herramienta de línea de comandos para Mac y Linux
Con la [Herramienta de línea de comandos de Linux](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), utilice lo siguiente: azure vm image create <image name> --location <Location of the data center> --OS Linux <LocationOfLocalVHD>

## <a name="see-also"></a>Otras referencias
* [Creación de una imagen de máquina virtual para Azure Marketplace](marketplace-publishing-vm-image-creation.md)
* [Configuración de Azure PowerShell](marketplace-publishing-powershell-setup.md)

