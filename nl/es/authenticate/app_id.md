---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-19"

keywords: authentication swift, security swift, forgot password swift, social swift, identity provider swift, tentantid swift, cloud directory swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Adición de autenticación de usuario
{: #appid}

La seguridad de las aplicaciones es increíblemente complicada. Para la mayoría de los desarrolladores, es una de las tareas más difíciles de la creación de una app. ¿Cómo puede estar seguro de que está protegiendo la información de los usuarios? Al integrar {{site.data.keyword.appid_full}} en sus apps, puede proteger recursos y añadir autenticación, incluso aunque no tenga mucha experiencia en seguridad.

Al exigir a los usuarios que inicien sesión, puede almacenar datos de usuario como, por ejemplo, preferencias de la app (o información de los perfiles sociales públicos), y luego utilizar estos datos para personalizar la experiencia de cada usuario dentro de la app. {{site.data.keyword.appid_short_notm}} proporciona un registro en la infraestructura, pero también puede traer las pantallas de inicio de sesión de su propia marca para su uso con el directorio en la nube.

Para ver todas las formas en que puede utilizar {{site.data.keyword.appid_short_notm}} y la información de arquitectura, consulte [Acerca de {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-about#about).

## Antes de empezar
{: #prereqs-appid}

Primero, asegúrese de cumplir los siguientes requisitos previos:
* CocoaPods (versión 1.1.0 o posterior)
* iOS (versión 9 o posterior)
* MacOS (versión 10.11.5 o posterior)
* Xcode (versión 9.0.1 o posterior)

## Paso 1. Creación de una instancia de {{site.data.keyword.appid_short_notm}}
{: #create-instance-appid}

Cree una instancia del servicio {{site.data.keyword.appid_short_notm}}:

1. En el [catálogo de {{site.data.keyword.cloud_notm}}](https://{DomainName}/catalog){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"), seleccione {{site.data.keyword.appid_short_notm}}. Se abre la pantalla de configuración del servicio.
2. Dé un nombre a la instancia de servicio, o utilice el nombre preestablecido.
3. Seleccione el plan de precios y pulse **Crear**.

## Paso 2. Instalación del SDK de Swift de iOS
{: #install-sdk-appid}

El servicio proporciona un SDK para facilitar la programación de la app. El SDK debe estar instalado en el código de la app.

1. Abra el directorio de proyecto Xcode existente en el archivo `Podfile`.

2. En el destino del proyecto, añada una dependencia para el pod de `IBMCloudAppID`. Asegúrese de que el mandato `use_frameworks!` también esté bajo su destino, tal como se muestra en el siguiente ejemplo.
    ```swift
  target '<yourTarget>' do
     use_frameworks!
        pod 'IBMCloudAppID'
    end
    ```
    {: pre}

3. Descargue la dependencia de `IBMCloudAppID`.
    ```
    pod install --repo-update
    ```
    {: pre}

## Paso 3. Inicialización del SDK
{: #initialize-sdk-appid}

Después de inicializar el SDK en la app, puede empezar a configurar las preferencias de {{site.data.keyword.appid_short_notm}}.

1. Vaya a **Configuración del proyecto > Prestaciones > Keychain Sharing** y habilite el uso compartido de la cadena de claves en el proyecto Xcode.

2. Vaya a **Configuración del proyecto > Información > Tipos de URL** y añada el valor siguiente a los recuadros de texto **Esquema de URL** e **Identificador**.
  ```
  $(PRODUCT_BUNDLE_IDENTIFIER)
  ```
  {: codeblock}

3. Añada la siguiente importación al archivo `AppDelegate.swift`.
  ```swift
  import IBMCloudAppID
  ```
  {: codeblock}

4. Pase los parámetros `tenantID` y `AppID_region` para inicializar el SDK. Un lugar común, aunque no obligatorio, donde poner el código es en el método `application:didFinishLaunchingWithOptions` del `AppDelegate` de la app.
  ```swift
  AppID.getInstance().initialize(getApplicationContext(), <tenantId>, <AppID_region>);
  ```
  {: codeblock}
  
  * `tenantID`: El ID de arrendatario es un identificador exclusivo que se utiliza para inicializar la app. Puede encontrar el valor en el panel de control de {{site.data.keyword.appid_short_notm}} seleccionando el separador **Credenciales de servicio** y pulsando **Ver credenciales**.
  * `AppID_region`: La región de {{site.data.keyword.appid_short_notm}} es la región de {{site.data.keyword.cloud_notm}} en la que está trabajando con el servicio. Esta región se puede encontrar en el panel de control de servicio y puede ser `AppID.REGION_US_SOUTH`, `AppID.REGION_SYDNEY` y `AppID.REGION_UK`.

5. Añada el código siguiente al archivo `AppDelegate`.
    ```swift
  func application(_ application: UIApplication, open url: URL, options :[UIApplicationOpenURLOptionsKey : Any]) -> Bool {
            return AppID.sharedInstance.application(application, open: url, options: options)
      }
    ```
    {: codeblock}

## Paso 4. Gestión de la experiencia de inicio de sesión
{: #managing-signin-appid}

Un proveedor de identidad proporciona la información de autenticación para los usuarios para que pueda autorizarlos. Con {{site.data.keyword.appid_short_notm}}, puede utilizar los proveedores de identidad social como Facebook y Google+ o puede gestionar un registro de usuarios con el directorio en la nube.

Puede actualizar las configuraciones en cualquier momento sin actualizar el código de la app.
{: tip}

### Proveedores de identidad social
{: #social-appid}

Con {{site.data.keyword.appid_short_notm}}, puede utilizar proveedores de identidad social como Facebook y Google+ para proteger sus apps.

Para configurar los proveedores de identidad social:

1. Abra el panel de control de {{site.data.keyword.appid_short_notm}} en **Proveedores de identidad > Gestionar**.
2. Establezca los proveedores de identidad que desee utilizar en **Activado**. Puede utilizar cualquier combinación de proveedores de identidad, pero si desea traer pantallas de inicio de sesión personalizadas, solo tiene que habilitar el Directorio de nube.
3. Actualice la [configuración predeterminada](/docs/services/appid?topic=appid-social#social) a sus propias credenciales. {{site.data.keyword.appid_short_notm}} proporciona las credenciales de IBM que puede utilizar para probar el servicio, pero, antes de publicar la app, tiene que actualizar la configuración.
4. Personalice la pantalla de inicio de sesión para mostrar la imagen y los colores de su elección.
5. Para llamar al widget de inicio de sesión con la app, añada el siguiente mandato al código.
    ```swift
    import IBMCloudAppID
    class delegate : AuthorizationDelegate {
        public func onAuthorizationSuccess(accessToken: AccessToken, identityToken: IdentityToken, response:Response?) {
            //Usuario autenticado
		}

        public func onAuthorizationCanceled() {
            //Autenticación cancelada por el usuario
        }

        public func onAuthorizationFailure(error: AuthorizationError) {
            //Se ha producido una excepción
        }
    }

    AppID.sharedInstance.loginWidget?.launch(delegate: delegate())
    ```
    {: codeblock}

### Directorio en la nube
{: #cloud-dir-appid}

Con {{site.data.keyword.appid_short_notm}}, puede gestionar su propio registro de usuarios denominado directorio en la nube. El directorio en la nube permite a los usuarios registrarse e iniciar sesión en sus apps móviles y web utilizando su correo electrónico y una contraseña.

Para traer sus propias pantallas de interfaz de usuario de marca, solo se puede habilitar el directorio en la nube como proveedor de identidad.
{: tip}

Para configurar el directorio en la nube:

1. Abra el panel de control de {{site.data.keyword.appid_short_notm}} en el separador **Gestión de proveedores de identidad** y establezca el directorio en la nube en **Activado**.
2. Configure los [valores de directorios y mensajes](/docs/services/appid?topic=appid-cloud-directory).
4. Elija las combinaciones de pantallas de inicio de sesión que desee mostrar y coloque el código para llamar a dichas pantallas en la aplicación.
    * Iniciar sesión
        ```swift
        class delegate : TokenResponseDelegate {
            public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
            /* Usuario autenticado */
            }

            public func onAuthorizationFailure(error: AuthorizationError) {
            /* Se ha producido una excepción */
            }
        }

        AppID.sharedInstance.obtainTokensWithROP(username: username, password: password, delegate: delegate())
        ```
        {: codeblock}

    * Registro
        ```swift
        class delegate : AuthorizationDelegate {
          public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             if accessToken == nil && identityToken == nil {
              /* hace falta la verificación por correo electrónico */
              return
             }
           /* Usuario autenticado */
          }

          public func onAuthorizationCanceled() {
              /* Autenticación cancelada por el usuario */
          }

          public func onAuthorizationFailure(error: AuthorizationError) {
              /* Se ha producido una excepción */
          }
        }

        AppID.sharedInstance.loginWidget?.launchSignUp(delegate: delegate())
        ```
        {: codeblock}

    * Contraseña olvidada
        ```swift
        class delegate : AuthorizationDelegate {
           public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
              /* olvido de contraseña finalizado, en este caso, accessToken e identityToken serán null. */
           }

           public func onAuthorizationCanceled() {
               /* olvido de contraseña cancelado por el usuario */
           }

           public func onAuthorizationFailure(error: AuthorizationError) {
               /* Se ha producido una excepción */
           }
        }

        AppID.sharedInstance.loginWidget?.launchForgotPassword(delegate: delegate())
        ```
        {: codeblock}

    * Detalles de la cuenta
        ```swift

         class delegate : AuthorizationDelegate {
             public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             }

             public func onAuthorizationCanceled() {
             }

             public func onAuthorizationFailure(error: AuthorizationError) {
             }
         }

         AppID.sharedInstance.loginWidget?.launchChangeDetails(delegate: delegate())
        ```
        {: codeblock}

    * Cambio de contraseña
        ```swift
         class delegate : AuthorizationDelegate {
             public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             }

             public func onAuthorizationCanceled() {
             }

             public func onAuthorizationFailure(error: AuthorizationError) {
             }
          }

          AppID.sharedInstance.loginWidget?.launchChangePassword(delegate: delegate())
        ```
        {: codeblock}


## Paso 5. Comprobación de la app
{: #testing-appid}

¿Todo se ha configurado correctamente? Puede probarlo ahora.

1. Abra la app con el emulador de Xcode.
2. Mediante la GUI, vaya a través del proceso de inicio de sesión en la aplicación. Si ha configurado el directorio en la nube, asegúrese de que todas las pantallas se visualicen como tiene previsto.
3. Actualice los proveedores de identidades o la pantalla del widget de inicio de sesión en el panel de control de {{site.data.keyword.appid_short_notm}}.
4. Repita los pasos 1 y 2 para ver que los cambios se implementan inmediatamente. No es necesario realizar actualizaciones en el código de la app.

¿Tiene problemas? Consulte [resolución de problemas de {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-troubleshooting).

## Pasos siguientes
{: #next-appid notoc}

¡Buen trabajo! Ha añadido un nivel de seguridad a la app. Mantenga el ritmo probando una de las opciones siguientes:

* Obtenga más información y aproveche todas las características que {{site.data.keyword.appid_short_notm}} tiene que ofrecer, [consulte los documentos](/docs/services/appid?topic=appid-getting-started#getting-started).
* Los kits de inicio son una de las formas más rápidas de utilizar las características de {{site.data.keyword.cloud_notm}}. Vea los kits de inicio disponibles en el [panel de control de desarrollador de Mobile](https://{DomainName}/developer/mobile/dashboard){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"). Descargue el código. Ejecute la app.
