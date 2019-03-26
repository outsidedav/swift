---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Recopilación de analíticas móviles
{: #mobile_analytics}

{{site.data.keyword.mobileanalytics_short}} on {{site.data.keyword.cloud_notm}} proporciona a los desarrolladores, a los administradores y a las partes interesadas del negocio información sobre el rendimiento de sus apps para móviles y cómo se están utilizando. Con el servicio de {{site.data.keyword.mobileanalytics_short}} puede hacer lo siguiente:

 - **Obtener una perspectiva inmediata**: Consulte las métricas de rendimiento y uso en tiempo real.

 - **Efectuar implementaciones en cuestión de minutos**: Cree una instancia de servicio en {{site.data.keyword.cloud_notm}}, añada el SDK al proyecto, pegue dos líneas de código a la aplicación y ya podrá recopilar numerosas métricas predefinidas.

 - **Recopila los datos que desea**: Instrumente apps con sucesos personalizados, vea cómo interactúan los usuarios con la app, realice el seguimiento de las compras y supervise la actividad de la app.

 - **Consultar de un vistazo las métricas**: La consola de {{site.data.keyword.mobileanalytics_short}} ofrece gráficos ya preparados, sin necesidad de escribir consultas.

 - **Centrarse en lo que considera importante**: Filtre las métricas por tiempo, adaptador, aplicación, versión de aplicación, sistema operativo, versión de sistema operativo o por modelo de dispositivo.

 - **Detectar problemas rápidamente**: Supervise el estado de bloqueo. Establezca activadores de alertas en las métricas esenciales y enrute alertas a cualquier punto final REST.

 - **Identificación de la causa raíz del problema**: Utilice el registro personalizado en la aplicación y cargue de forma automática los registros para buscarlos desde la consola. Obtenga un mayor detalle de los sucesos de bloqueo para consultar los seguimientos de pila.

## Antes de empezar
{: #prereqs-analytics}

Primero, asegúrese de cumplir los siguientes requisitos previos:

 - iOS 8.0+ / watchOS 2.0+
 - Xcode 7.3, 8.0
 - Swift 2.2 - 3.0
 - CocoaPods o Carthage

## Paso 1. Creación de una instancia de {{site.data.keyword.mobileanalytics_short}}
{: #create-analytics}

1. En el catálogo de {{site.data.keyword.cloud_notm}}, pulse **Móvil** > **{{site.data.keyword.mobileanalytics_short}}**. Se abre la pantalla de configuración del servicio.
2. Dé un nombre a la instancia de servicio, o utilice el nombre preestablecido.
3. Pulse **Crear**.
4. En el panel de navegación, pulse **Conexiones** para seleccionar una app y enlazarla al servicio. Puede enlazar la instancia de servicio a su app más adelante si la deja desenlazada durante la creación.

## Paso 2. Instalación del SDK de Swift de iOS
{: #install-analytics-swift}

El servicio proporciona SDK específicos de la plataforma para simplificar el desarrollo de aplicaciones. Los SDK de Swift de {{site.data.keyword.cloud_notm}} Mobile Services se pueden instalar con CocoaPods o Carthage. Para obtener más información, consulte [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics).

Puede instrumentar la aplicación móvil utilizando el SDK de {{site.data.keyword.mobileanalytics_full}}. El SDK de Swift está disponible para iOS y watchOS.

1. Asegúrese de configurar Xcode correctamente. Para ver cómo se configura el entorno de desarrollo de iOS, consulte el [sitio web del desarrollador de Apple ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.apple.com/support/xcode/){: new_window}. Consulte los [requisitos de Xcode ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#requirements){: new_window} para Client SDK Swift Analytics.

2. Habilite la API de ubicación añadiendo una propiedad en el archivo `Info.plist` en la carpeta del proyecto de la app. Por ejemplo, `Privacidad - Descripción de uso de ubicación` y escriba una justificación adecuada para añadir la API de ubicación, como "La app requiere que el servicio de ubicación esté habilitado".

El SDK de {{site.data.keyword.mobileanalytics_short}} se distribuye con [CocoaPods ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cocoapods.org/){: new_window} y [Carthage ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/Carthage/Carthage#getting-started){: new_window}, que son gestores de dependencias para los proyectos de Cocoa. CocoaPods y Carthage descargan artefactos de forma automática de los repositorios para ponerlos a disposición de la aplicación. Seleccione CocoaPods o Carthage:

### CocoaPods
{: #cocoapods-analytics}

1. Siga las [instrucciones del SDK de Swift de {{site.data.keyword.Bluemix_notm}} Mobile Services ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#cocoapods){: new_window} en GitHub para instalar `BMSAnalytics` utilizando CocoaPods y añádalo al Podfile.

2. Tras instalar el SDK del cliente de iOS, [importe e inicialice](sdk.html#initalize-ma-sdk) el SDK del cliente de Analytics.   

### Carthage
{: #carthage-analytics}

Si no utiliza CocoaPods, puede añadir infraestructuras al proyecto mediante [Carthage ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos){: new_window}.

1. Siga las [instrucciones de instalación de Carthage ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#carthage){: new_window} en GitHub para instalar `BMSAnalytics`.

2. Tras instalar el SDK del cliente de iOS, importe e inicialice el SDK del cliente de Analytics.

## Paso 3. Inicialización del SDK
{: #initialize-analytics}

Utilizando {{site.data.keyword.mobileanalytics_short}} puede recopilar las siguientes categorías de datos, cada una de las cuales requiere un nivel de preparación distinto:

**Datos predefinidos**: Esta categoría incluye información genérica de uso y de los dispositivos que se aplica a todas las aplicaciones. Indica el volumen, la frecuencia o el tiempo de uso de una aplicación. Los datos predefinidos se recopilan automáticamente una vez inicializado el SDK de {{site.data.keyword.mobileanalytics_short}} en la aplicación.

**Mensajes de registro de aplicaciones**: Esta categoría permite que los desarrolladores añadan líneas de código a la aplicación que pueden registrar mensajes personalizados para ayudar con el desarrollo y a la depuración. El desarrollador asigna un nivel de gravedad/detalle a cada mensaje de registro.

**Sucesos personalizados**: Esta categoría incluye datos que define usted mismo y que son específicos de la app. Estos datos muestran los sucesos que se producen en la app, como las vistas de páginas, las pulsaciones de botones o las compras dentro de la app. Además de inicializar el SDK de {{site.data.keyword.mobileanalytics_short}} en la app, debe añadir una línea de código para cada suceso personalizado que desee seguir.

## Paso 4. Identificación del valor de la clave de API de credenciales de servicio
{: #analytics-clientkey}

Identifique el valor **Clave de API** antes de configurar el SDK de cliente. La clave de API es necesaria para inicializar el SDK de cliente.

1. Abra el panel de control del servicio {{site.data.keyword.mobileanalytics_short}}.
2. Expanda **Ver credenciales** para revelar el valor de la Clave de API. Necesita el valor de la clave de API al inicializar el SDK de cliente de {{site.data.keyword.mobileanalytics_short}}.

## Paso 5.  Inicialización de la aplicación para recopilar análisis
{: #initalize-ma-sdk}

Inicialice la aplicación para habilitar el envío de registros al servicio {{site.data.keyword.mobileanalytics_short}}.

1. Importe el SDK de cliente.

    El SDK de Swift está disponible para iOS y watchOS. Importe las infraestructuras `BMSCore` y `BMSAnalytics`; para ello, añada las siguientes sentencias `import` al inicio del archivo del proyecto `AppDelegate.swift`:
	```swift
	import BMSCore
	import BMSAnalytics
	```
	{: codeblock}  

2. Inicialice el SDK de cliente de {{site.data.keyword.mobileanalytics_short}} en la aplicación.

	Primero, inicialice la clase `BMSClient` colocando el código de inicialización en el método `application(_:didFinishLaunchingWithOptions:)` de delegado de la aplicación o en la ubicación que mejor se ajuste a su proyecto.
	```swift
	BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Asegúrese de que apunta a su región
	```
	{: codeblock}

	Debe inicializar `BMSClient` con el parámetro **bluemixRegion**. En el inicializador, el valor **bluemixRegion** especifica el despliegue de {{site.data.keyword.Bluemix_notm}} que está utilizando.

3. Inicialice Analytics utilizando el objeto de la aplicación y asignándole el nombre de su aplicación.

	El nombre que seleccione para la aplicación (`your_app_name_here`) se muestra en la consola de {{site.data.keyword.mobileanalytics_short}} como nombre de la aplicación. El nombre de la aplicación se utiliza como filtro para buscar registros de aplicaciones en el panel de control. Si utiliza el mismo nombre de aplicación en varias plataformas (por ejemplo, iOS), puede ver todos los registros de esa aplicación con el mismo nombre, independientemente de la plataforma desde la que se han enviado los registros.

	También necesita el valor [Clave de API](#analytics-clientkey).
	```swift
	Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, collectLocation: true, deviceEvents: .lifecycle, .network)
	```
    {: codeblock}

4. La aplicación se inicializa ahora y está lista para recopilar analíticas. A continuación puede enviar datos analíticos al servicio {{site.data.keyword.mobileanalytics_short}}.		

## Paso 6. Recopilación de analíticas de uso
{: #usage-analytics}

Puede configurar el SDK de cliente de {{site.data.keyword.mobileanalytics_short}} para que registre analíticas de uso y envíe los datos registrados al servicio {{site.data.keyword.mobileanalytics_short}}.

Utilice las siguientes API para empezar a registrar y enviar analíticas de uso:
```swift
// Inhabilitar el registro de las analíticas de uso (p. ej., para ahorrar espacio de disco)
// El registro está habilitado de forma predeterminada

Analytics.isEnabled = false

// Habilitar el registro de las analíticas de uso

Analytics.isEnabled = true

// Enviar las analíticas de uso registradas al servicio de Mobile Analytics

Analytics.send(completionHandler: { (response: Response?, error: Error?) in
	    if let response = response {
	  // Gestionar aquí el resultado correcto del envío de Analytics.
            }
    if let error = error {
      // Gestionar aquí el resultado erróneo del envío de Analytics.
    }
})
```
{: codeblock}

Análisis de uso de ejemplo para el registro de un suceso:
```swift
// Registrar un suceso de analíticas personalizado
let eventObject = ["customProperty": "propertyValue"]
Analytics.log(metadata: eventObject)
```
{: codeblock}

## Paso 7. Uso del registrador
{: #analytics-logger}

El SDK de cliente de {{site.data.keyword.mobileanalytics_full}} proporciona una infraestructura de registro que se parece a otras infraestructuras de registro con las que puede estar familiarizado, como `java.util.logging` o `log4j`. La infraestructura de registro admite varias instancias del registrador por paquete, distintos niveles de registro, la captura de seguimientos de pilas del bloqueo de una aplicación, etc.

También puede configurar los datos registrados para almacenarlos en el dispositivo en el que se ejecuta la aplicación y enviar posteriormente los registros del dispositivo al servicio {{site.data.keyword.mobileanalytics_short}}.

La infraestructura de registro del SDK de cliente de {{site.data.keyword.mobileanalytics_short}} admite los siguientes niveles de registro, que aparecen ordenados de menos a más detallados, con directrices de uso recomendado:

  * `FATAL`: se usa para bloqueos irrecuperables. El nivel `FATAL` está reservado para errores de registro irrecuperables que ven los usuarios como un bloqueo de aplicación
  * `ERROR`: se usa para excepciones o errores de protocolo de red no esperados
  * `WARN`: se usa para registrar avisos de uso que no se consideran errores críticos, como el uso de API en desuso o en caso de una respuesta de red lenta
  * `INFO`: se usa para registrar sucesos de inicialización y otros datos que podrían ser importantes, pero no urgentes
  * `DEBUG`: se usa para notificar sentencias de depuración para que los desarrolladores puedan resolver defectos de la aplicación

### Caso de ejemplo de nivel de registro
{: #log-level-scenario}

Si el nivel de registro está configurado en `FATAL`, el registrador captura las excepciones no capturadas, pero no captura ningún registro que conduzca al suceso de bloqueo. Puede establecer un nivel de registro más detallado para garantizar que también se capturen los registros que puedan conducir a una entrada de registro `FATAL`, como `WARN` y `ERROR`.

Cuando el nivel de registro es `DEBUG`, también obtiene registros del SDK de cliente de Mobile Analytics, que se incluye cuando se envían los registros.

### Ejemplo de uso del registrador
{: #sample-logger-usage}

**Nota:** Asegúrese de que ha configurado la aplicación para que utilice el SDK de cliente de {{site.data.keyword.mobileanalytics_short}} antes de utilizar la infraestructura de registro.

En los siguientes fragmentos de código se muestra un ejemplo de uso del registrador:
```swift
// Configurar el registrador. Está inhabilitado de forma predeterminada;

Logger.isLogStorageEnabled = true

// Cambiar el nivel de registro mínimo (opcional). El valor predeterminado es - LogLevel.debug

Logger.logLevelFilter = LogLevel.info

// Se pueden crear varias instancias de registro para organizar sus registros

let logger1 = Logger.logger(name: "feature1Logger")
let logger2 = Logger.logger(name: "feature2Logger")

// Registrar mensajes con distintos niveles

logger1.debug(message: "debug message for feature 1")

// El mensaje logger1.debug no se registra porque logLevelFilter está establecido en info

logger2.info(message: "info message for feature 2")

// Enviar registros al servicio Mobile Analytics

Logger.send(completionHandler: { (response: Response?, error: Error?) in
        if let response = response {
        logger.debug(message: "Status code: \(response.statusCode)")
        logger.debug(message: "Response: \(response.responseText)")
    }
    if let error = error {
        logger.error(message: "Error: \(error)")
    }
})
```
{: codeblock}

Por motivos de privacidad, puede inhabilitar la salida del registrador para las aplicaciones compiladas en modo de publicación. De forma predeterminada, la clase Logger imprime registros en la consola de Xcode. En la configuración de compilación del destino, añada un indicador `-D RELEASE_BUILD` a la sección **Otros indicadores Swift** de la configuración de compilación de la versión.
{: tip}

## Paso 8: Registro de datos de ubicación
{: #location-logging}

La ubicación del dispositivo móvil se puede registrar desde la app mediante esta API suministrada:
```swift
Analytics.logLocation();
```

La API permite a la app para enviar su ubicación (como latitud y longitud) al servidor entre sesiones de app. Además de las llamadas explícitas a la API `location-logging`, el SDK envía la ubicación del dispositivo para cada sesión de app. La ubicación se envía para el contexto de sesión de app inicial y el contexto de la sesión de app de conmutación de usuario (es decir, el cambio de un usuario entre una sesión de app). La API de ubicación debe estar habilitada en la inicialización del SDK.

Para llamar a la API `location-logging`, establezca el parámetro `collectLocation` en `true` en la inicialización del SDK:
```swift
Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
```

## Paso 9. Realización de una solicitud de red
{: #network-requests}

Puede configurar el SDK de cliente de {{site.data.keyword.mobileanalytics_short}} para [realizar una solicitud de red](/docs/mobile/sdk_network_request.html). Asegúrese de haber inicializado `BMSClient` y `BMSAnalytics` y de haber importado los SDK de cliente.

## Paso 10. Notificar analíticas de bloqueo
{: #report-crash-analytics}

Puede ver [datos de bloqueo de aplicación](app-monitoring.html#monitor-app-crash) enviando información de analíticas y de registro a {{site.data.keyword.mobileanalytics_short}}.

El método `Analytics.send()` rellena las tablas **Visión general de bloqueos** y **Bloqueos** en la página **Bloqueos**. Puede habilitar los gráficos utilizando el proceso de inicialización y de envío para su análisis.

El método `Logger.send()` rellena las tablas **Resumen de bloqueos** y **Detalles de bloqueo** en la página **Resolución de problemas**. Debe habilitar la aplicación para almacenar y enviar registros para rellenar los gráficos, añadiendo una sentencia en el código de la aplicación:
```swift
Logger.isLogStorageEnabled = true
Logger.logLevelFilter = LogLevel.Fatal // o superior
```
{: codeblock}

Consulte el [ejemplo de uso del registrador](sdk.html##sample-logger-usage) de iOS.

## Paso 11. Seguimiento de usuarios activos
{: #trackingusers}

Si la aplicación reconoce usuarios exclusivos en un dispositivo, puede realizar un seguimiento del número de usuarios que están utilizando de forma activa la aplicación; para ello pase el nombre del usuario a {{site.data.keyword.mobileanalytics_short}}.

Habilite el seguimiento de usuarios inicializando {{site.data.keyword.mobileanalytics_short}} con `hasUserContext=true`. De lo contrario, {{site.data.keyword.mobileanalytics_short}} solamente captura un usuario por dispositivo.

Añada el siguiente código para rastrear cuando el usuario inicie la sesión:
```swift
Analytics.userIdentity = "username"
```
{: codeblock}

## Paso 12. Comprobación de la app
{: #appid_testing}

¿Todo se ha configurado correctamente? Es el momento de probarlo.

1. Abra la app. Si tiene una aplicación web, utilice un navegador. Si tiene una aplicación cliente de iOS, ábrala con el emulador Xcode.
2. Compile y ejecute la aplicación en el emulador o dispositivo.
3. Mediante la GUI, vaya a través del proceso de inicio de sesión en la aplicación.
4. Vaya a la consola de {{site.data.keyword.mobileanalytics_short}} para ver las analíticas de uso para su aplicación. También puede supervisar la aplicación [definiendo alertas](/docs/services/mobileanalytics/app-monitoring.html#alerts) y [supervisando bloqueos de la app](/docs/services/mobileanalytics/app-monitoring.html#monitor-app-crash).

## Qué hacer a continuación
{: #next-analytics notoc}

 - Para obtener más información sobre el servicio, consulte la [documentación](/docs/services/mobileanalytics/index.html#getting-started-tutorial).

 - Para ver una visión general de los servicios para móviles y {{site.data.keyword.Bluemix_notm}}, consulte [Iniciación a las apps para móvil en IBM Cloud](/docs/services/mobile/index.html).

 - Los kits de inicio son una de las formas más rápidas de aprovechar las prestaciones de {{site.data.keyword.cloud_notm}}. Vea todos los kits de inicio disponibles en el [panel de control de desarrollador de Mobile](https://cloud.ibm.com/developer/mobile/dashboard). Descargue el código. Ejecute la app.

 - Puede utilizar la [IU de Swagger](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/) para consultar de forma rápida la documentación de la API REST.
