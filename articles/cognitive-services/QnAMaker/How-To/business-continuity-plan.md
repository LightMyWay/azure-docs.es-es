---
title: 'Plan de continuidad empresarial: QnA Maker'
titleSuffix: Azure Cognitive Services
description: El objetivo principal del plan de continuidad empresarial consiste en crear un punto de conexión de base de conocimiento resistente, que garantizaría que el bot o la aplicación que lo consume no tengan tiempos de inactividad.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 10d3809de590a79b6efa86e3d55fbbe535ea13b6
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53413122"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Cómo crear un plan de continuidad empresarial para el servicio QnA Maker

El objetivo principal del plan de continuidad empresarial consiste en crear un punto de conexión de base de conocimiento resistente, que garantizaría que el bot o la aplicación que lo consume no tengan tiempos de inactividad.

![Plan de continuidad empresarial de QnA Maker](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

La idea de alto nivel como se representa anteriormente es la siguiente:

1. Configure dos [servicios QnA Maker](../How-To/set-up-qnamaker-service-azure.md) paralelos en [Regiones emparejadas de Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

2. Mantenga sincronizados los índices principal y secundario de Azure Search. Use el ejemplo de GitHub [aquí](https://github.com/pchoudhari/QnAMakerBackupRestore) para ver cómo realizar una copia de seguridad de los índices de Azure y cómo restaurarlos.

3. Realice una copia de seguridad de Application Insights con la [exportación continua](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry).

4. Una vez configuradas las pilas principal y secundaria, use [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/) para configurar los dos puntos de conexión y establecer un método de enrutamiento.

5. Debe crear un certificado SSL para el punto de conexión de Traffic Manager. [Enlace el certificado SSL](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl) en App Service.

6. Por último, use el punto de conexión de Traffic Manager del bot o de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Elegir capacidad para la implementación de QnA Maker](../Tutorials/choosing-capacity-qnamaker-deployment.md)
