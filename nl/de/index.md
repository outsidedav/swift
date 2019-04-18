---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: getting started swift, custom app, create app swift, stater kit swift, apple app swift, swift dependency, ios development

subcollection: swift

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

Das folgende Lernprogramm zeigt Ihnen, wie Sie mit {{site.data.keyword.mobileanalytics_full}} durch die Verwendung eines leeren Starter-Kits aus der [{{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") ohne großen Aufwand eine mobile Swift-App erstellen können. Ausgehend von der Konsole fügen Sie den
{{site.data.keyword.mobileanalytics_short}}-Service hinzu und laden den
Code herunter. Anschließend können Sie die iOS-App lokal in Xcode ausführen,
konfigurieren und überwachen.

## Schritt 1. Voraussetzungen für Entwicklung
{: #dev-requirements-swift}

Damit Sie in die iOS-Entwicklung in
{{site.data.keyword.cloud_notm}} einsteigen können, müssen die
folgenden Voraussetzungen erfüllt sein.

### Betriebssystem
{: #swift-os-requirements}

Es hat sich bewährt, zur Entwicklung von Swift-Apps die neueste
unterstützte Mac OS-Hardware zu verwenden und bei Tests die neuesten
iOS-Releases zu nutzen. Melden Sie sich für ein [Apple Developer](https://developer.apple.com/){: new_window}-Konto ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") an, um das Testen auf physischen Einheiten zu ermöglichen.

### iOS und Xcode
{: #ios_and_xcode}

- Installieren Sie [Xcode 8+](https://developer.apple.com/xcode/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") (oder höher).
- Führen Sie die Bereitstellung auf [Einheiten mit iOS 8](https://support.apple.com/downloads/ios){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") (oder höher) durch.
- Prüfen Sie vor der Übergabe von Apps an Apple die [Richtlinien für die Übergabe an den App-Store](https://developer.apple.com/app-store/guidelines/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link").

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

* **Mit Carthage** – Einige SDKs sind auch über die Abhängigkeitsmanager von [Carthage](https://github.com/Carthage/Carthage){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") bzw. des [Swift-Paketmanagers](https://swift.org/package-manager/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") verfügbar. Führen Sie die folgenden Schritte aus, um Carthage für das Abhängigkeitsmanagement zu verwenden:

  Installieren Sie [Homebrew](https://brew.sh/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") als Unterstützung für die Carthage-Installation:
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

1. Melden Sie sich bei der [{{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") an.
2. Klicken Sie auf **App erstellen**.
3. Auf der Seite [Leerer Starter](https://cloud.ibm.com/developer/appledevelopment/create-app){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") können Sie die Standardkonfiguration verwenden oder die Felder wie erforderlich aktualisieren. Achten Sie darauf, dass
als Sprache **iOS Swift** ausgewählt ist. Klicken Sie auf **Erstellen**.

## Schritt 3. {{site.data.keyword.cloudant_short_notm}}-Service hinzufügen
{: #resources-swift}

Sie können nun Services zu Ihrer Swift-Anwendung hinzufügen. Fügen Sie für dieses Lernprogramm den {{site.data.keyword.cloudant_short_notm}}-Service zu Ihrer Swift-App hinzu, wodurch eine vollständig verwaltete, verteilte `JSON`-Dokumentdatenbank erstellt wird. Cloudant verwaltet Ihre App mit Skalierbarkeit, hoher Verfügbarkeit und Dauerhaftigkeit in einem einfachen Framework, das Ihre Daten sicher und synchron hält.

1. Klicken Sie auf der Seite **App-Details** auf **Service hinzufügen**.
2. Wählen Sie die Einstellung **Daten** aus und klicken Sie auf **Weiter**.
3. Wählen Sie die Einstellung **Cloudant** aus und klicken Sie auf **Weiter**.
4. Klicken Sie auf **Erstellen**.
5. Sobald der Service erstellt wurde, klicken Sie darauf, um ihn zu starten. Wählen Sie auf dieser neuen Seite die Option **Cloudant-Dashboard starten** aus, um mit der Erstellung einer Datenbank und der Erstellung von JSON-Dokumenten zu beginnen.  Dies kann alternativ programmgesteuert erfolgen.

## Schritt 4. Code herunterladen und Client-SDKs konfigurieren
{: #run-locally-swift}

Klicken Sie zum Herunterladen des Codes auf **Code herunterladen** unter `Apps` > `Your App`. Der heruntergeladene Code sowie auch einiger Basisinitialisierungscode ist im Lieferumfang des [SwiftCloudant-SDKs](https://github.com/cloudant/swift-cloudant){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") enthalten. Die Client-SDKs sind in CocoaPods und Swift Package Manager verfügbar. Bei dieser Lösung wird CocoaPods verwendet.

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

  Die Serviceberechtigungsnachweise sind Teil der Datei `BMSCredentials.plist`.
  {: note}

## Schritt 6. Eigene Datenbankoperationen erstellen
{: #build_ops-swift}

Sie verfügen nun über eine funktionierende Datenbankverbindung und SDK-Installation und können mit der Erstellung der grundlegenden [Erstell-, Lese-, Aktualisierungs- und Löschoperationen](/docs/swift/data?topic=swift-cloudant#cloudant) für Ihren jeweiligen Anwendungsfall beginnen.

## Nächste Schritte
{: #next-swift}

### Weitere Services hinzufügen
{: #moreresources-swift}

Direkt von der Konsole aus können Sie weitere Services zu Ihrer iOS-App
hinzufügen, beispielsweise die folgenden häufig genutzten Services:

* [Push-Benachrichtigungsservice
hinzufügen](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate#gettingstartedtemplate)
* [Benutzerauthentifizierung mit App-ID hinzufügen](/docs/services/appid?topic=appid-getting-started#getting-started)

### {{site.data.keyword.cloud_notm}}-Entwicklertool verwenden
{: #devtools-swift}

Sie können auch lernen, wie Swift-Apps entwickelt werden, indem Sie die [{{site.data.keyword.cloud_notm}}-Entwicklertools](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) verwenden, die einen Befehlszeilenansatz zum Erstellen, Entwickeln und Bereitstellen von vollständigen Web-, Mobil- und Mikroserviceanwendungen bieten.
