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

# Adición de API a apps de iOS
{: #api_connect}

Puede utilizar API Connect para gestionar las API en {{site.data.keyword.cloud}}, tanto si se mantienen dentro o fuera de {{site.data.keyword.cloud_notm}}. Aprenda a gestionar las API para que pueda controlar el uso, aumentar la adopción y realizar un seguimiento de las estadísticas.

## Creación de una instancia de API Connect
{: #create-apiconnect}

Vaya al [Catálogo](https://cloud.ibm.com/catalog/) y cree una instancia de API Connect para gestionar las API.

Utilice el `Menú->API` para acceder a la consola de API Connect Management.

![API Connect](images/apiconnect.png)

Si está definiendo su propio contrato de API antes de iniciar el desarrollo de fondo y frontal, utilice las herramientas de API Connect para acelerar este proceso. Puede trabajar con su equipo de desarrollo digital para crear y definir un contrato de API entre la app de iOS y la lógica del programa de fondo. Esta lógica se puede entregar utilizando [{{site.data.keyword.openwhisk}}](/docs/openwhisk/index.html) o a través del [tiempo de ejecución de Swift](/docs/runtimes/swift/index.html) con Kubernetes o [Cloud Foundry](/docs/cloud-foundry/index.html).

Una vez definida la API, puede definir Open API Specifications (Swagger) en varias herramientas distintas:

- [Editor de Swagger](http://editor.swagger.io/)
- [API Designer](https://www.ibm.com/support/knowledgecenter/en/SSFS6T/com.ibm.apic.toolkit.doc/task_apionprem_composing_apis.html)
- [Bucle de retorno](https://loopback.io/)

## Definición de la API gestionada
{: #define-apiconnect}

Puede definir un proxy de API que gestiona la pasarela de API entre la aplicación cliente y la lógica del programa de fondo. Utilice los pasos siguientes para crear un proxy utilizando YAML o JSON de la Open API Specification (documento de Swagger). 

1. Abra la consola `Menú -> API` y pulse el proxy de API.
2. Pulse **Definición de API Importar YAML o JSON**.
3. Seleccione el archivo YAML o JSON que ha creado anteriormente.
4. Guarde y exponga.

Debe configurar el punto final externo para que apunte al URL que enlaza a la aplicación de lógica de programa de fondo. 

## Creación de un programa de fondo de Swift
{: #create-backend-apiconnect}

Puede crear la app Swift de programa de fondo en función de esta API. 

Desde la Apple Development Console, realice los pasos siguientes:

1. Seleccione **Kits de inicio**.
2. Pulse **Crear app**.
3. Seleccione **Swift** como lenguaje.

Seleccione el archivo YAML y el archivo JSON y, a continuación, pulse **Crear**. Se ha creado la app Swift de fondo.

A continuación, puede **descargar** el código o **Desplegar en la nube**, y clonar el repositorio de GIT en la máquina local. Puede seguir las instrucciones de la Guía de conocimientos para abrir la app del lado del servidor en Xcode.

En la carpeta **Origen**, puede ver una ruta que define el archivo Swift que ha creado los puntos finales REST que se correlacionan con la API. 

Consulte el ejemplo siguiente que utiliza la API `PetStore` Open:
```swift
import Kitura
import KituraContracts

func initializePet_Routes(app: App) {
    app.router.post("\(basePath)/pet") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.put("\(basePath)/pet") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.get("\(basePath)/pet/findByStatus") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.get("\(basePath)/pet/findByTags") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.get("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.post("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.delete("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.post("\(basePath)/pet/:petId/uploadImage") { request, response, next in
        response.send(json: [:])
        next()
    }
}
```
{: codeblock}

Una vez que se ha definido la API mediante {{site.data.keyword.openwhisk_short}} o mediante un tiempo de ejecución completo de Swift de pila, y se ha creado la definición de API Connect, puede consumir la API en las apps de iOS.

## Consumo de la API en una app para móvil de app de iOS
{: #consume-apiconnect}

Para consumir la API de programa de fondo en la app de iOS, cree un kit de inicio de Mobile utilizando la consola de Apple. Si utiliza la vista Kit de inicio, cree un kit de inicio de iOS Swift de cualquier tipo.

Pulse **Añadir recurso** y seleccione una API. 

![Diálogo de API](../images/apidialog.png)

La API se añade a la app de iOS. Si *Descarga* el código para la app, podrá ver una carpeta incluida en las carpetas de origen de iOS que tiene el nombre de la API.

Siga los pasos de la Guía de conocimientos para `pod update` cualquier SDK dependiente en la app de iOS. 

La app de iOS incluye una carpeta con el enlace de SDK generado para la API. Esta carpeta incluye las tres siguientes subcarpetas `Activos`, `Origen` y `Docs`. 

![Carpeta de iOS](../images/sdkfolder.png)

En la carpeta `Activos` hay un archivo que gestiona el URL en la API, que de forma predeterminada es `localhost:3000`. Debe cambiar el valor para hacer referencia a la ruta de la API. La definición de la API consta de una sección Nombre de API y de una sección Ruta. Pulse **Copiar** al final de la ruta para copiar el URL. Compruebe que la opción *Exponer API gestionada* esté activada para que los clientes externos realicen llamadas de API.

![Ruta de API](../images/apiroute.png)  

Abra el archivo `PLIST` y sustituya el valor de host por el valor que se copia desde la ruta de la API que permite que el SDK llame a la API en el {{site.data.keyword.cloud_notm}}.

## Documentación
{: #docs-apiconnect}

Cuando el SDK se incluye en el proyecto de app de iOS, hay disponible un archivo *README.html* en la carpeta `Docs`. Abra la carpeta `Docs` en un navegador externo y lea las instrucciones sobre cómo utilizar el proyecto.

## Recreación del SDK después de un cambio de API
{: #change-apiconnect}

Si los cambios de API o las nuevas características pasan a estar disponibles, y se añade {{site.data.keyword.openwhisk}}, puede volver a crear el SDK del cliente utilizando el mandato `ibmcloud sdk`. Para obtener más información, ejemplos y ayuda de sintaxis, consulte la documentación de [Generador de SDK](/docs/cli/sdk/index.html).

Para habilitar la creación de un SDK, utilice el archivo YAML o JSON de Open API Specification (Swagger). Puede recuperar este archivo utilizando los recursos de gestión de API en el {{site.data.keyword.cloud_notm}}. 

1. Vaya a `Menú -> API -> API gestionadas`.
2. Seleccione la API desde la que desea recuperar la última Open API Specification. 
3. A continuación, seleccione el menú **Explorador**.

![Explorador de API](../images/downloadyaml.png)

4. Seleccione el icono de descarga para descargar el yaml para la API y guardar este archivo en el directorio de proyecto de app de iOS.

5. El paso siguiente consiste en ejecutar el mandato de CLI `ibmcloud sdk`. 
    ```
    ibmcloud sdk generate --ios --unzip --output ./MyAppFunctions -f ./mobile-bff-functions-1.0.0.yaml SDKMyFunctions
    ```
    {: codeblock}

    El SDK se vuelve a crear en el directorio de proyecto de app de iOS para que pueda seguir trabajando con la API.

## Referencia
{: #reference-apiconnect}

El SDK de ejemplo siguiente se crea para {{site.data.keyword.openwhisk_short}} desde el kit de inicio. Puede ver cada una de las Acciones y los fragmentos de código de Swift que puede incluir en la app de iOS.

### Métodos de API predeterminados
{: #default-methods-apiconnect}

 * [`getCreate`](#getCreate)
 * [`getDelete`](#getDelete)
 * [`getDeleteall`](#getDeleteall)
 * [`getRead`](#getRead)
 * [`getReadall`](#getReadall)
 * [`getUpdate`](#getUpdate)

### Uso de `getCreate`
{: #getcreate-apiconnect}

{: #getCreate}

```swift
public static func getCreate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parámetros para `getCreate`

- **completionHandler** (necesario)
    - El cierre toma como argumentos `Response?` y `Error?`.

### Autenticación con `getCreate`
{: #auth-getcreate}

No se necesita autenticación

### Ejemplo que utiliza `getCreate`
{: #example-getcreate}

```swift
DefaultAPI.getCreate() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### Uso de `getDelete`
{: #getdelete}

```swift
public static func getDelete(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parámetros para `getDelete`

- **completionHandler** (necesario)
    - El cierre toma como argumentos `Response?` y `Error?`.

### Autenticación con `getDelete`
{: #auth-getdelete}

No se necesita autenticación

### Ejemplo que utiliza `getDelete`
{: #example-getdelete}

```swift
DefaultAPI.getDelete() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### Uso de `getDeleteall`
{: #getdeleteall}

```swift
public static func getDeleteall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parámetros para `getDeleteall`

- **completionHandler** (necesario)
    - El cierre toma como argumentos `Response?` y `Error?`.

### Autenticación con `getDeleteall`
{: #auth-getdeleteall}

No se necesita autenticación

### Ejemplo que utiliza `getDeleteall`
{: #example-getdeleteall}

```swift
DefaultAPI.getDeleteall() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### Uso de `getRead`
{: #getread}

```swift
public static func getRead(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parámetros para `getRead`

- **completionHandler** (necesario)
    - El cierre toma como argumentos `Response?` y `Error?`.

### Autenticación con `getRead`
{: #auth-getread}

No se necesita autenticación

### Ejemplo que utiliza `getRead`
{: #example-getread}

```swift
DefaultAPI.getRead() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### Uso de `getReadall`
{: #getreadall}

```swift
public static func getReadall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parámetros para `getReadall`

- **completionHandler** (necesario)
    - El cierre toma como argumentos `Response?` y `Error?`.

### Autenticación con `getReadall`
{: #auth-getreadall}

No se necesita autenticación

### Ejemplo que utiliza `getReadall`
{: #example-getreadall}

```swift
DefaultAPI.getReadall() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### Uso de `getUpdate`
{: #getupdate}

```swift
public static func getUpdate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parámetros para `getUpdate`

- **completionHandler** (necesario)
    - El cierre toma como argumentos `Response?` y `Error?`.

### Autenticación con `getUpdate`
{: #auth-getupdate}

No se necesita autenticación

### Ejemplo que utiliza `getUpdate`
{: #example-getupdate}

```swift
DefaultAPI.getUpdate() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

