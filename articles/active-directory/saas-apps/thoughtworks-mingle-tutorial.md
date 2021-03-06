---
title: 'Tutorial: integración de Azure Active Directory con Thoughtworks Mingle | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Thoughtworks Mingle.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: a685b5702aa9f74f3e0abf2a06774a30ac0d996f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39437001"
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Tutorial: Integración de Azure Active Directory con Thoughtworks Mingle

En este tutorial, aprenderá a integrar Thoughtworks Mingle con Azure Active Directory (Azure AD).

La integración de Thoughtworks Mingle con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Thoughtworks Mingle
- Puede permitir que los usuarios inicien sesión automáticamente en Thoughtworks Mingle (inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Thoughtworks Mingle, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Thoughtworks Mingle

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregación de Thoughtworks Mingle desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a>Agregación de Thoughtworks Mingle desde la galería
Para configurar la integración de Thoughtworks Mingle en Azure AD, deberá agregar Thoughtworks Mingle desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Thoughtworks Mingle desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Thoughtworks Mingle**, seleccione **Thoughtworks Mingle** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Thoughtworks Mingle en la lista de resultados](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Thoughtworks Mingle con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Thoughtworks Mingle para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Thoughtworks Mingle.

Para establecer la relación de vínculo, en Thoughtworks Mingle, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Thoughtworks Mingle, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Thoughtworks Mingle](#create-a-thoughtworks-mingle-test-user)**: para tener un homólogo de Britta Simon en Thoughtworks Mingle que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Thoughtworks Mingle.

**Para configurar el inicio de sesión único de Azure AD con Thoughtworks Mingle, realice los pasos siguientes:**

1. En la página de integración de la aplicación **Thoughtworks Mingle** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

1. En la sección **Dominio y direcciones URL de Thoughtworks Mingle**, lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de Thoughtworks Mingle](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.mingle.thoughtworks.com`.

    > [!NOTE] 
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Thoughtworks Mingle](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) para obtener este valor. 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/thoughtworks-mingle-tutorial/tutorial_general_400.png)

1. Inicie sesión en su sitio de compañía de **Thoughtworks Mingle** como administrador.

1. Haga clic en la pestaña **Admin** (Administrador) y, luego, en **SSO Config** (Configuración de SSO).
   
    ![Pestaña de administrador](./media/thoughtworks-mingle-tutorial/ic785157.png "Configuración de SSO")

1. En la sección **Configuración de SSO** , lleve a cabo estos pasos:
   
    ![Configuración de SSO](./media/thoughtworks-mingle-tutorial/ic785158.png "Configuración de SSO")
    
    a. Para cargar el archivo de metadatos, haga clic en **Elegir archivo**. 

    b. Haga clic en **Guardar cambios**.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más sobre la característica de documentación insertada aquí: [Vista previa: Administración de inicio de sesión único para aplicaciones empresariales en el nuevo Azure Portal]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/thoughtworks-mingle-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/thoughtworks-mingle-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Botón Agregar](./media/thoughtworks-mingle-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Cuadro de diálogo Usuario](./media/thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-thoughtworks-mingle-test-user"></a>Creación de un usuario de prueba de Thoughtworks Mingle

Para que los usuarios de Azure AD puedan iniciar sesión, deben aprovisionarse en la aplicación Thoughtworks Mingle con sus nombres de usuario de Azure Active Directory. En el caso de Thoughtworks Mingle, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión en su sitio de compañía de Thoughtworks Mingle como administrador.

1. Haga clic en **Perfil**.
   
    ![Su primer proyecto](./media/thoughtworks-mingle-tutorial/ic785160.png "Su primer proyecto")

1. Haga clic en la pestaña **Admin** (Administrador) y, luego, en **Users** (Usuarios).
   
    ![Usuarios](./media/thoughtworks-mingle-tutorial/ic785161.png "Usuarios")

1. Haga clic en **Nuevo usuario**.
   
    ![Nuevo usuario](./media/thoughtworks-mingle-tutorial/ic785162.png "nuevo usuario")

1. En la página del cuadro de diálogo **Nuevo usuario** , realice los pasos siguientes:
   
    ![Cuadro de diálogo Nuevo usuario](./media/thoughtworks-mingle-tutorial/ic785163.png "Nuevo usuario")  
 
    a. En los cuadros de texto **Sign-in name** (Nombre de inicio de sesión), **Display name** (Nombre para mostrar), **Choose password** (Elegir contraseña) y **Confirm password** (Confirmar contraseña), escriba la información pertinente de una cuenta de Azure AD válida que desee aprovisionar. 

    b. En **User type** (Tipo de usuario), seleccione **Full user** (Usuario completo).

    c. Haga clic en **Crear este perfil**.

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Thoughtworks Mingle ofrecida por Thoughtworks Mingle para aprovisionar cuentas de usuario de AAD.
> 

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Thoughtworks Mingle.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Thoughtworks Mingle, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Thoughtworks Mingle**.

    ![Vínculo a Thoughtworks Mingle en la lista de aplicaciones](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

El objetivo de esta sección es probar la configuración del inicio de sesión único de Azure AD mediante el panel de acceso.

Al hacer clic en el icono de Thoughtworks Mingle en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Thoughtworks Mingle.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/thoughtworks-mingle-tutorial/tutorial_general_203.png

