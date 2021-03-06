---
title: Uso de etiquetas en Azure Content Moderator | Microsoft Docs
description: Content Moderator incluye etiquetas predeterminadas, pero puede crear etiquetas personalizadas para moderar el contenido específico de su negocio.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: c462ff2937453f942db7fdd5b751f3356b6fe715
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310086"
---
# <a name="about-tags"></a>Acerca de las etiquetas #

Además de las dos etiquetas predeterminadas **isadult** (**a**) e **isracy** (**r**), puede crear etiquetas personalizadas para un examen más dirigido. Estas etiquetas personalizadas están luego disponibles para que los revisores humanos las asignen a imágenes o texto.

## <a name="create-tags"></a>Creación de etiquetas ##

1.  Seleccione las etiquetas en la pestaña Settings (Configuración).

  ![Etiquetas de moderación de contenido](images/tags-1.png)

2.  Escriba un código corto de dos letras para la etiqueta.
3.  Escriba un nombre para la etiqueta. Mantenga el nombre corto y descriptivo. Por ejemplo, **isbullying**.
4.  Escriba una descripción.
5.  Haga clic en Agregar.
6.  Cuando haya terminado de crear las etiquetas, haga clic en Save (Guardar).

![Definición de etiquetas de moderación de contenido](images/tags-2-define.png)

## <a name="using-custom-tags"></a>Uso de etiquetas personalizadas ##

Se pueden usar etiquetas personalizadas durante la revisión humana. Se muestran en la vista previa y el revisor las selecciona haciendo clic en ellas.

![Uso de etiquetas de moderación de contenido](images/tags-3-use.png)

Puede desactivar distintas etiquetas para distintas revisiones con solo activar o desactivar su visibilidad.
 
![Deshabilitación de etiquetas de moderación de contenido](images/tags-4-disable.png)

Aunque no puede eliminar las dos etiquetas predeterminadas, **isadult** e **isracy**, puede eliminar cualquier etiqueta personalizada que haya definido. Haga clic en la papelera junto a la etiqueta que quiere eliminar.

![Eliminación de etiquetas de moderación de contenido](images/tags-5-delete.png)

## <a name="next-steps"></a>Pasos siguientes ##

Para información sobre cómo usar etiquetas para la moderación de imágenes, consulte [Revisión de imágenes moderadas](Review-Moderated-Images.md).
