---
title: 'Aptitudes cognitivas en desuso: Azure Search'
description: Esta página contiene una lista de aptitudes de Cognitive Search que se consideran en desuso y no se admitirán en un futuro próximo.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 85f02acf05e89db3e22dd24ecd0e100a9ec4af6f
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53311735"
---
#    <a name="deprecated-cognitive-search-skills"></a>Aptitudes de Cognitive Search en desuso

En este documento se describen las aptitudes cognitivas que se consideran en desuso. Use la siguiente guía para el contenido:

* Nombre de aptitud: el nombre de la aptitud que quedará en desuso; se asigna al atributo @odata.type.
* Última versión de API disponible: la última versión de la API pública de Azure Search a través de la cual se pueden crear o actualizar conjuntos de aptitudes que contienen la correspondiente aptitud en desuso.
* Finalización del soporte técnico: el último día después del cual se considera en desuso la aptitud correspondiente. Los conjuntos de habilidades creados anteriormente todavía funcionan, pero se recomienda a los usuarios realizar la migración desde una aptitud en desuso.
* Recomendaciones: ruta de migración hacia delante para usar una aptitud admitida. Se aconseja a los usuarios seguir las recomendaciones para continuar recibiendo soporte técnico.

## <a name="microsoftskillstextnamedentityrecognitionskill"></a>Microsoft.Skills.Text.NamedEntityRecognitionSkill

### <a name="last-available-api-version"></a>Última versión de API disponible

2017-11-11-preview

### <a name="end-of-support"></a>Finalización del soporte técnico

15 de febrero de 2019

### <a name="recommendations"></a>Recomendaciones 

Use [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) en su lugar. Proporciona la mayor parte de la funcionalidad de NamedEntityRecognitionSkill con una calidad superior. También tiene una información más completa en sus campos de salida compleja.

Para migrar a la [aptitud de reconocimiento de entidades con nombre](cognitive-search-skill-entity-recognition.md), tendrá que realizar uno o varios de los siguientes cambios en la definición de la aptitud. Puede actualizar la definición de aptitud mediante la [API de actualización de conjuntos de aptitudes](https://docs.microsoft.com/rest/api/searchservice/update-skillset).

_Nota_: Actualmente, no se admite la puntuación de confianza como concepto. Se admitirá en un futuro próximo. El parámetro `minimumPrecision` existe en `EntityRecognitionSkill` para usarse en un futuro y para la compatibilidad con versiones anteriores.

1. *(Obligatorio)* Cambie `@odata.type` de `"#Microsoft.Skills.Text.NamedEntityRecognitionSkill"` a `"#Microsoft.Skills.Text.EntityRecognitionSkill"`.

2. *(Opcional)* Si está usando la salida `entities`, use la salida de colección compleja `namedEntities` de `EntityRecognitionSkill` en su lugar. Puede usar `targetName` en la definición de aptitud que se asigne a una anotación denominada "`entities`".

3. *(Opcional)* Si no especifica explícitamente `categories`, `EntityRecognitionSkill` puede devolver un tipo diferente de categorías, además de las que eran compatibles con `NamedEntityRecognitionSkill`. Si no desea este comportamiento, asegúrese de establecer explícitamente el parámetro `categories` en `["Person", "Location", "Organization"]`.

    _Definiciones de migración de ejemplo_

    * Migración simple

        _(Antes) Definición de aptitud NamedEntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
        _(Después) Definición de aptitud EntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
    
    * Migración ligeramente complicada

        _(Antes) Definición de aptitud NamedEntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "entities"
            }
            ]
        }
        ```
        _(Después) Definición de aptitud EntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person", "Location", "Organization" ],
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "namedEntities",
                "targetName": "entities"
            }
            ]
        }
        ```

## <a name="see-also"></a>Otras referencias

+ [Aptitudes predefinidas](cognitive-search-predefined-skills.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Aptitud cognitiva Reconocimiento de entidades con nombre](cognitive-search-skill-entity-recognition.md)
