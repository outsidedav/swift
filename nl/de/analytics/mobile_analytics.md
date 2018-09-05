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

# Analysedaten für Mobilgeräte erfassen
{: #mobile_analytics}

{{site.data.keyword.mobileanalytics_short}} on
{{site.data.keyword.cloud_notm}} vermittelt Entwicklern, IT-Administratoren
und Verantwortlichen Erkenntnisse über die Leistung und die Verwendung ihrer
mobilen Apps. Der {{site.data.keyword.mobileanalytics_short}}-Service
ermöglicht Folgendes:

 - **Sofortiger Gewinn von Erkenntnissen** -
Leistungs- und Nutzungsmetriken werden in Echtzeit angezeigt.

 - **Minutenschnelle Implementierung** - Sie
erstellen eine Serviceinstanz in {{site.data.keyword.cloud_notm}}, Sie
fügen das SDK zu Ihrem Projekt hinzu, Sie fügen zwei Codezeilen in Ihrer
Anwendung ein und schon können Sie Dutzende vordefinierter Metriken erfassen.

 - **Erfassung der gewünschten Daten** - Sie können
Apps mit angepassten Ereignissen instrumentieren, die Interaktion von Benutzern
mi Ihrer App erkennen, Einkäufe verfolgen und die Aktivität der App überwachen.

 - **Anzeige von Metriken auf einen Blick** - Die
{{site.data.keyword.mobileanalytics_short}}-Konsole bietet
gebrauchsfertige Diagramme, für die keine Abfragen geschrieben werden müssen.

 - **Konzentration auf wichtige Aspekte** - Sie
können die Metriken nach Zeit, Adapter, Anwendung, Anwendungsversion,
Betriebssystem, Betriebssystemversion oder Einheitenmodell filtern.

 - **Zeitnahe Erkennung von Problemen** - Sie können
den Ausfallstatus überwachen, Alert-Trigger für kritische Metriken festlegen
und Alerts zu jedem beliebigen REST-Endpunkt weiterleiten.

 - **Fehlerbehebung der eigentlichen Ursache** - Sie
können die angepasste Protokollierung in Ihrer Anwendung nutzen und die
Protokolle automatisch in die Konsole hochladen sowie dort durchsuchen. Durch
ein Drilldown bei Ausfallereignissen können Sie Stack-Traces anzeigen.

## Vorbereitende Schritte

Stellen Sie zunächst sicher, dass die folgenden Voraussetzungen gegeben sind:

 - iOS 8.0+ / watchOS 2.0+
 - Xcode 7.3, 8.0
 - Swift 2.2 - 3.0
 - Cocoapods oder Carthage

## Schritt 1. Instanz von
{{site.data.keyword.mobileanalytics_short}} erstellen
{: #mobile_analytics_create}

1. Klicken Sie im {{site.data.keyword.cloud_notm}}-Katalog auf
**Mobil** > **{{site.data.keyword.mobileanalytics_short}}**. Die Anzeige für die Servicekonfiguration wird geöffnet.
2. Vergeben Sie für die Serviceinstanz einen Namen oder verwenden Sie den voreingestellten Namen.
3. Klicken Sie auf **Erstellen**.
4. Klicken Sie im Navigationsbereich
auf **Verbindungen**, um eine App auszuwählen und an Ihren
Service zu binden. Falls Sie während der Erstellung keine Bindung herstellen,
können Sie Ihre App auch später an die Serviceinstanz binden.

## Schritt 2. Swift-SDK für iOS installieren

Der Service stellt plattformspezifische SDKs bereit, um die
Anwendungsentwicklung zu vereinfachen. Die
{{site.data.keyword.cloud_notm}}-Swift-SDKs für Mobile-Services können
entweder mit Cocoapods oder Carthage installiert werden. Weitere Informationen
finden Sie unter der Adresse [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics).

Mithilfe des SDK für {{site.data.keyword.mobileanalytics_full}}
können Sie Ihre mobile Anwendung instrumentieren. Das Swift-SDK ist für iOS und
watchOS verfügbar.

1. Stellen Sie sicher, dass Sie Xcode ordnungsgemäß konfiguriert haben. Weitere
Informationen zur Einrichtung Ihrer iOS-Entwicklungsumgebung, finden Sie
auf der [Apple-Website für Entwickler ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.apple.com/support/xcode/){: new_window}. Lesen
Sie auch die Informationen
zu den
[Xcode-Voraussetzungen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#requirements){: new_window} für die Swift-Analyse mit dem Client-SDK.

2. Aktivieren Sie die Standort-API, indem Sie in der Datei
`Info.plist`, die sich im Projektordner Ihrer App befindet,
eine Eigenschaft hinzufügen. Fügen Sie beispielsweise
`Datenschutz - Beschreibung der Standortnutzung` hinzu und
fügen Sie eine aussagekräftige Begründung für das Hinzufügen der Standort-API
hinzu (Beispiel: "Für die App muss der standortbezogene Service aktiviert
sein.").

Das {{site.data.keyword.mobileanalytics_short}}-SDK wird
mit [CocoaPods ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cocoapods.org/){: new_window} und
[Carthage ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/Carthage/Carthage#getting-started){: new_window}
bereitgestellt; hierbei handelt es sich um die Abhängigkeitsmanager für
Cocoa-Projekte. CocoaPods und Carthage laden Artefakte automatisch aus den
Repositorys herunter, um sie für Ihre Anwendung zur Verfügung zu stellen. Gehen
Sie folgendermaßen vor, um CocoaPods oder Carthage auszuwählen:

### CocoaPods
{: #cocoapods}

1. Befolgen Sie die
[Anweisungen zum Swift-SDK für {{site.data.keyword.Bluemix_notm}} Mobile-Services ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#cocoapods){: new_window}
bei
GitHub, um `BMSAnalytics` mittels Cocoapods zu installieren
und zu Ihrer Poddatei hinzuzufügen.

2. Nach der Installation des Client-SDK für iOS müssen Sie das
Client-SDK für die Analyse [importieren
und initialisieren](sdk.html#initalize-ma-sdk). 

### Carthage
{: #carthage}

Falls Sie nicht CocoaPods verwenden, können Sie Frameworks mithilfe von
[Carthage ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos){: new_window} zu
Ihrem Projekt hinzufügen.

1. Befolgen Sie die
[Installationsanweisungen für Carthage ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#carthage){: new_window} bei
GitHub, um `BMSAnalytics` zu installieren.

2. Nach der Installation des Client-SDK für iOS müssen Sie das
Client-SDK für die Analyse importieren und initialisieren.

## Schritt 3. SDK initialisieren

Mittels {{site.data.keyword.mobileanalytics_short}} können Sie
die folgenden Kategorien von Daten erfassen, die jeweils einen
unterschiedlichen Instrumentierungsumfang erfordern:

**Vordefinierte Daten**: Diese Kategorie beinhaltet
allgemeine Verwendungs- und Geräteinformationen, die für alle Anwendungen
gelten. Sie gibt den Umfang, die Häufigkeit oder die Dauer der Anwendungsnutzung
an. Vordefinierte Daten werden automatisch erfasst, nachdem Sie das
{{site.data.keyword.mobileanalytics_short}}-SDK in Ihrer Anwendung
initialisiert haben.

**Anwendungsprotokollnachrichten**: Diese Kategorie
ermöglicht es dem Entwickler, Codezeilen in der gesamten Anwendung
hinzuzufügen, die angepasste Nachrichten protokollieren können, um die
Entwicklung und das Debugging zu unterstützen. Der Entwickler weist jeder
Protokollnachricht eine Priorität bzw. eine Detailstufe zu.

**Angepasste Ereignisse**: Diese Kategorie enthält
Daten, die Sie selbst definieren und die speziell für Ihre App bestimmt sind. Diese
Daten stellen Ereignisse dar, die innerhalb Ihrer App stattfinden, wie
beispielsweise Seitenaufrufe, Auswahl von Schaltflächen oder Einkäufe innerhalb
der App. Zusätzlich zum Initialisieren des
{{site.data.keyword.mobileanalytics_short}}-SDK in Ihrer App müssen
Sie für jedes benutzerdefinierte Ereignis, das Sie überwachen möchten, eine
Codezeile hinzufügen.

## Schritt 4. API-Schlüsselwert für Serviceberechtigungsnachweise ermitteln
{: #analytics-clientkey}

Bevor Sie das Client-SDK konfigurieren, müssen Sie den Wert für Ihren
**API-Schlüssel** ermitteln. Der API-Schlüssel wird zur
Initialisierung des Client-SDK benötigt.

1. Öffnen Sie das
Dashboard für den {{site.data.keyword.mobileanalytics_short}}-Service.
2. Erweitern Sie **Berechtigungsnachweise anzeigen**, um den API-Schlüsselwert anzuzeigen. 
Sie benötigen den API-Schlüsselwert, wenn Sie das Client-SDK für {{site.data.keyword.mobileanalytics_short}} initialisieren.

## Schritt 5.  Anwendung zur Erfassung von Analysedaten initialisieren
{: #initalize-ma-sdk}

Sie initialisieren Ihre Anwendung, damit Protokolle an den
{{site.data.keyword.mobileanalytics_short}}-Service gesendet werden.

1. Importieren Sie das Client-SDK.

    Das Swift-SDK ist für iOS und watchOS verfügbar. Importieren Sie
die Frameworks
`BMSCore` und `BMSAnalytics`, indem Sie die
folgenden Anweisungen `import` am Beginn Ihrer Projektdatei
`AppDelegate.swift` hinzufügen:
	```
	import BMSCore
	import BMSAnalytics
	```
	{: codeblock}  

2. Initialisieren Sie das Client-SDK für
{{site.data.keyword.mobileanalytics_short}} in Ihrer
Anwendung.

	Initialisieren Sie zuerst die Klasse `BMSClient`,
indem Sie den Initialisierungscode in der Methode
`application(_:didFinishLaunchingWithOptions:)` Ihrer
Anwendungsdelegierung oder an einer Position angeben, die für Ihr Projekt am
besten geeignet ist.
	```
	BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Make sure that you point to your region
	```
	{: codeblock}

	Sie müssen `BMSClient` mit dem Parameter
**bluemixRegion** initialisieren. Im
Initialisierungsoperator gibt der Wert **bluemixRegion**
an, welche {{site.data.keyword.Bluemix_notm}}-Bereitstellung von Ihnen verwendet wird.

3. Initialisieren Sie die Analyse, indem Sie das Anwendungsobjekt
verwenden und ihm den Namen Ihrer Anwendung geben.

	Der Name, den Sie für Ihre Anwendung auswählen
(`your_app_name_here`) wird in der
{{site.data.keyword.mobileanalytics_short}}-Konsole als Anwendungsname
angezeigt. Der Anwendungsname wird bei der Suche nach Anwendungsprotokollen im
Dashboard als Filter verwendet. Wenn Sie denselben Anwendungsnamen
plattformübergreifend verwenden (z. B. iOS), können Sie alle Protokolle aus
dieser Anwendung unter demselben Namen sehen, unabhängig davon, von welcher
Plattform die Protokolle gesendet wurden.

	Sie benötigen außerdem den Wert für den
[API-Schlüssel](#analytics-clientkey).
	```
	Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, collectLocation: true, deviceEvents: .lifecycle, .network)
	```
    {: codeblock}

4. Die Anwendung ist nun initialisiert und zur Erfassung von
Analysedaten bereit. Als Nächstes können Sie Analysedaten an den
{{site.data.keyword.mobileanalytics_short}}-Service senden.		

## Schritt 6. Nutzungsanalyse erfassen
{: #app-monitoring-gathering-analytics}

Sie können das Client-SDK von
{{site.data.keyword.mobileanalytics_short}}
so konfigurieren, dass die Nutzungsanalyse aufgezeichnet wird und die
aufgezeichneten Daten an den
{{site.data.keyword.mobileanalytics_short}}-Service gesendet werden.

Verwenden Sie die folgenden APIs, um mit dem Aufzeichnen und Senden der
Nutzungsanalyse zu beginnen:
```
// Disable recording of usage analytics (eg: to save disk space)
// Recording is enabled by default

Analytics.isEnabled = false

// Enable recording of usage analytics

Analytics.isEnabled = true

// Send recorded usage analytics to the Mobile Analytics Service

Analytics.send(completionHandler: { (response: Response?, error: Error?) in
    if let response = response {

	  // Handle Analytics send success here.
    }
    if let error = error {
        // Handle Analytics send failure here.
    }
})
```
{: codeblock}

Nutzungsanalysebeispiel für die Protokollierung eines Ereignisses:
```
// Log a custom analytics event
let eventObject = ["customProperty": "propertyValue"]
Analytics.log(metadata: eventObject)
```
{: codeblock}

## Schritt 7. Protokollfunktion verwenden

Das Client-SDK für {{site.data.keyword.mobileanalytics_full}}
bietet ein Protokollierungsframework, das Ähnlichkeit mit anderen
Protokollframeworks besitzt, mit denen Sie möglicherweise bereits vertraut
sind (z. B. `java.util.logging` oder `log4j`). 
Das Protokollierungsframework unterstützt mehrere Protokollfunktionsinstanzen
pro Paket, verschiedene Protokollebenen, die Erfassung von Stack-Traces für
einen Anwendungsausfall und vieles mehr.

Sie können die Konfiguration auch so definieren, dass die protokollierten
Daten auf dem Gerät, auf dem die Anwendung ausgeführt wird, gespeichert und die
Geräteprotokolle später an den
{{site.data.keyword.mobileanalytics_short}}-Service gesendet werden.

Das
Protokollierungsframework des
Client-SDK für {{site.data.keyword.mobileanalytics_short}}
unterstützt die folgenden Protokollebenen, die in aufsteigender
Reihenfolge der Ausführlichkeit mit den empfohlenen Verwendungsrichtlinien
aufgelistet sind:

  * `FATAL` - Verwenden Sie diese Ebene für nicht
behebbare Abstürze oder Blockierungen. Die Ebene `FATAL` ist
für die Protokollierung von nicht behebbaren Fehlern reserviert, die sich den
Benutzern als Absturz der Anwendung darstellen.
  * `ERROR` - Verwenden Sie diese Ebene für unerwartete
Ausnahmebedingungen oder unerwartete Netzprotokollfehler.
  * `WARN` - Verwenden Sie diese Ebene zur
Protokollierung von Nutzungswarnungen, die nicht als kritische Fehler gelten,
beispielsweise die Verwendung von veralteten APIs oder langsame Antworten aus
dem Netz.
  * `INFO` - Verwenden Sie diese Ebene für die Meldung
von Initialisierungsereignissen und anderen möglicherweise wichtigen, jedoch
nicht dringenden Daten.
  * `DEBUG` - Verwenden Sie diese Ebene zur Meldung von
Debuganweisungen, die Entwicklern bei der Behebung von Anwendungsfehlern helfen.

### Protokollebenenszenario
{: #log-level-scenario}

Wenn für die Protokollfunktion die Ebene `FATAL`
konfiguriert ist, erfasst die Protokollfunktion nicht abgefangene
Ausnahmebedingungen, jedoch keine Protokolle, die zum Absturzereignis geführt
haben. Sie können eine ausführlichere Ebene für die Protokollfunktion
festlegen, um sicherzustellen, dass auch diejenigen Protokolle erfasst werden,
die zu einem Eintrag des Typs `FATAL` bei der
Protokollfunktion geführt haben (z. B. `WARN` und
`ERROR`).

Wenn für die Protokollfunktion die Ebene `DEBUG`
festgelegt ist, erhalten Sie außerdem Protokolle zum Client-SDK für
Mobile Analytics, die beim Senden von Protokollen einbezogen werden.

### Beispiel für die Verwendung der Protokollfunktion
{: #sample-logger-usage}

**Hinweis:** Stellen Sie vor Verwendung des
Protokollierungsframeworks sicher, dass Ihre Anwendung für die Verwendung des
Client-SDK für {{site.data.keyword.mobileanalytics_short}} konfiguriert
wurde.

Die folgenden Code-Snippets zeigen ein Beispiel für die Verwendung der
Protokollfunktion:
```
// Configure Logger. Disabled by default;

Logger.isLogStorageEnabled = true

// Change the minimum log level (optional). Default is - LogLevel.debug

Logger.logLevelFilter = LogLevel.info

// Multiple log instances can be created to organize your logs

let logger1 = Logger.logger(name: "feature1Logger")
let logger2 = Logger.logger(name: "feature2Logger")

// Log messages with different levels

logger1.debug(message: "debug message for feature 1")

// logger1.debug message isn't logged as logLevelFilter is set to info

logger2.info(message: "info message for feature 2")

// Send logs to the Mobile Analytics Service

Logger.send(completionHandler: { (response: Response?, error: Error?) in
        if let response = response {
        logger.debug(message: "Status code: \(response.statusCode)")
        logger.debug(message: "Response: \(response.responseText)")
    }
    if let error = error {
        logger.error(message: "Error: \(error)")
    }
})
```
{: codeblock}

Sie können die Ausgabe der Protokollfunktion bei Bedenken hinsichtlich
des Datenschutzes für Anwendungen inaktivieren, die im Freigabemodus erstellt
sind. Standardmäßig gibt die Klasse der Protokollfunktion die Protokolle in der
Xcode-Konsole aus. Fügen Sie in den Buildeinstellungen für Ihr Ziel
das Flag `-D
RELEASE_BUILD` zum Abschnitt **Other Swift Flags**
der Buildkonfiguration für das Release hinzu.
{: tip}

## Schritt 8. Standortdaten protokollieren
{: #location-logging}

Der Standort des Mobilgeräts kann aus der App über die folgende
bereitgestellte API protokolliert werden:
```
Analytics.logLocation();
```

Die API versetzt die App in die Lage, ihren Standort (in Form von
Breiten- und Längengrad) zwischen App-Sitzungen an den Server zu senden. Anders
als explizite Aufrufe der
API `location-logging` sendet das SDK den Standort des Geräts
für jede App-Sitzung. Der Standort wird für den anfänglichen Kontext der
App-Sitzung sowie den Kontext der App-Sitzung bei einem Benutzerwechsel (also
dem Wechsel eines Benutzers zwischen App-Sitzungen) gesendet. Die Standort-API
muss bei der Initialisierung des SDK aktiviert worden sein.

Legen Sie zum Aufrufen der API `location-logging` den
Parameter
`collectLocation` in der SDK-Initialisierung mit
`true` fest.
```
Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
```

## Schritt 9. Netzanforderung ausgeben
{: #network-requests}

Sie können das Client-SDK von
{{site.data.keyword.mobileanalytics_short}} so konfigurieren, dass eine
[Netzanforderung](/docs/mobile/sdk_network_request.html)
ausgegeben wird. Achten Sie darauf, dass Sie
`BMSClient` und `BMSAnalytics` bereits
initialisiert sowie die Client-SDKs importiert haben.

## Schritt 10. Analysedaten für Absturz melden
{: #report-crash-analytics}

Sie können
[Daten über
Anwendungsabstürze](app-monitoring.html#monitor-app-crash) anzeigen, indem Sie Analysedaten und
Protokollinformationen an
{{site.data.keyword.mobileanalytics_short}} senden.

Die Methode `Analytics.send()` füllt die Tabellen
**Absturzübersicht** und **Abstürze**
auf der Seite **Abstürze**. Diagramme werden durch den
Initialisierungs- und Sendeprozess für die Analyse ermöglicht; eine spezielle
Konfiguration ist nicht erforderlich.

Die Methode `Logger.send()` füllt die Tabellen
**Absturzübersicht** und **Absturzdetails**
auf
der Seite **Fehlerbehebung**. Sie müssen das Speichern und
Senden von Protokollen für das Füllen der Diagramme durch Ihre Anwendung
ermöglichen, indem Sie eine Anweisung zu Ihrem Anwendungscode hinzufügen:
```
Logger.isLogStorageEnabled = true
Logger.logLevelFilter = LogLevel.Fatal // or greater
```
{: codeblock}

Weitere Informationen können Sie dem
[Beispiel für die Verwendung der
Protokollfunktion](sdk.html##sample-logger-usage) unter iOS entnehmen.

## Schritt 11. Aktive Benutzer verfolgen
{: #trackingusers}

Wenn Ihre Anwendung eindeutige Benutzer auf einem Gerät erkennen kann,
können
Sie verfolgen, wie viele Benutzer die Anwendung aktiv verwenden, indem Sie den
Benutzernamen des aktiven Benutzers an
{{site.data.keyword.mobileanalytics_short}} übergeben.

Aktivieren Sie die Benutzerüberwachung, indem Sie
{{site.data.keyword.mobileanalytics_short}} mit `hasUserContext = true` initialisieren. 
Andernfalls erfasst {{site.data.keyword.mobileanalytics_short}} nur einen Benutzer pro Einheit.

Fügen Sie den folgenden Code hinzu, um zu verfolgen, wann sich der
Benutzer anmeldet:
```
Analytics.userIdentity = "username"
```
{: codeblock}

## Schritt 12. App testen
{: #appid_testing}

Jetzt können Sie testen, ob Sie alles richtig konfiguriert haben.

1. Öffnen Sie Ihre App. Wenn es sich um eine Webanwendung handelt,
verwenden Sie einen Browser. Wenn es sich um eine iOS-Clientanwendung handelt,
nutzen Sie den Xcode-Emulator.
2. Kompilieren Sie die Anwendung und führen Sie sie im Emulator oder auf
dem Gerät aus.
3. Durchlaufen Sie den Prozess für die Anmeldung bei Ihrer Anwendung
mithilfe der GUI.
4. Rufen Sie die {{site.data.keyword.mobileanalytics_short}}-Konsole auf, um die Nutzungsanalyse für Ihre Anwendung anzuzeigen. Zur
Überwachung Ihrer Anwendung können Sie auch
[Alerts
festlegen](/docs/services/mobileanalytics/app-monitoring.html#alerts) und
[App-Abstürze
überwachen](/docs/services/mobileanalytics/app-monitoring.html#monitor-app-crash).

## Nächste Schritte
{: #what-to-do-next notoc}

 - Weitere Informationen zu diesem Service finden Sie in der [Dokumentation](/docs/services/mobileanalytics/index.html#getting-started-tutorial).

 - Eine Einführung in die Arbeit mit Mobile-Services und
{{site.data.keyword.Bluemix_notm}} finden Sie im Abschnitt
[Einführung für mobile Apps in IBM
Cloud](/docs/services/mobile/index.html).

 - Starter-Kits bieten eine der schnellsten Möglichkeiten zur Nutzung
des Leistungsspektrums von {{site.data.keyword.cloud_notm}}. Im
[Dashboard für Entwickler für Mobilgeräte](https://console.bluemix.net/developer/mobile/dashboard) werden alle verfügbaren Starter-Kits angezeigt. Sie können den Code herunterladen und die App ausführen.

 - Mithilfe der
[Swagger-Benutzerschnittstelle](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/)
können Sie die REST-API-Dokumentation schnell überprüfen.
