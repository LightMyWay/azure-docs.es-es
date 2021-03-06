---
title: 'Azure AD Connect: Migración de la federación a la sincronización de hash de contraseña para Azure AD | Microsoft Docs'
description: Información acerca de cómo mover un entorno de identidades híbrido a la sincronización de hash de contraseña.
services: active-directory
author: billmath
manager: mtillman
ms.reviewer: martincoetzer
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/13/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a14e630c23af3e0228bf4806851f29cfab199215
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54103985"
---
# <a name="migrate-from-federation-to-password-hash-synchronization-for-azure-ad"></a>Migración de la federación a la sincronización de hash de contraseña para Azure AD
El siguiente documento proporciona instrucciones para migrar desde AD FS a la sincronización de hash de contraseña.

>[!NOTE]
>[Aquí](https://aka.ms/ADFSTOPHSDPDownload) puede descargar una copia de este documento.


## <a name="prerequisites-for-the-migration"></a>Requisitos previos para la migración 
Para poder realizar la migración es preciso cumplir los siguientes requisitos previos.
### <a name="update-azure-ad-connect"></a>Actualización de Azure AD Connect

Para realizar correctamente los pasos necesarios para migrar a la autenticación de paso a través, es preciso tener, como mínimo, [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0. Esta versión contiene cambios importantes en la forma en que se realiza la conversión del inicio de sesión y reduce el tiempo total para migrar de la federación a la autenticación en la nube, que pasa de durar potencialmente horas a minutos.

> [!IMPORTANT]
> La documentación, las herramientas y los blogs obsoletos indican que la conversión de usuarios es un paso obligatorio cuando se convierten Federados en Administrados. Tenga en cuenta que ya no es preciso convertir usuarios y que Microsoft está trabajando para actualizar la documentación y las herramientas para reflejarlo.

Para actualizar Azure AD Connect a la versión más reciente, siga estas [instrucciones de actualización](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

### <a name="password-hash-synchronization-required-permissions"></a>Permisos necesarios para la sincronización del hash de contraseñas

Azure AD Connect se puede configurar mediante Configuración rápida o Instalación personalizada. Si utilizó la opción Instalación personalizada, es posible que no encuentre los permisos necesarios para la [sincronización de hash de contraseña](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-accounts-permissions).

La cuenta del servicio Microsoft Azure Active Directory Connect necesita los siguientes permisos para poder sincronizar los hashes de las contraseñas password.

* Replicación de cambios de directorio

* Replicación de todos los cambios de directorio

Este es un buen momento para validar los permisos que están en vigor en todos los dominios del bosque.

### <a name="plan-migration-method"></a>Planeamiento del método de migración

Hay dos métodos para migrar de la autenticación federada a sincronización de hash de contraseñas y el SSO de conexión directa. El método que utilice dependerá de la forma en que AD FS se configuró originalmente. 



- **Opción A: mediante Azure AD Connect**. Si AD FS se configuró originalmente mediante Azure AD Connect, el cambio a Sincronización de hash de contraseña como método de inicio de sesión del usuario se debe realizar a través del asistente de Azure AD Connect.   
Si se usa Azure AD Connect, este ejecuta el cmdlet Set-MsolDomainAuthentication automáticamente al cambiar el método de inicio de sesión del usuario, por lo que no tendrá ningún control en el momento en que se anule la federación de todos los dominios federados comprobados en el inquilino de Azure AD.

> [!NOTE]
> En la actualidad, no se puede evitar que se anule la federación de todos los dominios del inquilino cuando se cambia el inicio de sesión del usuario a Sincronización de hash de contraseña cuando AAD Connect se utilizó originalmente para configurar automáticamente AD FS.  



- **Opción B: mediante Azure AD Connect con PowerShell**. Este método solo se puede usar cuando AD FS no se haya configurado originalmente con Azure AD Connect. Sigue siendo preciso cambiar el método de inicio de sesión del usuario mediante el asistente de Azure AD Connect, pero la diferencia principal es que el cmdlet no se ejecutará automáticamente Set-MsolDomainAuthentication, porque no se detecta la existencia de una granja de servidores de AD FS y, por consiguiente, no se pierde el control sobre qué dominios se convierten y en qué orden se realiza dicha conversión.

Para saber qué método se debe utilizar, siga los pasos de la próxima sección.

#### <a name="verify-current-user-sign-in-settings"></a>Comprobación de la configuración del inicio de sesión del usuario actual

Para comprobar la configuración del inicio de sesión de usuario actual, inicie sesión en el portal de Azure AD, [https://aad.portal.azure.com](https://aad.portal.azure.com/), con una cuenta de administrador global.

En la sección Inicio de sesión del usuario, compruebe que Federación está habilitada y que las opciones Inicio de sesión único de conexión directa y Autenticación de paso a través están deshabilitadas. Compruebe también el estado de la opción Sincronización de contraseñas, que debe aparecer como Deshabilitada, salvo que se haya activado previamente.

![Imagen 5](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image1.png)

#### <a name="verify-azure-ad-connect-configuration"></a>Comprobación de la configuración de Azure AD Connect

   1. Vaya a su servidor de Azure AD Connect, inicie Azure AD Connect y seleccione Configurar. 
   2. En la pantalla Tareas adicionales, seleccione Ver configuración actual y, después, seleccione Siguiente.</br>
   ![Imagen 31](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image2.png)</br>
   
   3. En la pantalla Review Your Solution (Revise su solución), tome nota del estado de Sincronización de contraseñas.</br> 

   Si el valor de la opción Sincronización de hash de contraseña es Deshabilitado, tendrá que seguir los pasos de esta guía para habilitarla. Si el valor de la opción Sincronización de hash de contraseña es Habilitado, puede omitir sin riesgo alguno la sección [Paso 1: Habilitar Sincronización de hash de contraseña](#step-1--enable-password-hash-synchronization) de esta guía.
   4. En la pantalla Review Your Solution (Revise su solución), desplácese hacia abajo hasta Servicios de federación de Active Directory (AD FS).</br>
 
   Si ve que la configuración de AD FS se encuentra en esta sección, puede estar seguro de que AD FS se configuró originalmente mediante Azure AD Connect y, por consiguiente, la conversión de los dominios federados a administrados se puede realizar mediante la opción "Cambiar inicio de sesión de usuario". Este proceso se detalla en la sección **Opción A: cambio de Federación a Sincronización de hash de contraseña mediante Azure AD Connect**.
   5. Si la opción Servicios de federación de Active Directory (AD FS) no aparece en la configuración actual, deberá convertir manualmente los dominios de federados a administrados mediante PowerShell. El proceso para hacerlo se detalla en la sección **Opción B: cambio de Federación a Sincronización de hash de contraseña mediante Azure AD Connect y PowerShell**.

### <a name="document-current-federation-settings"></a>Documentación de la configuración actual de la federación

Para encontrar la configuración actual de la federación, ejecute el cmdlet Get-MsolDomainFederationSettings.

El comando es:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName YourDomain.extention | fl *
```

Por ejemplo: 

``` PowerShell
Get-MsolDomainFederationSettings -DomainName Contoso.com | fl *
```
Valide los valores que puedan haberse personalizado en la documentación del diseño e implementación de Federación, en concreto PreferredAuthenticationProtocol, SupportsMfa y PromptLoginBehavior.

A continuación encontrará más información acerca de estos valores.

[Compatibilidad con el parámetro prompt=login de Servicios de federación de Active Directory](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)  
‎[Set-MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> Si el valor SupportsMfa está actualmente establecido en "True", significa que se usa una solución de MFA local para insertar un segundo factor en el flujo de autenticación del usuario. Esto no funcionará en escenarios de autenticación de Azure AD, por lo que tendrá que hacer uso del servicio de Azure MFA (basado en la nube) para realizar la misma función. Evalúe meticulosamente sus requisitos de MFA antes de continuar y asegúrese de que sabe cómo usar Azure MFA, las implicaciones de las licencias y el proceso de registro del usuario final antes de convertir los dominios. En nuestra guía de implementación de Azure MFA encontrará más información, y se puede encontrar en [https://aka.ms/deploymentplans](https://aka.ms/deploymentplans).

#### <a name="backup-federation-settings"></a>Copia de seguridad de la configuración de la federación

Aunque durante este proceso no se realizarán cambios en otros usuarios de confianza de la granja de servidores de AD FS, es aconsejable asegurarse de que tiene una copia de seguridad válida actual de la granja de servidores de AD FS que se puede restaurar. Puede hacerlo mediante la [herramienta de restauración rápida de AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool) de Microsoft, que es gratuita. Esta herramienta puede usarse para realizar copias de seguridad y restaurar AD FS, a una granja de servidores existente o a una nueva.

Si decide no utilizar dicha herramienta, como mínimo, debe exportar la relación de confianza para usuario autenticado "Plataforma de identidad de Microsoft Office 365" y todas las reglas de notificación personalizadas asociadas que pueda haber agregado. Esto se puede hacer mediante el siguiente ejemplo de PowerShell

``` PowerShell
(Get-AdfsRelyingPartyTrust -Name "Microsoft Office 365 Identity Platform") | Export-CliXML "C:\temp\O365-RelyingPartyTrust.xml"
```

## <a name="deployment-considerations-and-ad-fs-usage"></a>Consideraciones sobre la implementación y uso de AD FS

### <a name="validate-your-current-ad-fs-usage"></a>Validación del uso actual de AD FS

Antes de realizar la conversión de Federado a Administrado, es preciso examinar detenidamente cómo se usa actualmente AD FS para Azure AD u Office 365 y otras aplicaciones (relaciones de confianza para usuario autenticado). También se debe tener en cuenta la siguiente tabla:

| Si| Entonces |
|-|-|
| Va a conservar AD FS para otras aplicaciones.| Va a utilizar tanto AD FS como Azure AD y debe considerar como resultado la experiencia del usuario final. Es posible que los usuarios tengan que autenticarse dos veces en algunos escenarios, una vez en Azure AD (donde obtendrán SSO en adelante para otras aplicaciones como Office 365) y una segunda para todas las aplicaciones que aún están enlazadas a AD FS como una relación de confianza para usuario autenticado. |
| AD FS está muy personalizado y depende de valores de configuración concretos del archivo onload.js que no se pueden duplicar en Azure AD (por ejemplo, ha cambiado la forma en que se inicia sesión para que los usuarios solo especifiquen un formato SamAccountName para su nombre de usuario, en lugar de un nombre principal de usuario, o ha personalizado con marca la experiencia de inicio de sesión)| Antes de continuar, deberá comprobar que Azure AD pueden cumplir los requisitos de personalización actuales. Para más información, consulte las secciones acerca de la personalización de marca de AD FS y la personalización de AD FS.|
| Bloquea a los clientes de autenticación heredada mediante AD FS.| Considere la posibilidad de sustituir los controles para bloquear los clientes de autenticación heredada presentes actualmente en AD FS con una combinación de [controles de acceso condicional para la autenticación heredada](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) y [reglas de acceso de cliente de Exchange Online](https://aka.ms/EXOCAR).|
| Exija a los usuarios que ejecuten MFA con una solución de servidor de MFA local al realizar la autenticación en AD FS.| No podrá insertar un desafío de MFA mediante la solución MFA local en el flujo de autenticación de un dominio administrado; sin embargo, puede usar el servicio Azure MFA para ello, así que siga adelante una vez que se haya convertido el dominio. Si usuarios no utilizan Azure MFA actualmente, será preciso realizar un paso de registro del usuario de un solo uso que será preciso preparar y comunicar a los usuarios finales.|
| Use directivas de control de acceso (reglas de AuthZ) en AD FS para controlar el acceso a Office 365.| Considere la posibilidad de reemplazarlas por las [directivas de acceso condicional](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) de Azure AD y las [reglas de acceso de cliente de Exchange Online](https://aka.ms/EXOCAR).|

### <a name="considerations-for-common-ad-fs-customizations"></a>Consideraciones para las personalizaciones de AD FS comunes

#### <a name="inside-corporate-network-claim"></a>Dentro de una notificación de una red corporativa

La notificación InsideCorporateNetwork la emite AD FS si el usuario que realiza la autenticación está dentro de la red corporativa. Luego, esta notificación se puede pasar a Azure AD y usarse para omitir la autenticación multifactor en función de la ubicación de red de los usuarios. Consulte [Direcciones IP de confianza para usuarios federados](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud) para obtener información acerca de cómo determinar si la tiene actualmente habilitada en AD FS.

La notificación InsideCorporateNetwork dejará de estar estará disponible cuando los dominios se conviertan a la sincronización de hash de contraseña. Las [ubicaciones con nombre en Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) se pueden utilizar para reemplazar esta funcionalidad.

Una vez que se han configurado las ubicaciones con nombre, se configuran todas las directivas de acceso condicional para incluir o excluir las ubicaciones de red. "Todas las ubicaciones de confianza" o "IP de confianza de MFA" deben actualizarse para reflejar las ubicaciones con nombre recién creadas.

Consulte [¿Qué es la condición de ubicación en el acceso condicional de Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations) para más información.

#### <a name="hybrid-azure-ad-joined-devices"></a>Dispositivos unidos a Azure AD híbrido

La unión de un dispositivo a Azure AD permite crear reglas de acceso condicional que exigen que los dispositivos cumplan los estándares de acceso en cuanto a seguridad y cumplimiento de normas, y permite a los usuarios iniciar sesión en un dispositivo con un profesional o educativa, en lugar de una cuenta personal. Dispositivos unidos a Azure AD híbrido le permite unir a Azure AD sus dispositivos unidos a un dominio de AD. Es posible que un entorno federado se haya configurado con esta característica.

Para asegurarse de que la unión híbrida sigue funcionando en todos los dispositivos nuevos unidos al dominio una vez que los dominios se han convertido a la sincronización de hash de contraseña, Azure AD Connect debe configurarse para sincronizar con Azure AD las cuentas de equipos de Active Directory de los clientes de Windows 10. En el caso de cuentas de equipos con Windows 7 y Windows 8, la unión híbrida usará SSO de conexión directa para registrar el equipo en Azure AD, por lo que no es preciso sincronizarlas, como en el caso de los equipos con Windows 10. Sin embargo, tendrá que implementar una versión actualizada del archivo workplacejoin.exe (a través de un archivo .msi) en los clientes de nivel inferior para que se puedan registrar a sí mismos mediante la SSO de conexión directa. [Descargue el archivo .msi](https://www.microsoft.com/download/details.aspx?id=53554). 

Para más información, consulte [Configuración de dispositivos híbridos unidos a Azure Active Directory](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup).

#### <a name="branding"></a>Personalización de marca

La organización puede haber [personalizado las páginas de inicio de sesión de ADFS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) para que muestren información más adecuada para la organización. En ese caso, considere la posibilidad de realizar [personalizaciones similares en la página de inicio de sesión de Azure AD](https://docs.microsoft.com/azure/active-directory/customize-branding).

Aunque hay personalizaciones similares disponibles, cabe esperar que haya algunos cambios visuales. Es posible que desee incluir cambios esperados en las comunicaciones con los usuarios finales.

> [!NOTE]
> La personalización de marca de empresa solo está disponible si ha adquirido las licencias de Azure AD Premium o Básico, o si tiene una licencia de Office 365.

## <a name="planning-deployment-and-support"></a>Planeación de la implementación y el soporte técnico

### <a name="plan-the-maintenance-window"></a>Planeación de la ventana de mantenimiento

Aunque el proceso de conversión de dominios es relativamente rápido, Azure AD puede enviar solicitudes de autenticación a sus servidores de AD FS durante un período máximo de 4 horas una vez finalizada la conversión del dominio. Durante estas cuatro horas y en función de las distintas memorias caché del lado de servicio, es posible que Azure AD no acepte estas autenticaciones y los usuarios reciban un error, ya que podrán autenticarse correctamente con AD FS todavía, pero Azure AD no aceptará ningún token emitido del usuario, porque la confianza de la federación se ha eliminado.

> [!NOTE]
> Esto solo afectará a los usuarios que accedan a los servicios mediante un explorador durante el periodo posterior a esta conversión hasta que se borre la caché del lado del servicio. Los clientes heredados (Exchange ActiveSync, Outlook 2010/2013) no deberían verse afectados, ya que Exchange Online mantiene una caché de sus credenciales durante un período de tiempo que se utiliza para volver a autenticar al usuario en modo silencioso sin necesidad de volver a AD FS. Las credenciales almacenadas en el dispositivo para estos clientes se utilizan para volver a autenticarse en modo silencioso una vez que se borra la caché y, por lo tanto, los usuarios no deben recibir ningún aviso de contraseña como resultado del proceso de conversión de dominio. Por el contrario, para los clientes de autenticación moderna (aplicaciones de Office 2013/2016, IOS y Android), utilizan un token de actualización válido para obtener nuevos tokens de acceso para el acceso continuo a los recursos en lugar de volver a AD FS y, por lo tanto, son inmunes a cualquier aviso de contraseña como resultado del proceso de conversión de dominio y seguirán funcionando sin necesidad de ninguna configuración adicional.

> [!IMPORTANT]
> No cierre el entorno de AD FS ni quite la relación de confianza para usuario autenticado hasta que haya comprobado que todos los usuarios se autentican correctamente mediante la autenticación en la nube de Office 365.

### <a name="plan-for-rollback"></a>Planeación para la reversión

Si surge algún problema importante que no se pueda resolver rápidamente, puede decidir revertir la solución a la opción de Federación. Es importante planear qué hacer si la implementación no funciona según lo previsto. Si se produce un error en la conversión del dominio o de los usuarios durante la implementación, o si necesita revertir a la opción Federación, debe saber cómo mitigar el impacto de una interrupción del servicio y reducir el impacto que ello produce en los usuarios.

#### <a name="rolling-back"></a>Reversión

Consulte la documentación acerca del diseño e implementación de la federación para ver los detalles de su implementación concreta. El proceso debe implicar:

* Convertir dominios administrados a federados mediante Convert-MSOLDomainToFederated 

* Si fuera necesario, configurar reglas de notificaciones adicionales.

### <a name="plan-change-communications"></a>Planeamiento de comunicaciones de cambio

Una parte importante del planeamiento de la implementación y soporte técnico es garantizar que a los usuarios finales se les informa proactivamente acerca de los cambios y lo que pueden experimentar o deben realizar. 

Una vez que se hayan implementado tanto la sincronización de hash de contraseña como el SSO de conexión directa, la experiencia de inicio de sesión del usuario final cambiará cuando acceda a Office 365 y a otros recursos asociados autenticados mediante Azure AD. Los usuarios externos a la red verán ahora solo la página de inicio de sesión de Azure AD, en lugar de ser redirigidos a la página basada en formularios que presentan los servidores proxy de aplicación web externos.

Hay varios elementos para planear la estrategia de comunicación. Entre ellas se incluyen las siguientes:

* Notificación a los usuarios de las funcionalidades próximas y publicadas mediante
  * Correo electrónico y otros canales de comunicación interna
  * Objetos visuales como pósteres
  * Comunicaciones en directo ejecutivas u otras comunicaciones
* Determinación de quién va a personalizar y quien va a enviar las comunicaciones, así como cuándo se enviarán.

## <a name="implementing-your-solution"></a>Implementación de la solución

Ahora que ha planeado la solución, está listo para implementarla. La implementación incluye los siguientes componentes:

1. Habilitación de la sincronización de hash de contraseñas

2. Preparación para el inicio de sesión único de conexión directa

3. Cambio del método de inicio de sesión a Sincronización de hash de contraseñas y habilitación de SSO de conexión directa

### <a name="step-1--enable-password-hash-synchronization"></a>Paso1: Habilitación de la sincronización de hash de contraseña

El primer paso para implementar esta solución es habilitar Sincronización de hash de contraseña en el asistente de Azure AD Connect. Sincronización de Hash de contraseña es una característica opcional que se puede habilitar en entornos que usan Federación sin que ello afecte al flujo de autenticación. En este caso, Azure AD Connect iniciará la sincronización de los hashes de contraseña sin que ello afecte a los usuarios que inician sesión mediante la federación.

Ese es el motivo por el que se recomienda realizar este paso como una tarea de preparación antes de cambiar el método de inicio de sesión de dominios. Esto le dará tiempo suficiente para validar que la sincronización de hash de contraseña funciona correctamente.

Para habilitación Sincronización de hash de contraseñas:

   1. En el servidor de Azure AD Connect, abra el asistente y seleccione Configurar.
   2. Seleccione Personalizar las opciones de sincronización y, después, seleccione Siguiente.
   3. En la pantalla Conectar a Azure AD, indique el nombre de usuario y la contraseña de un administrador global.
   4. En la pantalla Conectar sus directorios, haga clic en Siguiente.
   5. En la pantalla Filtrado de dominios y unidades organizativas, haga clic en Siguiente.
   6. En la pantalla Características opcionales, seleccione Sincronización de contraseña y seleccione Siguiente.
   ![Imagen 21](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image6.png)</br>
   7. Seleccione Siguiente en las pantallas restantes y Configurar en la última pantalla.
   8. Azure AD Connect empezará a sincronizar los hashes de contraseña en la próxima sincronización.

Una vez que Sincronización de hash de contraseña se haya habilitado, los hashes de contraseña de todos los usuarios del ámbito de sincronización de Azure AD Connect se volverán a crear y escribir en Azure AD. En función del número de usuarios, esta operación puede durar entre varios minutos y varias horas.

En los planeamientos, debe calcular que en una hora se pueden procesar aproximadamente 20 000 usuarios.

Para validar que Sincronización de hash de contraseña funciona correctamente, use la tarea de solución de problemas del asistente de Azure AD Connect.

   1. Abra una nueva sesión de Windows PowerShell en el servidor de Azure AD Connect mediante la opción Ejecutar como administrador.
   2. Ejecute Set-ExecutionPolicy RemoteSigned o Set-ExecutionPolicy Unrestricted.
   3. Inicie el asistente de Azure AD Connect.
   4. Vaya a la página Additional Tasks (Tareas adicionales), seleccione Troubleshoot (Solucionar problemas) y haga clic en Next (Siguiente).
   5. En la página de solución de problemas, haga clic en Iniciar para iniciar el menú de solución de problemas en PowerShell.
   6. En el menú principal, seleccione Solución de problemas de sincronización de contraseñas.
   7. En el submenú, seleccione Password Synchronization does not work at all (La sincronización de hash de contraseñas no funciona en absoluto).

Si aparece algún problema, use la información de [este artículo](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization) para solucionarlo.

### <a name="step-2--prepare-for-seamless-sso"></a>Paso 2: Preparación para el SSO de conexión directa

Para que los dispositivos utilicen el SSO de conexión directa, solo debe agregar una dirección URL de Azure AD en la configuración de la zona de Intranet de los usuarios mediante una directiva de grupo de Active Directory.

De forma predeterminada, el explorador calcula automáticamente la zona correcta (Internet o intranet) de una dirección URL específica. Por ejemplo, "http://contoso/" se asigna a la zona de intranet, mientras que "http://intranet.contoso.com/" se asigna a la zona de Internet (porque la dirección URL contiene un punto). Los exploradores no enviarán vales de Kerberos a un punto de conexión en la nube, como la dirección URL de Azure AD, a menos que agregue explícitamente la dirección URL a la zona de intranet del explorador.

Siga los [pasos para implementar](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) los cambios necesarios en los dispositivos.

> [!IMPORTANT]
> Hacer este cambio no modificará la forma en que los usuarios inician sesión en Azure AD. Sin embargo, es importante que esta configuración se aplique a todos los dispositivos antes de iniciar el paso 3. Tenga en cuenta también que los usuarios que inicien sesión en dispositivos que no han recibido esta configuración solo tendrán que escribir el nombre de usuario y contraseña para iniciar sesión Azure AD.

### <a name="step-3--change-sign-in-method-to-phs-and-enable-seamless-sso"></a>Paso 3: Cambio del método de inicio de sesión a Sincronización de hash de contraseña y habilitación de SSO de conexión directa

#### <a name="option-a---switch-from-federation-to-phs-by-using-azure-ad-connect"></a>Opción A: Cambio de Federación a Sincronización de hash de contraseña mediante Azure AD Connect

Utilice este método cuando la instancia de AD FS se haya configurado inicialmente con Azure AD Connect. Este método no se puede usar si la instancia de AD FS no se ha configurado inicialmente con Azure AD Connect. En primer lugar **cambie el método de inicio de sesión del usuario**

   1. En el servidor de Azure AD Connect, abra el asistente.
   2. Seleccione Cambiar inicio de sesión de usuario y, después, seleccione Siguiente.
   ![Imagen 27](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image7.png)</br>
   3. En la pantalla **Conectar a Azure AD**, indique el nombre de usuario y la contraseña de un **administrador global**.
   4. En la pantalla **Inicio de sesión de usuario**, cambiar el botón de opción de Federación con AD FS a Sincronización de hash de contraseña y asegúrese de seleccionar la casilla No convertir las cuentas de usuario, ya es un paso en desuso y se eliminará de la próxima versión de AAD Connect. Seleccione también Habilitar el inicio de sesión único y, después, seleccione **Siguiente**.
   ![Imagen 29](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image8.png)</br>
   
   > [!NOTE]
   > A partir de la versión 1.1.880.0 de Azure AD Connect, la casilla Inicio de sesión único de conexión directa está habilitada de forma predeterminada.
   
   > [!IMPORTANT]
   > Puede ignorar sin riesgo alguno las advertencias que indican que la conversión del usuario y la sincronización de hash de contraseña total son pasos obligatorios para realizar la conversión de federación a autenticación en la nube. Tenga en cuenta que estos pasos ya no son necesarios, las próximas versiones de Azure AD Connect no tendrán una opción para convertir a usuarios. Si aún ve dichas advertencias, compruebe que ejecuta la versión más reciente de Azure AD Connect y que usa la versión más reciente de esta guía. Para más información, consulte la [sección Actualización de Azure AD Connect](#_Update_Azure_AD).
   
   5. En la pantalla Habilitar el inicio de sesión único, escriba las credenciales de la cuenta del administrador del dominio y, después, seleccione Siguiente.
   ![Imagen 35](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image9.png)</br>
   
   > [!NOTE]
   > Las credenciales del administrador de dominio son necesarias para habilitar el inicio de sesión único de conexión directa, ya que el proceso realiza las siguientes acciones que requieren estos permisos elevados. Las credenciales de administrador de dominio no se almacenan en Azure AD Connect ni en Azure AD. Solo se utilizan para habilitar la característica y después se descartan después de finalizar correctamente.
   >  * Se crea una cuenta de equipo denominada AZUREADSSOACC (que representa a Azure AD) en la instancia local de Active Directory (AD).
   >  * La clave de descifrado de Kerberos de la cuenta de equipo se comparte de manera segura con Azure AD.
   >  * Además, se crean dos nombres de entidad de seguridad de servicio (SPN) de Kerberos que representan las dos direcciones URL que se usan en el inicio de sesión de Azure AD.
   >  * Las credenciales de administrador de dominio no se almacenan en Azure AD Connect ni en Azure AD. Solo se utilizan para habilitar la característica y después se descartan después de finalizar correctamente.
   
   6. En la pantalla Listo para configurar, asegúrese de que la casilla Inicie el proceso de sincronización cuando se complete la configuración esté activada. A continuación, seleccione Configurar.
   ![Imagen 36](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image10.png)</br>
   
   > [!IMPORTANT]
   > En este momento, todos los dominios federados se cambiarán a Autenticación administrada, que usará Sincronización de hash de contraseñas como método de autenticación.
       
   7. Abra el portal de Azure AD, seleccione Azure Active Directory y, después, seleccione Azure AD Connect.
   8. Compruebe que Federación está deshabilitado mientras que las opciones Inicio de sesión único de conexión directa y Autenticación de paso a través están habilitadas.  
  ![Imagen 37](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image11.png)</br>
   9. Vaya a [Pruebas y pasos siguientes](#testing-and-next-steps).
   
   > [!IMPORTANT]
   > Omita la sección Opción B: Cambio de Federación a Sincronización de hash de contraseña mediante Azure AD Connect y PowerShell, ya que los pasos de esta sección no se aplican.  

#### <a name="option-b---switch-from-federation-to-phs-using-azure-ad-connect-and-powershell"></a>Opción B: Cambio de Federación a Sincronización de hash de contraseña con Azure AD Connect y PowerShell

Use esta opción cuando la federación no se ha configurado inicialmente mediante Azure AD Connect.

Como parte de este proceso, habilitará SSO de conexión directa y cambiar los dominios de Federado a Administrado.

   1. En el servidor de Azure AD Connect, abra el asistente.
   2. Seleccione Cambiar inicio de sesión de usuario y, después, seleccione Siguiente. 
   3. En la pantalla Conectar a Azure AD, indique el nombre de usuario y la contraseña de un administrador global.
   4. En la pantalla Inicio de sesión de usuario, cambie el botón de radio No configurar a Sincronización de hash de contraseña, seleccione Habilitar el inicio de sesión único y, después, seleccione Siguiente.
   
   Antes del cambio: ![Imagen 20](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image12.png)</br>

   Después del cambio:  
   ![Imagen 22](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image13.png)</br>
   
   > [!NOTE]
   > A partir de la versión 1.1.880.0 de Azure AD Connect, la casilla Inicio de sesión único de conexión directa está habilitada de forma predeterminada.
   
   5. En la pantalla Habilitar el inicio de sesión único, escriba las credenciales de la cuenta del administrador del dominio y, después, seleccione Siguiente.
   
   > [!NOTE]
   > Las credenciales del administrador de dominio son necesarias para habilitar el inicio de sesión único de conexión directa, ya que el proceso realiza las siguientes acciones que requieren estos permisos elevados. Las credenciales de administrador de dominio no se almacenan en Azure AD Connect ni en Azure AD. Solo se utilizan para habilitar la característica y después se descartan después de finalizar correctamente.
   > * Se crea una cuenta de equipo denominada AZUREADSSOACC (que representa a Azure AD) en la instancia local de Active Directory (AD).
   > * La clave de descifrado de Kerberos de la cuenta de equipo se comparte de manera segura con Azure AD.
   > * Además, se crean dos nombres de entidad de seguridad de servicio (SPN) de Kerberos que representan las dos direcciones URL que se usan en el inicio de sesión de Azure AD.
   
   6. En la pantalla Listo para configurar, asegúrese de que la casilla Inicie el proceso de sincronización cuando se complete la configuración esté activada. A continuación, seleccione Configurar.
   ![Imagen 41](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image15.png)</br>
   Si se selecciona Configurar, SSO de conexión directa se configurará según el paso de versiones preliminares. La configuración de Sincronización de hash de contraseña no se modificará, ya que se ha habilitado previamente.
   
   > [!IMPORTANT]
   > No se realizará ningún cambio en la forma en que los usuarios inician sesión en este momento.  
   
   7. En el Portal de Azure AD, compruebe que el valor de Federación sigue siendo Habilitado y que el de Inicio de sesión único de conexión directa es Habilitado.
   ![Imagen 42](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image16.png)

#### <a name="convert-domains-from-federated-to-managed"></a>Convertir dominios de federado a administrado

En este momento, Federación sigue habilitada y operativa para los dominios. Para continuar con la implementación, todos los dominio deben convertirse de Federado a Administrado para forzar la autenticación del usuario a través de Sincronización de hash de contraseña.

> [!IMPORTANT]
> No todos los dominios necesitan convertirse al mismo tiempo, puede elegir empezar con un dominio de prueba en el inquilino de producción o el dominio con el menor número de usuarios.

La conversión se realiza mediante el módulo de PowerShell de Azure AD.

   1. Abra PowerShell e inicie sesión en Azure AD mediante una cuenta de administrador global.  
   2. Para convertir el primero dominio, ejecute el comando siguiente:  
   
   ``` PowerShell
   Set-MsolDomainAuthentication -Authentication Managed -DomainName <domainname>
   ```
   
   3. Abra el portal de Azure AD, seleccione Azure Active Directory y, después, seleccione Azure AD Connect.
   4. Compruebe que el dominio se ha convertido a Administrado, para lo que debe ejecutar el siguiente comando:
   
   ``` PowerShell
   Get-MsolDomain -DomainName <domainname>
   ```

## <a name="testing-and-next-steps"></a>Pruebas y pasos siguientes

### <a name="test-authentication-with-phs"></a>Prueba de la autenticación con Sincronización de hash de contraseña

Cuando el inquilino usaba la federación, los usuarios se redirigían de la página de inicio de sesión de Azure AD al entorno de AD FS. Ahora que el inquilino está configurado para utilizar Sincronización de hash de contraseña en lugar de Federación, los usuarios no se redirigirán a AD FS y en su lugar iniciarán sesión directamente mediante la página de inicio de sesión de Azure AD.

Abra Internet Explorer en modo InPrivate para evitar que el inicio de sesión único de conexión directa inicie sesión automáticamente y vaya a la página de inicio de sesión de Office 365 ([https://portal.office.com](https://portal.office.com/)). Escriba el nombre principal del usuario y haga clic en Siguiente. Asegúrese de escribir el nombre principal de usuario de un usuario híbrido que se haya sincronizado desde la instancia de Active Directory local y que se haya federado previamente. El usuario verá la pantalla donde va a escribir el nombre de usuario y la contraseña.

![Imagen 9](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image18.png)

![Imagen 12](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image19.png)

Una vez escrita la contraseña, se le redirigirá al portal de Office 365.

![Imagen 17](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image20.png)

### <a name="test-seamless-single-sign-on"></a>Prueba del inicio de sesión único de conexión directa

Inicie sesión en un equipo unido a un dominio que esté conectado a la red corporativa. Abra Internet Explorer y vaya a una de las direcciones URL siguientes:  
  
[https://myapps.microsoft.com/contoso.com](https://myapps.microsoft.com/contoso.com) [https://myapps.microsoft.com/contoso.onmicrosoft.com](https://myapps.microsoft.com/contoso.onmicrosoft.com) (reemplace Contoso por su dominio).

El usuario se redirigirá brevemente a la página de inicio de sesión de Azure AD y verá el mensaje "Intentando iniciar sesión" y no se le pedirá ni un nombre de usuario ni una contraseña.

![Imagen 24](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image21.png)

A continuación, se le redirigirá al usuario e iniciará sesión en el panel de acceso correctamente:

> [!NOTE]
> El inicio de sesión único de conexión directa funciona en los servicios de Office 365 que admiten sugerencias de dominio (por ejemplo, myapps.microsoft.com/contoso.com). El portal de Office 365 (portal.office.com) actualmente no admite sugerencias de dominio y por lo tanto se espera que los usuarios tengan que escribir su nombre principal de usuario. Cuando se introduce un nombre principal de usuario, el inicio de sesión único de conexión directa puede recuperar el vale de Kerberos en nombre del usuario e iniciar sesión sin necesidad de escribir una contraseña. 

> [!TIP]
> Considere la posibilidad de implementar la [unión híbrida de Azure AD en Windows 10](https://docs.microsoft.com/azure/active-directory/device-management-introduction) para mejorar la experiencia de inicio de sesión único.

### <a name="removal-of-the-relying-party-trust"></a>Eliminación de la relación de confianza para usuario autenticado

Cuando haya comprobado que todos los usuarios y clientes se están autenticando correctamente mediante Azure AD, se puede considerar seguro quitar la relación de confianza para usuario autenticado en Office 365.

Si AD FS no se utiliza para otros fines (se han configurado otras relaciones de confianza para usuario autenticado), es seguro retirar AD FS ahora.

### <a name="rollback"></a>Reversión

Si surge algún problema importante que no se pueda resolver rápidamente, puede decidir revertir la solución a la opción de Federación.

Consulte la documentación acerca del diseño e implementación de la federación para ver los detalles de su implementación concreta. El proceso debe implicar:

* Convertir dominios administrados a federados mediante Convert-MSOLDomainToFederated

* Si fuera necesario, configurar reglas de notificaciones adicionales.

### <a name="enable-synchronization-of-userprincipalname-updates"></a>Habilitación de la sincronización de actualizaciones de userPrincipalName

Históricamente, las actualizaciones para el atributo UserPrincipalName con el servicio de sincronización desde una instancia local han estado bloqueadas, a menos que se cumplieran las siguientes condiciones:

* El usuario está administrado (no federado).

* Al usuario no se le ha asignado una licencia.

Para obtener instrucciones sobre cómo comprobar o habilitar esta característica, consulte el siguiente artículo:

[Sincronización de actualizaciones de userPrincipalName](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsyncservice-features).

### <a name="troubleshooting"></a>solución de problemas

El equipo de soporte técnico debe saber solucionar cualquier problema de autenticación que surja durante el cambio de federación a administrado, o después. Use la siguiente documentación de solución de problemas para ayudar al equipo de soporte técnico a familiarizarse con los pasos comunes para la solución de problemas comunes y las acciones adecuadas que pueden ayudar a aislar y resolver el problema.

[Solución de problemas de Sincronización de hash de contraseña en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization)

[Solución de problemas de Inicio de sesión único de conexión directa en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sso)  

## <a name="roll-over-the-seamless-sso-kerberos-decryption"></a>Sustitución del descifrado de Kerberos de inicio de sesión único de conexión directa

Es importante implementar con frecuencia la clave de descifrado de Kerberos de la cuenta de equipo AZUREADSSOACC (que representa a Azure AD) creada en el bosque local de AD. Recomendamos encarecidamente que sustituya la clave de descifrado de Kerberos al menos cada 30 días para que se corresponda con la forma en que los miembros del dominio de Active Directory envían los cambios de contraseña. Como no hay ningún dispositivo asociado conectado al objeto de la cuenta del equipo AZUREADSSOACC, la sustitución debe realizarse manualmente.

Siga estos pasos en el servidor local donde está ejecutando Azure AD Connect para iniciar la sustitución de la clave de descifrado de Kerberos.

¿[Cómo puedo implementar la clave de descifrado de Kerberos de la cuenta de equipo AZUREADSSOACC](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq)?

## <a name="next-steps"></a>Pasos siguientes

- [Conceptos de diseño de Azure AD Connect](plan-connect-design-concepts.md)
- [Selección de la autenticación adecuada](https://docs.microsoft.com/azure/security/azure-ad-choose-authn)
- [Topologías admitidas](plan-connect-design-concepts.md)
