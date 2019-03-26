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

# iOS-Apps mit APIs ausstatten
{: #api_connect}

Mit API Connect können Sie APIs in
{{site.data.keyword.cloud}} verwalten. Hierbei ist es ohne Belang, ob
die Verwaltung innerhalb oder außerhalb von
{{site.data.keyword.cloud_notm}} erfolgt. Hier erfahren Sie, wie Sie
Ihre APIs verwalten, damit Sie die Verwendung steuern, die Akzeptanz erhöhen
und Statistikdaten überwachen können.

## Instanz von API Connect erstellen
{: #create-apiconnect}

Rufen Sie den [Katalog](https://cloud.ibm.com/catalog/) auf und erstellen Sie eine Instanz von API
Connect, um Ihre APIs zu verwalten.

Greifen Sie durch Auswahl der Optionen `Menü > APIs`
auf die Managementkonsole von API Connect zu.

![API Connect](images/apiconnect.png)

Wenn Sie Ihren eigenen API-Vertrag definieren, bevor Sie mit der Back-End-und Front-End-Entwicklung beginnen, verwenden Sie die API Connect-Tools, um diesen Prozess zu beschleunigen. Zusammen mit Ihrem Team für die digitale
Entwicklung können Sie den API-Vertrag zwischen Ihrer iOS-App und Ihrer
Back-End-Logik aufstellen und definieren. Diese Logik kann mithilfe von
[{{site.data.keyword.openwhisk}}](/docs/openwhisk/index.html) oder über die
[Swift-Laufzeitumgebung](/docs/runtimes/swift/index.html)
mit Kubernetes bzw. [Cloud Foundry](/docs/cloud-foundry/index.html) bereitgestellt werden.

Nachdem Sie Ihre API definiert haben, können Sie eine Reihe von Tools
verwenden, um die Open API-Spezifikationen
(Swagger) festzulegen:

- [Swagger-Editor](http://editor.swagger.io/)
- [API Designer](https://www.ibm.com/support/knowledgecenter/en/SSFS6T/com.ibm.apic.toolkit.doc/task_apionprem_composing_apis.html)
- [Loopback](https://loopback.io/)

## Verwaltete API definieren
{: #define-apiconnect}

Sie können einen API-Proxy definieren, der das API-Gateway zwischen Ihrer
Clientanwendung und Ihrer Back-End-Logik verwaltet. Führen Sie die folgenden
Schritte aus, um einen Proxy unter Verwendung Ihrer Open API-Spezifikation
(Swagger-Dokument) im YAML- oder JSON-Format zu erstellen. 

1. Öffnen Sie die Konsole durch Auswahl von `Menü > APIs` und klicken Sie auf den API-Proxy.
2. Klicken Sie auf **YAML- oder JSON-Datei mit API-Definition importieren**.
3. Wählen Sie die zuvor erstellte YAML- oder JSON-Datei aus.
4. Speichern Sie die Datei und machen Sie sie zugänglich.

Sie müssen den externen Endpunkt so konfigurieren, dass er auf die URL
verweist, die Links zu Ihrer Back-End-Logikanwendung enthält. 

## Swift-Back-End erstellen
{: #create-backend-apiconnect}

Sie können Ihre Swift-Back-End-App auf der Basis dieser API erstellen. 

Führen Sie in der Entwicklerkonsole für Apple die folgenden
Schritte
aus:

1. Wählen Sie **Starter-Kits** aus.
2. Klicken Sie auf **App erstellen**.
3. Wählen Sie **Swift** als Sprache aus.

Wählen Sie die YAML- bzw. JSON-Datei aus und klicken Sie dann auf
**Erstellen**. Die Swift-Back-End-App wird jetzt erstellt.

Nun können Sie den Code **herunterladen** oder
**in der Cloud bereitstellen** und Ihr GIT-Repository auf
der lokalen Maschine klonen. Anhand der Anweisungen im Knowledge
Guide können Sie die serverseitige App in Xcode öffnen.

Im **Quellenordner** sehen Sie eine Route, mit der
die Swift-Datei definiert wird, von der die REST-Endpunkte erstellt wurden,
die der API zugeordnet sind. 

Im folgenden Beispiel wird die Open API von `PetStore` verwendet:
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

Nachdem die API mithilfe von
{{site.data.keyword.openwhisk_short}} oder einer vollständigen
Swift-Stack-Laufzeit definiert und die API Connect-Definition erstellt wurde,
können Sie die API in Ihren iOS-Apps einsetzen.

## API in einer mobilen iOS-App einsetzen
{: #consume-apiconnect}

Um die API in Ihrer iOS-App einzusetzen, erstellen Sie unter Verwendung
der Konsole für Apple ein Starter-Kit für Mobilgeräte. Erstellen Sie in der
Ansicht des Starter-Kits ein Swift-Starter-Kit für iOS mit einem beliebigen Typ.

Klicken Sie auf **Ressource hinzufügen** und wählen Sie eine API aus. 

![API-Dialog](../images/apidialog.png)

Die API wird zu Ihrer iOS-App hinzugefügt. Falls Sie
den Code für die App *herunterladen*, sehen Sie unter den
iOS-Quellenordnern einen Ordner, der nach der API benannt ist.

Befolgen Sie die Schritte im Knowledge Guide, um für alle etwaigen
abhängige SDKs in Ihrer iOS-App ein `Pod-Update` durchzuführen. 

Die iOS-App enthält einen Ordner mit der Bindung des generierten
SDK für die API. Dieser Ordner enthält die drei Unterordner
`Assets`, `Source` und
`Docs`. 

![iOS-Ordner](../images/sdkfolder.png)

Im Ordner `Assets` ist eine Datei enthalten, die die URL
zu Ihrer API verwaltet; standardmäßig hat sie den Wert `localhost:3000`. Sie müssen den Wert so ändern, dass er auf die API-Route verweist. Die
API-Definition enthält einen Abschnitt "API Name and Route". Klicken Sie am Ende der Route auf **Kopieren**, um die URL zu kopieren. Vergewissern Sie sich, dass die Option *Verwaltete API
zugänglich machen* aktiviert ist, damit externe Clients API-Aufrufe
vornehmen können.

![API-Route](../images/apiroute.png)  

Öffnen Sie die Datei `PLIST` und ersetzen Sie den
Hostwert durch den aus der API-Route kopierten Wert, der es dem SDK ermöglicht,
die API in der {{site.data.keyword.cloud_notm}} aufzurufen.

## Dokumentation
{: #docs-apiconnect}

Wenn das SDK in Ihr Projekt für die iOS-App einbezogen worden ist, ist im
Ordner `Docs` die Datei *README.html* verfügbar. Öffnen Sie den Ordner `Docs` in einem externen Browser und lesen Sie
die Anweisungen zur Verwendung des Projekts.

## SDK nach API-Änderung erneut erstellen
{: #change-apiconnect}

Falls sich die API ändert oder neue Funktionen verfügbar werden und
{{site.data.keyword.openwhisk}} hinzugefügt wurde, können Sie das
Client-SDK mithilfe des Befehls `ibmcloud sdk` erneut
erstellen. Weitere Informationen sowie Beispiele und Hilfe zur Syntax enthält
die Dokumentation von
[SDK Generator](/docs/cli/sdk/index.html).

Um die Erstellung eines SDK zu ermöglichen, verwenden Sie die YAML- oder
JSON-Datei mit der Open API-Spezifikation (Swagger). Diese Datei können Sie mit
den Funktionen für das API-Management in der
{{site.data.keyword.cloud_notm}} abrufen. 

1. Navigieren Sie zu `Menü > APIs > Verwaltete APIs`.
2. Wählen Sie die API aus, für die Sie die neueste Open
API-Spezifikation abrufen wollen. 
3. Wählen Sie dann das Menü **Explorer** aus.

![API-Explorer](../images/downloadyaml.png)

4. Wählen Sie das Downloadsymbol aus, um die YAML-Datei für die API
herunterzuladen, und speichern Sie diese Datei im Projektverzeichnis für die
iOS-App.

5. Als nächster Schritt wird der CLI-Befehl `ibmcloud
sdk` ausgeführt. 
    ```
    ibmcloud sdk generate --ios --unzip --output ./MyAppFunctions -f ./mobile-bff-functions-1.0.0.yaml SDKMyFunctions
    ```
    {: codeblock}

    Das SDK wird im Projektverzeichnis für Ihre iOS-App erneut erstellt
und Sie können die Arbeit mit Ihrer API fortsetzen.

## Referenzinformationen
{: #reference-apiconnect}

Das folgende Beispiel-SDK für
{{site.data.keyword.openwhisk_short}} wird aus dem Starter-Kit
erstellt. Nachfolgend sind alle Aktionen und Swift-Code-Snippets dargestellt,
die Sie in Ihre iOS-App einbeziehen können.

### API-Standardmethoden
{: #default-methods-apiconnect}

 * [`getCreate`](#getCreate)
 * [`getDelete`](#getDelete)
 * [`getDeleteall`](#getDeleteall)
 * [`getRead`](#getRead)
 * [`getReadall`](#getReadall)
 * [`getUpdate`](#getUpdate)

### `getCreate` verwenden
{: #getcreate-apiconnect}

{: #getCreate}

```swift
public static func getCreate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parameter für `getCreate`

- **completionHandler** (erforderlich)
    - Der Abschluss verwendet `Response?` und
`Error?` als Argumente.

### Authentifzierung mit `getCreate` durchführen
{: #auth-getcreate}

Keine Authentifizierung erforderlich

### Beispiel mit Verwendung von `getCreate`
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

### Methode `getDelete` verwenden
{: #getdelete}

```swift
public static func getDelete(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parameter für `getDelete`

- **completionHandler** (erforderlich)
    - Der Abschluss verwendet `Response?` und
`Error?` als Argumente.

### Authentifzierung mit `getDelete` durchführen
{: #auth-getdelete}

Keine Authentifizierung erforderlich

### Beispiel mit Verwendung von `getDelete`
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

### Methode `getDeleteall` verwenden
{: #getdeleteall}

```swift
public static func getDeleteall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parameter für `getDeleteall`

- **completionHandler** (erforderlich)
    - Der Abschluss verwendet `Response?` und
`Error?` als Argumente.

### Authentifzierung mit `getDeleteall` durchführen
{: #auth-getdeleteall}

Keine Authentifizierung erforderlich

### Beispiel mit Verwendung von `getDeleteall`
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

### Methode `getRead` verwenden
{: #getread}

```swift
public static func getRead(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parameter für `getRead`

- **completionHandler** (erforderlich)
    - Der Abschluss verwendet `Response?` und
`Error?` als Argumente.

### Authentifzierung mit `getRead` durchführen
{: #auth-getread}

Keine Authentifizierung erforderlich

### Beispiel mit Verwendung von `getRead`
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

### Methode `getReadall` verwenden
{: #getreadall}

```swift
public static func getReadall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parameter für `getReadall`

- **completionHandler** (erforderlich)
    - Der Abschluss verwendet `Response?` und
`Error?` als Argumente.

### Authentifzierung mit `getReadall` durchführen
{: #auth-getreadall}

Keine Authentifizierung erforderlich

### Beispiel mit Verwendung von `getReadall`
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

### Methode `getUpdate` verwenden
{: #getupdate}

```swift
public static func getUpdate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Parameter für `getUpdate`

- **completionHandler** (erforderlich)
    - Der Abschluss verwendet `Response?` und
`Error?` als Argumente.

### Authentifzierung mit `getUpdate` durchführen
{: #auth-getupdate}

Keine Authentifizierung erforderlich

### Beispiel mit Verwendung von `getUpdate`
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

