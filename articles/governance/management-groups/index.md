---
title: Organización de los recursos con grupos de administración de Azure
description: Obtenga información sobre los grupos de administración y cómo utilizarlos.
author: rthorn17
manager: rithorn
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 9/28/2018
ms.author: rithorn
ms.openlocfilehash: b5a99ff8cfc0a915b70c6d90b8aa04d020177d54
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50748177"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organización de los recursos con grupos de administración de Azure

Si su organización tiene varias suscripciones, puede que necesite una manera de administrar el acceso, las directivas y el cumplimiento de esas suscripciones de forma eficaz. Los grupos de administración de Azure proporcionan un nivel de ámbito por encima de las suscripciones. Las suscripciones se organizan en contenedores llamados "grupos de administración" y aplican sus condiciones de gobierno a los grupos de administración. Todas las suscripciones dentro de un grupo de administración heredan automáticamente las condiciones que se aplican al grupo de administración. Los grupos de administración proporcionan capacidad de administración de nivel empresarial a gran escala con independencia del tipo de suscripciones que tenga.

A modo de ejemplo, puede aplicar directivas a un grupo de administración que limite las regiones disponibles para la creación de máquinas virtuales. Esta directiva se aplicaría a todos los grupos de administración, las suscripciones y los recursos de ese grupo de administración, al permitir únicamente que se creen máquinas virtuales en esa región.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Jerarquía de los grupos de administración y las suscripciones

Puede compilar una estructura flexible de grupos de administración y suscripciones para organizar los recursos en una jerarquía para una administración unificada de las directivas y el acceso. El siguiente diagrama muestra un ejemplo de creación de una jerarquía para el gobierno mediante grupos de administración.

![árbol](./media/MG_overview.png)

Mediante la creación de una jerarquía similar a este ejemplo puede aplicar una directiva, por ejemplo, las ubicaciones de máquina virtual limitadas a la región Oeste de EE. UU. en el grupo "Grupo de administración del equipo de infraestructura" para habilitar el cumplimiento interno y las directivas de seguridad. Esta directiva se heredará en ambas suscripciones de EA en ese grupo de administración y se aplicará a todas las máquinas virtuales de esas suscripciones. Como esta directiva se hereda del grupo de administración en las suscripciones, el propietario de recursos o suscripciones no puede modificar esta directiva de seguridad, lo que permite una gobernabilidad mejorada.

Otro escenario en el que usaría grupos de administración es para proporcionar acceso de usuario a varias suscripciones. Al mover varias suscripciones bajo ese grupo de administración, tiene la posibilidad de crear una asignación de [control de acceso basado en rol](../../role-based-access-control/overview.md) (RBAC) en el grupo de administración, que heredará ese acceso en todas las suscripciones.
Sin la necesidad de crear scripts de asignaciones de RBAC en varias suscripciones, una asignación en el grupo de administración puede permitir a los usuarios tener acceso a todo lo que necesitan.

### <a name="important-facts-about-management-groups"></a>Hechos importantes acerca de los grupos de administración

- Se admiten 10 000 grupos de administración en un único directorio.
- Un árbol de grupo de administración puede admitir hasta seis niveles de profundidad.
  - Este límite no incluye el nivel raíz o de suscripción.
- Cada grupo de administración y suscripción admite solo un elemento primario.
- Cada grupo de administración puede tener varios elementos secundarios.
- Todas las suscripciones y grupos de administración están dentro de una única jerarquía en cada directorio. Consulte [Hechos importantes sobre el grupo de administración raíz](#important-facts-about-the-root-management-group) para conoce las excepciones durante la versión preliminar.

## <a name="root-management-group-for-each-directory"></a>Un grupo de administración raíz para cada directorio

Cada directorio tiene un grupo de administración de nivel superior único denominado "raíz".
Este grupo de administración raíz está integrado en la jerarquía de manera que contiene todos los grupos de administración y suscripciones. Este grupo de administración raíz permite que las directivas globales y las asignaciones de control de acceso basado en rol se apliquen en el nivel de directorio. Los [administradores de directorio necesitan elevar sus privilegios](../../role-based-access-control/elevate-access-global-admin.md) para ser inicialmente el propietario del grupo raíz. Una vez que el administrador es el propietario del grupo, puede asignar cualquier control de acceso basado en rol a otros usuarios o grupos del directorio para administrar la jerarquía.

### <a name="important-facts-about-the-root-management-group"></a>Hechos importantes acerca de los grupos de administración raíz

- El nombre y el identificador del grupo de administración raíz se proporcionan de forma predeterminada. El nombre para mostrar se puede actualizar en cualquier momento para mostrarse diferente en Azure Portal.
  - El nombre será "Grupo raíz de inquilino".
  - El identificador será el de Azure Active Directory.
- El grupo de administración raíz no se puede mover ni eliminar, a diferencia de los demás.  
- Todas las suscripciones y los grupos de administración se incluyen en el grupo de administración raíz único del directorio.
  - Todos los recursos del directorio pertenecen al grupo de administración raíz para la administración global.
  - Las suscripciones nuevas pertenecen de manera predeterminada y automática al grupo de administración raíz cuando se crean.
- Todos los clientes de Azure pueden ver el grupo de administración raíz, pero no todos los clientes tienen acceso para administrar ese grupo de administración raíz.
  - Todos los usuarios con acceso a una suscripción pueden ver el contexto de dónde está esa suscripción en la jerarquía.  
  - A ninguno se le concede acceso de forma predeterminada al grupo de administración raíz. Los administradores globales de directorio son los únicos usuarios que pueden elevarse ellos mismos para obtener acceso.  Una vez que tengan acceso, los administradores de directorio pueden asignar cualquier rol RBAC a otros usuarios para la administración.  

> [!IMPORTANT]
> Todas las asignaciones de acceso de los usuarios o de directivas en el grupo de administración raíz **se aplican a todos los recursos dentro del directorio**.
> Por este motivo, todos los clientes deben evaluar la necesidad de tener los elementos definidos en este ámbito.
> Las asignaciones de directivas y de acceso de usuario deberían ser obligatorias solo en este ámbito.  

## <a name="initial-setup-of-management-groups"></a>Instalación inicial de los grupos de administración

Cuando algún usuario comienza usando grupos de administración, se produce un proceso de configuración inicial. El primer paso es que el grupo de administración raíz se crea en el directorio. Una vez creado este grupo, todas las suscripciones existentes que existen en el directorio se convierten en elementos secundarios del grupo de administración raíz. El motivo de este proceso es asegurarse de que hay solo una jerarquía de grupos de administración en un directorio. La jerarquía única dentro del directorio permite a los clientes administrativos aplicar directivas y un acceso global que otros clientes dentro del directorio no puedan omitir. Nada que se haya asignado en la raíz se aplicará a todos los grupos de administración, suscripciones, grupos de recursos y recursos dentro del directorio al tener una jerarquía dentro del directorio.

## <a name="trouble-seeing-all-subscriptions"></a>Problemas para ver todas las suscripciones

Algunos de los directorios que empezaron a usar grupos de administración durante la versión preliminar (antes del 25 de junio de 2018) podrían experimentar el problema de que no se apliquen todas las suscripciones a la jerarquía.  Esto se debe a que los procesos para aplicar suscripciones en la jerarquía se implementaron después de realizar una asignación de roles o directivas en el grupo de administración raíz del directorio.

### <a name="how-to-resolve-the-issue"></a>Cómo resolver el problema

Hay dos opciones de autoservicio para resolver este problema.

1. Eliminar todas las asignaciones de roles y directivas del grupo de administración raíz
    1. Mediante la eliminación de todas las asignaciones de roles y directivas del grupo de administración raíz, el servicio repondrá todas las suscripciones en la jerarquía durante el siguiente ciclo nocturno.  El motivo de esta comprobación es asegurarse de que no se ha dado ningún acceso accidental ni asignación de directiva a todas las suscripciones de los inquilinos.
    1. La mejor manera de realizar este proceso sin que afecte a los servicios es aplicar las asignaciones de roles o directivas un nivel por debajo del grupo de administración raíz. Después, puede quitar todas las asignaciones del ámbito raíz.
1. Llamar a la API directamente para iniciar el proceso de reposición
    1. Cualquier cliente autorizado del directorio puede llamar a las API *TenantBackfillStatusRequest* o *StartTenantBackfillRequest*. Cuando se llama a StartTenantBackfillRequest API, esta comienza el proceso de configuración inicial de mover todas las suscripciones a la jerarquía. Este proceso también inicia la aplicación de todas las suscripciones nuevas para que constituyan un elemento secundario del grupo de administración raíz. Este proceso se puede realizar sin cambiar ninguna asignación en el nivel raíz ya que, como usted dice, está bien que cualquier asignación de directiva o de acceso en la raíz se pueda aplicar a todas las suscripciones.

Si tiene preguntas acerca de este proceso de reposición, póngase en contacto con: managementgroups@microsoft.com  
  
## <a name="management-group-access"></a>Acceso al grupo de administración

Los grupos de administración de Azure admiten el [control de acceso basado en rol (RBAC) de Azure](../../role-based-access-control/overview.md) para todos los accesos a recursos y las definiciones de roles.
Estos permisos se heredan en los recursos secundarios que existen en la jerarquía. Cualquier rol de RBAC integrado puede asignarse a un grupo de administración que heredará la jerarquía para los recursos.
Por ejemplo, el colaborador de máquina virtual del rol de RBAC puede asignarse a un grupo de administración. Este rol no tiene ninguna acción en el grupo de administración, pero se heredará en todas las máquinas virtuales de ese grupo de administración.

El gráfico siguiente muestra la lista de roles y las acciones admitidas en los grupos de administración.

| Nombre de rol de RBAC             | Crear | Cambiar nombre | Move | Eliminar | Asignar acceso | Asignar directiva | Lectura  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Propietario                       | X      | X      | X    | X      | X             | X             | X     |
|Colaborador                 | X      | X      | X    | X      |               |               | X     |
|Colaborador MG*             | X      | X      | X    | X      |               |               | X     |
|Lector                      |        |        |      |        |               |               | X     |
|Lector MG*                  |        |        |      |        |               |               | X     |
|Colaborador de directivas de recursos |        |        |      |        |               | X             |       |
|Administrador de acceso de usuario   |        |        |      |        | X             |               |       |

*: Colaborador MG y Lector MG solo permiten a los usuarios realizar esas acciones en el ámbito del grupo de administración.  

### <a name="custom-rbac-role-definition-and-assignment"></a>Asignación y definición de roles de RBAC personalizados

Los roles de RBAC personalizados no se admiten en los grupos de administración en este momento. Consulte el [foro de comentarios sobre los grupos de administración](https://aka.ms/mgfeedback) para ver el estado de este elemento.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los grupos de administración, consulte:

- [Creación de grupos de administración para organizar los recursos de Azure](create.md)
- [Cambio, eliminación y administración de los grupos de administración](manage.md)
- [Instalación del módulo Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups)
- [Revisión de las especificaciones de la API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Instalación de la extensión de la CLI de Azure](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)
