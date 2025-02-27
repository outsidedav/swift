---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-06"

keywords: cloudant swift, store data swift, dbaas swift, cloudant instance swift, initialize sdk swift, create document swift, read document swift, delete document swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Almacenamiento de documentos en {{site.data.keyword.cloud_notm}}
{: #cloudant}

{{site.data.keyword.cloudantfull}} es una DBaaS (DataBase as a Service) orientada a documentos. Almacena los datos como documentos en formato JSON. Para su creación, se tienen en cuenta la escalabilidad, la alta disponibilidad y la durabilidad, y es fácil configurarlos para utilizarlos en aplicaciones Swift. Incluye una amplia gama de opciones de indexación, como MapReduce,
{{site.data.keyword.cloudant_short_notm}} Query,
indexación de texto completo e indexación geoespacial. Las capacidades de réplica permiten mantener fácilmente los datos sincronizados entre los clústeres de bases de datos, los equipos de sobremesa y los dispositivos móviles. 
{: shortdesc}

Para ver todas las formas en las que puede utilizar {{site.data.keyword.cloudant_short_notm}}, consulte [Aspectos básicos de {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/basics?topic=cloudant-ibm-cloudant-basics#cloudant-nosql-db-basics).

## Antes de empezar
{: #prereqs-cloudant}

Primero, asegúrese de cumplir los siguientes requisitos previos:
 * CocoaPods (versión 1.1.0 o posterior)
 * iOS (versión 9 o posterior)
 * MacOS (versión 10.11.5 o posterior)
 * Xcode (versión 9.0.1 o posterior)

El [{{site.data.keyword.cloudant_short_notm}} SDK for Swift](https://github.com/cloudant/swift-cloudant){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") se crea con Swift 3.2. Si está pensando en utilizar {{site.data.keyword.cloudant_short_notm}} con Kitura, consulte [Kitura-CouchDB Library](https://github.com/IBM-Swift/Kitura-CouchDB){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"), que se crea con Swift 4.0.
{: tip}

## Paso 1. Creación de una instancia de {{site.data.keyword.cloudant_short_notm}}
{: #create-instance-cloudant}

Consulte la
[Guía de aprendizaje para la creación de una instancia de IBM Cloudant en {{site.data.keyword.cloud_notm}}](/docs/services/Cloudant/tutorials?topic=cloudant-creating-an-ibm-cloudant-instance-on-ibm-cloud#creating-an-ibm-cloudant-instance-on-ibm-cloud){:new_window} para crear una instancia del servicio.

## Paso 2. Instalación del SDK
{: #install-sdk-cloudant}

### Instalación del SDK de Swift de iOS
{: #install-swift-sdk-cloudant}

Utilice el SDK de Cloudant de Swift para facilitar la programación de la app. El SDK debe estar instalado en el código de la app.

1. Abra el directorio de proyecto Xcode existente en el archivo `Podfile`.
2. En el destino de proyectos, añada una dependencia para el pod `SwiftCloudant`. Asegúrese de que el mandato `use_frameworks!` también esté bajo su destino, tal como se muestra en el siguiente ejemplo.
    ```
    target '<yourTarget>' do
      use_frameworks!
        pod 'SwiftCloudant', :git => 'https://github.com/cloudant/swift-cloudant.git'
    end
    ```
    {: screen}

3. Descargue la dependencia de `SwiftCloudant`.
    ```
    pod install
    ```
    {: codeblock}

### Instalación del SDK de Swift de lado del servidor
{: #install-serverside-cloudant}

Para utilizarlo con el Gestor de paquetes de Swift para el desarrollo de ladro del servidor, añada la línea siguiente a las dependencias de `Package.swift`:
```swift
.Package(url: "https://github.com/cloudant/swift-cloudant.git")
```
{: codeblock}

## Paso 3. Inicialización del SDK
{: #initialize-cloudant}

Después de inicializar el SDK en la app, puede empezar por utilizar {{site.data.keyword.cloudant_short_notm}} para almacenar datos.

1.  Añada la importación siguiente al archivo `AppDelegate.swift` o al archivo swift de lado del servidor.
    ```swift
    import SwiftCloudant
    ```
    {: codeblock}

2. Inicialice la conexión a la base de datos.
    ```swift
    let cloudantURL = NSURL(string: "https://username.cloudant.com")!
    let client = CouchDBClient(url: cloudantURL, username: "username", password: "password")
    let dbName = "database"
    ```
    {: codeblock}

### Operaciones básicas
{: #basic-operations-cloudant}

Estas operaciones básicas muestran las acciones fundamentales para crear, leer y suprimir sus documentos utilizando el cliente inicializado.

#### Crear un documento
```swift
let create = PutDocumentOperation(id: "doc1", body: ["hello": "world"], databaseName: dbName) {(response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while creating a document. Error: \(error)")
    } else {
        print("Created document \(response?["id"]) with revision id \(response?["rev"])")
    }
}
client.add(operation: create)
```
{: codeblock}

#### Leer un documento
```swift
let read = GetDocumentOperation(id: "doc1", databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while reading a document. Error: \(error)")
    } else {
        print("Read document: \(response)")
    }
}
client.add(operation: read)
```
{: codeblock}

#### Suprimir un documento
```swift
let delete = DeleteDocumentOperation(id: "doc1",
    revision: "1-revisionidhere",
    databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while deleting a document. Error: \(error)")
    } else {
        print("Document deleted")
    }
}
client.add(operation: delete)
```
{: codeblock}

## Paso 4. Comprobación de la app
{: #cloudant_testing}

¿Todo se ha configurado correctamente? Pruébelo.

1. Ejecute la aplicación, asegurándose de empezar la inicialización y las operaciones respectivas, como por ejemplo la creación de un documento.
2. Vuelva a la instancia de servicio de {{site.data.keyword.cloudant_short_notm}} creada anteriormente en el navegador web y abra el panel de control del servicio.
3. Seleccione la base de datos que se utiliza, y podrá ver los documentos en el panel de control.

¿Tiene problemas? Consulte la [Referencia de la API de {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/api?topic=cloudant-ibm-cloudant-basics#api-reference-overview).

## Pasos siguientes
{: #cloudant_next notoc}

¡Buen trabajo! Ha añadido un nivel de persistencia segura a la app. Mantenga el ritmo probando una de las opciones siguientes:

* Visualice el código fuente de [{{site.data.keyword.cloudant_short_notm}} SDK for Swift](https://github.com/cloudant/swift-cloudant){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").
* Los kits de inicio son una de las formas más rápidas de utilizar las prestaciones de {{site.data.keyword.cloud_notm}}. El kit de inicio **Infinite Scrolling with Cloudant NoSQL for iOS** muestra cómo ampliar un `ViewController` para visualizar los datos utilizando la paginación. Este patrón de app es común para los desarrolladores de iOS, y es un buen ejemplo para ilustrar las prestaciones de {{site.data.keyword.cloudant_short_notm}}. Vea los kits de inicio disponibles en el [panel de control de desarrollador de Mobile](https://{DomainName}/developer/mobile/dashboard){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"). Descargue el código. Ejecute la app.
* Obtenga más información y aproveche todas las características que {{site.data.keyword.cloudant_short_notm}} ofrece: [consulte la documentación](/docs/services/Cloudant?topic=cloudant-ibm-cloudant-basics).
