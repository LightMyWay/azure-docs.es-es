---
title: Problemas conocidos y soluciones
titleSuffix: Azure Machine Learning service
description: Obtenga una lista de los problemas conocidos, soluciones alternativas y soluciones para Azure Machine Learning Service.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 4a4f1691162ab9c9fbd5bc8802ecf7ebc4894d74
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53193678"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Problemas conocidos y solución de problemas del servicio Azure Machine Learning
 
En este artículo obtendrá ayuda para buscar y corregir los errores que se producen al usar el servicio Azure Machine Learning. 

## <a name="sdk-installation-issues"></a>Problemas de instalación de SDK

**Mensaje de error: no se puede desinstalar 'PyYAML'** 

SDK de Azure Machine Learning para Python: PyYAML es un proyecto instalado de Distutils. Por lo tanto, no podemos determinar con precisión qué archivos le pertenecen en caso de una desinstalación parcial. Para continuar con la instalación del SDK sin tener en cuenta este error, use:
```Python 
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="trouble-creating-azure-machine-learning-compute"></a>Problemas al crear la instancia de Proceso de Azure Machine Learning
Es posible que algunos usuarios que crearon su área de trabajo de Azure Machine Learning en Azure Portal antes de la versión disponible de forma general no puedan crear la instancia de Proceso de Azure Machine Learning en esa área de trabajo. Puede generar una solicitud de soporte técnico en el servicio o crear una nueva área de trabajo mediante el portal o el SDK para desbloquearse a sí mismo inmediatamente. 

## <a name="image-building-failure"></a>Error de creación de imagen

Error de creación de imagen al implementar el servicio web. La solución alternativa es agregar "pynacl==1.2.1" como una dependencia pip al archivo Conda para la configuración de la imagen.  

## <a name="fpgas"></a>FPGA
No podrá implementar modelos en FPGA hasta que haya solicitado y se haya aprobado para la cuota FPGA. Para solicitar acceso, rellene el formulario de solicitud de cuota: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Problemas de Databricks y Azure Machine Learning.

1. Recomendación para el clúster de Databricks:
   
   Cree el clúster de Azure Databricks como v4.x con Python 3. Se recomienda un clúster de alta simultaneidad.
 
2. Error de instalación del SDK de AML en Databricks cuando se instalan varios paquetes.

   Algunos paquetes, como `psutil`, pueden provocar conflictos. Para evitar errores de instalación, inmovilice la versión lib para instalar paquetes. Este problema está relacionado con Databricks y no con el SDK de Azure ML, aunque también se puede encontrar en otras bibliotecas. Ejemplo:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
   ```
   Como alternativa, puede usar scripts de init si sigue experimentando problemas de instalación con las bibliotecas de Python. Este no es un enfoque oficialmente compatible. Puede hacer referencia a [este documento](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

3. Al usar Machine Learning automatizado en Databricks, si ve `Import error: numpy.core.multiarray failed to import`

   Solución alternativa: importe la biblioteca de Python `numpy==1.14.5` a su clúster de Databricks con el procedimiento para crear una biblioteca que [instalar y adjuntar](https://docs.databricks.com/user-guide/libraries.html#create-a-library).

## <a name="azure-portal"></a>Azure Portal
Si ve directamente el área de trabajo desde un vínculo de recurso compartido desde el SDK o el portal, no podrá ver la página de información general normal con la información de suscripción en la extensión. Tampoco será capaz de cambiar a otra área de trabajo. Si necesita ver otra área de trabajo, la solución consiste en ir directamente a la [Azure Portal](https://portal.azure.com) y buscar el nombre del área de trabajo.

## <a name="diagnostic-logs"></a>Registros de diagnóstico
A veces puede resultar útil proporcionar información de diagnóstico al solicitar ayuda. Aquí es donde se encuentran los archivos de registro:

## <a name="resource-quotas"></a>Cuotas de recursos

Obtenga información sobre la [cuotas de recursos](how-to-manage-quotas.md) que puede encontrar al trabajar con Azure Machine Learning.

## <a name="get-more-support"></a>Obtener más soporte técnico

Puede enviar las solicitudes de soporte técnico y obtener ayuda de soporte técnico, foros, etc. [Más información...](support-for-aml-services.md)
