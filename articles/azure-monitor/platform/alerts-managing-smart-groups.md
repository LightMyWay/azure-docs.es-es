---
title: Administración de grupos inteligentes
description: Administración de grupos inteligentes creados en sus instancias de alerta
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: a76b82764b926a228dcb544e272e280789ca7356
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53345844"
---
# <a name="manage-smart-groups"></a>Administración de grupos inteligentes
Los [grupos inteligentes](https://aka.ms/smart-groups) usan algoritmos de aprendizaje automático para agrupar las alertas en función de su concurrencia o similitud, a fin de que el usuario ahora pueda administrar grupos inteligentes en lugar de tener que administrar individualmente cada alerta. En este artículo le guiaremos por el proceso de obtener acceso a grupos inteligentes y de usarlos en Azure Monitor.
1.  Para ver los grupos inteligentes creados para sus instancias de alerta puede hacer una de las siguientes opciones:
     1. Hacer clic en **Grupos inteligentes** en la página **Resumen de alertas**.    
    ![Supervisión](./media/alerts-managing-smart-groups/sg-alerts-summary.jpg)
     2. Hacer clic en Alertas por grupos inteligentes (Alerts by Smart Groups) en la página de todas las alertas.   
     ![Supervisión](./media/alerts-managing-smart-groups/sg-all-alerts.jpg)
2.  Esto le llevará a la vista con la lista de todos los grupos inteligentes creados mediante las instancias de alerta. En lugar de examinar varias alertas, ahora puede usar los grupos inteligentes.   
![Supervisión](./media/alerts-managing-smart-groups/sg-list.jpg)
3.  Al hacer clic en cualquier grupo inteligente, se abre la página de detalles, donde puede ver el motivo de la agrupación, junto con las alertas que lo componen. Esta agregación le permite lidiar con un único grupo inteligente, en lugar de examinar varias alertas.   
![Supervisión](./media/alerts-managing-smart-groups/sg-details.jpg)

