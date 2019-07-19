---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: watson swift, tone anaylzer swift, cocoapods swift, swift sdk install, starter kit watson

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

Der {{site.data.keyword.ibmwatson}}-Service {{site.data.keyword.toneanalyzershort}} ermöglicht es Ihrer App, Emotionen und Tonfälle in Texten zu verstehen. Dieser Service ermöglicht es
Ihnen, die Dialoge mit Ihren Benutzern besser zu verstehen, und versetzt die
Benutzer in die Lage, die Wahrnehmung ihrer schriftlichen Kommunikation durch
den Kommunikationspartner besser einzuschätzen.

## Funktionsweise
{: #how-it-works-tone}

1. Ihre App wählt einen zu analysierenden Text aus (z. B. eine
Textnachricht oder einen Twitter-Feed).
2. Ihre App sendet den Text mithilfe des Swift-SDK für Watson an den
{{site.data.keyword.toneanalyzershort}}-Service.
3. Der Service analysiert den Text mithilfe der linguistischen Analyse,
um Emotionen und Tonfälle zu erkennen.
4. Die Analyse des Service wird durch das [Swift-SDK für Watson](https://github.com/watson-developer-cloud/swift-sdk) an Ihre App zurückgegeben.

## Vorbereitende Schritte
{: #prereqs-tone}

Stellen Sie sicher, dass die folgenden Voraussetzungen verfügbar sind:

* OS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage oder Swift Package Manager

Sie können das [Swift-SDK für Watson](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") installieren, indem Sie [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") oder den [Swift-Paketmanager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") verwenden. Bei Verwendung von CocoaPods für die Verwaltung von Abhängigkeiten erhalten Sie nur die erforderlichen Frameworks und nicht das gesamte Swift-SDK für Watson. Wenn Sie noch nicht mit CocoaPods gearbeitet haben, können Sie es problemlos installieren:

```bash
$ sudo gem install cocoapods
```
{: codeblock}

## Schritt 1. Tone Analyzer-Instanz erstellen
{: #create-instance-tone}

Stellen Sie eine Instanz des
{{site.data.keyword.toneanalyzershort}}-Service bereit:

1. Wählen Sie im {{site.data.keyword.cloud_notm}}-Katalog den
Eintrag für {{site.data.keyword.toneanalyzershort}} aus. Die Anzeige für die Servicekonfiguration wird geöffnet.
2. Vergeben Sie für die Serviceinstanz einen Namen oder verwenden Sie den voreingestellten Namen.
3. Wählen Sie im Menü "Verbinden" eine
App aus,
falls Sie Ihre Instanz an eine App binden möchten.
4. Wählen Sie einen Preistarif aus und klicken Sie auf
**Erstellen**.
5. Wählen Sie die Registerkarte
**Berechtigungsnachweise** aus, um Ihre
Serviceberechtigungsnachweise anzuzeigen. Diese Werte werden verwendet, um von
der App eine Verbindung zum Service herzustellen.

## Schritt 2. Abhängigkeiten herunterladen und erstellen
{: #download-depend-tone}

Erstellen Sie mit einem Texteditor Ihrer Wahl eine neue `Podfile` im Stammverzeichnis Ihres Projekts (in dem sich die Datei `.xcodeproj` befindet), indem Sie `pod init` ausführen. Fügen Sie anschließend eine Zeile hinzu, um das {{site.data.keyword.conversationshort}}-Framework des Swift-SDK für Watson als Abhängigkeit anzugeben:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonToneAnalyzerV3'
```
{: codeblock}

Für eine Produktions-App sollten Sie eine bestimmte [Versionsvoraussetzung](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") angeben, um unerwartete Änderungen aus neuen Releases des Swift-SDKs für Watson zu vermeiden.

Da die Datei `Podfile` jetzt vorhanden ist, können Sie die Abhängigkeiten herunterladen. Navigieren Sie mithilfe eines Terminals zum Stammverzeichnis Ihres Projekts und führen Sie dann CocoaPods aus:

```console
pod install
```
{: codeblock}

Mit Cocoapods wird das {{site.data.keyword.toneanalyzershort}}-Framework heruntergeladen und im Ordner `Pods/` Ihres Projekts erstellt.

Zur Vermeidung von Pod-Erstellungsfehlern müssen Sie die Datei mit der Endung `.xcworkspace` und nicht mit der Endung `.xcodeproj` öffnen, wenn Sie das Projekt in Xcode öffnen.
{: tip}

## Schritt 3. Text in Ihrer App analysieren
{: #analyze-text-tone}

Die folgenden Beispiele unterstützen Sie beim Hinzufügen von {{site.data.keyword.toneanalyzershort}}-Funktionen zu Ihrer Anwendung, in der Regel in der Schnittstelle `ViewController.swift`. Mithilfe der folgenden Beispiele können Sie die Tone Analyzer-Aufrufe für Ihren Anwendungsfall erweitern.

1. Fügen Sie eine Importanweisung für Tone Analyzer hinzu:
  ```swift
  import ToneAnalyzer
  ```
  {: codeblock}

2. Instanziieren Sie den Tone Analyzer-Service:
  ```swift
  let toneAnalyzer = ToneAnalyzer(version: "yyyy-mm-dd", apiKey: "your-api-key-here")
  ```
  {: codeblock}

  Ziehen Sie die [Dokumentation zu Versionsparametern](https://{DomainName}/apidocs/tone-analyzer#versioning){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") zu Rate oder verwenden Sie das Datum, an dem der Service {site.data.keyword.conversationshort}} erstellt wurde.
  {: tip}

3. Geben Sie Text für die Analyse an und verarbeiten Sie die Ergebnisse:
  ```swift
  // Text to analyze
  let review = """
      I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
  """

  // Analyze text
  let input = ToneInput(text: review)
  toneAnalyzer.tone(toneInput: .toneInput(input)) { response, error in
      if let error = error {
          print(error)
          return
      }

      guard let tones = response?.result?.documentTone.tones else {
          print("Failed to analyze the tone input")
          return
      }

      for tone in tones {
          print("\(tone.toneName): \(tone.score)")
              }
  }
  ```
  {: codeblock}

  Wenn Sie den Code ausführen, wird die folgende Analyse in der Konsole angezeigt:
  ```
  Sadness: 0.575803
Tentative: 0.867377
  ```
  {: screen}

4. Ziehen Sie für Informationen zur Funktionalität Ihrer Anwendung die [Dokumentation zu Tone Analyzer](https://watson-developer-cloud.github.io/swift-sdk/services/ToneAnalyzerV3/index.html){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") des Watson-SDKs zu Rate.

## Starter-Kits verwenden
{: #tone_starterkits}

[Starter-Kits](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") bieten eine der schnellsten Möglichkeiten zur Nutzung des Leistungsspektrums von {{site.data.keyword.cloud_notm}}. Den
{{site.data.keyword.toneanalyzershort}}-Service können Sie verwenden,
indem Sie das Starter-Kit **Tone Analyzer for iOS with
Watson** auswählen. Dieser Service nutzt Deep-Learning-Funktionen, um
Textpassagen auszuwerten. Die Anwendung "Tone Analyzer" erkennt anhand einer
Reihe von Kategorien den Tonfall des Sprechers (glücklich, traurig,
vertrauensvoll und mehr).

So beginnen Sie die Verwendung dieses Starter-Kits:

1. Wählen Sie das Starter-Kit aus, das sich [hier](https://{DomainName}/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") befindet.
2. Erstellen Sie das Projekt mit den Standardservices.
3. Laden Sie das Projekt herunter, indem Sie auf
**Code herunterladen** klicken. Die
Serviceberechtigungsnachweise werden in der Datei
`BMSCredentials.plist` in den entsprechenden Schlüsselfeldern
eingefügt.

## Nächste Schritte
{: #tone_next notoc}

Hervorragend! Sie haben {{site.data.keyword.toneanalyzershort}} zu Ihrer App hinzugefügt. Nutzen Sie diesen Schwung und probieren
Sie gleich eine der folgenden Optionen aus:

* Sehen Sie sich das [Swift-SDK für Watson unter GitHub](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") an und erkunden Sie weitere unterstützte Watson-Services.
* Weitere Informationen finden Sie in [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link").
