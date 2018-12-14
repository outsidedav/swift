---

copyright:
  years: 2018
lastupdated: "2018-11-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Guía de aprendizaje de iniciación
{: #set_up}

{{site.data.keyword.cloud}} ofrece soluciones y servicios para ayudar a los desarrolladores de Swift a crear aplicaciones que integren la seguridad, la IA y el valor que sus clientes exigen. Con una amplia cartera de ofertas y SDK, puede utilizar estos servicios y llevar aplicaciones innovadoras al mercado rápidamente. En esta guía de programación de Swift se muestra cómo añadir servicios a una aplicación Swift nueva o existente, ya sea un cliente de iOS o un Swift del lado del servidor.
{: shortdesc}

En la siguiente guía de aprendizaje se muestra cómo crear fácilmente una app móvil Swift con {{site.data.keyword.mobileanalytics_full}} utilizando un kit de iniciación vacío desde la [Consola del desarrollador para Apple de {{site.data.keyword.cloud_notm}}](https://console.bluemix.net/developer/appledevelopment/starter-kits). Desde la consola, añada el servicio {{site.data.keyword.mobileanalytics_short}}, descargue el código, ejecute la app de iOS localmente en Xcode, configure y supervise la app.

## Paso 1. Requisitos para los desarrolladores
{: #dev-requirements}

Para empezar a utilizar el desarrollo de iOS en {{site.data.keyword.cloud_notm}}, asegúrese de que cumple los requisitos siguientes.

### Sistema operativo

La práctica recomendada para desarrollar apps de Swift consiste en utilizar el hardware soportado más reciente de MacOS, y probarlas con los releases más recientes de iOS. Regístrese para una cuenta de [Desarrollador de Apple](https://developer.apple.com/) para habilitar las pruebas en un dispositivo físico.

### iOS y Xcode
{: #ios_and_xcode}

- Instale [Xcode 8+](https://developer.apple.com/xcode/) (o superior).
- Despliegue en [dispositivos iOS 8](https://support.apple.com/downloads/ios) (o superior).
- Revise las [Directrices de envío de la App Store](https://developer.apple.com/app-store/guidelines/) antes de enviar apps a Apple.

### SDK y gestión de dependencias

Las herramientas siguientes garantizan que puede instalar los SDK nativos para que funcionen con los distintos {{site.data.keyword.cloud_notm}} Services.

1. Instale [CocoaPods](https://cocoapods.org/) para dar soporte a los SDK de {{site.data.keyword.cloud_notm}}:
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
2. Instale [Homebrew](https://brew.sh/) para ayudar a instalar Carthage:
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

3. Instale [Carthage](https://github.com/Carthage/Carthage) para dar soporte a los SDK de Watson:
  ```
  brew install carthage
  ```
  {: codeblock}

## Paso 2. Creación de una app Swift de iOS personalizada
{: #create-ios-app}

1. Inicie la sesión en la [Consola del desarrollador para Apple de {{site.data.keyword.cloud_notm}}](https://console.bluemix.net/developer/appledevelopment/starter-kits).
2. Pulse **Crear app**.
3. En la página [Iniciador vacío](https://console.bluemix.net/developer/appledevelopment/create-app), puede utilizar la configuración predeterminada, o actualizar los campos según sea necesario. Asegúrese de que **iOS Swift** sea el lenguaje seleccionado. Pulse **Crear**.

## Paso 3. Adición del servicio {{site.data.keyword.mobileanalytics_short}}
{: #adding-services}

1. En la página Detalles de la app, pulse el botón **Añadir recurso**.
2. Seleccione **Móvil** y pulse **Siguiente**.
3. Seleccione **{{site.data.keyword.mobileanalytics_short}}**
y pulse **Siguiente**.
4. Pulse **Crear**.

## Paso 4. Descarga del código y configuración de los SDK de cliente
{: #run-locally}

Para descargar el código, pulse **Descargar código** en `Apps` > `Su app`. El código descargado se suministra con los SDK de cliente de **{{site.data.keyword.mobileanalytics_short}}** incluidos. Los SDK de cliente están disponibles en CocoaPods y Carthage. Para esta solución, utilice CocoaPods.

1. Descomprima el código descargado. A continuación, utilizando un terminal, vaya a la carpeta extraída. 
  ```
  cd <Name of Project>
  ```
2. La carpeta incluye un podfile con las dependencias necesarias. Ejecute el mandato siguiente para instalar las dependencias (SDK de cliente):
  ```
  pod install
  ```
  {: codeblock}

## Paso 5. Configuración de la app para utilizar {{site.data.keyword.mobileanalytics_short}}
{: #configure-analytics}

1. Abra `.xcworkspace` en Xcode y vaya a `AppDelegate.swift`.
  **Nota**: Asegúrese de abrir siempre el nuevo espacio de trabajo de Xcode, en lugar del archivo de proyecto Xcode original: `MyApp.xcworkspace`.
   ![Open Xcode](images/Xcode.png)

  `BMSCore` es el SDK central y es la base para los SDK de Mobile Client. `BMSClient` es una clase de `BMSCore` y se inicializa en `AppDelegate.swift`. Junto con `BMSCore`, el SDK de {{site.data.keyword.mobileanalytics_short}} ya se ha importado en el proyecto.
  
2. El código de inicialización de Analytics ya está incluido, tal como se muestra en el siguiente fragmento de código:
  ```
  // El SDK del cliente de Analytics se configura para registrar sucesos de ciclo de vida.
         	Analytics.initialize(appName:dictionary["appName"] as? String,
        			     apiKey: dictionary["analyticsApiKey"] as? String,
        	        	     deviceEvents: .lifecycle)

        	// Habilitar Logger (inhabilitado de forma predeterminada) y establecer el nivel en ERROR (DEBUG de forma predeterminada).
        	Logger.isLogStorageEnabled = true
        	Logger.logLevelFilter = .error
  ```
  {: codeblock}

  **Nota:** Las credenciales de servicio forman parte del archivo `BMSCredentials.plist`.

3. Recopilación de analíticas de uso mediante `logger`. Navegue hasta el archivo `ViewController.swift` para ver el código siguiente.
  ```
   func didBecomeActive(_ notification: Notification) {
       Analytics.send()
       Logger.send()
    }
  ```
  {: codeblock}

   Para las características avanzadas de análisis y de registro, consulte [Recopilación de uso de Analítica](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics) y [registro](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger).
   {:tip}

## Paso 6. Supervisión de la app con {{site.data.keyword.mobileanalytics_short}}
El servicio {{site.data.keyword.mobileanalytics_short}} proporciona información esencial sobre el uso y el rendimiento de aplicaciones para los propietarios y los desarrolladores de aplicaciones para móviles. Gracias a {{site.data.keyword.mobileanalytics_short}}, los propietarios y los desarrolladores de aplicaciones pueden ver lo que sucede en el lado del usuario. Pueden utilizar este conocimiento para compilar aplicaciones mejoradas que sean relevantes para los usuarios y que destaquen en el inmenso océano de aplicaciones para móviles.

El servicio incluye la consola de {{site.data.keyword.mobileanalytics_short}}, en la que los desarrolladores y los propietarios de aplicaciones pueden supervisar el rendimiento de las aplicaciones para móviles, consultar las estadísticas de uso y buscar registros de dispositivo.

1. Abra el servicio `{{site.data.keyword.mobileanalytics_short}}` desde la app móvil que ha creado o pulse los tres puntos verticales junto al servicio y seleccione `Abrir panel de control`.
2. Puede ver Usuarios LIVE, Sesiones y otros datos de app inhabilitando la `modalidad de demostración`. Puede filtrar la información de analíticas por los siguientes criterios:
    * Fecha.
    * Aplicación.
    * Sistema operativo.
    * Versión de la app.
         ![{{site.data.keyword.mobileanalytics_short}}](images/mobile_analytics.png)
3. [Pulse aquí](https://console.bluemix.net/docs/services/mobileanalytics/app-monitoring.html#monitoringapps) para establecer alertas, Supervisar bloqueos de apps y supervisar solicitudes de red.

## Pasos siguientes
{: #next-steps}

### Adición de más servicios
Puede añadir más servicios a la app de iOS directamente desde la consola web, como por ejemplo los siguientes servicios utilizados habitualmente:

* [Adición del servicio de notificaciones push](/docs/services/mobilepush/index.html)
* [Adición de autenticación de usuario con App ID](/docs/services/appid/index.html)

### Utilización de herramientas de desarrollador de {{site.data.keyword.cloud_notm}}
También puede aprender a desarrollar apps de Swift utilizando las [herramientas de desarrollador de {{site.data.keyword.cloud_notm}}](../cli/index.html), que ofrecen un enfoque de línea de mandatos para crear, desarrollar y desplegar aplicaciones web, móviles y de microservicios completas.

