---
title: Introducción al desarrollo de aplicaciones en SQL Database | Microsoft Docs
description: Aprenda sobre las bibliotecas de conectividad disponibles y los procedimientos recomendados para las aplicaciones que se conectan a SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: genemi
manager: craigg
ms.date: 06/20/2018
ms.openlocfilehash: 707e10f77bf00ed12f09a23e490105f52ceed4ab
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51241606"
---
# <a name="sql-database-application-development-overview"></a>Introducción al desarrollo de aplicaciones en SQL Database
Este artículo le lleva por las consideraciones básicas que debe tener en cuenta un desarrollador al escribir código para conectarse a Azure SQL Database.

> [!TIP]
> Para ver un tutorial sobre cómo crear servidores y firewalls basados en servidores, consultar las propiedades del servidor, establecer la conexión y consultar la base de datos maestra, crear una base de datos de ejemplo y una base de datos vacía, consultar las propiedades de la base de datos, establecer la conexión y consultar la base de datos de ejemplo, consulte este [tutorial introductorio](sql-database-get-started-portal.md).
>

## <a name="language-and-platform"></a>Plataforma y lenguaje
Existen ejemplos de código para diferentes lenguajes de programación y plataformas. Puede encontrar vínculos a los ejemplos de código en: 

* Más información: [Bibliotecas de conexiones para SQL Database y SQL Server](sql-database-libraries.md).

## <a name="tools"></a>Herramientas 
Puede aprovechar herramientas de código abierto como [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli) o [VS Code](https://code.visualstudio.com/). Además, Azure SQL Database funciona con herramientas de Microsoft como [Visual Studio](https://www.visualstudio.com/downloads/) y [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).  También puede usar el Portal de administración de Azure, PowerShell y API de REST, que le ayudan a obtener más productividad.

## <a name="resource-limitations"></a>Limitaciones de recursos
Azure SQL Database administra los recursos disponibles para una base de datos mediante dos mecanismos diferentes: regulación de recursos y aplicación de límites. Para más información, consulte:

- [Límites de modelo de recursos basados en DTU para bases de datos únicas](sql-database-dtu-resource-limits-single-databases.md)
- [Límites de modelo de recursos basado en DTU: grupos elásticos](sql-database-dtu-resource-limits-elastic-pools.md)
- [Límites de recursos basados en núcleos virtuales para bases de datos únicas](sql-database-vcore-resource-limits-single-databases.md)
- [Límites de recursos basados en núcleos virtuales para grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md)

## <a name="security"></a>Seguridad
Azure SQL Database proporciona recursos para limitar el acceso, proteger los datos y supervisar las actividades en SQL Database.

* Más información: [Protección de SQL Database](sql-database-security-overview.md).

## <a name="authentication"></a>Autenticación
* Azure SQL Database admite inicios de sesión y usuarios de autenticación de SQL Server, así como de [autenticación de Azure Active Directory](sql-database-aad-authentication.md) .
* Debe especificar una base de datos concreta, en lugar de la base de datos *maestra* predeterminada.
* No puede usar la instrucción **USE NombreDeMiBaseDeDatos;** de Transact-SQL en SQL Database para cambiar a otra base de datos.
* Más información: [Seguridad de SQL Database: administrar la seguridad del inicio de sesión y el acceso a la base de datos](sql-database-manage-logins.md).

## <a name="resiliency"></a>Resistencia
Cuando se produce un error transitorio al conectar con SQL Database, el código debe reintentar la llamada.  Recomendamos que en la lógica de reintento se haga uso de la lógica de interrupción, de este modo no se sobrecargará la SQL Database con los reintentos de varios clientes a la vez.

* Ejemplos de código: puede ver ejemplos de código que ilustran la lógica de reintento en el lenguaje de su elección en: [Bibliotecas de conexiones para SQL Database y SQL Server](sql-database-libraries.md).
* Más información: [Códigos de error para las aplicaciones cliente de SQL Database: error de conexión de base de datos y otros problemas](sql-database-develop-error-messages.md).

## <a name="managing-connections"></a>Administración de conexiones
* En la lógica de conexión de cliente, reemplace el tiempo de espera predeterminado para que sea de 30 segundos.  El valor predeterminado de 15 segundos es demasiado corto para conexiones que dependen de Internet.
* Si usa un [grupo de conexiones](https://msdn.microsoft.com/library/8xx3tyca.aspx), asegúrese de cerrar la conexión en el momento en que el programa no la esté usando activamente y no esté preparándose para volver a usarla.

## <a name="network-considerations"></a>Consideraciones sobre la red
* En el equipo que hospeda el programa cliente, asegúrese de que el firewall permita la comunicación TCP saliente en el puerto 1433.  Más información: [Configuración del firewall en Azure SQL Database mediante Azure Portal](sql-database-configure-firewall-settings.md).
* Si el programa cliente se conecta a SQL Database mientras el cliente se ejecuta en una máquina virtual (VM) de Azure, debe abrir determinados intervalos de puerto en la máquina virtual. Más información: [Puertos más allá de 1433 para ADO.NET 4.5 y SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).
* En ocasiones, las conexiones de cliente a Azure SQL Database omiten el proxy e interactúan directamente con la base de datos. Los puertos que no sean 1433 se convierten en puertos importantes. Para más información, consulte [Arquitectura de conectividad de Azure SQL Database](sql-database-connectivity-architecture.md) y [Puertos más allá de 1433 para ADO.NET 4.5 y SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="data-sharding-with-elastic-scale"></a>Particionamiento de datos con la escala elástica
La escala elástica simplifica el proceso de escalado y reducción horizontal. 

* [Modelos de diseño para las aplicaciones SaaS multiinquilino con Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* [Enrutamiento dependiente de los datos](sql-database-elastic-scale-data-dependent-routing.md).
* [Introducción a la vista previa de Escalado elástico de Azure SQL Database](sql-database-elastic-scale-get-started.md).

## <a name="next-steps"></a>Pasos siguientes
Explore todas las [funcionalidades de SQL Database](sql-database-technical-overview.md).
