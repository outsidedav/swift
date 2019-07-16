---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: swift-cfenv, service bindings swift, environment swift, swift configuration, cloudenvironment swift, VCAP_SERVICES swift, swift credentials

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Configuración del entorno Swift
{: #configuration}

El desarrollo nativo en la nube tiene dos prácticas estrechamente relacionadas que se cruzan en la forma en que manejan los datos de configuración. La primera es que debe crear artefactos inmutables para minimizar la probabilidad de que se introduzcan errores a medida que la aplicación pase de desarrollo a producción. La segunda es que debe esforzarse por la paridad entre sus entornos de desarrollo y de producción para evitar problemas creados por el código específico del entorno. 

La gestión de la configuración de servicio y las credenciales (enlaces de servicio) varía entre plataformas. Cloud Foundry almacena detalles de enlace de servicio en un objeto JSON en serie que se pasa a la aplicación como una variable de entorno `VCAP_SERVICES`. Kubernetes almacena los enlaces de servicio como atributos JSON o sin formato en serie en `ConfigMaps` o `Secrets`, que se pueden pasar a la aplicación contenerizada como variables de entorno o montarse como un volumen temporal. El desarrollo local tiene su propia configuración, ya que las pruebas locales a menudo son un derivado simplificado de lo que se ejecuta en la nube. Trabajar con estas variaciones de una forma portátil sin tener vías de acceso de código específicas del entorno puede ser un reto.

Puede seguir directrices sencillas para ayudarle a escribir aplicaciones portátiles, y programas de utilidad que puede utilizar para encapsular los enlaces de servicio (u otra configuración) en ubicaciones específicas del entorno. Tanto si necesita añadir soporte de nube a las aplicaciones existentes como si tiene que crear apps con los Kits de inicio, el objetivo es proporcionar portabilidad para que las apps Swift se puedan utilizar en varias plataformas de despliegue.

## Adición de {{site.data.keyword.cloud_notm}} a aplicaciones Swift existentes
{: #addcloud-env}

La vía de acceso para obtener los valores de entorno puede diferir de un entorno de nube a otro. La biblioteca [CloudEnvironment](https://github.com/IBM-Swift/CloudEnvironment){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") abstrae la configuración del entorno y las credenciales de varios proveedores de nube para que la app Swift pueda acceder a la información de forma coherente ejecutándose localmente o en Cloud Foundry, Cloud Foundry Enterprise Environment, Kubernetes, {{site.data.keyword.openwhisk}} o instancias virtuales. La abstracción de las credenciales se proporciona mediante la biblioteca `CloudEnvironment`, que utiliza internamente
[Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") para la configuración de Cloud Foundry y [Configuration](https://github.com/IBM-Swift/Configuration){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") como gestor de configuraciones.

Con `CloudEnvironment`, puede abstraer detalles de nivel bajo del código de origen de la aplicación definiendo una clave de búsqueda que la aplicación Swift puede utilizar para buscar su valor correspondiente.

La biblioteca `CloudEnvironment` proporciona una clave de búsqueda coherente que se puede utilizar en el código fuente. A continuación, la biblioteca busca en una matriz de patrones de búsqueda para encontrar un objeto JSON con los atributos de configuración o las credenciales de servicio. 

### Adición del paquete de CloudEnvironment a la aplicación Swift
{: #add-cloudenv}

Para utilizar el paquete `CloudEnvironment` en la aplicación Swift, especifíquelo en la sección **dependencies:** del archivo `Package.swift`:
```swift
.package(url: "https://github.com/IBM-Swift/CloudEnvironment.git", from: "8.0.0"),
```
{: codeblock}

A continuación, añada el código de instrumentación siguiente a la aplicación:
```swift
import CloudEnvironment

let cloudEnv = CloudEnv()
```
{: codeblock}

### Acceso a credenciales
{: #access-credentials}

Ahora que se inicializa la biblioteca `CloudEnvironment`, puede acceder a las credenciales tal como se muestra en los ejemplos siguientes:
```swift
let cloudantCredentials = cloudEnv.getCloudantCredentials(name: "cloudant-credentials")
// cloudantCredentials.username, cloudantCredentials.password, cloudantCredentials.url, etc.
let objStorageCredentials = cloudEnv.getObjectStorageCredentials(name: "object-storage-credentials")
// objStorageCredentials.username, objStorageCredentials.password, objStorageCredentials.projectID, etc.

let service1Credentials = cloudEnv.getDictionary("service1-credentials")
let service1CredentialsStr = cloudEnv.getString("service1-credentials")

// Obtener un PUERTO y URL predeterminados
let port = cloudEnv.port
let url = cloudEnv.url
```
{: codeblock}

En este ejemplo se proporciona acceso a los conjuntos de credenciales para servicios, que ahora se pueden utilizar para inicializar conexiones con estos [servicios soportados o un ejemplo genérico](https://github.com/IBM-Swift/CloudEnvironment#supported-services){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"). Consulte
[Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") para ver la configuración específica de Cloud Foundry, y los [detalles de configuración](https://github.com/IBM-Swift/Configuration){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") sobre la carga de datos de configuración.

## Comprensión de las credenciales de servicio
{: #service_creds}

La biblioteca `CloudEnvironment` utiliza un archivo denominado `mappings.json`, que se encuentra en el directorio `config`, para comunicar dónde se almacenan las credenciales para cada servicio. El archivo `mappings.json` da soporte a la búsqueda de valores que utilizan los tres tipos de patrón de búsqueda siguientes:
- **`cloudfoundry`**: Un tipo de patrón que se utiliza para buscar un valor en la variable de entorno de servicios de Cloud Foundry (`VCAP_SERVICES`). Para Cloud Foundry Enrprise Edition, consulte esta [Guía de aprendizaje de iniciación](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started) para obtener más información.
- **`env`**: Un tipo de patrón utilizado para buscar un valor que se correlaciona con una variable de entorno, como en Kubernetes o Functions.
- **`file`**: Un tipo de patrón que se utiliza para buscar un valor en un archivo JSON. La vía de acceso debe ser relativa a la carpeta raíz de la aplicación Swift.

La matriz de valores que están asociados a una clave de búsqueda se busca en el orden en el que aparecen.

A continuación se muestra un ejemplo de un archivo `mappings.json`:
```javascript
{
    "cloudant-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-cloudant-db",
                "env:my_awesome_cloudant_db_credentials",
                "file:localdev/my-awesome-cloudant-db-credentials.json"
            ]
        }
    },
    "object-storage-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-object-storage",
                "env:my_awesome_object_storage_credentials",
                "file:localdev/my-awesome-object-storage-credentials.json"
            ]
        }
    }
}
```
{: codeblock}

En este ejemplo, `cloudant-credentials` y `object-storage-credentials` son las claves de búsqueda que utiliza la aplicación Swift para buscar las credenciales correspondientes. Según la matriz de patrones de búsqueda, el gestor de configuración busca en primer lugar `my-awesome-cloudant-db` en `VCAP_SERVICES`, a continuación busca `my_awesome_cloudant_db_credentials` como una variable de entorno. Si esto no está definida, busca el contenido de `my-awesome-object-storage-credentials.json`
en la carpeta `localdev`. 

Cuando la aplicación se ejecuta localmente, puede utilizar las credenciales almacenadas en un archivo, como `localdev/my-awesome-object-storage-credentials.json`, tal como se muestra en el ejemplo anterior. La información de conexión para los servicios a los que desea acceder localmente, como por ejemplo nombre de usuario, contraseña y nombre de host, se debe almacenar en este archivo. 

Por motivos de seguridad, los archivos de credenciales no residen en repositorios. En el ejemplo anterior, se utiliza una carpeta `localdev` para almacenar las credenciales locales, de modo que debe añadir esta carpeta al archivo `.gitignore` para evitar una confirmación accidental. Si utiliza una app de Kit de inicio, esta carpeta se crea para usted y está presente en el archivo `.gitignore`.

Para obtener más información sobre el archivo `mappings.json`, consulte la sección [Comprensión de la credencial de servicio](#service_creds).

## Utilización del gestor de configuración de Swift desde apps del kit de inicio
{: #configmanager-swift}

Las apps Swift que se crean con [kits de inicio](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") automáticamente incluyen las credenciales y configuración necesarias para su ejecución en local, así como en muchos destinos de despliegue en la nube, como
[Kubernetes](/docs/containers?topic=containers-getting-started), [Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf), [{{site.data.keyword.cfee_full_notm}}](/docs/cloud-foundry?topic=cloud-foundry-about), [un servidor virtual (VSI)](/docs/vsi?topic=virtual-servers-getting-started-tutorial) o
[{{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-getting_started).

  El despliegue de VSI está disponible para algunos kits de inicio. Para utilizar esta característica, vaya al
[panel de control de {{site.data.keyword.cloud_notm}}](https://{DomainName}) y pulse **Crear una app** en el mosaico **Apps**.
  {: note}

La creación básica del gestor de configuración se puede encontrar en `Sources/Application/Application.swift`. Cuando crea una app de Kit de inicio basada en Swift con servicios, se crea automáticamente una carpeta `config` y un archivo `mappings.json`. Si descarga la app, la carpeta `config` incluye un archivo `localdev-config.json` que tiene todas las credenciales para los servicios y está presente en el archivo `.gitignore`.

## Pasos siguientes
{: #next-configß notoc}

Consulte nuestras tres bibliotecas para que sus aplicaciones se abstraigan de sus entornos:

* [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")
* [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")
* [Configuración](https://github.com/IBM-Swift/Configuration){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")
