---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 4b0c8ad227b9aa8a0692ff453c3751aaa38a4849
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165383"
---
|Nombre de parámetro| Escriba | DESCRIPCIÓN| Valores posibles|
|-|-|-|-|
| /ServerMode|Obligatorio|Especifica si se deben instalar los servidores de configuración y de procesos, o solo el servidor de procesos|CS<br>PS|
|/InstallLocation|Obligatorio|La carpeta donde se instalan los componentes| Cualquier carpeta del equipo|
|/MySQLCredsFilePath|Obligatorio|La ruta de acceso al archivo en que se almacenan las credenciales del servidor MySQL|El archivo debe tener el formato especificado a continuación|
|/VaultCredsFilePath|Obligatorio|La ruta de acceso del archivo de credenciales del almacén|Ruta de acceso de archivo válido|
|/EnvType|Obligatorio|El tipo de entorno que desea proteger |VMware<br>NonVMware|
|/PSIP|Obligatorio|Dirección IP de la NIC que se usará para la transferencia de datos de replicación| Cualquier dirección IP válida|
|/CSIP|Obligatorio|Dirección IP de la NIC en la que el servidor de configuración realiza la escucha| Cualquier dirección IP válida|
|/PassphraseFilePath|Obligatorio|Ruta de acceso completa a la ubicación del archivo de frase de contraseña|Ruta de acceso de archivo válido|
|/BypassProxy|Opcional|Especifica si el servidor de configuración se conecta a Azure sin proxy|Para hacerlo, obtenga este valor desde Venu|
|/ProxySettingsFilePath|Opcional|Configuración del proxy (el proxy predeterminado requiere autenticación, o un proxy personalizado)|El archivo debe tener el formato especificado a continuación|
|DataTransferSecurePort|Opcional|Número de puerto en el PSIP que se usará para los datos de replicación| Número de puerto válido (el valor predeterminado es 9433)|
|/SkipSpaceCheck|Opcional|Omitir comprobación de espacio para disco de caché| |
|/AcceptThirdpartyEULA|Obligatorio|La marca implica la aceptación de los términos de licencia de terceros| |
|/ShowThirdpartyEULA|Opcional|Muestra los términos de licencia de terceros. Si se proporciona como entrada, se omiten todos los demás parámetros| |
