---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# App mit Kitura erstellen
{: #kitura}

[Kitura](http://www.kitura.io) ist ein
serverseitiges Swift-Framework für die Erstellung von iOS-Back-End-Programmen
und -Webanwendungen. Dieses Framework erstellt REST-APIs, die von der
iOS-Anwendung aus mithilfe von SDKs für URL-Sitzungen (z. B. Alamofire, RestKit
oder das von Kitura selbst bereitgestellte SDK
[KituraKit](https://github.com/ibm-swift/kiturakit))
aufgerufen werden können.

Kitura kann bei allen Services und Funktionen, die von
{{site.data.keyword.cloud}} bereitgestellt werden (inklusive
{{site.data.keyword.appid_short}},
{{site.data.keyword.mobilepushshort}} und
{{site.data.keyword.mobileanalytics_short}}) sowie in Datenbanken, das
maschinelle Lernen und andere Services integriert werden. Kitura kann dann
unter Verwendung einer der beiden Cloud Foundry- oder Docker-Plattformen
(Kubernetes-basiert) in {{site.data.keyword.cloud}} bereitgestellt und
automatisch skaliert werden.

Kitura stellt eine [Befehlszeilenschnittstelle (CLI)](https://www.kitura.io/guides/kituracli/gettingstarted.html) namens `kitura` zur Verfügung, die das Erstellen, Testen und Bereitstellen von Kitura-Anwendungen vereinfacht. Anwendungen, die
mithilfe der Kitura-CLI erstellt werden, besitzen eine vollständige
Unterstützung für die Bereitstellung in jeder beliebigen Cloud, die Cloud
Foundry-, Docker- und Kubernetes-Technologien unterstützt. Falls Sie die
Erstellung speziell für {{site.data.keyword.cloud_notm}} vornehmen,
empfiehlt es sich jedoch, entweder die IBM Entwicklerkonsole für Apple
oder {{site.data.keyword.dev_cli_notm}} zu verwenden. Da beide Methoden
dieselbe zugrunde liegende Technologie nutzen, erstellen die Entwicklerkonsole für Apple und IBM Developer Tools außerdem automatisch ein gehostetes
Projekt und eine Bereitstellungspipeline und stellen die von Ihrer Anwendung
benötigten Services bereit.

## Vorbereitende Schritte
{: #prereqs-kitura}

Stellen Sie zunächst sicher, dass die folgenden Voraussetzungen gegeben
sind:  

* iOS 11.0+  
* Xcode 9.0  
* Swift 4.0+  
* CocoaPods  

## Schritt 1. Kitura-Projekt mit dem Browser erstellen
{: #create_kitura}

1. Navigieren Sie in der Entwicklerkonsole für Apple zum Abschnitt
mit den Starter-Kits. Wählen Sie ein vordefiniertes Starter-Kit wie z. B. "Swift for
Backend for Frontend API" aus oder erstellen Sie nach Auswahl der Kachel
**Projekt erstellen** ein angepasstes Projekt. Klicken Sie
auf **Projekt erstellen**.

2. Geben Sie Ihrem Projekt einen Namen und wählen Sie die Position aus,
an der Ihr Projekt bereitgestellt werden soll. Wenn Sie sich hinsichtlich der
Position, an der die Anwendung bereitgestellt werden soll, unsicher sind,
verwenden Sie die Standardwerte, da diese später geändert werden können.

3. Wählen Sie Swift als Sprache aus. Daraufhin wird ein serverseitiges
Swift-Projekt erstellt. Außerdem werden die Optionen für Host und Domäne
angezeigt, die die URL für die Anwendung bilden. Verwenden Sie bei Unsicherheit
die Standardwerte; Sie können auch Ihre eigene angepasste Domäne aus einem
Domänenprovider angeben, in der sich die Anwendung befinden soll.

4. Für die REST-API, die Sie erstellen wollen, können Sie eine Open
API-Definition (auch "Swagger" genannt) angeben. Falls eine solche Definition
vorhanden ist, wird eine REST-API erstellt, die die entsprechenden
Handlerfunktionen in Kitura enthält. Wenn Sie nicht über eine Open
API-Definition verfügen, stellt dies kein Problem dar, denn mit den Router-APIs
von Kitura können Sie ohne großen Aufwand REST-APIs manuell erstellen.

5. Klicken Sie
auf **Projekt erstellen**.

Jetzt wird ein Projekt erstellt, das jedoch noch keine zusätzlichen
Services nutzt. Sie können Services hinzufügen, indem Sie auf die
Schaltfläche **Ressource hinzufügen** oder auf die Schaltfläche
**Code herunterladen** klicken, um den Code für das Projekt
abzurufen. Services können ohne Weiteres auch zu einem vorhandenen Projekt
hinzugefügt werden.

## Schritt 2. Services hinzufügen
{: #add_services-kitura}

1. Klicken Sie auf die Schaltfläche **Ressource hinzufügen**, um Services hinzuzufügen. Servicekategorien werden angezeigt. Wählen Sie beispielsweise
**Daten** aus, um die verfügbaren Datenbanken anzuzeigen,
und wählen Sie
**Cloudant NoSQL DB** aus.
2. Wählen Sie einen Preistarif für den Service aus (z. B. "Lite") und
klicken Sie auf
**Erstellen**.

Nun wird eine Instanz des Service erstellt, die
Berechtigungsnachweise für die Anwendung werden zur Verfügung gestellt und in
einigen Fällen wird der benötigte Code zu Ihrem Projekt hinzugefügt, damit die
richtige Verbindung zum Service enthalten ist. Jetzt können Sie weitere Services
hinzufügen,
indem Sie die Schaltfläche **Ressource hinzufügen**
auswählen oder auf **Code herunterladen**
klicken, um den Code für das Projekt abzurufen.

Nachdem Sie Ihr Projekt heruntergeladen haben, können Sie mit der Arbeit
an Ihrer App beginnen.

## Schritt 3. Anwendung mit Xcode entwickeln
{: #develop_xcode-kitura}

Nachdem Sie Ihr Projekt heruntergeladen haben, können Sie es mit Hilfe
von Xcode ändern und entwickeln und anschließend Ihre geänderte Anwendung zur
Bereitstellung in der Cloud hochladen.

1. Erstellen Sie ein Xcode-Projekt.  
  Bevor Sie Ihr Projekt in Xcode verwenden können, müssen Sie mit dem Befehl
des Swift-Paketmanagers eine Xcode-Projektdatei erstellen. Führen Sie den
folgenden Befehl im Stammverzeichnis Ihres heruntergeladenen Projekts aus. Dies
ist dasjenige Verzeichnis, das die Datei `Package.swift`
enthält:
  ```
  swift package generate-xcodeproj
  ```
  {: codeblock}

  In demselben Verzeichnis wird daraufhin ein Xcode-Projekt erstellt, das
Sie anschließend öffnen können.

2. Legen Sie das Xcode-Ziel für das Projekt fest.  
  Zur Ausführung des Kitura-Servers müssen Sie das Schema bearbeiten, indem Sie
in der Symbolleiste auf den Abschnitt
**projektname-Package** klicken und im Menü das Ziel
**projektname** auswählen. Überprüfen Sie, ob als Zielgerät
**Mein Mac** festgelegt ist.

3. Führen Sie den Kitura-Server lokal aus. 
  Klicken Sie auf **Ausführen** oder verwenden Sie die Tastenkombination `⌘+R`, um den Kitura-Server zu starten. Nachdem der Server
gestartet wurde, können Sie überprüfen, ob die folgenden Standard-URLs für
Kitura aktiv sind:
  * Kitura-Überwachung:
[http://localhost:8080/swiftmetrics-dash/]()
  * Kitura-Statusprüfung:
[http://localhost:8080/health]()

## Schritt 5. REST-APIs hinzufügen
{: #add_restapi-kitura}

Das Gerüst eines Kitura-Servers wird erstellt, das jedoch keine
REST-APIs bereitstellt, die durch eine iOS-Anwendung genutzt werden können. Sie
können REST-APIs in Kitura mit minimalem Codierungsaufwand hinzufügen. Die
folgenden Schritte zeigen, wie Sie eine REST-API für Anforderungen
`GET` von `/meals` hinzufügen, mit denen die Objekte des Typs `Meal` zurückgegeben werden, die auf dem Kitura-Server gespeichert sind.

1. Fügen Sie ein Struct `Meal` zur Datei
`Sources/Application/Application.swift` hinzu:  
  Fügen Sie vor der Deklaration der `App`-Klasse die folgenden Zeilen hinzu, um
ein Struct "Meal" zu erstellen, das dem Codable-Protokoll entspricht:  
  ```swift
	struct Meal: Codable {    
	    var name: String
	    var photo: Data
	    var rating: Int
	}
  ```
  {: codeblock}

2. Fügen Sie zur Datei
`Sources/Application/Application.swift` ein
Wörterverzeichnis hinzu, um Objekte
`Meal` zu speichern, indem Sie `let cloudEnv =
CloudEnv()` zum folgenden Codeabschnitt hinzufügen:
  ```swift
  private var mealStore: [String: Meal] = [:]
  ```
  {: codeblock}

3. Fügen Sie einen Handler für Anforderungen `GET`
von
`/meals` zur Datei
`Sources/Application/Application.swift` hinzu, indem Sie den
folgenden Code in der Funktion `postInit()` hinzufügen:  
  ```swift
  router.get("/meals", handler: loadHandler)
  ```
  {: codeblock}

4. Implementieren Sie die Funktion "loadHandler" in der Datei `Sources/Application/Application.swift`, indem Sie den folgenden Code
als weitere Funktion in der Klasse `App` hinzufügen:
  ```swift
  func loadHandler(completion: ([Meal]?, RequestError?) -> Void ) {
      let meals: [Meal] = self.mealStore.map({ $0.value })
    completion(meals, nil)
  }
  ```
  {: codeblock"}

Sie verfügen nun über eine REST-API für Anforderungen `GET`
von
`/meals`, die mit einem Array aus
`Meals` oder einem Fehler antwortet.

5. Führen Sie das Projekt aus.  
  Klicken Sie auf **Ausführen** oder verwenden Sie die Tastenkombination `⌘+R`. Wählen Sie bei entsprechender
Aufforderung die Option **Eingehende Netzverbindungen zulassen**
aus.

6. Testen Sie die REST-API mithilfe der Anforderung `GET`, die abgleicht, wie Web-Browser Daten von einem Server anfordern. 

  Sie können die REST-API mit der folgenden URL testen:  
  ```swift
  * `GET /meals`:	[http://localhost:8080/meals]()
  ```
  {: codeblock}

  Es wird ein leeres Array `[]` zurückgegeben, weil
gegenwärtig keine Objekte des Typs
`Meal` in `mealStore` gespeichert sind. 

Das Lernprogramm [FoodTracker Backend](https://github.com/IBM/FoodTrackerBackend) zeigt Ihnen, wie Sie eine Gruppe von REST-APIs für das Speichern, Abrufen und
Löschen von Objekten des Typs `Meal` aus einer iOS-Anwendung
heraus erstellen können (das Lernprogramm behandelt auch das
Speichern der Daten in einer Datenbank).
{: tip}

## Schritt 6. KituraKit in Ihrer iOS-Anwendung installieren
{: #install-kiturakit}

Die REST-APIs, die mithilfe des Kitura-Servers erstellt wurden, sind
Standard-Web-APIs, die unabhängig von der verwendeten Clientbibliothek oder
der
Sprache, in der der Client geschrieben ist, von jeder Anwendung verwendet
werden können. Dies bedeutet, dass Sie Alamofire, RestKit oder URLSession
verwenden können, um Verbindungen zum Server herzustellen. Kitura bietet zudem
einen maßgeschneiderten, optimierten Client-Connector, um die
REST-APIs von iOS im Format von KituraKit zu vereinfachen. 

KituraKit stellt ein Spiegelbild der APIs für Router-Handler bereit, die
in Kitura verwendet werden, was mit nur geringem Aufwand die gemeinsame Nutzung von
Swift-Typen zwischen dem Client und dem Server ermöglicht. Diese Funktion macht
die explizite Ausführung von JSON-Codierung und -Decodierung der Daten
überflüssig, die an den Server gesendet oder von ihm empfangen werden.

Die folgenden Schritte zeigen, wie Sie KituraKit in Ihrer iOS-Anwendung
installieren und zum Aufruf der erstellten REST-API `GET
/meals` mittels Kitura verwenden:

1. Erstellen Sie im Stammverzeichnis Ihrer iOS-Anwendung eine Poddatei,
wenn dort noch keine vorhanden ist:
  ```
  pod init
  ```
  {: codeblock}

2. Bearbeiten Sie die Poddatei und legen Sie die globale Plattform iOS
11 für Ihr Projekt fest, indem Sie die folgende Zeile ersetzen:
  ```pod
  # platform :ios, '9.0'
  ```
  {: codeblock}

  Geben Sie stattdessen den folgenden Code an:
	```pod
  platform :ios, '11.0'
  ```
  {: codeblock}

3. Fügen Sie KituraKit zur Poddatei hinzu, indem Sie Folgendes unter "# Pods
for <application name>" hinzufügen:
  ```pod
  pod 'KituraKit', :git => 'https://github.com/IBM-Swift/KituraKit.git', :branch => 'pod'
  ```
  {: codeblock}

4. Speichern Sie die Poddatei und installieren Sie KituraKit mit dem folgenden Befehl:
  ```
  pod install
  ```
  {: codeblock}

5. Öffnen Sie die Anwendung erneut in Xcode, indem Sie den Arbeitsbereich
(nicht das Projekt) öffnen. Jetzt können Sie dieselbe Definition für "Meal" zur
Ihrer iOS-Anwendung hinzufügen, die beim Kitura-Server verwendet wurde:
  ```swift
  struct Meal: Codable {    
      var name: String
      var photo: Data
      var rating: Int
  }
  ```
  {: codeblock}

  Verwenden Sie den folgenden Code, damit eine Verbindung zum Kitura-Server
hergestellt wird:
  
  ```swift
  import KituraKit
  
  /* Connect to the Kitura server on http://localhost:8080 */
  guard let client = KituraKit(baseURL: "http://localhost:8080") else {
      print("Error creating KituraKit client")
      return
  }
  
  /* Make a request to `GET /meals`, expecting a [Meal] or a RequestError */
  client.get("/meals") { (meals: [Meal]?, error: RequestError?) in
      /* Check for an error */
      guard error == nil else {
          print("Error saving meal to Kitura: \(error!)")
          return
      }
      /* Check for meals */
      guard let meals = meals else {
          print("Received Meals!")
          return
      }
  }
  ```
  {: codeblock}

Der Kitura-Server wird unter Verwendung von "client.get("/meals")" mit einem
Callback aufgerufen, der den Parametern aus dem Completion-Handler von Kitura
entspricht.

Das Lernprogramm
[FoodTrackerBackend] (https://github.com/IBM/FoodTrackerBackend)
zeigt Ihnen, wie Sie eine Gruppe von REST-APIs für das Speichern, Abrufen und
Löschen von Objekten des Typs "Meal" aus einer iOS-Anwendung
heraus erstellen können (das Lernprogramm behandelt auch das Speichern der
Daten in einer Datenbank).
{: tip}

## Nächste Schritte
{: #next-kitura notoc}

Nachdem Sie einen Kitura-Server erstellt haben, von dem eine REST-API bereitgestellt wird, die durch Ihre iOS-Anwendung aufgerufen wird, können Sie nun Ihren Server in {{site.data.keyword.cloud_notm}} bereitstellen. Für Bereitstellungen [Deployments](/docs/swift/deploying_apps.html) können Sie Container mit Kubernetes, sichere Container, Cloud Foundry, Cloud Foundry Enterprise Environment oder virtuelle Instanzen verwenden.
