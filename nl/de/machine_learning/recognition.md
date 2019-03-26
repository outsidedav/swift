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

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

Der {{site.data.keyword.visualrecognitionfull}}-Service
ermöglicht es Ihrer App, visuellen Inhalt durch maschinelles Lernen schnell und
präzise zu kennzeichnen, zu klassifizieren und zu trainieren. Mit dem Service
können Sie praktisch jeden visuellen Inhalt klassifizieren, ein eigenes
angepasstes Modell in wenigen Minuten trainieren und Gesichter erkennen.

## Funktionsweise
{: #how-it-works-recognition}

1. Ihre App wählt eine Gruppe von Bildern für die Analyse aus.
2. Ihre App sendet das Bild mithilfe des Swift-SDK für Watson an den {{site.data.keyword.visualrecognitionshort}}-Service.
3. Der Service analysiert das Bild unter Verwendung der
Klassifikationsanalyse, damit Szenen, Objekte, Gesichter u. a. erkannt
werden.
4. Die Analyse des Service wird durch das Swift-SDK für Watson an Ihre
App zurückgegeben.

## Vorbereitende Schritte
{: #prereqs-recognition}

Stellen Sie sicher, dass die folgenden Voraussetzungen verfügbar sind:

* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage oder Swift Package Manager

Sie können das [Swift-SDK für Watson](https://github.com/watson-developer-cloud/swift-sdk) mithilfe von [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage) oder [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager) installieren. Bei Verwendung von CocoaPods (https://cocoapods.org/) zur Verwaltung von Abhängigkeiten erhalten Sie nur die von Ihnen benötigten Frameworks und nicht das gesamte Swift-SDK für Watson. Wenn Sie noch nicht mit CocoaPods gearbeitet haben, können Sie es problemlos installieren:

```console
sudo gem install cocoapods
```
{: codeblock}

## Schritt 1. Visual Recognition-Instanz erstellen
{: #create-instance-recognition}

Stellen Sie eine Instanz des
{{site.data.keyword.visualrecognitionshort}}-Service bereit:

1. Wählen Sie im {{site.data.keyword.cloud_notm}}-Katalog den
Eintrag für
**{{site.data.keyword.visualrecognitionshort}}**
aus. Die Anzeige für die Servicekonfiguration wird geöffnet.
2. Vergeben Sie für die Serviceinstanz einen Namen oder verwenden Sie den voreingestellten Namen.
3. Wählen Sie im Menü **Verbinden** eine App aus,
falls Sie Ihre Instanz an eine App binden möchten.
4. Wählen Sie einen Preistarif aus und klicken Sie auf
**Erstellen**.
5. Wählen Sie die Registerkarte
**Berechtigungsnachweise** aus, um Ihre
Serviceberechtigungsnachweise anzuzeigen. Diese Werte werden verwendet, um von
der App eine Verbindung zum Service herzustellen.

## Schritt 2. Abhängigkeiten herunterladen und erstellen
{: #download-depend-recognition}

Erstellen Sie mit einem Texteditor Ihrer Wahl eine neue `Podfile` im Stammverzeichnis Ihres Projekts (in dem sich die Datei `.xcodeproj` befindet), indem Sie `pod init` ausführen. Fügen Sie anschließend eine Zeile hinzu, um das {{site.data.keyword.visualrecognitionshort}}-Framework des Swift-SDK für Watson als Abhängigkeit anzugeben:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

Für eine Produktions-App können Sie eine spezielle
[Versionsvoraussetzung](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions)
angeben, um unerwartete Änderungen aus neuen Releases des Swift-SDK für Watson
zu verhindern.

Da die Datei `Podfile` jetzt vorhanden ist, können Sie die Abhängigkeiten herunterladen. Navigieren Sie mithilfe eines Terminals zum Stammverzeichnis Ihres Projekts und führen Sie dann CocoaPods aus:

```console
pod install
```
{: codeblock}

Mit Cocoapods wird das {{site.data.keyword.visualrecognitionshort}}-Framework heruntergeladen und im Ordner `Pods/` Ihres Projekts erstellt.

Zur Vermeidung von Pod-Erstellungsfehlern müssen Sie die Datei mit der Endung `.xcworkspace` und nicht mit der Endung `.xcodeproj` öffnen, wenn Sie das Projekt in Xcode öffnen.
{: tip}

## Schritt 3. Bilder in Ihrer App analysieren
{: #analyze-images-recognition}

Die folgenden Beispiele unterstützen Sie beim Hinzufügen von {{site.data.keyword.visualrecognitionshort}}-Funktionen zu Ihrer Anwendung, in der Regel in der Schnittstelle `ViewController.swift`. Mithilfe der folgenden Beispiele können Sie die Visual Recognition-Aufrufe für Ihren Anwendungsfall erweitern.

1. Fügen Sie eine Importanweisung für Visual Recognition hinzu:
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. Übergeben Sie den API-Schlüssel und die Version (Sie können das
heutige Datum verwenden), um das SDK zu initialisieren:
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  Sie können die [Versionsparameterdokumentation](https://cloud.ibm.com/apidocs/visual-recognition#versioning) zurate ziehen oder das das Datum verwenden, an dem der {site.data.keyword.conversationshort}}-Service erstellt wurde.
  {: tip}

3. Fügen Sie den folgenden Code hinzu, um ein Bild zu klassifizieren:
  ```swift
  let url = "your-image-url"
  visualRecognition.classify(url: url) { response, error in
      if let error = error {
          print(error)
    }
      guard let classifiedImages = response?.result else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

Es gibt mehrere Klassifizierungsmethoden, die vom Visual Recognition-Framework unterstützt werden. Ziehen Sie die [Visual Recognition-Dokumentation](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html) des Watson-SDK zurate, um herauszufinden, welche am besten zu Ihrer Anwendung passt.
{: tip}

## Starter-Kits verwenden
{: #recognition_starterkits}

[Starter-Kits](https://cloud.ibm.com/developer/appledevelopment/starter-kits) bieten eine der schnellsten Möglichkeiten zur Nutzung des Leistungsspektrums von {{site.data.keyword.cloud_notm}}. Den
{{site.data.keyword.visualrecognitionshort}}-Service können Sie
verwenden, indem Sie das Starter-Kit
**Visual Recognition for iOS with Watson** auswählen. Dieser
Service wertet Ihre Bilder aus und klassifiziert sie. Wenn Sie neue oder
vorhandene Bilder von Ihrem Mobilgerät aus hochladen, beginnt die Visual
Recognition-App umgehend mit der Kennzeichnung und Klassifizierung des
Bildinhalts.

So beginnen Sie:
1. Wählen Sie das
[hier](https://cloud.ibm.com/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson)
verfügbare Starter-Kit aus.
2. Erstellen Sie das Projekt mit den Standardservices.
3. Laden Sie das Projekt herunter, indem Sie auf
**Code herunterladen** klicken. Die
Serviceberechtigungsnachweise werden in der Datei
`BMSCredentials.plist` in den entsprechenden Schlüsselfeldern
eingefügt.

## Nächste Schritte
{: #recognition_next}

Hervorragend! Visual Recognition ist jetzt für Ihre App verfügbar. Nutzen Sie diesen Schwung und probieren
Sie gleich eine der folgenden Optionen aus:
* Sehen Sie sich das [Swift-SDK für Watson](https://github.com/watson-developer-cloud/swift-sdk){:new_window} an und erkunden Sie weitere unterstützte Watson-Services.
* Lesen Sie weitere Informationen zu
[IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/).
