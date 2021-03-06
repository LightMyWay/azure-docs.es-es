---
title: 'Inicio rápido: Reconocimiento de voz en Python con el SDK del servicio Voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en Python mediante el SDK del servicio Voz.
services: cognitive-services
author: chlandsi
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: chlandsi
ms.openlocfilehash: 7610b12b351b2652df7ade603711d4d92e587292
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53723916"
---
# <a name="quickstart-using-the-speech-service-from-python"></a>Inicio rápido: Uso del servicio Voz desde Python

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo se muestra cómo usar el servicio Voz mediante el SDK de Voz de Python. Se ilustra cómo reconocer la voz por la entrada del micrófono.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, presentamos una lista de requisitos previos:

* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).
* [Python 3.5 (64 bits)](https://www.python.org/downloads/) o superior.
* El paquete del SDK de Voz de Python está disponible para Windows (x64), Mac (Mac OS X versión 10.12 o posterior) y Linux (Ubuntu 16.04 o 18.04 en x64).
* En Ubuntu, ejecute los siguientes comandos para la instalación de los paquetes necesarios:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

* En Windows, también necesita [Microsoft Visual C++ Redistributable para Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) para su plataforma.

## <a name="get-the-speech-sdk-python-package"></a>Obtención del paquete de Python del SDK de Voz

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

El paquete de Python del SDK de Voz de Cognitive Services se puede instalar desde [PyPI](https://pypi.org/) mediante este comando en la línea de comandos:

```sh
pip install azure-cognitiveservices-speech
```

La versión actual del SDK de Speech de Cognitive Services es `1.2.0`.

## <a name="support-and-updates"></a>Soporte técnico y actualizaciones

Las actualizaciones para el paquete de Python del SDK de Voz se distribuirán mediante PyPI y se anunciarán en la página [Notas de la versión](./releasenotes.md).
Si hay disponible una nueva versión, puede actualizarse a ella con el comando `pip install --upgrade azure-cognitiveservices-speech`.
Para comprobar qué versión está instalada actualmente, inspeccione la variable `azure.cognitiveservices.speech.__version__`.

Si tiene un problema o falta una característica, eche un vistazo a nuestra [página de soporte técnico](./support.md).

## <a name="create-a-python-application-using-the-speech-sdk"></a>Creación de una aplicación de Python mediante el SDK de Voz

### <a name="running-the-sample-in-a-terminal"></a>Ejecución del ejemplo en un terminal

Puede copiar el [código](#quickstart-code) de este inicio rápido en un archivo de origen `quickstart.py` y ejecutarlo en el IDE o en la consola.

```sh
python quickstart.py
```

También, puede descargar este tutorial de inicio rápido como un cuaderno de [Jupyter](https://jupyter.org) del [repositorio de ejemplos de Voz de Cognitive Services](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) y ejecutarlo como un cuaderno.

### <a name="quickstart-code"></a>Código de inicio rápido

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/quickstart.py#code)]

### <a name="installing-the-speech-sdk-python-package-and-running-the-sample-in-visual-studio-code"></a>Instalación del paquete de Python del SDK de Voz y ejecución del ejemplo en Visual Studio Code

1. [Descargue](https://www.python.org/downloads/) e instale una versión de 64 bits (3.5 o posterior) de Python en el equipo.
1. [Descargue](https://code.visualstudio.com/Download) e instale Visual Studio Code.
1. Abra Visual Studio Code e instale la extensión de Python; para ello, seleccione **File** > **Preferences** > **Extensions** (Archivo > Preferencias > Extensiones) en el menú y busque "Python".
   ![Instalación de la extensión de Python](media/sdk/qs-python-vscode-python-extension.png)
1. Cree una carpeta para almacenar el proyecto, por ejemplo, con el Explorador de Windows.
1. En Visual Studio Code, haga clic en el icono **File** (Archivo) y, luego, abra la carpeta que ha creado.
   ![Abrir carpeta](media/sdk/qs-python-vscode-python-open-folder.png)
1. Cree un archivo de código fuente de Python `speechsdk.py`; para ello, haga clic en el icono de nuevo archivo.
   ![Crear archivo](media/sdk/qs-python-vscode-python-newfile.png)
1. Copie, pegue y guarde el [código de Python](#quickstart-code) en el archivo recién creado.
1. Inserte la información de suscripción del servicio Voz.
1. Si ya se ha seleccionado un intérprete de Python, se mostrará en el lado izquierdo de la barra de estado en la parte inferior de la ventana.
   Si no, puede mostrar una lista de intérpretes de Python disponibles; para ello, abra la **paleta de comandos** (`Ctrl+Shift+P`) y escriba **Python: Select Interpreter** (Seleccionar intérprete) y elija uno adecuado.
1. Si aún no está instalado el paquete de Python del SDK de Voz para el intérprete de Python que ha seleccionado, puede hacerlo fácilmente desde Visual Studio Code.
   Para instalar el paquete del SDK de Voz, abra un terminal; para ello, muestre de nuevo la paleta de comandos (`Ctrl+Shift+P`) y escriba **Terminal: Create New Integrated Terminal** (Crear terminal integrado).
   En el terminal que se abre, escriba el comando `python -m pip install azure-cognitiveservices-speech`, o el que sea apropiado para su sistema.
1. Para ejecutar el ejemplo de código, haga clic con el botón derecho en alguna parte dentro del editor y seleccione **Run Python File in Terminal** (Ejecutar archivo de Python en terminal).
   Diga algunas palabras cuando se le pida y el texto transcrito se mostrará al poco tiempo.
   ![Ejecutar el ejemplo](media/sdk/qs-python-vscode-python-run.png)

Si tiene problemas para seguir estas instrucciones, consulte el [tutorial de Python para Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial) con información más amplia.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de los ejemplos de Python en GitHub](https://aka.ms/csspeech/samples)
