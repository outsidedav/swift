---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-21"

keywords: foodtrackerbackend, kitura swift, urlsession sdk, alamofire, restkit, kiturakit, kitura, xcode kitura, meals swift, rest api kitura, rest api swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Creación de una app con Kitura
{: #kitura}

[Kitura](http://www.kitura.io){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") es una infraestructura de Swift del lado del servidor para crear programas de fondo y aplicaciones web de iOS. Esta infraestructura crea API REST que se pueden invocar desde la aplicación de iOS utilizando los SDK URLSession tales como Alamofire, RestKit o el SDK [KituraKit](https://github.com/ibm-swift/kiturakit){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") proporcionado por Kitura.

Kitura es capaz de integrarse con todos los servicios y características que proporciona {{site.data.keyword.cloud}}, incluidos {{site.data.keyword.appid_short}}, {{site.data.keyword.mobilepushshort}} y {{site.data.keyword.mobileanalytics_short}}, así como bases de datos, aprendizaje automático y otros servicios. A continuación, Kitura puede desplegarse y escalarse automáticamente utilizando las plataformas de Cloud Foundry o Docker (basadas en Kubernetes) en {{site.data.keyword.cloud}}.

Kitura proporciona una `kitura` [interfaz de línea de mandatos (CLI)](https://www.kitura.io/guides/kituracli/gettingstarted.html){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") que simplifica la creación, construcción, pruebas y despliegue de aplicaciones de Kitura. Las aplicaciones que se crean utilizando la CLI de Kitura incluyen soporte completo para desplegar en cualquier nube que dé soporte a las tecnologías de Cloud Foundry, Docker y Kubernetes. Sin embargo, si está creando específicamente para {{site.data.keyword.cloud_notm}}, se recomienda utilizar la IBM Apple Development Console en el navegador o utilizar la {{site.data.keyword.dev_cli_notm}}. Además, si bien ambos métodos comparten tecnología subyacente, la Apple Development Console y las IBM Developer Tools crean una app alojada y un conducto de despliegue, así como suministran los servicios que necesita la aplicación.

## Antes de empezar
{: #prereqs-kitura}

Primero, asegúrese de cumplir los siguientes requisitos previos:  

* iOS 11.0+  
* Xcode 9.0  
* Swift 4.0+  
* CocoaPods  

## Paso 1. Creación de una app Kitura utilizando el navegador
{: #create_kitura}

1. Vaya a la sección Kits de inicio de la Apple Development Console. Seleccione un iniciador predefinido, como por ejemplo "Swift for Backend for Frontend API" o cree una app personalizada utilizando el mosaico **Crear app**. Pulse **Crear app**.

2. Proporcione un nombre para su app y seleccione dónde desea que se despliegue la app. Si no está seguro de dónde se va a desplegar la aplicación, utilice los valores predeterminados, ya que se pueden cambiar más adelante.

3. Seleccione el lenguaje Swift. Se crea una app Swift del lado del servidor. También se muestran las opciones de Host y Dominio, que forman el URL para la aplicación. Si no está seguro, utilice los valores predeterminados y también puede proporcionar su propio dominio personalizado a partir de un proveedor de dominio en el que va a residir la aplicación.

4. Puede proporcionar una definición de OpenAPI (también conocida como Swagger) para la API REST que desea compilar. Si tiene una definición de este tipo, se crea una API REST que incluye las funciones de manejador correspondientes en Kitura. Si no tiene una definición de OpenAPI, no se preocupe porque Kitura hace que sea fácil crear manualmente API REST utilizando sus API de Router.

5. Pulse **Crear app**.

Se creará una app, pero uno que no utiliza todavía ningún servicio adicional. Puede añadir servicios pulsando **Añadir servicio** o el botón **Descargar código** para obtener el código para la app. También puede añadir fácilmente servicios a una app existente.

## Paso 2. Adición de servicios
{: #add_services-kitura}

1. Pulse **Añadir servicio** para añadir servicios. Se muestran las categorías de servicio. Por ejemplo, seleccione **Datos** para ver las bases de datos disponibles y seleccione **Base de datos Cloudant NoSQL**.
2. Seleccione un plan de precios para el servicio, por ejemplo Lite, y pulse **Crear**.

Se crea una instancia del servicio que proporciona las credenciales para la aplicación y, en algunos casos, añade el código necesario a la app para incluir la conexión correcta al servicio. Ahora puede añadir más servicios utilizando el botón **Añadir servicio** o pulsando **Descargar código** para obtener el código para la app.

Tras descargar la app, puede empezar a trabajar con ella.

## Paso 3. Desarrollo de la aplicación con Xcode
{: #develop_xcode-kitura}

Después de descargar la app, puede modificarla y desarrollarla utilizando Xcode y, a continuación, cargar la aplicación modificada para desplegarla en la nube.

1. Cree un proyecto Xcode.  
  Antes de poder utilizar el proyecto en Xcode, debe crear un archivo de proyecto Xcode utilizando el mandato Swift Package Manager. Ejecute el mandato siguiente desde el directorio raíz del proyecto descargado, donde reside el archivo `Package.swift`:
  ```
  swift package generate-xcodeproj
  ```
  {: codeblock}

  Se crea un proyecto Xcode en el mismo directorio que se puede abrir.

2. Establezca el destino de Xcode para el proyecto.  
  Para ejecutar el servidor de Kitura, debe editar el esquema pulsando la sección **project_name-Package** en la barra de herramientas y seleccionando el destino **project_name** en el menú. Compruebe que el dispositivo de destino esté establecido en **My Mac**.

3. Ejecute el servidor de Kitura localmente. 
  Pulse **Ejecutar** o utilice el atajo de teclado `⌘+R` para iniciar el servidor de Kitura. Una vez que se ha iniciado el servidor, puede comprobar que se están ejecutando los siguientes URL de Kitura estándar:
  * Kitura Monitoring: [http://localhost:8080/swiftmetrics-dash/]()
  * Kitura Health check: [http://localhost:8080/health]()

## Paso 4. Adición de API REST
{: #add_restapi-kitura}

Se crea un servidor de Kitura esqueleto, pero no proporciona ninguna API REST que pueda utilizar una aplicación iOS. Añada API REST en Kitura con codificación mínima. Siga los pasos siguientes para añadir una API REST para las solicitudes `GET` en `/meals`, que se ha diseñado para devolver los objetos `Meal` almacenados por el servidor de Kitura.

1. Añada una estructura `Meal` al archivo `Sources/Application/Application.swift`:  
  Antes de la declaración de la clase `App`, añada lo siguiente para crear una estructura Meal que se ajuste al protocolo Codable:  
  ```swift
	struct Meal: Codable {    
	    var name: String
	    var photo: Data
	    var rating: Int
	}
  ```
  {: codeblock}

2. Añada un Diccionario al archivo `Sources/Application/Application.swift` para almacenar objetos `Meal` añadiendo `let cloudEnv = CloudEnv()` a la siguiente sección de código:
  ```swift
  private var mealStore: [String: Meal] = [:]
  ```
  {: codeblock}

3. Añada un manejador para las solicitudes `GET` en `/meals` en el archivo `Sources/Application/Application.swift` añadiendo el código siguiente en la función `postInit()`:  
  ```swift
  router.get("/meals", handler: loadHandler)
  ```
  {: codeblock}

4. Implemente la función loadHandler en el archivo `Sources/Application/Application.swift` añadiendo el código siguiente como otra función en la clase `App`:
  ```swift
  func loadHandler(completion: ([Meal]?, RequestError?) -> Void ) {
      let meals: [Meal] = self.mealStore.map({ $0.value })
    completion(meals, nil)
  }
  ```
  {: codeblock"}

Ahora tiene una API REST para las solicitudes `GET` en `/meals` que responde con una matriz de `Meals` o un error.

5. Ejecute el proyecto.  
  Pulse **Ejecutar** o utilice el atajo de teclado `⌘+R`. Si se le solicita, seleccione **Permitir conexiones de red entrantes**.

6. Pruebe la API REST utilizando una solicitud `GET`, que coincide con la forma en que los navegadores web solicitan datos desde un servidor. 

  Puede probar la API REST utilizando el siguiente URL:  
  ```swift
  * `GET /meals`:	[http://localhost:8080/meals]()
  ```
  {: codeblock}

  Se devuelve una matriz vacía `[]` porque actualmente no hay ningún objeto `Meal` almacenado en el `mealStore`. 

Puede utilizar la guía de aprendizaje del [Programa de fondo FoodTracker](https://github.com/IBM/FoodTrackerBackend){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"), que le ayuda a crear un conjunto de API REST para almacenar, captar y suprimir objetos `Meal` desde una aplicación iOS, incluido el almacenamiento de los datos en una base de datos.
{: tip}

## Paso 5. Instalación de KituraKit en la aplicación de iOS
{: #install-kiturakit}

Las API REST creadas utilizando el servidor de Kitura son API web estándares, que se pueden utilizar desde cualquier aplicación independientemente de la biblioteca de cliente que se utiliza o de en qué lenguaje está escrito el cliente. Eso significa que puede utilizar Alamofire, RestKit o URLSession para realizar conexiones con el servidor. Kitura también proporciona un conector de cliente optimizado y a medida para simplificar la llamada a sus API REST desde iOS, en forma de KituraKit. 

KituraKit proporciona una imagen duplicada de las API de manejador de direccionador utilizadas en Kitura, lo que hace posible compartir los tipos Swift entre el cliente y el servidor con poco esfuerzo. Esta característica elimina la necesidad de llevar a cabo explícitamente la codificación JSON y la descodificación de los datos que se envían o se reciben desde el servidor.

En los pasos siguientes se muestra cómo instalar KituraKit en la aplicación de iOS y utilizarlo para llamar a la API REST `GET /meals` creada utilizando Kitura:

1. Cree un Podfile en la raíz del directorio de aplicación iOS si aún no tiene uno:
  ```
  pod init
  ```
  {: codeblock}

2. Edite el Podfile para establecer una plataforma global de iOS 11 para el proyecto sustituyendo la línea siguiente:
  ```pod
  # platform :ios, '9.0'
  ```
  {: codeblock}

  Con el código siguiente:
	```pod
  platform :ios, '11.0'
  ```
  {: codeblock}

3. Añada KituraKit a Podfile añadiendo lo siguiente en `# Pods for <application name>`:
  ```pod
  pod 'KituraKit', :git => 'https://github.com/IBM-Swift/KituraKit.git', :branch => 'pod'
  ```
  {: codeblock}

4. Guarde el Podfile e instale KituraKit utilizando el mandato siguiente:
  ```
  pod install
  ```
  {: codeblock}

5. Vuelva a abrir la aplicación en Xcode abriendo el espacio de trabajo (no el proyecto). Ahora puede añadir la misma definición `Meal` a la aplicación de iOS que se utiliza en el servidor de Kitura:
  ```swift
  struct Meal: Codable {    
      var name: String
      var photo: Data
      var rating: Int
  }
  ```
  {: codeblock}

  Y utilice el código siguiente para realizar una conexión al servidor de Kitura:
  
  ```swift
  import KituraKit
  
  /* Conectar con el servidor de Kitura en http://localhost:8080 */
  guard let client = KituraKit(baseURL: "http://localhost:8080") else {
      print("Error creating KituraKit client")
      return
  }
  
  /* Realizar una solicitud a `GET /meals`, esperando [Meal] o RequestError */
  client.get("/meals") { (meals: [Meal]?, error: RequestError?) in
      /* Comprobar errores */
      guard error == nil else {
          print("Error saving meal to Kitura: \(error!)")
          return
      }
      /* Comprobar comidas (meals) */
      guard let meals = meals else {
          print("Received Meals!")
          return
      }
  }
  ```
  {: codeblock}

El servidor de Kitura se llama utilizando `client.get("/meals")` con una devolución de llamada que coincide con los parámetros del manejador de terminación de Kitura.

Puede utilizar la guía de aprendizaje [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"), que le ayuda a crear un conjunto de API REST para almacenar, captar y suprimir objetos `Meal` desde una aplicación iOS, incluido el almacenamiento de los datos en una base de datos.
{: tip}

## Pasos siguientes
{: #next-kitura notoc}

Ahora que tiene un servidor de Kitura que proporciona una API REST a la que puede llamar la aplicación de iOS, está preparado para desplegar el servidor en {{site.data.keyword.cloud_notm}}. [Deployments](/docs/swift?topic=swift-deploy_apps-swift#deploy_apps-swift) se puede realizar utilizando envases con Kubernetes, contenedores seguros, Cloud Foundry, Cloud Foundry Enterprise Environment o instancias virtuales.
