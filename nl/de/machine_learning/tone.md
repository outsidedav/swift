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

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

Der IBM Watson Tone Analyzer-Service versetzt Ihre App in die Lage,
Emotionen und Tonfälle in Texten zu verstehen. Dieser Service ermöglicht es
Ihnen, die Dialoge mit Ihren Benutzern besser zu verstehen, und versetzt die
Benutzer in die Lage, die Wahrnehmung ihrer schriftlichen Kommunikation durch
den Kommunikationspartner besser einzuschätzen.

## Funktionsweise
{: ##how-it-works}

1. Ihre App wählt einen zu analysierenden Text aus (z. B. eine
Textnachricht oder einen Twitter-Feed).
2. Ihre App sendet den Text mithilfe des Swift-SDK für Watson an den
{{site.data.keyword.toneanalyzershort}}-Service.
3. Der Service analysiert den Text mithilfe der linguistischen Analyse,
um Emotionen und Tonfälle zu erkennen.
4. Die Analyse des Service wird durch das Swift-SDK für Watson an Ihre App zurückgegeben.

## Vorbereitende Schritte
{: ###before-you-begin}

Stellen Sie zunächst sicher, dass die folgenden Voraussetzungen gegeben
sind:
<ul>
  <li>iOS 8.0+</li>
  <li>Xcode 9.0+</li>
  <li>Swift 3.2+ bzw. Swift 4.0+</li>
  <li>Carthage</li>
</ul>

Es empfiehlt sich,
[Carthage](https://github.com/Carthage/Carthage) zur
Verwaltung von Abhängigkeiten und zum Build des Swift-SDK für Watson für Ihre
Anwendung zu verwenden. Wenn Sie noch nicht mit Carthage gearbeitet haben,
können Sie es mit [Homebrew](http://brew.sh/) installieren:

```bash
$ brew update
$ brew install carthage
```
{: codeblock}

## Schritt 1. Tone Analyzer-Instanz erstellen
{: #create-and-configure-an-instance-of-tone-analyzer}

Stellen Sie eine Instanz des
{{site.data.keyword.toneanalyzershort}}-Service bereit:

1. Wählen Sie im {{site.data.keyword.cloud_notm}}-Katalog den
Eintrag für {{site.data.keyword.toneanalyzershort}} aus. Die Anzeige
für die Servicekonfiguration wird geöffnet.
2. Vergeben Sie für die Serviceinstanz einen Namen oder verwenden Sie
den voreingestellten Namen.
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
{: ###download-and-build-dependencies}

Erstellen Sie mit einem Texteditor Ihrer Wahl eine neue Datei namens
`Cartfile` im Stammverzeichnis Ihres Projekts (in dem sich die
Datei `.xcodeproj` befindet). Fügen Sie anschließend eine
Zeile hinzu, um das Swift-SDK für Watson als Abhängigkeit anzugeben:

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

Für eine Produktions-App können Sie eine spezielle
[Versionsvoraussetzung](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement)
angeben, um unerwartete Änderungen aus neuen Releases des Swift-SDK für Watson
zu verhindern.

Da die Datei `Cartfile` jetzt vorhanden ist, können Sie
die Abhängigkeiten herunterladen und erstellen. Navigieren Sie mithilfe eines
Terminals zum Stammverzeichnis Ihres Projekts und führen Sie dann Carthage aus:
  
  ```bash
  $ carthage update --platform iOS
  ```
  {: codeblock}

Carthage lädt das Swift-SDK für Watson herunter und erstellt dessen
Frameworks im Ordner `Carthage/Build/iOS` Ihres Projekts.

## Schritt 3. Frameworks zu einer App hinzufügen
{: ###add-frameworks-to-your-app}

### Schritte zum Verknüpfen von Tone Analyzer

Nachdem die Frameworks des Swift-SDK für Watson durch Carthage erstellt
worden sind, müssen Sie jetzt das Tone Analyzer-Framework mit Ihrer App
verknüpfen.

1. Öffnen Sie Ihre App in Xcode und wählen Sie Ihr Projekt aus, um die
Projekteinstellungen zu öffnen.
2. Wählen Sie Ihr App-Ziel aus und öffnen Sie dann die Registerkarte
**General**.
3. Blättern Sie zum Abschnitt "Linked Frameworks and Libraries" vor und
klicken Sie auf das Symbol `+`.
4. Wählen Sie in dem aufgerufenen Fenster **Add
Other** aus und navigieren Sie zum Verzeichnis
`Carthage/Build/iOS`. Wählen Sie
**ToneAnalyzerV3.framework** aus, um das Framework mit Ihrer
App zu verknüpfen.

### Schritte zum Kopieren von Tone Analyzer

Zusätzliche zum _Verknüpfen_ des Tone Analyzer-Frameworks
müssen Sie das Framework auch in die App _kopieren_, damit es zur
Laufzeit zugänglich ist. In Xcode gibt es mehrere unterschiedliche
Möglichkeiten, ein Framework zu kopieren oder zu integrieren. Sie können jedoch
ein Carthage-Script verwenden, um einen speziellen
[Fehler bei der
Übergabe im App Store](http://www.openradar.me/radar?id=6409498411401216) zu vermeiden.

1. Navigieren Sie, während die Einstellungen Ihres App-Ziels in Xcode
geöffnet sind, zur Registerkarte **Build Phases**.
2. Klicken Sie auf das Symbol `+` und wählen Sie
**New Run Script Phase** aus.
3. Fügen Sie den Befehl `/usr/local/bin/carthage
copy-frameworks` zur Scriptausführungsphase (Run Script Phase) hinzu.
4. Fügen Sie das Tone Analyzer-Framework zur Liste "Input
Files" hinzu:
`$(SRCROOT)/Carthage/Build/iOS/ToneAnalyzerV3.framework`.

Jetzt können Sie in Ihrer App mit dem Swift-SDK für Watson arbeiten.

## Schritt 4. Text in Ihrer App analysieren
{: #analyze-text-in-your-app}

1. Öffnen Sie die Datei `ViewController.swift` in Xcode.
2. Fügen Sie eine Importanweisung für Tone Analyzer hinzu:
    ```swift
    import ToneAnalyzerV3
    ```
    {: codeblock}

3. Erstellen Sie eine leere Funktion namens
`toneAnalyzerExample` und rufen Sie diese Funktion aus
`viewDidLoad` heraus auf.
4. Fügen Sie den folgenden Code zur Funktion
`toneAnalyzerExample` hinzu. Denken Sie daran, den
Benutzernamen und das Kennwort für Ihren Service zu aktualisieren.
  ```swift
  import UIKit
  import ToneAnalyzerV3

  class ViewController: UIViewController {

      override func viewDidLoad() {
          super.viewDidLoad()
          toneAnalyzerExample()
      }

      func toneAnalyzerExample() {

          // instantiate service
          let toneAnalyzer = ToneAnalyzer(
              username: "your-username-here",
              password: "your-password-here",
              version: "yyyy-mm-dd"
          )

          // text to analyze
          let review = """
              I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
          """

          // analyze text
          let toneInput = ToneInput(text: review)
          let failure = { (error: Error) in print(error) }
          toneAnalyzer.tone(toneInput: toneInput, contentType: "application/json", failure: failure) { analysis in
              for tone in analysis.documentTone.tones! {
                  print("\(tone.toneName): \(tone.score)")
              }
          }
      }
  }
  ```
  {: codeblock}

Wenn Sie Ihre App ausführen, wird die folgende Analyse in der
Konsole angezeigt:
```
Sadness: 0.575803
Tentative: 0.867377
```
{: screen}

## Starter-Kits verwenden
{: #tone_starterkits}


[Starter-Kits](https://console.bluemix.net/developer/appledevelopment/starter-kits)
bieten eine der schnellsten Möglichkeiten zur Nutzung des Leistungsspektrums
von {{site.data.keyword.cloud_notm}}. Den
{{site.data.keyword.toneanalyzershort}}-Service können Sie verwenden,
indem Sie das Starter-Kit **Tone Analyzer for iOS with
Watson** auswählen. Dieser Service nutzt Deep-Learning-Funktionen, um
Textpassagen auszuwerten. Die Anwendung "Tone Analyzer" erkennt anhand einer
Reihe von Kategorien den Tonfall des Sprechers (glücklich, traurig,
vertrauensvoll und mehr).

So beginnen Sie die Verwendung dieses Starter-Kits:

1. Wählen Sie das
[hier](https://console.bluemix.net/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson)
verfügbare Starter-Kit aus.
2. Erstellen Sie das Projekt mit den Standardservices.
3. Laden Sie das Projekt herunter, indem Sie der Projektseite auf
**Code herunterladen** klicken. Die
Serviceberechtigungsnachweise werden in der Datei
`BMSCredentials.plist` in den entsprechenden Schlüsselfeldern
eingefügt.

## Nächste Schritte
{: #tone_next}

Hervorragend! Sie haben {{site.data.keyword.toneanalyzershort}}
zu Ihrer App hinzugefügt. Nutzen Sie diesen Schwung und probieren Sie
gleich eine der folgenden Optionen aus:

* Sehen Sie sich das [Swift-SDK für Watson bei GitHub](https://github.com/watson-developer-cloud/swift-sdk) an.
* Lesen Sie weitere Informationen zu [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/).

