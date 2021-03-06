---
title: Entrega y reintento de entrega de Azure Event Grid
description: Describe cómo Azure Event Grid entrega eventos y cómo administra los mensajes no entregados.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: 4d53c33daefaadb4c58ce500a5d564af7988b606
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077095"
---
# <a name="event-grid-message-delivery-and-retry"></a>Entrega y reintento de entrega de mensajes de Event Grid

En este artículo se describe cómo Azure Event Grid administra los eventos cuando no se confirma la entrega.

Event Grid ofrece entrega duradera. Entrega cada mensaje por lo menos una vez en cada suscripción. Los eventos se envían inmediatamente al punto de conexión registrado de cada suscripción. Si un punto de conexión no acusa recibo de un evento, Event Grid reintenta la entrega del evento.

Actualmente, Event Grid envía cada evento individualmente a los suscriptores. El suscriptor recibe una matriz con un solo evento.

## <a name="retry-schedule-and-duration"></a>Programación y duración de los reintentos

Event Grid usa una directiva de reintentos de retroceso exponencial para la entrega de eventos. Si un punto de conexión no responde o devuelve un código de error, Event Grid reintenta la entrega según la siguiente programación:

1. 10 segundos
2. 30 segundos
3. 1 minuto
4. 5 minutos
5. 10 minutos
6. 30 minutos
7. 1 hora

Event Grid agrega una pequeña selección aleatoria a todos los pasos de reintento. Después de una hora, la entrega del evento se vuelve a intentar cada una hora.

De forma predeterminada, Event Grid expira todos los eventos que no se entregan en 24 horas. Puede [personalizar la directiva de reintentos](manage-event-delivery.md) al crear una suscripción al evento. Proporcione el número máximo de intentos de entrega (el valor predeterminado es 30) y el periodo de vida de los eventos (el valor predeterminado es 1440 minutos).

## <a name="dead-letter-events"></a>Eventos fallidos

Si Event Grid no puede entregar un evento, puede enviar el evento no entregado a una cuenta de almacenamiento. Este proceso se conoce como colas de eventos fallidos. De forma predeterminada, Event Grid no tiene activada esta opción. Para habilitarla, debe especificar una cuenta de almacenamiento para contener los eventos no entregados al crear la suscripción a eventos. Puede extraer eventos de esta cuenta de almacenamiento para resolver las entregas.

Event Grid envía un evento a la ubicación de la cola de mensajes fallidos cuando ha intentado todos los reintentos. Si Event Grid recibe un código de respuesta 400 (solicitud incorrecta) o 413 (entidad de solicitud demasiado grande), envía inmediatamente el evento al punto de conexión de la cola de mensajes fallidos. Estos códigos de respuesta indican que la entrega del evento nunca se realizará correctamente.

Hay un retraso de cinco minutos entre el último intento de entregar un evento y su entrega a la ubicación de mensajes fallidos. Este retraso está pensado para reducir el número de operaciones de almacenamiento de blobs. Si la ubicación de la cola de mensajes fallidos no está disponible durante cuatro horas, se elimina el evento.

Antes de establecer la ubicación de mensajes fallidos, debe tener una cuenta de almacenamiento con un contenedor. Puede proporcionar el punto de conexión de este contenedor al crear la suscripción a eventos. El punto de conexión tiene este formato: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

Es posible que desee recibir una notificación cuando un evento se envía a la ubicación de la cola de mensajes fallidos. Para utilizar Event Grid para responder a eventos no entregados, [cree una suscripción a eventos](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) para el almacenamiento de blobs de los eventos fallidos. Cada vez que el almacenamiento de blobs de eventos fallidos recibe un evento no entregado, Event Grid lo notifica al controlador. El controlador responderá con las acciones que debe realizar para conciliar los eventos no entregados.

Para obtener un ejemplo de cómo configurar una ubicación de la cola de mensajes fallidos, consulte [Cola de mensajes fallidos y directivas de reintento](manage-event-delivery.md).

## <a name="message-delivery-status"></a>Estado de entrega de mensajes

Event Grid usa códigos de respuesta HTTP para acusar recibo de eventos. 

### <a name="success-codes"></a>Códigos de éxito

Los siguientes códigos de respuesta HTTP response indican que un evento se ha entregado correctamente en el webhook. Event Grid considera la entrega finalizada.

- 200 OK
- 202 - Aceptado

### <a name="failure-codes"></a>Códigos de error

Los siguientes códigos de respuesta HTTP indican que el intento de entrega de un evento no se ha realizado.

- 400 - Solicitud incorrecta
- 401 No autorizado
- 404 No encontrado
- 408 - Tiempo de espera de solicitud
- 413 Entidad de solicitud demasiado larga
- 414 - URI de solicitud demasiado largo
- 429 Demasiadas solicitudes
- Error de servidor interno 500
- Servicio no disponible 503
- Tiempo de espera de puerta de enlace 504

## <a name="next-steps"></a>Pasos siguientes

* Para ver el estado de las entregas de eventos, consulte [Supervisar la entrega de mensajes de Event Grid](monitor-event-delivery.md).
* Para personalizar las opciones de entrega de eventos, consulte [Cola de mensajes fallidos y directivas de reintento](manage-event-delivery.md).