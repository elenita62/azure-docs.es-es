---
title: ¿Qué es Azure Reservations? | Microsoft Docs
description: Obtenga información sobre Azure Reservations y precios para ahorrar en máquinas virtuales, SQL Database, Azure Cosmos DB y otros costos de recursos.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2018
ms.author: cwatson
ms.openlocfilehash: a0fb5eccf14aee07005ab345f1af293f341f9215
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47422956"
---
# <a name="what-are-azure-reservations"></a>¿Qué es Azure Reservations?

Con Azure Reservations ahorrará gastos al pagar por adelantado por un año o tres años de máquina virtual, capacidad de proceso de SQL Database, rendimiento de Azure Cosmos DB u otros recursos de Azure. El pago adelantado le permite obtener un descuento en los recursos que utiliza. Reservations puede reducir considerablemente los costos en máquinas virtuales, capacidad de proceso de SQL Database, Azure Cosmos DB u otros recursos en hasta un 72 % en precios de pago por uso. Reservations ofrece un descuento en la facturación y no afecta al estado del entorno de ejecución de los recursos.

Puede comprar una reserva en [Azure Portal](https://aka.ms/reservations). Para obtener más información, consulte los temas siguientes:

- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Pago por adelantado por recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Pago por adelantado para recursos de Azure Cosmos DB con capacidad reservada de Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md)

## <a name="why-should-i-buy-a-reservation"></a>¿Por qué debería comprar una reserva?

Si tiene máquinas virtuales, Azure Cosmos DB o SQL Database que se ejecuten durante largos períodos, la compra de una reserva le proporcionará la opción más rentable. Por ejemplo, si ejecuta continuamente cuatro instancias de un servicio sin una reserva, se le cobra según las tarifas de pago por uso. Si compra una reserva para esos recursos, obtendrá inmediatamente el descuento por reserva. Ya no se aplicarán a los recursos las tarifas de pago por uso.

## <a name="what-charges-does-a-reservation-cover"></a>¿Qué cargos cubre una reserva?

- Instancia reservada de máquina virtual: una reserva solo cubre el costo de proceso de máquina virtual. No cubre cargos por software adicional, administración de redes ni almacenamiento.
- Núcleo virtual reservado de SQL Database: solo se incluyen con una reserva los costos de proceso. La licencia se factura por separado.
- Capacidad reservada de Azure Cosmos DB: una reserva abarca el rendimiento aprovisionado de los recursos, no cubre los cargos de almacenamiento y de redes. 

Para las máquinas virtuales de Windows y SQL Database, puede cubrir los costos de licencia de Windows con la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>¿Quién puede comprar una reserva?

Los clientes de Azure que tengan estos tipos de suscripción pueden comprar una reserva:

- Tipo de oferta de suscripción Contrato Enterprise (MS-AZR-0017P).
- Tipo de oferta de suscripción [Pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/) (MS-AZR-003P). Debe tener el rol de "propietario" en la suscripción para comprar una reserva.
- Los asociados de los proveedores de soluciones en la nube (CSP) pueden usar Azure Portal o el [Centro de partners](https://docs.microsoft.com/partner-center/azure-reservations) para comprar Azure Reservations.

Un descuento de reserva solo se aplica a los recursos asociados con los tipos de suscripción Enterprise, Pago por uso o CSP.

## <a name="how-is-a-reservation-billed"></a>¿Cómo se factura una reserva?

La reserva se cobra en el método de pago vinculado a la suscripción. Si tiene una suscripción Enterprise, el costo de la reserva se deducirá del saldo del compromiso monetario. Si este no es suficiente para cubrir el costo de la reserva, se le cobrará el monto restante. Si tiene una suscripción de pago por uso, el cargo se aplicará inmediatamente en la tarjeta de crédito vinculada a su cuenta. Si se le factura por factura, verá los cargos la próxima vez que reciba una.

## <a name="how-is-the-reservation-discount-applied"></a>¿Cómo se aplica el descuento por la reserva?

El descuento por reserva se aplica a la utilización de recursos que coincide con los atributos seleccionados al comprar la reserva. Estos atributos incluyen el ámbito en el que se ejecutan las máquinas virtuales, SQL Database, Azure Cosmos DB u otros recursos coincidentes. Por ejemplo, si quiere un descuento por la reserva para sus máquinas virtuales Estándar D2 en la región Oeste de EE. UU., seleccione la suscripción en la que se ejecutan las máquinas virtuales. Si, por el contrario, sus máquinas virtuales se ejecutan en diferentes suscripciones dentro de su cuenta o inscripción, seleccione el ámbito compartido. El ámbito compartido le permite aplicar el descuento de la reserva en varias suscripciones. También puede cambiar el ámbito después de la compra de la reserva. Para más información, consulte [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md).

Un descuento de reserva solo se aplica a los recursos asociados con los tipos de suscripción Enterprise, Pago por uso o CSP. Los recursos que se ejecutan en una suscripción con otros tipos de oferta no recibirán el descuento por la reserva. Para las inscripciones Enterprise, las suscripciones de Desarrollo/pruebas de Enterprise no tienen disponibles las ventajas de las reservas.

Para entender mejor cómo afecta Reservations a la facturación, vea los temas siguientes:

-  [Información sobre el descuento de Azure Reserved VM Instances](billing-understand-vm-reservation-charges.md)
- [Información sobre el descuento de Azure Reservations](billing-understand-vm-reservation-charges.md)
- [Aplicación del descuento por reserva a Azure Cosmos DB](billing-understand-cosmosdb-reservation-charges.md)
- [Descubra cómo se aplica el descuento del plan de software de SUSE Linux Enterprise](billing-understand-suse-reservation-charges.md)

## <a name="what-happens-when-the-reservation-term-expires"></a>¿Qué ocurre cuando expira el plazo de reserva?

Al final del plazo de reserva, el descuento en la facturación expira y la máquina virtual, SQL Database, Azure Cosmos DB u otros recursos se facturan según los precios de pago por uso. Azure Reservations no se renueva automáticamente. Para seguir disfrutando del descuento de facturación, debe comprar una reserva nueva para los servicios y el software aplicables.

## <a name="discount-applies-to-different-sizes-with-instance-size-flexibility"></a>El descuento se aplica a diferentes tamaños con flexibilidad de tamaño de instancia

Al comprar una reserva, puede aplicar el descuento a otras instancias con atributos que están dentro del mismo grupo de tamaño. La flexibilidad de la cobertura del descuento depende del tipo de reserva y los atributos que elija al comprar la reserva.

- Instancias reservadas de máquina virtual: al comprar la reserva, si selecciona **Optimizado para**: **flexibilidad de tamaño de instancia**, la cobertura de descuento dependerá del tamaño de máquina virtual que elija. La reserva se puede aplicar a los tamaños de máquinas virtuales (VM) en el mismo grupo de series de tamaño. Si quiere saber más, vea [Flexibilidad en el tamaño de las máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- Plan de software de SUSE Linux Enterprise: depende de la cobertura de descuento en las vCPU de las máquinas virtuales donde se ejecuta el software SUSE. Para más información, vea[Descubra cómo se aplica el descuento del plan de software SUSE Linux Enterprise](billing-understand-suse-reservation-charges.md).
- Capacidad reservada de SQL Database: la cobertura del descuento depende del nivel de rendimiento que elija. Para más información, vea [Aprenda cómo se aplica un descuento en la reserva de Azure a las instancias de SQL Database](billing-understand-reservation-charges.md).
- Capacidad reservada de Azure Cosmos DB: la cobertura del descuento depende del rendimiento aprovisionado. Para más información, vea [Aplicación del descuento por reserva a Azure Cosmos DB](billing-understand-cosmosdb-reservation-charges.md).

## <a name="next-steps"></a>Pasos siguientes

Empiece a ahorrar en máquinas virtuales con la compra de una [instancia reservada de máquina virtual](../virtual-machines/windows/prepay-reserved-vm-instances.md), [capacidad reservada de SQL Database](../sql-database/sql-database-reserved-capacity.md) o [capacidad reservada de Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md).

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

- [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Costos de software de Windows no incluidos con reservas](billing-reserved-instance-windows-software-costs.md)
- [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene más preguntas, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
