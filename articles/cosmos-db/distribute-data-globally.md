---
title: Distribución global de los datos con Azure Cosmos DB
description: Obtenga más información sobre replicación geográfica, arquitectura multimaestro, conmutación por error y recuperación de datos a escala mundial con bases de datos globales de Azure Cosmos DB, un servicio de base de datos con varios modelos distribuido globalmente.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.openlocfilehash: 3599875f96c6bd79ecace1d59c3580027fab3168
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54040360"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Distribución de datos global con Azure Cosmos DB

Las aplicaciones actuales deben estar siempre en línea y tener una alta capacidad de respuesta. Para lograr baja latencia y alta disponibilidad, las instancias de estas aplicaciones deben implementarse en centros de datos que están cerca de sus usuarios. Estas aplicaciones se implementan normalmente en varios centros de datos y se denominan "distribuidas globalmente". Las aplicaciones distribuidas globalmente necesitan una base de datos distribuida globalmente que puede replicar de forma transparente los datos en cualquier lugar del mundo para permitir que las aplicaciones funcionen en una copia de los datos que están cerca de los usuarios. 

Azure Cosmos DB es un servicio de base de datos distribuida globalmente que está diseñado para proporcionar baja latencia, escalabilidad elástica del rendimiento, semántica bien definida para la coherencia de datos y alta disponibilidad. En resumen, si la aplicación necesita un tiempo de respuesta rápido garantizado en cualquier parte del mundo, si necesita estar siempre en línea y precisa escalabilidad elástica e ilimitada de rendimiento y almacenamiento, considere la posibilidad de crear aplicaciones con Azure Cosmos DB.

Puede configurar sus bases de datos para que se distribuyan de manera global y estén disponibles en cualquiera de las regiones de Azure. Para reducir la latencia, coloque los datos en la ubicación más cercana a la de los usuarios. Elegir las regiones requeridas depende del alcance global de la aplicación y de dónde se encuentran los usuarios. Azure Cosmos DB replica sin problemas los datos dentro de la cuenta a todas las regiones asociadas con la cuenta. Proporciona una sola imagen de sistema de los contenedores y la base de datos de Azure Cosmos globalmente distribuida, para que la aplicación pueda leer y escribir de manera local. 

Con Azure Cosmos DB, puede agregar o quitar las regiones asociadas con su cuenta en cualquier momento. La aplicación no necesita pausarse o volver a implementarse para agregar o quitar una región. Sigue ofreciendo una alta disponibilidad en todo momento debido a las funcionalidades de hospedaje múltiple que proporciona el servicio.

## <a name="key-benefits-of-global-distribution"></a>Ventajas clave de distribución global

**Compilación de aplicaciones globales activo/activo.** Con la característica de arquitectura multimaestro, cada región es una región de escritura. También se puede leer. La característica de arquitectura multimaestro también garantiza:

- Escalabilidad de escritura elástica ilimitada. 
- 99,999 % de disponibilidad de lectura y escritura en todo el mundo.
- Garantía de lecturas y escrituras atendidas en menos de 10 milisegundos en el percentil 99.

Mediante las API de hospedaje múltiple de Azure Cosmos DB, la aplicación sabe cuál es la región más cercana. Con esta información, puede enviar solicitudes a esa región. La región más cercana se identifica sin ningún cambio de configuración. Cuando agrega y quita regiones en la cuenta de Azure Cosmos DB, no es necesario volver a implementar la aplicación. La aplicación sigue teniendo alta disponibilidad.

**Creación de aplicaciones con alta capacidad de respuesta.** La aplicación puede diseñarse fácilmente para realizar lecturas y escrituras casi en tiempo real. Se pueden usar latencias inferiores a 10 milisegundos en todas las regiones que se elijan para la base de datos. Azure Cosmos DB gestiona internamente la replicación de datos entre regiones. Como resultado, se garantiza el nivel de coherencia seleccionado para la cuenta de Azure Cosmos DB.

Muchas aplicaciones se benefician de las mejoras de rendimiento que se incluyen con la capacidad de realizar escrituras en varias regiones (locales). Algunas aplicaciones que requieren coherencia fuerte prefieren canalizar todas las escrituras a una sola región. Para estas aplicaciones, Azure Cosmos DB admite configuraciones de una y varias regiones.

**Creación de aplicaciones de alta disponibilidad.** La ejecución de una base de datos en varias regiones aumenta la disponibilidad de la base de datos. Si alguna región no está disponible, otras regiones gestionan automáticamente las solicitudes de la aplicación. Azure Cosmos DB ofrece el 99,999 % de disponibilidad de lectura y escritura para las bases de datos de varias regiones.

**Mantenimiento de la continuidad empresarial durante interrupciones regionales.** Azure Cosmos DB admite [conmutación por error automática](how-to-manage-database-account.md#automatic-failover) durante una interrupción regional. Durante una interrupción regional, Azure Cosmos DB sigue manteniendo sus SLA de rendimiento, coherencia, disponibilidad y latencia. Para ayudar a garantizar que toda la aplicación tiene una alta disponibilidad, Azure Cosmos DB ofrece la API de conmutación por error manual para simular una interrupción regional. Con esta API, puede realizar maniobras de continuidad empresarial regulares.

**Escalabilidad global del rendimiento de lectura y escritura.** Con la característica de arquitectura multimaestro, puede escalar de manera elástica el rendimiento de lectura y escritura en todo el mundo. La característica de arquitectura multimaestro garantiza que el rendimiento que la aplicación configura en un contenedor o una base de datos de Azure Cosmos DB se entrega en todas las regiones. El rendimiento también está protegido mediante [Acuerdos de Nivel de Servicio (SLA) con respaldo financiero](https://aka.ms/acdbsla).

**Elección entre varios modelos de coherencia bien definidos.** El protocolo de replicación de Azure Cosmos DB ofrece cinco modelos de coherencia prácticos, intuitivos y bien definidos. Cada modelo tiene un equilibrio entre coherencia y rendimiento. Use estos modelos de coherencia para compilar con facilidad las aplicaciones distribuidas globalmente.

## <a id="Next Steps"></a>Pasos siguientes

Lea más sobre la distribución global en estos artículos:

* [Distribución global en segundo plano](global-dist-under-the-hood.md)
* [Configuración de los clientes para el hospedaje múltiple](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Incorporación o eliminación de regiones de una cuenta de Azure Cosmos DB](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Creación de una directiva de resolución de conflictos personalizada para cuentas de API de SQL](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)