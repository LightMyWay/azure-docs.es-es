---
title: 'Tutorial: Cortar una imagen con el SDK de Bing Visual Search'
description: Use el SDK de Bing Visual Search para obtener información sobre áreas específicas de una imagen.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: c3a06eef594ef3a7b2dda146ad76f648c07dc666
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742157"
---
# <a name="tutorial-crop-an-image-with-the-bing-visual-search-sdk-for-c"></a>Tutorial: Recortar una imagen con el SDK de Bing Visual Search para C#

El SDK de Bing Visual Search le permite recortar una imagen antes de buscar imágenes en línea que sean similares. Esta aplicación recorta a una sola persona de una imagen que contiene varias personas y, a continuación, devuelve los resultados de búsqueda que contienen imágenes similares en línea.

El código fuente completo para esta aplicación está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchCropImage.cs) con anotaciones y control de errores adicionales.

En este tutorial se muestra cómo:

> [!div class="checklist"]
> * Enviar una solicitud mediante el SDK de Bing Visual Search
> * Recortar un área de imagen para buscar con Bing Visual Search
> * Recibir y gestionar la respuesta
> * Buscar las direcciones URL de elementos de acción en la respuesta

## <a name="prerequisites"></a>Requisitos previos

* Cualquier edición de [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* Si usa Linux/MacOS, esta aplicación puede ejecutarse con [Mono](http://www.mono-project.com/).
- El paquete [NuGet Custom Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) instalado. 
    - En el Explorador de soluciones en Visual Studio, haga clic con el botón derecho en el proyecto y seleccione `Manage NuGet Packages` en el menú. Instale el paquete `Microsoft.Azure.CognitiveServices.Search.CustomSearch`. Al instalar el paquete NuGet Custom Search, también se instalarán los ensamblados siguientes:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="specify-the-image-crop-area"></a>Especificación del área de recorte de la imagen

Esta aplicación recorta un área de esta imagen del equipo de liderazgo de Microsoft. Esta área de recorte se define mediante las coordenadas superior izquierda e inferior derecha, representadas como un porcentaje de toda la imagen.  

![Equipo de responsables sénior de Microsoft](./media/MS_SrLeaders.jpg)

Esta imagen se recorta mediante la creación de un objeto `ImageInfo` del el área de recorte y la carga del objeto `ImageInfo` en una solicitud `VisualSearchRequest`. El objeto `ImageInfo` también incluye la dirección URL de la imagen.

```csharp
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```

## <a name="search-for-images-similar-to-the-crop-area"></a>Búsqueda de imágenes similares al área de recorte

La variable `VisualSearchRequest` contiene la información acerca del área de recorte de la imagen y su dirección URL. El método `VisualSearchMethodAsync()` obtiene los resultados.

```csharp
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>Obtención de los datos de dirección URL de ImageModuleAction

Los resultados de Bing Visual Search son objetos `ImageTag`.  Cada etiqueta contiene una lista de objetos `ImageAction`.  Cada elemento `ImageAction` contiene un campo `Data` que es una lista de valores que dependen del tipo de acción.

Puede imprimir los distintos tipos con el código siguiente:

```csharp
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);
```

La aplicación completa devuelve:


|ActionType  |URL  | |
|---------|---------|---------|
|PagesIncluding WebSearchURL     |         |         
|MoreSizes WebSearchURL     |         |         
|VisualSearch WebSearchURL    |         |         
|ImageById WebSearchURL     |         |         
|RelatedSearches WebSearchURL     |         |         
|Entity -> WebSearchUrl     | https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5380.1        |         
|TopicResults -> WebSearchUrl    |  https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx,5382.1        |         
|ImageResults -> WebSearchUrl    |  https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5384.1        |         

Tal y como se muestra en el texto anterior, el tipo de acción `Entity` contiene una consulta de Bing Search que devuelve información acerca de una persona, lugar o cosa reconocibles.  Los tipos `TopicResults` y `ImageResults` contienen consultas de imágenes relacionadas. Las direcciones URL de la lista vinculan a los resultados de Bing Search.


## <a name="get-urls-for-pagesincluding-actiontype-images"></a>Obtener las direcciones URL para las imágenes PagesIncluding ActionType

Para la obtención de las direcciones URL de imágenes reales se requiere una conversión que lea un elemento `ActionType` como `ImageModuleAction`, que contenga un elemento `Data` con una lista de valores.  Cada valor es la dirección URL de una imagen.  El siguiente código convierte el tipo de acción `PagesIncluding` en `ImageModuleAction` y lee los valores.

```csharp
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Compilar una aplicación web de una sola página](tutorial-bing-visual-search-single-page-app.md)

[Respuesta de Visual Search](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)