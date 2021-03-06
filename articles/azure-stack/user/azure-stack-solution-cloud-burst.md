---
title: Creación de soluciones de escalado de toda la nube con Azure | Microsoft Docs
description: Aprenda a crear soluciones de escalado de toda la nube con Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 57624133b249a8ec2ece90eac4a64729e4d15151
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968210"
---
# <a name="tutorial-create-cross-cloud-scaling-solutions-with-azure"></a>Tutorial: Creación de soluciones de escalado de toda la nube con Azure

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Aprenda a crear una solución para toda la nube que proporcione un proceso desencadenado manualmente para cambiar de una aplicación web hospedada en Azure Stack a una aplicación web hospedada en Azure con escalado automático mediante Traffic Manager, con la garantía de una utilidad en la nube flexible y escalable cuando hay poca carga.

Con este patrón, puede que el inquilino no esté preparado para ejecutar la aplicación en la nube pública. Sin embargo, puede que no sea económicamente viable para la empresa mantener la capacidad necesaria en el entorno local para controlar los picos en la demanda de la aplicación. El inquilino puede aprovechar la elasticidad de la nube pública en su solución local.

En este tutorial, creará un entorno de ejemplo para:

> [!div class="checklist"]
> - Crear una aplicación web de varios nodos.
> - Configurar y administrar el proceso de implementación continua (CD).
> - Publicar la aplicación web en Azure Stack.
> - Crear una versión.
> - Supervisar y realizar un seguimiento de las implementaciones.

> [!Tip]  
> ![hybrid-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack es una extensión de Azure. Azure Stack aporta la agilidad y la innovación de la informática en la nube a su entorno local y hace posible la única nube híbrida que le permite crear e implementar aplicaciones híbridas en cualquier parte.  
> 
> En las notas del producto [Consideraciones de diseño para aplicaciones híbridas](https://aka.ms/hybrid-cloud-applications-pillars) se revisan los pilares de la calidad de software (selección de ubicación, escalabilidad, disponibilidad, resistencia, manejabilidad y seguridad) para diseñar, implementar y usar aplicaciones híbridas. Las consideraciones de diseño ayudan a optimizar el diseño de aplicaciones híbridas y reducen los desafíos en los entornos de producción.

## <a name="prerequisites"></a>Requisitos previos

-   Suscripción de Azure. Si es necesario, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de comenzar.

- Un sistema integrado de Azure Stack o la implementación del kit de desarrollo de Azure Stack.
    - Encontrará instrucciones para la instalación de Azure Stack en [Instalación del Kit de desarrollo de Azure Stack](../asdk/asdk-install.md).
    - [https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1) Esta instalación puede tardar algunas horas en completarse.

-   Implemente servicios PaaS de [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) para Azure Stack.

-   Cree [planes u ofertas](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) dentro del entorno de Azure Stack.

-   [Cree una suscripción de inquilino](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) dentro del entorno de Azure Stack.

-   Cree una aplicación web dentro de la suscripción de inquilino. Anote la nueva dirección URL de aplicación web, ya que la usará más adelante.

-   Implemente la máquina virtual de Azure Pipelines dentro de la suscripción de inquilino.

-   Se requiere una máquina virtual Windows Server 2016 con .NET 3.5. Esta máquina virtual se compilará en la suscripción del inquilino en Azure Stack como el agente de compilación privado.

-   [Windows Server 2016 con la imagen de máquina virtual de SQL 2017](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-vm-image#add-a-vm-image-through-the-portal) está disponible en el Marketplace de Azure Stack. Si esta imagen no está disponible, trabaje junto con un operador de Azure Stack para garantizar que se agrega al entorno.

## <a name="issues-and-considerations"></a>Problemas y consideraciones

### <a name="scalability-considerations"></a>Consideraciones sobre escalabilidad

El componente clave del escalado de toda la nube es la posibilidad de proporcionar un escalado inmediato a petición entre la infraestructura en la nube pública y local, que ofrezca unos servicios coherentes y confiables según la demanda.

### <a name="availability-considerations"></a>Consideraciones sobre disponibilidad

Asegúrese de que las aplicaciones implementadas localmente están configuradas para una alta disponibilidad mediante la configuración del hardware local y la implementación de software.

### <a name="manageability-considerations"></a>Consideraciones sobre la manejabilidad

La solución para toda la nube garantiza una administración sin problemas y una interfaz familiar entre entornos. Se recomienda usar PowerShell para la administración multiplataforma.

## <a name="cross-cloud-scaling"></a>Escalado de toda la nube

### <a name="obtain-a-custom-domain-and-configure-dns"></a>Obtención de un dominio personalizado y configuración de DNS

Actualice el archivo de zona DNS para el dominio. Azure AD comprobará la propiedad del nombre de dominio personalizado. Use [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) para los registros DNS de Azure/Office 365/external dentro de Azure, o bien agregue la entrada DNS a [un registrador DNS diferente](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/).

1.  Registre un dominio personalizado con un registrador público.

2.  Inicie sesión en el registrador de nombres de dominio para el dominio. Puede que se requiera un administrador autorizado para realizar las actualizaciones de DNS. 

3.  Actualice el archivo de zona DNS para el dominio. Para ello, agregue la entrada DNS que Azure AD proporcione. (La entrada DNS no afectará al enrutamiento de correo electrónico ni a los comportamientos de hospedaje web). 

### <a name="create-a-default-multi-node-web-app-in-azure-stack"></a>Creación de una aplicación web predeterminada con varios nodos en Azure Stack

Configure la canalización de integración e implementación continuas (CI/CD) híbrida para implementar la aplicación web en Azure y Azure Stack e insertar automáticamente los cambios en ambas nubes.

> [!Note]  
> Se requiere Azure Stack con las imágenes adecuadas sindicadas para ejecutarse (Windows Server y SQL) y la implementación de App Service. Revise la sección "[Antes de empezar a trabajar con App Service en Azure Stack](../azure-stack-app-service-before-you-get-started.md)" de la documentación de App Service para el operador de Azure Stack.

### <a name="add-code-to-azure-repos"></a>Adición de código a Azure Repos

Azure Repos

1. Inicie sesión en Azure Repos con una cuenta que tenga derechos de creación de proyectos en Azure Repos.

    La canalización de CI/CD híbrida se puede aplicar al código de aplicación y al código de infraestructura. Use [plantillas de Azure Resource Manager](https://azure.microsoft.com/resources/templates/) tanto para el desarrollo en la nube hospedado como para el privado.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image1.JPG)

2. **Clone el repositorio**; para ello, cree y abra la aplicación web predeterminada.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image2.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Creación de una implementación de aplicación web autocontenida para App Services en ambas nubes

1.  Edite el archivo **WebApplication.csproj**. Seleccione **Runtimeidentifier** y agregue **win10-x64**. (Consulte la documentación de [Implementaciones autocontenidas](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd)). 

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image3.png)

2.  Inserte el código en Azure Repos mediante Team Explorer.

3.  Confirme que el código de la aplicación se ha insertado en Azure Repos.

## <a name="create-the-build-definition"></a>Creación de la definición de compilación

1. Inicie sesión en Azure Pipelines para confirmar la posibilidad de crear definiciones de compilación.

2. Agregue el código **-r win10-x64**. Esto es necesario para activar una implementación autocontenida con .Net Core.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image4.png)

3. Ejecute la compilación. El proceso de [compilación de implementación autocontenida](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) publicará los artefactos que se pueden ejecutar en Azure y Azure Stack.

## <a name="use-an-azure-hosted-agent"></a>Uso de un agente hospedado de Azure

El uso de un agente hospedado en Azure Pipelines es una opción adecuada para compilar e implementar aplicaciones web. Microsoft Azure realiza automáticamente el mantenimiento y las actualizaciones, lo que permite el desarrollo, la prueba y la implementación de forma continua e ininterrumpida.

### <a name="manage-and-configure-the-cd-process"></a>Administración y configuración del proceso de CD

Azure Pipelines y6 Azure DevOps Server proporcionan una canalización con una gran capacidad de configuración y administración para versiones de varios entornos, como desarrollo, ensayo, control de calidad (QA) y producción, lo que incluye las aprobaciones necesarias en etapas específicas.

## <a name="create-release-definition"></a>Creación de la definición de versión

![Texto alternativo](media/azure-stack-solution-cloud-burst/image5.png)

1.  Seleccione el símbolo **+** para agregar una nueva versión en la **pestaña Versiones** de la página Compilación y versión de VSO.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image6.png)

2. Aplique la plantilla Implementación de Azure App Service.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image7.png)

3. En Agregar artefacto, agregue el artefacto para la aplicación de compilación de nube de Azure.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image8.png)

4. En la pestaña Canalización, seleccione el vínculo **Fase, Tarea** del entorno y establezca los valores del entorno de nube de Azure.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image9.png)

5. Establezca el **nombre del entorno** y seleccione la **suscripción** de Azure como el punto de conexión de la nube de Azure.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image10.png)

6. En Nombre del entorno, establezca el **nombre del servicio de aplicación de Azure** necesario.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image11.png)

7. Escriba **Hospedado VS2017** en Cola de agentes para el entorno hospedado de la nube de Azure.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image12.png)

8. En el menú de implementación de Azure App Service, seleccione el **paquete o carpeta** válidos para el entorno. Seleccione **Aceptar** para la **ubicación de carpeta**.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image13.png)

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image14.png)

9. Guarde todos los cambios y vuelva a la **canalización de versión**.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image15.png)

10. Agregue un nuevo artefacto mediante la selección de la compilación de la aplicación de Azure Stack.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image16.png)

11. Agregue un entorno más; para ello, aplique la plantilla Implementación de Azure App Service.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image17.png)

12. Asigne al nuevo entorno el nombre Azure Stack.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image18.png)

13. Busque el entorno de Azure Stack en la pestaña **Tarea**.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image19.png)

14. Seleccione la suscripción para el punto de conexión de Azure Stack.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image20.png)

15. Establezca el nombre de la aplicación web de Azure Stack como el nombre del servicio de aplicación.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image21.png)

16. Seleccione el agente de Azure Stack.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image22.png)

17. En la sección de implementación de Azure App Service, seleccione el **paquete o carpeta** válidos para el entorno. Seleccione **Aceptar** para la ubicación de carpeta.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image23.png)

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image24.png)

18. En la pestaña Variable, agregue una variable denominada `VSTS\_ARM\_REST\_IGNORE\_SSL\_ERRORS`, establezca su valor como **true** y defina el ámbito en Azure Stack.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image25.png)

19. Seleccione el icono del desencadenador de implementación **Continuo** en ambos artefactos y habilite el desencadenador de implementación **Continua**.

    ![Texto alternativo](media/azure-stack-solution-cloud-burst/image26.png)

20. Seleccione el icono de condiciones **previas a la implementación** en el entorno de Azure Stack y establezca el desencadenador en **After release** (Tras el lanzamiento).

21. Guarde todos los cambios.

> [!Note]  
> Algunos valores de configuración de las tareas se han definido automáticamente como [variables de entorno](https://docs.microsoft.com/azure/devops/pipelines/release/variables?view=vsts&tabs=batch#custom-variables) cuando se creó una definición de versión desde una plantilla. Estos valores de configuración no se pueden modificar en la configuración de tareas; para hacerlo, debe seleccionar el elemento del entorno principal.

## <a name="publish-to-azure-stack-via-visual-studio"></a>Publicación en Azure Stack a través de Visual Studio

Mediante la creación de puntos de conexión, una compilación de Visual Studio Online (VSTO) puede implementar aplicaciones de Azure Service en Azure Stack. Azure Pipelines se conecta con el agente de compilación, que, a su vez, se conecta con Azure Stack.

1.  Inicie sesión en VSTO y vaya a la página de configuración de la aplicación.

2.  En **Configuración**, seleccione **Seguridad**.

3.  En **Grupos de VSTS**, seleccione **Creadores de puntos de conexión**.

4.  En la pestaña **Miembros**, seleccione **Agregar**.

5.  En **Agregar usuarios y grupos**, escriba un nombre de usuario y seleccione ese usuario en la lista de usuarios.

6.  Seleccione **Save changes** (Guardar los cambios).

7.  En la lista **Grupos de VSTS**, seleccione **Administradores de puntos de conexión**.

8.  En la pestaña **Miembros**, seleccione **Agregar**.

9.  En **Agregar usuarios y grupos**, escriba un nombre de usuario y seleccione ese usuario en la lista de usuarios.

10. Seleccione **Save changes** (Guardar los cambios).

Ahora que existe la información del punto de conexión, la conexión de Azure Pipelines con Azure Stack está lista para su uso. El agente de compilación de Azure Stack obtiene instrucciones de Azure Pipelines y, a continuación, el agente transmite la información del punto de conexión para la comunicación con Azure Stack.

## <a name="develop-the-application-build"></a>Desarrollo de la aplicación

> [!Note]  
> Se requiere Azure Stack con las imágenes adecuadas sindicadas para ejecutarse (Windows Server y SQL) y la implementación de App Service. Revise la sección "[Antes de empezar a trabajar con App Service en Azure Stack](../azure-stack-app-service-before-you-get-started.md)" de la documentación de App Service para el operador de Azure Stack.

Use [plantillas de Azure Resource Manager como código de aplicación web](https://azure.microsoft.com/resources/templates/) de Azure Repos para implementar en ambas nubes.

### <a name="add-code-to-a-azure-repos-project"></a>Adición de código a un proyecto de Azure Repos

1.  Inicie sesión en Azure Repos con una cuenta que tenga derechos de creación de proyectos en Azure Stack. La captura de pantalla siguiente muestra cómo conectarse al proyecto HybridCICD.

2.  **Clone el repositorio**; para ello, cree y abra la aplicación web predeterminada.

#### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Creación de una implementación de aplicación web autocontenida para App Services en ambas nubes

1.  Edite el archivo **WebApplication.csproj**: Seleccione **Runtimeidentifier** y agregue win10-x64. Para más información, consulte la documentación de la [implementación independiente](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd).

2.  Compruebe el código en Azure Repos mediante Team Explorer.

3.  Confirme que el código de la aplicación se ha insertado en Azure Repos.

### <a name="create-the-build-definition"></a>Creación de la definición de compilación

1.  Inicie sesión en Azure Pipelines con una cuenta que pueda crear una definición de compilación.

2.  Vaya a la página **Build Web Application** (Compilar aplicación web) del proyecto.

3.  En **Argumentos**, agregue el código **-r win10-x64**. Esto es necesario para desencadenar una implementación independiente con .Net Core.

4.  Ejecute la compilación. El proceso de [compilación de implementación autocontenida](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) publicará los artefactos que se pueden ejecutar en Azure y Azure Stack.

#### <a name="use-an-azure-hosted-build-agent"></a>Uso de un agente de compilación hospedado en Azure

El uso de un agente de compilación hospedado en Azure Pipelines es una opción adecuada para compilar e implementar aplicaciones web. Microsoft Azure realiza automáticamente el mantenimiento y las actualizaciones, lo que permite un ciclo de desarrollo ininterrumpido y continuo.

### <a name="configure-the-continuous-deployment-cd-process"></a>Configuración del proceso de implementación continua (CD)

Azure Pipelines y Azure DevOps Server proporcionan una canalización con una gran capacidad de configuración y administración para versiones de varios entornos, como desarrollo, ensayo, control de calidad (QA) y producción. Este proceso puede incluir la necesidad de aprobaciones en determinadas fases del ciclo de vida de la aplicación.

#### <a name="create-release-definition"></a>Creación de la definición de versión

La creación de una definición de versión es el último paso en el proceso de compilación de la aplicación. Esta definición de versión se usa para crear una versión e implementar una compilación.

1.  Inicie sesión en Azure Pipelines y vaya a **Compilación y versión** del proyecto.

2.  En la pestaña **Versiones**, seleccione **[ + ]** y, a continuación, elija **Crear definición de versión**.

3.  En **Seleccionar una plantilla**, elija **Implementación de Azure App Service** y, a continuación, seleccione **Aplicar**.

4.  En **Agregar artefacto**, en **Origen (definición de compilación)**, seleccione la aplicación de compilación en la nube de Azure.

5.  En la pestaña **Canalización**, seleccione el vínculo **1 fase**, **1 tarea** para **Ver tareas de entorno**.

6.  En la pestaña **Tareas**, escriba Azure como **Nombre del entorno** y seleccione AzureCloud Traders-Web EP en la lista **Suscripción de Azure**.

7.  Escriba el **nombre de la instancia de Azure App Service**, que es `northwindtraders` en la siguiente captura de pantalla.

8.  Para la fase de agente, seleccione **VS2017 hospedado** en la lista **Cola de agentes**.

9.  En **Implementar Azure App Service**, seleccione el valor de **Paquete o carpeta** válido para el entorno.

10. En **Seleccionar archivo o carpeta**, seleccione **Aceptar** para **Ubicación**.

11. Guarde todos los cambios y vuelva a **Canalización**.

12. En la pestaña **Canalización**, seleccione **Agregar artefacto** y elija **NorthwindCloud Traders-Vessel** en la lista **Origen (definición de compilación)**.

13. En **Seleccionar una plantilla**, agregue otro entorno. Elija **Implementación de Azure App Service** y, a continuación, seleccione **Aplicar**.

14. Escriba `Azure Stack` como **Nombre del entorno**.

15. En la pestaña **Tareas**, busque y seleccione Azure Stack.

16. En la lista **Suscripción de Azure**, seleccione **AzureStack Traders-Vessel EP** para el punto de conexión de Azure Stack.

17. Escriba el nombre de la aplicación web de Azure Stack como el **Nombre de App Service**.

18. En **Selección de agente**, elija **AzureStack - b Douglas Fir** en la lista **Cola de agentes**.

19. En **Implementar Azure App Service**, seleccione el valor de **Paquete o carpeta** válido para el entorno. En **Seleccionar archivo o carpeta**, seleccione **Aceptar** para la **Ubicación** de la carpeta.

20. En la pestaña **Variables**, busque la variable denominada `VSTS\_ARM\_REST\_IGNORE\_SSL\_ERRORS`. Establezca el valor de la variable en **true** y establezca su ámbito en **Azure Stack**.

21. En la pestaña **Canalización**, seleccione el icono del **Desencadenador de implementación continua** para el artefacto NorthwindCloud Traders-Web y establezca **Desencadenador de implementación continua** en **Habilitado**. Lo mismo para el artefacto **NorthwindCloud Traders-Vessel**.

22. En el entorno de Azure Stack, seleccione el icono de **Condiciones anteriores a la implementación** y establezca el desencadenador en **Después de la versión**.

23. Guarde todos los cambios.

> [!Note]  
> Algunos valores de configuración de las tareas se han definido automáticamente como [variables de entorno](https://docs.microsoft.com/azure/devops/pipelines/release/variables?view=vsts&tabs=batch#custom-variables) cuando se creó una definición de versión desde una plantilla. Esta configuración no puede modificarse en la configuración de la tarea, pero se puede modificar en los elementos del entorno primario.

## <a name="create-a-release"></a>Creación de una versión

1.  En la pestaña **Canalización**, abra la lista **Versión** y elija **Crear versión**.

2.  Escriba una descripción para la versión, compruebe que se seleccionan los artefactos correctos y, a continuación, elija **Crear**. Transcurridos unos instantes, aparece un mensaje emergente que indica que se ha creado la nueva versión y el nombre de la versión se muestra como un vínculo. Elija el vínculo para ver la página de resumen de la versión.

3.  La página de resumen de la versión muestra detalles acerca de la versión. En la siguiente captura de pantalla de "Release-2", la sección **Entornos** muestra el **Estado de implementación** para Azure como "En curso" y el estado para Azure Stack como "Correcto". Cuando el estado de implementación para el entorno de Azure cambie a "Correcto", aparece un mensaje emergente que indica que la versión está lista para su aprobación. Cuando una implementación está pendiente o se ha producido un error, se muestra un icono de información **(i)** azul. Desplácese sobre el icono para ver un elemento emergente que contiene la razón del retraso o los errores.

4.  Otras vistas, como la lista de versiones, también muestran un icono que indica que la aprobación está pendiente. El elemento emergente para este icono muestra el nombre del entorno y más detalles relacionados con la implementación. Es fácil para un administrador consultar el progreso general de las versiones y ver qué versiones están pendientes de aprobación.

## <a name="monitor-and-track-deployments"></a>Supervisión y seguimiento de implementaciones

1.  En la página de resumen **Release-2**, seleccione **Registros**. Durante una implementación, esta página muestra el registro en directo del agente. El panel izquierdo muestra el estado de cada operación de la implementación para cada entorno.

2.  Elija el icono de persona en la columna **Acción** correspondiente a una aprobación previa o posterior a la implementación para ver quién aprobó (o rechazó) la implementación y el mensaje que proporcionó.

3.  Una vez finalizada la implementación, se muestra el archivo de registro completo en el panel derecho. Seleccione cualquier **Paso** en el panel izquierdo para ver el archivo de registro de un único paso, como **Initialize job** (Inicializar trabajo). La posibilidad de ver registros individuales facilita realizar el seguimiento y la depuración de partes individuales de la implementación general. **Guarde** el archivo de registro de un paso o **descargue todos los registros como zip**.

4.  Abra la pestaña **Resumen** para ver información general sobre la versión. Esta vista muestra detalles de la compilación, los entornos en los que se implementó, el estado de implementación y otra información sobre la versión.

5.  Seleccione un vínculo de entorno (**Azure** o **Azure Stack**) para ver información acerca de las implementaciones existentes y las pendientes en un entorno específico. Use estas vistas como un modo rápido para comprobar que la misma compilación se implementó en ambos entornos.

6.  Abra la **aplicación de producción implementada** en el explorador. Por ejemplo, para el sitio web de Azure App Services, abra la dirección URL [http://[your-app-name\].azurewebsites.net](http:// [your-app-name].azurewebsites.net).

**La integración de Azure y Azure Stack proporciona una solución escalable para toda la nube**

Un servicio de nube múltiple flexible y sólido proporciona seguridad de los datos, copia de seguridad y redundancia, disponibilidad coherente y rápida, almacenamiento y distribución escalables, y compatibilidad con el enrutamiento con replicación geográfica. Este proceso desencadenado manualmente garantiza una conmutación confiable y eficaz entre las aplicaciones web hospedadas, lo que garantiza la disponibilidad inmediata de los datos más importantes. 

## <a name="next-steps"></a>Pasos siguientes
- Para más información sobre los patrones de nube de Azure, consulte [Patrones de diseño en la nube](https://docs.microsoft.com/azure/architecture/patterns).
