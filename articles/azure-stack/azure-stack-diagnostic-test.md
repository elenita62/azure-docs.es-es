---
title: Ejecución de una prueba de validación en Azure Stack | Microsoft Docs
description: Recopilación de archivos de registro de diagnósticos en Azure Stack
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 10/24/2018
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: 4f95fb5f2199e8c276b78a83391f3814303a9470
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024634"
---
# <a name="run-a-validation-test-for-azure-stack"></a>Ejecución de una prueba de validación para Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*
 
Puede validar el estado de Azure Stack. Si hay algún problema, póngase en contacto con el Soporte de servicio al cliente de Microsoft. Soporte le pedirá que ejecute **Test-AzureStack** desde el nodo de administración. La prueba de validación permite aislar el error. A continuación, Soporte puede analizar los registros detallados, centrarse en el área donde se produjo el error y trabajar con usted para resolver el problema.

## <a name="run-test-azurestack"></a>Ejecución de Test-AzureStack

Si hay algún problema, póngase en contacto con el Soporte de servicio al cliente de Microsoft y ejecute **Test-AzureStack**.

1. Tiene un problema.
2. Póngase en contacto con el Soporte de servicio al cliente de Microsoft.
3. Ejecute **Test-AzureStack** desde el punto de conexión con privilegios.
    1. Acceda al punto de conexión con privilegios. Para obtener instrucciones, consulte [Uso del punto de conexión con privilegios en Azure Stack](azure-stack-privileged-endpoint.md). 
    2. En ASDK, inicie sesión en el host de administración como **AzureStack\CloudAdmin**.  
    En los sistemas integrados, tendrá que utilizar la dirección IP del punto de conexión con privilegios en la administración proporcionada por el proveedor de hardware de OEM.
    3. Abra PowerShell como administrador.
    4. Ejecute: `Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint`
    5. Ejecute: `Test-AzureStack`
4. Si ninguna de las pruebas notifica error, ejecute: `Get-AzureStackLog -FilterByRole SeedRing -OutputPath <Log output path>`. El cmdlet recopilará los registros de Test-AzureStack. Para más información sobre los registros de diagnóstico, consulte [Registros de diagnóstico de Azure Stack](azure-stack-diagnostics.md). No debe recopilar registros ni ponerse en contacto con el Soporte técnico y el servicio al cliente de Microsoft (CSS) si las pruebas devuelven WARN.
5. Envíe los registros **SeedRing** al Soporte de servicio de Microsoft. Este le ayudará a resolver el problema.

## <a name="reference-for-test-azurestack"></a>Referencia de Test-AzureStack

Esta sección contiene información general sobre el cmdlet Test-AzureStack y un resumen del informe de validación.

### <a name="test-azurestack"></a>Test-AzureStack

Valida el estado de Azure Stack. El cmdlet notifica el estado del hardware y el software de Azure Stack. El personal de Soporte puede utilizar este informe para reducir el tiempo necesario para resolver casos de soporte técnico de Azure Stack.

> [!Note]  
> **Test-AzureStack** puede detectar errores que no producen interrupciones en la nube, como un único disco erróneo o un único error en el nodo físico del host.

#### <a name="syntax"></a>Sintaxis

````PowerShell
  Test-AzureStack
````

#### <a name="parameters"></a>Parámetros

| Parámetro               | Valor           | Obligatorio | Valor predeterminado |
| ---                     | ---             | ---      | ---     |
| ServiceAdminCredentials | string    | Sin        | FALSE   |
| DoNotDeployTenantVm     | SwitchParameter | Sin        | FALSE   |
| AdminCredential         | PSCredential    | Sin        | N/D      |
| Enumerar                    | SwitchParameter | Sin        | FALSE   |
| Ignorar                  | string          | Sin        | N/D      |
| Include                 | string          | Sin        | N/D      |
| BackupSharePath         | string          | Sin        | N/D      |
| BackupShareCredential   | PSCredential    | Sin        | N/D      |


El cmdlet Test-AzureStack cmdlet admite los parámetros habituales: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable y OutVariable. Para más información, consulte [About Common Parameters](http://go.microsoft.com/fwlink/?LinkID=113216) (Acerca de parámetros habituales). 

### <a name="examples-of-test-azurestack"></a>Ejemplos de Test-AzureStack

En los ejemplos siguientes se supone que ha iniciado sesión como **CloudAdmin** y que va a acceder al punto de conexión con privilegios (PEP). Para obtener instrucciones, consulte [Uso del punto de conexión con privilegios en Azure Stack](azure-stack-privileged-endpoint.md). 

#### <a name="run-test-azurestack-interactively-without-cloud-scenarios"></a>Ejecución de Test-AzureStack de forma interactiva sin escenarios en la nube

En una sesión PEP, ejecute:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack
````

#### <a name="run-test-azurestack-with-cloud-scenarios"></a>Ejecución de Test-AzureStack con escenarios en la nube

Puede usar **Test-AzureStack** para ejecutar escenarios en la nube en Azure Stack. Entre los escenarios se incluyen los siguientes:

 - Creación de grupos de recursos
 - Creación de planes
 - Creación de ofertas
 - Creación de cuentas de almacenamiento
 - Creación de una máquina virtual
 - Realización de operaciones de blob mediante la cuenta de almacenamiento que creó en el escenario de prueba
 - Realización de operaciones de cola mediante la cuenta de almacenamiento que creó en el escenario de prueba
 - Realización de operaciones de tabla mediante la cuenta de almacenamiento que creó en el escenario de prueba

Los escenarios en la nube requieren credenciales de administrador en la nube. 
> [!Note]  
> No se pueden ejecutar los escenarios en la nube mediante las credenciales de los Servicios de federación de Active Directory (AD FS). Solo se puede acceder al cmdlet **Test-AzureStack** mediante el punto de conexión con privilegios. Sin embargo, este no admite las credenciales de AD FS.

Escriba el nombre de usuario del administrador de la nube en formato UPN serviceadmin@contoso.onmicrosoft.com (Azure AD). Cuando se le solicite, escriba la contraseña en la cuenta de administrador en la nube.

En una sesión PEP, ejecute:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -ServiceAdminCredentials <Cloud administrator user name>
````

#### <a name="run-test-azurestack-without-cloud-scenarios"></a>Ejecución de Test-AzureStack sin escenarios en la nube

En una sesión PEP, ejecute:

````PowerShell
  $session = New-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Invoke-Command -Session $session -ScriptBlock {Test-AzureStack}
````

#### <a name="list-available-test-scenarios"></a>Enumeración de los escenarios de prueba disponibles:

En una sesión PEP, ejecute:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -List
````

#### <a name="run-a-specified-test"></a>Ejecutar una prueba específica

En una sesión PEP, ejecute:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
````

Para excluir pruebas específicas:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint  -Credential $localcred
    Test-AzureStack -Ignore AzsInfraPerformance
````

### <a name="run-test-azurestack-to-test-infrastructure-backup-settings"></a>Ejecución de Test-AzureStack para probar la configuración de copia de seguridad de la infraestructura

Antes de configurar la copia de seguridad de la infraestructura, puede probar la ruta de acceso y las credenciales del recurso compartido de copia de seguridad mediante la prueba **AzsBackupShareAccessibility**.

En una sesión PEP, ejecute:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential <PSCredentials-for-backup-share>
````
Después de configurar la copia de seguridad, puede ejecutar AzsBackupShareAccessibility para asegurarse de que el recurso compartido sea accesible desde ERCS, en una sesión PEP, ejecute:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint  -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility
````

Para probar las nuevas credenciales con el recurso compartido de copia de seguridad configurada, en una sesión PEP, ejecute:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential <PSCredential for backup share>
````

### <a name="validation-test"></a>Prueba de validación

En la tabla siguiente se resumen las pruebas de validación ejecutadas por **Test-AzureStack**.

| NOMBRE                                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Resumen de la infraestructura de hospedaje en la nube de Azure Stack                                                                                  |
| Resumen de los servicios de almacenamiento de Azure Stack                                                                                              |
| Resumen de la instancia de rol de la infraestructura de Azure Stack                                                                                  |
| Uso de la infraestructura de hospedaje en la nube de Azure Stack                                                                              |
| Capacidad de la infraestructura de Azure Stack                                                                                               |
| Resumen de API y del portal de Azure Stack                                                                                                |
| Resumen del certificado de Azure Resource Manager de Azure Stack                                                                                               |
| Roles de controlador de administración de la infraestructura, controlador de red, servicios de almacenamiento y de infraestructura de puntos de conexión con privilegios          |
| Instancias de roles de controlador de administración de la infraestructura, controlador de red, servicios de almacenamiento y de infraestructura de puntos de conexión con privilegios |
| Resumen de roles de la infraestructura de Azure Stack                                                                                           |
| Servicios Service Fabric en la nube de Azure Stack                                                                                         |
| Rendimiento de la instancia de rol de la infraestructura de Azure Stack                                                                              |
| Resumen del rendimiento del host en la nube de Azure Stack                                                                                        |
| Resumen del consumo de recursos del servicio Azure Stack                                                                                  |
| Eventos críticos de la unidad de escalado de Azure Stack (últimas 8 horas)                                                                             |
| Resumen de los discos físicos de los servicios de almacenamiento de Azure Stack                                                                               |
|Resumen de accesibilidad del recurso compartido de copia de seguridad de Azure Stack                                                                                     |

## <a name="next-steps"></a>Pasos siguientes

 - Para más información sobre las herramientas de diagnóstico de Azure Stack y el registro de problemas, consulte [Herramientas de diagnóstico de Azure Stack](azure-stack-diagnostics.md).
 - Para más información sobre la solución de problemas, consulte [Solución de problemas de Microsoft Azure Stack](azure-stack-troubleshooting.md)
