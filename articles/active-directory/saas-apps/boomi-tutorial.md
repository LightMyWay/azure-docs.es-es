---
title: 'Tutorial: Integración de Azure Active Directory con Boomi | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Boomi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 40d034ff-7394-4713-923d-1f8f2ed8bf36
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: jeedes
ms.openlocfilehash: cf925e0e0e7b6b4c10b6b21d17214f91473a9026
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432956"
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Tutorial: Integración de Azure Active Directory con Boomi

En este tutorial, aprenderá a integrar Boomi con Azure Active Directory (Azure AD).

La integración de Boomi con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Boomi.
- Puede permitir que los usuarios inicien sesión automáticamente en Boomi (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Boomi, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en Boomi

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Boomi desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-boomi-from-the-gallery"></a>Adición de Boomi desde la galería
Para configurar la integración de Boomi en Azure AD, deberá agregar Boomi desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Boomi desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Boomi**, seleccione **Boomi** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Boomi en la lista de resultados](./media/boomi-tutorial/tutorial_boomi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Boomi con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Boomi para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Boomi.

Para establecer la relación de vínculo, en Boomi, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Boomi, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Boomi](#create-a-boomi-test-user)**: para tener un homólogo de Britta Simon en Boomi que esté vinculado a su representación en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Boomi.

**Para configurar el inicio de sesión único de Azure AD con Boomi, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Boomi**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/boomi-tutorial/tutorial_boomi_samlbase.png)

1. En la sección **Dominio y direcciones URL de Boomi**, lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de Boomi](./media/boomi-tutorial/tutorial_boomi_url.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL como: `https://platform.boomi.com/`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://platform.boomi.com/sso/<boomi-tenant>/saml`.

    > [!NOTE] 
    > El valor de dirección URL de respuesta no es real. Actualícelo con la dirección URL de respuesta real. Póngase en contacto con el [equipo de soporte técnico de Boomi](https://boomi.com/company/contact/) para obtener este valor.
 
1. La aplicación Boomi espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección "**Atributos de usuario**" de la página de integración de aplicaciones. La siguiente captura de pantalla le muestra un ejemplo de esto.
    
    ![Configurar inicio de sesión único](./media/boomi-tutorial/tutorial_attribute.png)

1. En la sección **Atributos de usuario** del cuadro de diálogo **Inicio de sesión único**, para cada fila que se muestra en la tabla siguiente, realice los pasos siguientes:

    | Nombre del atributo | Valor de atributo |
    | -------------- | --------------- |
    | FEDERATION_ID | user.mail |
    
    a. Haga clic en **Agregar atributo** para abrir el cuadro de diálogo **Agregar atributo**.
    
    ![Configurar inicio de sesión único](./media/boomi-tutorial/tutorial_officespace_04.png)
    
    ![Configurar inicio de sesión único](./media/boomi-tutorial/tutorial_attribute_05.png)
    
    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.
    
    c. En la lista **Valor**, seleccione el atributo que se muestra para esa fila.
    
    d. Haga clic en **Aceptar**.

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/boomi-tutorial/tutorial_boomi_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/boomi-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Boomi**, haga clic en **Configurar Boomi** para abrir la ventana **Configurar inicio de sesión**. Copie la **dirección URL de servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Configuración de Boomi](./media/boomi-tutorial/tutorial_boomi_configure.png) 

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de Boomi. 

1. Vaya a **Company Name** (Nombre de la compañía) y haga clic en **Set up** (Configurar).

1. Haga clic en la pestaña **SSO Options** (Opciones de SSO) y realice los siguientes pasos.

    ![Configuración del inicio de sesión único en la aplicación](./media/boomi-tutorial/tutorial_boomi_11.png)

    a. Marque la casilla **Enable SAML Single Sign-On** (Habilitar el inicio de sesión único de SAML).

    b. Haga clic en **Import** (Importar) para cargar el certificado descargado de Azure AD en **Identity Provider Certificate** (Certificado del proveedor de identidades).
    
    c. En el cuadro de texto **URL de inicio de sesión del proveedor de identidades**, coloque el valor de **SAML Single Sign-On Service URL** (Dirección URL del servicio de inicio de sesión único de SAML) en la ventana de configuración de aplicaciones de Azure AD.

    d. Como **ubicación del identificador de federación**, seleccione el botón de radio **Federation Id is in FEDERATION_ID Attribute element** (El id. de federación está en el elemento de atributo FEDERATION_ID). 

    e. Haga clic en el botón **Guardar** .

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más sobre la característica de documentación insertada aquí: [Vista previa: Administración de inicio de sesión único para aplicaciones empresariales en el nuevo Azure Portal]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/boomi-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/boomi-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/boomi-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/boomi-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
  
### <a name="create-a-boomi-test-user"></a>Creación de un usuario de prueba de Boomi

Para permitir que los usuarios de Azure AD inicien sesión en Boomi, tienen que aprovisionarse en Boomi. En el caso de Boomi, el aprovisionamiento es una tarea manual.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Para aprovisionar una cuenta de usuario, realice estos pasos:

1. Inicie sesión en su sitio de la compañía de Boomi como administrador.

1. Después de iniciar sesión, vaya a **User Management** (Administración de usuarios) y vaya a **Users** (Usuarios).

    ![Usuarios](./media/boomi-tutorial/tutorial_boomi_001.png "Usuarios")

1. Haga clic en el icono **+**, se abre el cuadro de diálogo **Add/Maintain User Roles** (Agregar o mantener roles de usuario).

    ![Usuarios](./media/boomi-tutorial/tutorial_boomi_002.png "Usuarios")

    ![Usuarios](./media/boomi-tutorial/tutorial_boomi_003.png "Usuarios")

    a. En el cuadro de texto **Dirección de correo electrónico del usuario**, escriba la dirección de correo electrónico de un usuario, por ejemplo, BrittaSimon@contoso.com.
    
    b. En el cuadro de texto **Nombre**, escriba el nombre del usuario, en este caso, Britta.

    c. En el cuadro de texto **Apellidos**, escriba el nombre del usuario, en este caso, Simon.
    
    d. Escriba el **id. de federación** del usuario. Cada usuario debe tener un id. de federación que identifica de forma única al usuario dentro de la cuenta.
    
    e. Asigne el rol **Usuario estándar** al usuario. No asigne el rol Administrador porque le proporcionaría acceso normal a Atmosphere, así como acceso de inicio de sesión único.
    
    f. Haga clic en **OK**.
    
    > [!NOTE]
    > El usuario no recibirá un correo electrónico de notificación de bienvenida con una contraseña que se pueda usar para iniciar sesión en la cuenta de AtomSphere porque su contraseña se administra mediante el proveedor de identidades. Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Boomi que proporcione Boomi para aprovisionar cuentas de usuario de AAD.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, permitirá que Britta Simon use el inicio de sesión único de Azure concediéndole acceso a Boomi.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a Boomi, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Boomi**.

    ![Vínculo a Boomi en la lista de aplicaciones](./media/boomi-tutorial/tutorial_boomi_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Boomi en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Boomi.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/boomi-tutorial/tutorial_general_01.png
[2]: ./media/boomi-tutorial/tutorial_general_02.png
[3]: ./media/boomi-tutorial/tutorial_general_03.png
[4]: ./media/boomi-tutorial/tutorial_general_04.png

[100]: ./media/boomi-tutorial/tutorial_general_100.png

[200]: ./media/boomi-tutorial/tutorial_general_200.png
[201]: ./media/boomi-tutorial/tutorial_general_201.png
[202]: ./media/boomi-tutorial/tutorial_general_202.png
[203]: ./media/boomi-tutorial/tutorial_general_203.png

