---
title: Configuración de SKU para una oferta de aplicación de Azure | Microsoft Docs
description: Cómo configurar las SKU para una aplicación administrada de Azure aplicación y una plantilla de solución de Azure.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: pbutlerm
ms.openlocfilehash: 0af7a7fd43bba46de6faa770bf3042fbf58a90f6
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53196294"
---
# <a name="azure-application-skus-tab"></a>Pestaña SKU de aplicación de Azure

En este artículo se describe cómo usar la pestaña SKU para crear SKU para una aplicación de Azure. 

>[!IMPORTANT] 
Los pasos para configurar una SKU son diferentes para una oferta de aplicación administrada y una oferta de plantilla de solución. Estas diferencias se documentan en este artículo. 

## <a name="configure-azure-application-skus"></a>Configuración de SKU de aplicación de Azure

### <a name="create-a-new-sku"></a>Creación de una nueva SKU

Siga estos pasos para crear una nueva SKU:

1. Seleccione la pestaña **SKU**.
2. En SKU, seleccione **+ Nueva SKU**.

    ![Mensaje Nueva SKU](./media/azureapp-plus-sku.png)

3. En la ventana emergente Nueva SKU, escriba un **Id. de SKU**. Este id. está limitado a 50 caracteres y debe constar únicamente de caracteres alfanuméricos en minúscula, guiones o caracteres de subrayado. El identificador de SKU no puede terminar con un guion.
4. Este identificador de SKU se muestra a los clientes en las direcciones URL de producto, las plantillas de Resource Manager (si corresponde) y los informes de facturación. No se puede modificar este identificador una vez publicada la oferta.

### <a name="sku-details-for-a-solution-template"></a>Detalles de la SKU de una plantilla de solución

Proporcione la siguiente configuración de la SKU:

- **Título**: título para la SKU. Este título aparece en la galería para este elemento.
- **Resumen**: descripción breve resumida de la SKU. (La longitud máxima es de 100 caracteres).
- **Description**: descripción detallada de la SKU.
- **Tipo de SKU**: una lista desplegable con estos valores: "Plantilla de solución" y "Aplicación administrada". Para este escenario, seleccione **Plantilla de solución**.
- **Cloud Availability** (Disponibilidad de la nube): ubicación de la SKU. El valor predeterminado es **Azure público**.
Azure público: Esta máquina virtual se implementará a los clientes de todas las regiones de Azure público que tengan la integración de Marketplace.
- **Nube de Azure Government**: Esta máquina virtual se implementará en la nube de Azure Government. Antes de publicar en [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), Microsoft recomienda que los publicadores prueben y validen que la solución funcione según lo previsto en el entorno. Para realizar copias intermedias y pruebas, solicite una [cuenta de prueba](https://azure.microsoft.com/offers/ms-azr-usgov-0044p/).

  >[!NOTE] 
  >Microsoft Azure Government es una nube de la comunidad gubernamental con acceso controlado para los clientes de asociados tribales, locales, estatales y federales de Estados Unidos y asociados aptos para abastecer a tales entidades.

- **Is this a Private SKU?** (¿Es una SKU privada?) - Seleccione Sí si esta SKU solo está disponible para un grupo selecto de clientes.

    ![Formulario Detalles de la SKU de una plantilla de solución](./media/azureapp-sku-details-solutiontemplate.png)

### <a name="sku-details-for-managed-application"></a>Detalles de la SKU para una aplicación administrada

La siguiente captura de pantalla muestra el formulario SKU Details (Detalles de la SKU) de una aplicación administrada.

   ![Formulario Detalles de la SKU para una aplicación administrada](./media/azureapp-sku-details-managedapplication.png)

Configure las siguientes opciones de la SKU:

- **Título**: título para la SKU. Este título aparece en la galería para este elemento.
- **Resumen**: descripción breve resumida de la SKU. (La longitud máxima es de 100 caracteres).
- **Description**: descripción detallada de la SKU.
- **Tipo de SKU**: una lista desplegable con estos valores: "Plantilla de solución" y "Aplicación administrada". En este escenario, seleccione **Aplicación administrada**.
- **Cloud Availability** (Disponibilidad de la nube): ubicación de la SKU. El valor predeterminado es **Azure público**.
- **Azure público**: Esta máquina virtual se implementará a los clientes de todas las regiones de Azure público que tengan la integración de Marketplace.
- **Nube de Azure Government**: Esta máquina virtual se implementará en la nube de Azure Government. Antes de publicar en [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), Microsoft recomienda que los publicadores prueben y validen que la solución funcione según lo previsto en el entorno. Para realizar copias intermedias y pruebas, solicite una [cuenta de prueba](https://azure.microsoft.com/offers/ms-azr-usgov-0044p/).

  >[!NOTE] 
  >Microsoft Azure Government es una nube de la comunidad gubernamental con acceso controlado para los clientes de asociados tribales, locales, estatales y federales de Estados Unidos y asociados aptos para abastecer a tales entidades.

- **Is this a Private SKU?** (¿Es una SKU privada?) - Seleccione Sí si esta SKU solo está disponible para un grupo selecto de clientes.
- **Disponibilidad de país/región**: Use **Seleccionar regiones** para ver la lista de países o regiones que están disponibles. Compruebe cada país o región y, luego, seleccione **Aceptar** para guardar sus selecciones. 

   ![Lista de disponibilidad por región y región](./media/azure-app-select-country-region.png)

- **Old Pricing** (Precios antiguos): Especifique el precio de la SKU, en USD por mes. Los precios se establecen en la moneda local con tasas de cambio actuales tras la configuración. Valide estos ajustes, puesto que, en última instancia, usted es el propietario de esta configuración. Para establecer o ver el precio de cada país o región individualmente, exporte la hoja de cálculo de precios e impórtela con precios personalizados.

  >[!NOTE]
  >Guarde los cambios de precios para habilitar la exportación/importación de los datos de precios.

- **Simplified Currency Pricing** (Precios de moneda simplificada): Especifique el precio de la SKU, en USD por mes. Debe ser igual que Old Pricing. Para más información, consulte [Precios en moneda simplificada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-update-existing-offer#simplified-currency-pricing).

### <a name="package-details-for-solution-template"></a>Detalles del paquete para una plantilla de solución

Proporcione los detalles del paquete siguientes:

- **Versión**: Versión del paquete que va a cargar. Las etiquetas de versión deben ser del tipo X.Y.Z, donde X, Y y Z son números enteros.
- **Package file (.zip)** (Archivo de paquete [.zip]): Este paquete contiene los siguientes archivos, guardados en un archivo ZIP.
  - MainTemplate.json: archivo de plantilla de implementación que se usa para implementar la solución o la aplicación y para crear los recursos que se definen en esta. Para más información, consulte [cómo crear archivos de plantilla de implementación](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template).
  - createUIDefinition.json: Azure Portal usa este archivo para generar la interfaz de usuario para el aprovisionamiento de esta solución o aplicación. Para más información, consulte [Creación de la interfaz de usuario de Azure Portal para una aplicación administrada](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview).

  >[!IMPORTANT] 
  >Este paquete debe contener los demás scripts o plantillas anidadas que sean necesarios para aprovisionar esta aplicación. La carpeta raíz debe contener los archivos MainTemplate.json y createUIDefinition.json.

   ![Detalles del paquete para una plantilla de solución](./media/azureapp-sku-pkgdetails-solutiontemplate.png)

### <a name="package-details-for-managed-application"></a>Detalles del paquete para una aplicación administrada

Proporcione los detalles del paquete siguientes:

- **Versión**: Versión del paquete que va a cargar. Las etiquetas de versión deben ser del tipo X.Y.Z, donde X, Y y Z son números enteros.
- **Package file (.zip)** (Archivo de paquete [.zip]): Este paquete contiene los siguientes archivos, guardados en un archivo ZIP.
  - applianceMainTemplate.json: Archivo de plantilla de implementación que se usa para implementar la solución o aplicación, y crear los recursos que se definen. Para más información, consulte [Inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal). 
  - applianceCreateUIDefinition.json: Azure Portal usa este archivo para generar la interfaz de usuario para el aprovisionamiento de esta solución o aplicación. Para más información, consulte [Creación de la interfaz de usuario de Azure Portal para una aplicación administrada](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview).
  - mainTemplate.json: Archivo de plantilla que solo contiene el recurso Microsoft.Solution/appliances. Para más información, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates). <br>
Tenga en cuenta las siguientes propiedades de clave de este recurso:
    - "kind": El valor debe ser "Marketplace" en el caso de una aplicación administrada de Marketplace.
    - "ManagedResourceGroupId": Grupo de recursos de la suscripción del cliente donde se implementarán todos los recursos definidos en applianceMainTemplate.json.
    - "PublisherPackageId": Cadena que identifica el paquete de forma exclusiva. Este valor se debe generar como se indica a continuación: es una concatenación de [publisherId].[OfferId]-preview[SKUID].[PackageVersion].

  >[!IMPORTANT] 
  >Este paquete debe contener los demás scripts o plantillas anidadas que sean necesarios para aprovisionar esta aplicación. Estos archivos deben estar en la carpeta raíz:  MainTemplate.json, applianceMainTemplate.json y applianceCreateUIDefinition.json.

- **Id. de inquilino**: Id. de inquilino de Azure Active Directory de su organización.
- **Enable JIT Access?** (¿Habilitar acceso JIT?): Seleccione **Sí** para habilitar el acceso de administración Just-In-Time para implementaciones de cliente con esta oferta.

  >[!NOTE] 
  >Si habilita JIT, debe actualizar el archivo CreateUiDefinition.json para admitir el acceso JIT.

   ![Detalles del paquete para una aplicación administrada](./media/azureapp-sku-pkgdetails-managedapplication.png)

Para una aplicación administrada, debe configurar Autorización y Configuración de directivas.

#### <a name="authorization"></a>Autorización

Agregue el identificador de Azure Active Directory (AD) del usuario, grupo o aplicación a los que quiere conceder permisos al grupo de recursos administrados. El id. de definición de roles indica el permiso que se concede. Puede ser un rol de Propietario, Colaborador o cualquier rol personalizado.

#### <a name="policy-settings"></a>Configuración de directivas

Agregue las directivas con las que cumple la aplicación administrada. Para más información acerca de las directivas de recursos de Azure, consulte [¿Qué es Azure Policy?](https://docs.microsoft.com/azure/azure-policy/azure-policy-introduction) 


   ![Autorización y configuración de directivas para una aplicación administrada](./media/azureapp-sku-details-managedapp-auth-policy.png)

**Para crear una nueva autorización:**

1. En **Autorización**, seleccione **+ Nueva autorización**.
2. Para **ID de la entidad de seguridad** , escriba el identificador de Azure Active Directory (AD) del usuario, grupo o aplicación a los que quiere conceder permisos al grupo de recursos administrados. La definición de roles indica el permiso que se concede.
3. Para **Definición de roles**, seleccione una de estas opciones en la lista desplegable:  Propietario o Colaborador. Para más información, consulte [Roles integrados en los recursos de Azure](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

>[!NOTE] 
>Se pueden agregar varias autorizaciones. Aun así, se recomienda crear un grupo de usuarios de Active Directory y especificar su identificador en "PrincipalId". Esto permite agregar más usuarios al grupo sin necesidad de actualizar la SKU.

**Para crear una nueva directiva:**

1. En **Configuración de directivas**, seleccione **+ Nueva directiva**.
2. En **Nombre de la directiva**, escriba un nombre para la directiva. La longitud máxima del nombre es de 50 caracteres.
3. Para **Directivas**, seleccione una de estas opciones en la lista desplegable. Seleccione la directiva que desea habilitar para el proveedor de datos cuando la aplicación use los datos. Para más información, consulte [Ejemplos de Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/index).

    ![Configuración de directivas para una aplicación administrada](./media/azureapp-sku-policy-settings.png)

4. Para **SKU de directiva**, seleccione Gratis o Estándar como tipo de SKU de la directiva. La SKU Estándar es obligatoria para las directivas de auditoría.

## <a name="next-steps"></a>Pasos siguientes

[Pestaña Marketplace](./cpp-marketplace-tab.md)