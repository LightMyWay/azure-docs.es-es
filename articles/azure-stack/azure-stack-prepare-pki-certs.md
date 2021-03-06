---
title: Preparación de certificados de infraestructura de clave pública de Azure Stack para la implementación de sistemas integrados o rotación de secretos | Microsoft Docs
description: Describe cómo preparar los certificados PKI de Azure Stack para sus sistemas integrados.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 7a4c0d62fd1ea675c6569840c26ed1fb8e388850
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53631488"
---
# <a name="prepare-azure-stack-pki-certificates-for-use-in-deployment-or-rotation"></a>Preparación de certificados PKI de Azure Stack para la implementación o rotación
Los archivos de certificado [obtenidos de la entidad de certificación que eligió](azure-stack-get-pki-certs.md) deben importarse y exportarse con propiedades que coincidan con los requisitos de certificado de Azure Stack.


## <a name="prepare-certificates-for-deployment"></a>Preparación de los certificados para la implementación
Siga estos pasos para preparar y validar los certificados PKI de Azure Stack que se usarán para implementar un nuevo entorno de Azure Stack o para rotar los secretos en un entorno existente de Azure Stack: 

### <a name="import-the-certificate"></a>Importación del certificado

1.  Copie las versiones de los certificados originales [obtenidas de la entidad de certificación de elección](azure-stack-get-pki-certs.md) en un directorio del host de implementación. 
  > [!WARNING]
  > No copie los archivos que ya se han importado, exportado o modificado de alguna forma de los archivos proporcionados directamente por la entidad de certificación.

1.  Haga clic con el botón derecho en el certificado y seleccione **Instalar certificado** o **Instalar PFX**, dependiendo de cómo se entrega el certificado desde la entidad de certificación.

1. En el **Asistente para importar certificados**, seleccione **Máquina local** como la ubicación de importación. Seleccione **Next** (Siguiente). En la siguiente pantalla, vuelva a hacer clic en Siguiente.

    ![Ubicación de importación de la máquina local](./media/prepare-pki-certs/1.png)

1.  Elija **Colocar todos los certificados en el siguiente almacén** y, luego, seleccione **Confianza empresarial** como ubicación. Haga clic en **Aceptar** para cerrar el cuadro de diálogo de selección de almacén de certificados y, luego, haga clic en **Siguiente**.

    ![Configurar el almacén de certificados](./media/prepare-pki-certs/3.png)

     a. Si va a importar un PFX, aparecerá un cuadro de diálogo adicional. En la página **Protección de clave privada**, escriba la contraseña de los archivos de certificado y, luego, habilite **Marcar esta clave como exportable. Esto permite realizar copias de seguridad o transportar las claves en un momento posterior**. Seleccione **Next** (Siguiente).

    ![Marcar clave como exportable](./media/prepare-pki-certs/2.png)

1. Haga clic en Finalizar para completar la importación.

### <a name="export-the-certificate"></a>Exportación de certificados

Abra la consola MMC del administrador de certificados y conéctese al almacén de certificados de la máquina local.

1. Abra Microsoft Management Console, en Windows 10 haga clic con el botón derecho en el menú Inicio y, después, haga clic en Ejecutar. Escriba **mmc** y haga clic en Aceptar.

1. Haga clic en Archivo, Agregar o quitar complemento y, después, seleccione certificados y haga clic en Agregar.

    ![Complemento Agregar certificados](./media/prepare-pki-certs/mmc-2.png)
 
1. Seleccione la cuenta Equipo, haga clic en Siguiente, seleccione Equipo local y, después, haga clic en Finalizar. Haga clic en Aceptar para cerrar la página Agregar o quitar complemento.

    ![Complemento Agregar certificados](./media/prepare-pki-certs/mmc-3.png)

1. Vaya a Certificados > Confianza empresarial > Ubicación de certificado. Compruebe que ve el certificado a la derecha.

1. En la barra de tareas de la consola del administrador de certificados, seleccione **Acciones** > **Todas las tareas** > **Exportar**. Seleccione **Next** (Siguiente).

  > [!NOTE]
  > Dependiendo del número de certificados de Azure Stack que tenga, es posible que necesite completar este proceso más de una vez.

1. Seleccione **Exportar la clave privada** y, después, haga clic en **Siguiente**.

1. En la sección Formato de archivo de exportación, seleccione **Exportar todas las propiedades extendidas** y, luego, haga clic en **Siguiente**.

1. Seleccione **Contraseña** y proporcione una contraseña para los certificados. Recuerde esta contraseña, ya que se utiliza como un parámetro de implementación. Seleccione **Next** (Siguiente).

1. Elija un nombre de archivo y la ubicación del archivo PFX que se va a exportar. Seleccione **Next** (Siguiente).

1. Seleccione **Finalizar**.

## <a name="next-steps"></a>Pasos siguientes
[Validar los certificados PKI](azure-stack-validate-pki-certs.md)
