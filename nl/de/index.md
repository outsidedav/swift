---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-31"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Lernprogramm zur Einführung
{: #getting_started_swift}

{{site.data.keyword.cloud}} bietet Lösungen und Services, mit denen Swift-Entwickler Anwendungen erstellen können, die dem Kundenbedarf an Sicherheit, künstlicher Intelligenz (KI) und Wertschöpfung Rechnung tragen. Dank eines
breit aufgestellten Portfolios von Produktangeboten und SDKs können Sie durch
die Nutzung dieser Services eine schnelle Markteinführung von innovativen
Anwendungen erreichen. In der Veröffentlichung zur Swift-Programmierung wird erläutert, wie
Sie Services zu einer neuen oder bestehenden Anwendung hinzufügen können, ganz
gleich, ob es sich um einen iOS-Client oder eine serverseitige Swift-Anwendung
handelt.
{: shortdesc}

Das folgende Lernprogramm zeigt Ihnen, wie
Sie mit {{site.data.keyword.mobileanalytics_full}} durch die Verwendung
eines leeren Starter-Kits aus
der
[{{site.data.keyword.cloud_notm}}-Entwicklerkonsole
für Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits) ohne großen Aufwand eine mobile Swift-App
erstellen können. Ausgehend von der Konsole fügen Sie den
{{site.data.keyword.mobileanalytics_short}}-Service hinzu und laden den
Code herunter. Anschließend können Sie die iOS-App lokal in Xcode ausführen,
konfigurieren und überwachen.

## Schritt 1. Voraussetzungen für die Entwicklung
{: #dev-requirements-swift}

Damit Sie in die iOS-Entwicklung in
{{site.data.keyword.cloud_notm}} einsteigen können, müssen die
folgenden Voraussetzungen erfüllt sein.

### Betriebssystem
{: #swift-os-requirements}

Es hat sich bewährt, zur Entwicklung von Swift-Apps die neueste
unterstützte Mac OS-Hardware zu verwenden und bei Tests die neuesten
iOS-Releases zu nutzen. Registrieren Sie sich für ein Konto des Typs
[Apple Developer](https://developer.apple.com/), um Tests auf
einer physischen Einheit zu ermöglichen.

### iOS und Xcode
{: #ios_and_xcode}

- Installieren Sie [Xcode
8+](https://developer.apple.com/xcode/) (oder höher).
- Führen Sie die Bereitstellung auf
[Einheiten mit iOS
8](https://support.apple.com/downloads/ios) (oder höher) durch.
- Prüfen Sie vor der Übergabe von Apps an Apple die Richtlinien
für die Übergabe an den App-Store, die Sie der Seite
[App Store
Submission Guidelines](https://developer.apple.com/app-store/guidelines/) entnehmen können.

### SDKs und Abhängigkeitsmanagement
{: #swift-sdk-management}

Die folgenden Tools gewährleisten, dass Sie die nativen SDKs zur
Verwendung mit den verschiedenen
{{site.data.keyword.cloud_notm}}-Services installieren können. Für das Abhängigkeitsmanagement können Sie CocoaPods oder Carthage verwenden.

* **Mit CocoaPods** - Installieren Sie [CocoaPods](https://cocoapods.org/) zur Unterstützung von {{site.data.keyword.cloud_notm}}-SDKs:
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}

* **Mit Carthage** - Einige SDKs sind auch über die [Carthage](https://github.com/Carthage/Carthage)- oder [Swift Package Manager](https://swift.org/package-manager/)-Abhängigkeitsmanager verfügbar. Führen Sie die folgenden Schritte aus, um Carthage für das Abhängigkeitsmanagement zu verwenden:

  Installieren Sie [Homebrew](https://brew.sh/) zur
Unterstützung der Carthage-Installation:
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

  Installieren Sie Carthage:
  ```
  brew install carthage
  ```
  {: codeblock}

## Schritt 2. Angepasste iOS-Swift-App erstellen
{: #create-ios-app-swift}

1. Melden Sie sich bei der
[{{site.data.keyword.cloud_notm}}-Entwicklerkonsole
für Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits) an.
2. Klicken Sie auf **App erstellen**.
3. Auf der Seite
[Leerer
Starter](https://cloud.ibm.com/developer/appledevelopment/create-app) können Sie die Standardkonfiguration
verwenden oder die Felder nach Bedarf aktualisieren. Achten Sie darauf, dass
als Sprache **iOS Swift** ausgewählt ist. Klicken Sie auf **Erstellen**.

## Schritt 3. {{site.data.keyword.cloudant_short_notm}}-Service hinzufügen
{: #resources-swift}

Sie können nun Services zu Ihrer Swift-Anwendung hinzufügen. Fügen Sie für dieses Lernprogramm den {{site.data.keyword.cloudant_short_notm}}-Service zu Ihrer Swift-App hinzu, wodurch eine vollständig verwaltete, verteilte `JSON`-Dokumentdatenbank erstellt wird. Cloudant verwaltet Ihre App mit Skalierbarkeit, hoher Verfügbarkeit und Dauerhaftigkeit in einem einfachen Framework, das Ihre Daten sicher und synchron hält.

1. Klicken Sie auf der Seite "App-Details" auf die Schaltfläche **Ressource hinzufügen**.
2. Wählen Sie die Einstellung **Daten** aus und klicken Sie auf **Weiter**.
3. Wählen Sie die Einstellung **Cloudant** aus und klicken Sie auf **Weiter**.
4. Klicken Sie auf **Erstellen**.
5. Sobald der Service erstellt wurde, klicken Sie darauf, um ihn zu starten. Wählen Sie auf dieser neuen Seite die Option **Cloudant-Dashboard starten** aus, um mit der Erstellung einer Datenbank und der Erstellung von JSON-Dokumenten zu beginnen. Dies kann alternativ programmgesteuert erfolgen.

## Schritt 4. Code herunterladen und Client-SDKs konfigurieren
{: #run-locally-swift}

Zum Herunterladen des Codes klicken Sie unter
`Apps` > `Ihre App` auf **Code
herunterladen**. Der heruntergeladene Code sowie auch einiger Basisinitialisierungscode ist im Lieferumfang des [SwiftCloudant SDK](https://github.com/cloudant/swift-cloudant) enthalten. Die Client-SDKs sind in CocoaPods und Swift Package Manager verfügbar. Bei dieser Lösung wird CocoaPods verwendet.

1. Extrahieren Sie den heruntergeladenen Code. Navigieren Sie dann mit einem Terminal zum extrahierten Ordner.
  ```
  cd <name_des_projekts>
  ```

2. Der Ordner enthält eine Poddatei mit den erforderlichen Abhängigkeiten. Führen Sie den folgenden Befehl aus, um die Abhängigkeiten (Client-SDKs) zu installieren:
  ```
  pod install
  ```
  {: codeblock}

## Schritt 5. Die App für die Verwendung der neuen Datenbank konfigurieren
{: #config-db-swift}

1. Öffnen Sie die Datei, die mit `.xcworkspace` endet in Xcode und navigieren zu `ViewController.swift`. `SwiftCloudant` ist das zentrale Cloudant-SDK. `CouchDBClient` ist eine Klasse von `SwiftCloudant` und wird in `ViewController.swift` initialisiert.

  Öffnen Sie immer den neuen Xcode-Arbeitsbereich und nicht die ursprüngliche XCode-Projektdatei: `MyApp.xcworkspace`.
  {: tip}

2. Der Datenbankinitialisierungscode ist, wie im folgenden Snippet gezeigt, bereits enthalten:
  ```swift
  /* Read the Cloudant credentials and intialize database connection */
  if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"), let dictionary = NSDictionary(contentsOfFile: contents) {
            let url = URL(string: dictionary["cloudantUrl"] as! String)
            let client = CouchDBClient(url:url!,
                                       username:dictionary["cloudantUsername"] as? String,
                                       password:dictionary["cloudantPassword"] as? String)
        }
  ```
  {: codeblock}

  Die Serviceberechtigungsnachweise sind Teil der Datei `BMSCredentials.plist`.{: note}

## Schritt 6. Eigene Datenbankoperationen erstellen
{: #build_ops-swift}

Sie verfügen nun über eine funktionierende Datenbankverbindung und SDK-Installation und können mit der Erstellung der grundlegenden [Erstell-, Lese-, Aktualisierungs- und Löschoperationen](./data/cloudant.html#basic-operations) für Ihren jeweiligen Anwendungsfall beginnen.

## Nächste Schritte
{: #next-swift}

### Weitere Services hinzufügen
{: #moreresources-swift}

Direkt von der Konsole aus können Sie weitere Services zu Ihrer iOS-App
hinzufügen, beispielsweise die folgenden häufig genutzten Services:

* [Push-Benachrichtigungsservice
hinzufügen](/docs/services/mobilepush/index.html)
* [Benutzerauthentifizierung mit App-ID hinzufügen](/docs/services/appid/index.html)

### {{site.data.keyword.cloud_notm}}-Entwicklertool verwenden
{: #devtools-swift}

Sie können auch lernen, wie Swift-Apps entwickelt werden, indem Sie die [{{site.data.keyword.cloud_notm}}-Entwicklertools](/docs/cli/index.html) verwenden, die einen Befehlszeilenansatz zum Erstellen, Entwickeln und Bereitstellen von vollständigen Web-, Mobil- und Mikroserviceanwendungen bieten.
