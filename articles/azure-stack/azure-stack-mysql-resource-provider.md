---
title: Usar bases de datos MySQL como PaaS en Azure Stack | Microsoft Docs
description: Aprenda cómo puede implementar el proveedor de recursos de MySQL y proporcionar bases de datos MySQL como servicio en Azure Stack.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 635948c28ffe5d5eaece372976e58d26e17214e3
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50084237"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Usar bases de datos MySQL en Microsoft Azure Stack

Las bases de datos MySQL son comunes en los sitios web y admiten muchas plataformas de sitio web. Por ejemplo, puede crear sitios web de WordPress con el complemento de plataforma como servicio (PaaS) de Web Apps.

Después de implementar el proveedor de recursos, puede hacer lo siguiente:

* Crear servidores MySQL y bases de datos mediante las plantillas de implementación de Azure Resource Manager.
* Proporcionar bases de datos MySQL como servicio.  

## <a name="mysql-resource-provider-adapter-architecture"></a>Arquitectura del adaptador del proveedor de recursos de MySQL

El proveedor de recursos consta de los componentes siguientes:

* **La máquina virtual (VM) del adaptador del proveedor de recursos de MySQL**, que es una máquina virtual Windows Server que ejecuta los servicios del proveedor.
* **El proveedor de recursos**, que procesa las solicitudes y accede a los recursos de las bases de datos.
* **Los servidores que hospedan el servidor MySQL**, que proporcionan capacidad para las bases de datos, denominados servidores host. Puede crear instancias de MySQL usted mismo o proporcionar acceso a instancias de MySQL externas. Visite la [Galería de inicio rápido de Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) para obtener una plantilla de ejemplo que pueda usar para hacer lo siguiente:

  * Crear un servidor MySQL en su lugar.
  * Descargar e implementar un servidor MySQL desde Azure Marketplace.

> [!NOTE]
> Los servidores de hospedaje que se instalan en sistemas integrados de Azure Stack se deben crear mediante una suscripción de inquilino. No se pueden crear a partir de la suscripción de proveedor predeterminada. Se deben crear desde el portal de inquilino o desde una sesión de PowerShell con un inicio de sesión adecuado. Todos los servidores de hospedaje son máquinas virtuales facturables y deben tener las licencias. El administrador del servicio puede ser el propietario de la suscripción de inquilino.

### <a name="required-privileges"></a>Privilegios necesarios

Esta cuenta del sistema debe tener los siguientes privilegios:

* **Base de datos:** crear, eliminar
* **Inicio de sesión:** crear, establecer, eliminar, conceder, revocar  

## <a name="next-steps"></a>Pasos siguientes

[Implementación del proveedor de recursos de MySQL](azure-stack-mysql-resource-provider-deploy.md)
