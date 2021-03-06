---
title: 'Inicio rápido: Reconocimiento de voz en JavaScript en Node.js mediante el SDK del servicio Voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a realizar un reconocimiento de voz en JavaScript en Node.js mediante el SDK del servicio Voz
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: fmegen
ms.openlocfilehash: 35652b169067bc545fa0d1fcc977bbaee79ec3aa
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53724443"
---
# <a name="quickstart-recognize-speech-in-javascript-in-nodejs-using-the-speech-service-sdk"></a>Inicio rápido: Reconocimiento de voz en JavaScript en Node.js mediante el SDK del servicio Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, aprenderá a crear un proyecto de Node.js mediante el enlace de JavaScript del SDK de Voz de Cognitive Services para transcribir voz a texto.
La aplicación se basa en el [SDK de Voz de Microsoft Cognitive Services](https://aka.ms/csspeech/npmpackage).

## <a name="prerequisites"></a>Requisitos previos

* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).
* Una versión actual de [Node.js](https://nodejs.org).

## <a name="create-a-new-project-folder"></a>Creación de una nueva carpeta de proyecto

Cree una carpeta nueva y vacía e inicialícela como un nuevo proyecto de JavaScript y Node.js.

```sh
npm init -f
```

Esto inicializará los archivos package.json con valores predeterminados. Probablemente deseará editar este archivo más adelante.

## <a name="install-the-speech-sdk-for-javascript-into-that-folder"></a>Instale el SDK de Voz para JavaScript en esa carpeta

Agregue el SDK de Voz mediante `npm install microsoft-cognitiveservices-speech-sdk` al proyecto de Node.js.

Esto descargará e instalará la versión más reciente del SDK de Voz y todos los requisitos previos de npmjs. El SDK se instalará en el directorio `node_modules` de la carpeta del proyecto.

## <a name="using-the-speech-sdk"></a>Uso del SDK de Voz

Cree un nuevo archivo llamado `index.js` en la carpeta y ábralo con un editor de texto.

> [!NOTE]
> Tenga en cuenta que en Node.js el SDK de Voz no admite los tipos de datos de micrófono o archivo. Estos solo se admiten en los exploradores web. En su lugar, utilice la interfaz de Stream para el SDK de Voz, ya sea a través de `AudioInputStream.createPushStream()` o `AudioInputStream.createPullStream()`.

En este ejemplo, usaremos la interfaz `PushAudioInputStream`.

Agregue el siguiente código JavaScript:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="running-the-sample-from-command-line"></a>Ejecución del ejemplo desde la línea de comandos

Para iniciar la aplicación, adapte `YourSubscriptionKey`, `YourServiceRegion` y `YourAudioFile.wav` a la configuración. A continuación, puede ejecutarla mediante una llamada al siguiente comando:

```sh
node index.js
```

Esto desencadenará un reconocimiento mediante el nombre de archivo proporcionado y presentará la salida en la consola.

Este es un ejemplo de salida que resulta de ejecutar `index.js` después de actualizar la clave de suscripción y de usar el archivo `whatstheweatherlike.wav`.

```json
SpeechRecognitionResult {
  "privResultId": "9E30EEBD41AC4571BB77CF9164441F46",
  "privReason": 3,
  "privText": "What's the weather like?",
  "privDuration": 15900000,
  "privOffset": 300000,
  "privErrorDetails": null,
  "privJson": {
    "RecognitionStatus": "Success",
    "DisplayText": "What's the weather like?",
    "Offset": 300000,
    "Duration": 15900000
  },
  "privProperties": null
}
```

## <a name="running-the-sample-from-visual-studio-code"></a>Ejecución del ejemplo en Visual Studio Code

También puede ejecutar el ejemplo desde Visual Studio Code. Siga estos pasos para instalar, abrir y ejecutar el inicio rápido:

1. Inicie Visual Studio Code y haga clic en "Abrir carpeta" y, a continuación, desplácese a la carpeta de inicio rápido

   ![Captura de pantalla de Abrir carpeta](media/sdk/qs-js-node-01-open_project.png)

1. En Visual Studio Code, abra una ventana de terminal.

   ![Captura de pantalla de la ventana de terminal](media/sdk/qs-js-node-02_open_terminal.png)

1. Ejecute npm para instalar las dependencias

   ![Captura de pantalla de instalación de npm](media/sdk/qs-js-node-03-npm_install.png)

1. Ahora ya puede abrir `index.js` y establecer un punto de interrupción

   ![Captura de pantalla de index.js con un punto de interrupción en la línea 16](media/sdk/qs-js-node-04-setup_breakpoint.png)

1. Para iniciar la depuración, presione la tecla F5 o seleccione Depurar/Iniciar depuración en el menú

   ![Captura de pantalla del menú de depuración](media/sdk/qs-js-node-05-start_debugging.png)

1. Cuando se alcanza un punto de interrupción, puede inspeccionar la pila de llamadas y las variables

   ![Captura de pantalla del depurador](media/sdk/qs-js-node-06-hit_breakpoint.png)

1. Se mostrarán todas las salidas en la ventana de la consola de depuración

   ![Captura de pantalla de la consola de depuración](media/sdk/qs-js-node-07-debug_output.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Explorar ejemplos de Node.js en GitHub](https://aka.ms/csspeech/samples)
