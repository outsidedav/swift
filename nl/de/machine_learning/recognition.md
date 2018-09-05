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

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

Der {{site.data.keyword.visualrecognitionfull}}-Service
ermöglicht es Ihrer App, visuellen Inhalt durch maschinelles Lernen schnell und
präzise zu kennzeichnen, zu klassifizieren und zu trainieren. Mit dem Service
können Sie praktisch jeden visuellen Inhalt klassifizieren, ein eigenes
angepasstes Modell in wenigen Minuten trainieren und Gesichter erkennen.

## Funktionsweise
{: ##how-it-works}

1. Ihre App wählt eine Gruppe von Bildern für die Analyse aus.
2. Ihre App sendet das Bild mithilfe des Swift-SDK für Watson an den {{site.data.keyword.visualrecognitionshort}}-Service.
3. Der Service analysiert das Bild unter Verwendung der
Klassifikationsanalyse, damit Szenen, Objekte, Gesichter u. a. erkannt
werden.
4. Die Analyse des Service wird durch das Swift-SDK für Watson an Ihre
App zurückgegeben.

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

## Schritt 1. Visual Recognition-Instanz erstellen
{: ###create-and-configure-an-instance-of-visual-recognition}

Stellen Sie eine Instanz des
{{site.data.keyword.visualrecognitionshort}}-Service bereit:

1. Wählen Sie im {{site.data.keyword.cloud_notm}}-Katalog den
Eintrag für
**{{site.data.keyword.visualrecognitionshort}}**
aus. Die Anzeige für die Servicekonfiguration wird geöffnet.
2. Vergeben Sie für die Serviceinstanz einen Namen oder verwenden Sie
den voreingestellten Namen.
3. Wählen Sie im Menü **Verbinden** eine App aus,
falls Sie Ihre Instanz an eine App binden möchten.
4. Wählen Sie einen Preistarif aus und klicken Sie auf
**Erstellen**.
5. Wählen Sie die Registerkarte
**Berechtigungsnachweise**
aus, um Ihre Serviceberechtigungsnachweise anzuzeigen. Diese Werte werden
verwendet, um von der App eine Verbindung zum Service herzustellen.

## Schritt 2. Abhängigkeiten herunterladen und erstellen
{: ###download-and-build-dependencies}

Erstellen Sie mit einem Texteditor Ihrer Wahl eine Datei namens
`Cartfile` im Stammverzeichnis Ihres Projekts (in dem sich die
Datei `.xcodeproj` befindet). Fügen Sie anschließend eine Zeile
hinzu, um
das Swift-SDK für Watson als Abhängigkeit anzugeben:
```
github "watson-developer-cloud/swift-sdk"
```
{: pre}

Für eine Produktions-App können Sie eine spezielle
[Versionsvoraussetzung](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement)
angeben, um unerwartete Änderungen aus neuen Releases des Swift-SDK für
Watson zu verhindern.

Da die Datei `Cartfile` jetzt vorhanden ist, können Sie
die Abhängigkeiten herunterladen und erstellen. Navigieren Sie mithilfe eines
Terminals zum Stammverzeichnis Ihres Projekts und führen Sie dann Carthage aus:

```bash
carthage update --platform iOS
```
{: pre}

Carthage lädt das Swift-SDK für Watson herunter und erstellt dessen
Frameworks im Ordner `Carthage/Build/iOS` Ihres Projekts.

## Schritt 3. Frameworks zu einer App hinzufügen
{: ###add-frameworks-to-your-app}

### Schritte zum Verknüpfen von Visual Recognition:

Nachdem die Frameworks des Swift-SDK für Watson durch Carthage erstellt
worden sind, müssen Sie jetzt das Visual Recognition-Framework mit Ihrer App
verknüpfen.

1. Öffnen Sie Ihre App in Xcode und wählen Sie Ihr Projekt aus, um die Projekteinstellungen zu öffnen.
2. Wählen Sie Ihr App-Ziel aus und öffnen Sie dann die Registerkarte
**General**.
3. Blättern Sie zum Abschnitt "Linked Frameworks and Libraries" vor und
klicken Sie auf das Symbol `+`.
4. Wählen Sie in dem aufgerufenen Fenster **Add
Other** aus und navigieren Sie zum Verzeichnis
`Carthage/Build/iOS`. Wählen Sie
**VisualRecognitionV3.framework** aus, um das Framework mit
Ihrer App zu verknüpfen.

### Schritte zum Kopieren von Visual Recognition:

Zusätzliche zum _Verknüpfen_ des
Visual Recognition-Frameworks müssen Sie das
Framework auch in die App _kopieren_, damit es zur Laufzeit zugänglich
ist. In Xcode gibt es mehrere unterschiedliche Möglichkeiten, ein Framework
zu kopieren oder zu integrieren. Sie können jedoch ein Carthage-Script
verwenden, um einen speziellen
[Fehler bei der
Übergabe im App Store](http://www.openradar.me/radar?id=6409498411401216) zu vermeiden.

1. Navigieren Sie, während die Einstellungen Ihres App-Ziels in Xcode
geöffnet sind, zur Registerkarte **Build Phases**.
2. Klicken Sie auf das Symbol `+` und wählen Sie
**New Run Script Phase** aus.
3. Fügen Sie den Befehl `/usr/local/bin/carthage
copy-frameworks` zur Scriptausführungsphase (Run Script Phase) hinzu.
4. Fügen Sie das Visual Recognition-Framework zur Liste **Input
Files** hinzu:
`$(SRCROOT)/Carthage/Build/iOS/VisualRecognitionV3.framework`.

Jetzt können Sie in Ihrer App mit dem Swift-SDK für Watson arbeiten.

## Schritt 4. Bilder in Ihrer App analysieren
{: #analyze-images-in-your-app}

1. Öffnen Sie die Datei `ViewController.swift` in Xcode.

1. Fügen Sie eine Importanweisung für Visual Recognition hinzu:
    ```swift
    import VisualRecognitionV3
    ```
    {: pre}

1. Übergeben Sie den API-Schlüssel und die Version (Sie können das
heutige Datum verwenden), um das SDK zu initialisieren:
```swift
    let visualRecognition = VisualRecognition(apiKey: "your-api-key", version: "yyyy-mm-dd")
    ```
    {: pre}

1. Fügen Sie den folgenden Code hinzu, um ein Bild zu klassifizieren:
```swift
    let url = "your-image-url"
    let failure = { (error: Error) in print(error) }
    visualRecognition.classify(url: url, failure: failure) { classifiedImages in
        print(classifiedImages)
    }
    ```
    {: pre}

## Starter-Kits verwenden
{: #recognition_starterkits}

[Starter-Kits](https://console.bluemix.net/developer/appledevelopment/starter-kits)
bieten eine der schnellsten Möglichkeiten zur Nutzung des
Leistungsspektrums von {{site.data.keyword.cloud_notm}}. Den
{{site.data.keyword.visualrecognitionshort}}-Service können Sie
verwenden, indem Sie das Starter-Kit
**Visual Recognition for iOS with Watson** auswählen. Dieser
Service wertet Ihre Bilder aus und klassifiziert sie. Wenn Sie neue oder
vorhandene Bilder von Ihrem Mobilgerät aus hochladen, beginnt die Visual
Recognition-App umgehend mit der Kennzeichnung und Klassifizierung des
Bildinhalts.

So beginnen Sie:
1. Wählen Sie das
[hier](https://console.bluemix.net/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson)
verfügbare Starter-Kit aus.
2. Erstellen Sie das Projekt mit den Standardservices.
3. Laden Sie das Projekt herunter, indem Sie der Projektseite auf
**Code herunterladen** klicken. Die
Serviceberechtigungsnachweise werden in der Datei
`BMSCredentials.plist` in den entsprechenden Schlüsselfeldern
eingefügt.

## Nächste Schritte
{: #recognition_next}

Hervorragend! Visual Recognition ist jetzt für Ihre App verfügbar. Nutzen Sie diesen Schwung und probieren Sie gleich eine der folgenden Optionen
aus:
* Sehen Sie sich das
[Swift-SDK für
Watson bei GitHub](https://github.com/watson-developer-cloud/swift-sdk) an.
* Lesen Sie weitere Informationen zu
[IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/).

