---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Lernprogramm zur Einführung
{: #set_up}

{{site.data.keyword.cloud}} bietet Lösungen und Services, mit
denen Swift-Entwickler und -Anwendungen dem Kundenbedarf an Sicherheit,
künstlicher Intelligenz und Wertschöpfung Rechnung tragen können. Dank eines
breit aufgestellten Portfolios von Produktangeboten und SDKs können Sie durch
die Nutzung dieser Services eine schnelle Markteinführung von Anwendungen erreichen. Im Handbuch für die Swift-Programmierung erfahren Sie, wie
Sie Services zu einer neuen oder bestehenden Anwendung hinzufügen können, ganz
gleich, ob es sich um einen iOS-Client oder eine serverseitige Swift-Anwendung
handelt.
{: shortdesc}

Das folgende Lernprogramm dient als Einstiegspunkt und zeigt Ihnen, wie
Sie mit {{site.data.keyword.mobileanalytics_full}} durch die Verwendung
eines leeren Starter-Kits aus
der
[{{site.data.keyword.cloud_notm}}-Entwicklerkonsole
für Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits) ohne großen Aufwand eine mobile Swift-App
erstellen können. Ausgehend von der Konsole fügen Sie den
{{site.data.keyword.mobileanalytics_short}}-Service hinzu und laden den
Code herunter. Anschließend können Sie die iOS-App lokal in Xcode ausführen,
konfigurieren und überwachen.

## Schritt 1. Voraussetzungen für die Entwicklung
{: #dev-requirements}

Damit Sie in die iOS-Entwicklung in
{{site.data.keyword.cloud_notm}} einsteigen können, müssen die
folgenden Voraussetzungen erfüllt sein.

### Betriebssystem

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

Die folgenden Tools gewährleisten, dass Sie die nativen SDKs zur
Verwendung mit den verschiedenen
{{site.data.keyword.cloud_notm}}-Services installieren können.

1. Installieren Sie [CocoaPods](https://cocoapods.org/)
zur Unterstützung von {{site.data.keyword.cloud_notm}}-SDKs:
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
2. Installieren Sie [Homebrew](https://brew.sh/) zur
Unterstützung der Carthage-Installation:
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

3. Installieren Sie
[Carthage](https://github.com/Carthage/Carthage) zur
Unterstützung der Watson-SDKs:
  ```
  brew install carthage
  ```
  {: codeblock}

## Schritt 2. Angepasste iOS-Swift-App erstellen
{: #create-ios-app}

1. Melden Sie sich bei der
[{{site.data.keyword.cloud_notm}}-Entwicklerkonsole
für Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits) an.
2. Klicken Sie auf **App erstellen**.
3. Auf der Seite
[Leerer
Starter](https://console.bluemix.net/developer/appledevelopment/create-app) können Sie die Standardkonfiguration
verwenden oder die Felder nach Bedarf aktualisieren. Achten Sie darauf, dass
als Sprache **iOS Swift** ausgewählt ist. Klicken Sie auf **Erstellen**.

## Schritt 3. {{site.data.keyword.mobileanalytics_short}}-Service hinzufügen
{: #adding-services}

1. Klicken Sie auf der Seite "App-Details" auf die Schaltfläche **Ressource hinzufügen**.
2. Wählen Sie die Einstellung **Mobil** aus und
klicken Sie auf **Weiter**.
3. Wählen Sie
**{{site.data.keyword.mobileanalytics_short}}** aus
und klicken Sie auf **Weiter**.
4. Klicken Sie auf **Erstellen**.

## Schritt 4. Code herunterladen und Client-SDKs konfigurieren
{: #run-locally}

Zum Herunterladen des Codes klicken Sie unter
`Apps` > `Ihre App` auf **Code
herunterladen**. Der heruntergeladene Code ist im Lieferumfang der
Client-SDKs von
**{{site.data.keyword.mobileanalytics_short}}**
enthalten. Die Client-SDKs sind in CocoaPods und Carthage verfügbar. Verwenden
Sie bei der hier beschriebenen Lösung CocoaPods.

1. Dekomprimieren Sie den heruntergeladenen Code. Navigieren Sie dann mit einem Terminal zum dekomprimierten Ordner.
  ```
  cd <name_des_projekts>
  ```
2. Der Ordner enthält eine Poddatei mit den erforderlichen Abhängigkeiten. Führen Sie den folgenden Befehl aus, um die Abhängigkeiten (Client-SDKs) zu installieren:
  ```
  pod install
  ```
  {: codeblock}

## Schritt 5. App zur Verwendung von {{site.data.keyword.mobileanalytics_short}} konfigurieren
{: #configure-analytics}

1. Öffnen Sie `.xcworkspace` in Xcode und
navigieren Sie zu `AppDelegate.swift`. **Hinweis:**
Achten Sie immer darauf, nicht die ursprüngliche XCode-Projektdatei, sondern den
neuen Xcode-Arbeitsbereich zu öffnen: `MyApp.xcworkspace`.
![Xcode öffnen](images/Xcode.png)

  `BMSCore` ist das Basis-SDK und bildet die Basis
der SDKs für mobile Clients. `BMSClient` ist eine Klasse
von BMSCore und wird in AppDelegate.swift initialisiert. Das
{{site.data.keyword.mobileanalytics_short}}-SDK wurde bereits zusammen
mit BMSCore in das Projekt importiert.
  
2. Der Initialisierungscode für Analytics ist, wie im folgenden Snippet gezeigt, bereits enthalten:
  ```
  // Analytics client SDK is configured to record lifecycle events.
         	Analytics.initialize(appName:dictionary["appName"] as? String,
        			     apiKey: dictionary["analyticsApiKey"] as? String,
        	        	     deviceEvents: .lifecycle)

        	// Enable Logger (disabled by default) and set level to ERROR (DEBUG by default).
        	Logger.isLogStorageEnabled = true
        	Logger.logLevelFilter = .error
  ```
  {: codeblock}

  **Hinweis:** Die Serviceberechtigungsnachweise
sind Teil der Datei `BMSCredentials.plist`.

3. Die Nutzungsanalyse wird mithilfe von `logger`
zusammengestellt. Navigieren Sie zur Datei
`ViewController.swift`; sie enthält den folgenden Code:
  ```
   func didBecomeActive(_ notification: Notification) {
       Analytics.send()
       Logger.send()
    }
  ```
  {: codeblock}

   Informationen zu erweiterten Analyse- und Protokollierungsfunktionen finden Sie unter [Nutzungsanalyse zusammenstellen](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics) und [Protokollierung](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger).
{:tip}

## Schritt 6. App mit {{site.data.keyword.mobileanalytics_short}} überwachen
Der {{site.data.keyword.mobileanalytics_short}}-Service
ermöglicht Entwicklern und Anwendungseignern wichtige Einblicke in die
Anwendungsnutzung und -leistung. Bei Verwendung von
{{site.data.keyword.mobileanalytics_short}} können Anwendungseigner und
Entwickler nachvollziehen, was auf der Benutzerseite geschieht. Dank dieser
Erkenntnisse können bessere Anwendungen erstellt werden, die für die Benutzer
interessant sind und sich aus der wahren Flut von mobilen Anwendungen
hervorheben.

Der Service beinhaltet die
{{site.data.keyword.mobileanalytics_short}}-Konsole, in der Entwickler
und Anwendungseigner die Leistung von mobilen Anwendungen überwachen, die
Nutzungsstatistiken anzeigen und die Einheitenprotokolle durchsuchen können.

1. Öffnen Sie ausgehend von der erstellten mobilen App den
`{{site.data.keyword.mobileanalytics_short}}`-Service oder
klicken Sie auf die drei vertikalen Punkte neben dem Service und wählen Sie die
Option `Dashboard öffnen` aus.
2. Wenn Sie den `Demomodus` inaktivieren, können Sie
tatsächlich aktive Benutzer, Sitzungen und weitere App-Daten anzeigen. Sie
können die Analyseinformationen nach den folgenden Kriterien filtern:
    * Datum
    * Anwendung
    * Betriebssystem
    * Version der App ![{{site.data.keyword.mobileanalytics_short}}](images/mobile_analytics.png)
3. Wenn Sie Alerts festlegen und App-Abstürze sowie Netzanforderungen überwachen wollen, klicken Sie [hier](https://console.bluemix.net/docs/services/mobileanalytics/app-monitoring.html#monitoringapps).

## Nächste Schritte
{: #next-steps}

### Weitere Services hinzufügen
Direkt von der Konsole aus können Sie weitere Services zu Ihrer iOS-App
hinzufügen, beispielsweise die folgenden häufig genutzten Services:

* [Push-Benachrichtigungsservice
hinzufügen](/push/push_notifications.html)
* [Benutzerauthentifizierung mit App-ID hinzufügen](/authenticate/app_id.html)

### {{site.data.keyword.cloud_notm}}-Entwicklertools verwenden
Lernen Sie, wie Sie Swift-Apps mit den
[{{site.data.keyword.cloud_notm}}-Entwicklertools](../cli/index.html)
entwickeln können, die einen befehlszeilenorientierten Ansatz für die
Entwicklung und Bereitstellung von umfassenden Web-, Mobil- und
Mikroserviceanwendungen verfolgen.

