---

copyright:
  years: 2017-2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Dokumente in {{site.data.keyword.cloud_notm}} speichern
{: #cloudant}

{{site.data.keyword.cloudantfull}} ist ein
dokumentorientiertes Produkt der Kategorie "DataBase as a Service" (DBaas). Es speichert Daten als Dokumente im JSON-Format. Es wurde im Hinblick
auf Skalierbarkeit, Hochverfügbarkeit und Dauerhaftigkeit konzipiert und kann
problemlos für den Einsatz in Swift-Anwendungen konfiguriert werden. Bereits im
Lieferzustand enthält es eine breite Vielzahl von Indexierungsoptionen, zu
denen MapReduce, {{site.data.keyword.cloudant_short_notm}} Query,
Volltextindexierung und georäumliche Indexierung gehören. Dank seiner
Replikationsfunktionen ist es denkbar einfach, Daten zwischen
Datenbankclustern, Desktop-PCs und Mobilgeräten synchron zu halten.
{:shortdesc}

Informationen zu allen Möglichkeiten für die Nutzung von
{{site.data.keyword.cloudant_short_notm}} finden Sie unter [Basisinformationen zu {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/basics/index.html#cloudant-nosql-db-basics).

## Vorbereitende Schritte

Stellen Sie zunächst sicher, dass die folgenden Voraussetzungen gegeben
sind:
 * CocoaPods (Version 1.1.0 oder höher)
 * iOS (Version 9 oder höher)
 * MacOS (Version 10.11.5 oder höher)
 * Xcode (Version 9.0.1 oder höher)

Das [{{site.data.keyword.cloudant_short_notm}}-SDK für Swift![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/cloudant/swift-cloudant) wurde mit Swift 3.2 erstellt. Falls Sie den Einsatz von
{{site.data.keyword.cloudant_short_notm}} mit Kitura planen, prüfen Sie
die [Bibliothek von
Kitura-CouchDB ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/IBM-Swift/Kitura-CouchDB), die mit Swift 4.0 erstellt wurde.
{: tip}

## Schritt 1. Instanz von
{{site.data.keyword.cloudant_short_notm}} erstellen

Informationen zur Bereitstellung einer Instanz des Service finden Sie
im Lernprogramm
[Cloudant NoSQL DB-Instanz in IBM Cloud erstellen![Symbol für externen Link](../images/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/docs/services/Cloudant/tutorials/create_service.html#creating-a-cloudant-nosql-db-instance-on-ibm-cloud){:new_window}.


## Schritt 2. SDK installieren

### Swift-SDK für iOS installieren

Das Cloudant-SDK für Swift vereinfacht die Codierung Ihrer App. Das SDK
muss in Ihrem App-Code installiert sein.

1. Öffnen Sie das vorhandene XCode-Projektverzeichnis für die
`Poddatei`.
2. Fügen Sie unter dem Ziel Ihres Projekts (Abschnitt "target") eine
Abhängigkeit für den Pod `SwiftCloudant` hinzu. Stellen Sie
sicher, dass der Befehl `use_frameworks!` ebenfalls wie im
folgenden Beispiel gezeigt unter Ihrem Ziel angegeben ist.
    ```
    target '<yourTarget>' do
      use_frameworks!
        pod 'SwiftCloudant', :git => 'https://github.com/cloudant/swift-cloudant.git'
    end
    ```
    {: screen}
3. Laden Sie die Abhängigkeit `SwiftCloudant` herunter.
    ```
    pod install
    ```
    {: pre}

### Serverseitiges Swift-SDK installieren

Zur Verwendung mit Swift Package Manager für die serverseitige
Entwicklung fügen Sie die folgende Zeile zu den Abhängigkeiten in Ihrer Datei
`Package.swift` hinzu:
```swift
.Package(url: "https://github.com/cloudant/swift-cloudant.git")
```
{: pre}

## Schritt 3. SDK initialisieren

Sobald Sie das SDK in Ihrer App initialisiert habe, können Sie
{{site.data.keyword.cloudant_short_notm}} zum Speichern von Daten
nutzen.

1.  Fügen Sie den folgenden Import zu Ihrer Datei
`AppDelegate.swift` oder zur serverseitigen Swift-Datei hinzu.
    ```
    import SwiftCloudant
    ```
    {:pre}
2. Initialisieren Sie die Verbindung zur Datenbank.
    ```swift
    let cloudantURL = NSURL(string: "https://username.cloudant.com")!
    let client = CouchDBClient(url: cloudantURL, username: "username", password: "password")
    let dbName = "database"
    ```
    {: codeblock}

### Basisoperationen
Die folgenden Basisoperationen veranschaulichen die grundlegenden
Aktionen zum Erstellen, Lesen und Löschen Ihrer Dokumente mithilfe des
initialisierten Clients.

#### Dokument erstellen
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

#### Dokument lesen
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

#### Dokument löschen
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


## Schritt 4. App testen
{: #cloudant_testing}

Jetzt können Sie testen, ob Sie alles richtig konfiguriert haben.

1. Führen Sie Ihre Anwendung aus; achten Sie dabei darauf, dass die
Initialisierung und die gewünschten Operationen (z. B. die Erstellung eines
Dokuments) aufgerufen werden.
2. Kehren Sie zu der {{site.data.keyword.cloudant_short_notm}}-Serviceinstanz zurück, die zuvor in Ihrem Web-Browser erstellt wurde, und
öffnen Sie das Service-Dashboard.
3. Wählen Sie die verwendete Datenbank aus. Daraufhin werden die
Dokumente im Dashboard angezeigt.

Falls Probleme auftreten, finden Sie in der [API-Referenz von {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/api/index.html#api-reference-overview) weitere Informationen.


## Nächste Schritte
{: #cloudant_next notoc}

Hervorragend! Sie haben Ihre App mit einer Ebene für die sichere
Datenspeicherung ausgestattet. Nutzen Sie diesen Schwung und probieren
Sie gleich eine der folgenden Optionen aus:

* Sehen Sie sich den Quellcode von [{{site.data.keyword.cloudant_short_notm}} SDK for Swift![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/cloudant/swift-cloudant) an.
* Starter-Kits bieten eine der schnellsten Möglichkeiten zur Nutzung des
Leistungsspektrums von IBM Cloud. Das Starter-Kit **Infinite Scrolling
with
Cloudant NoSQL for iOS** veranschaulicht, wie Sie einen
Anzeigecontroller so erweitern, dass Daten mit Seitenaufteilung angezeigt
werden. Dieses Muster einer App ist für iOS-Entwickler geläufig und ein sehr
gutes Beispiel für die Darstellung des Leistungsspektrums von {{site.data.keyword.cloudant_short_notm}}. Im [Dashboard für Entwickler für Mobilgeräte](https://console.bluemix.net/developer/mobile/dashboard) werden die verfügbaren Starter-Kits angezeigt. Sie können den Code herunterladen und die App ausführen.
* Lesen Sie weitere Informationen zu {{site.data.keyword.cloudant_short_notm}} in der [Dokumentation](/docs/services/Cloudant/index.html). Dort erfahren Sie auch, wie Sie alle gebotenen Funktionen nutzen können.
