---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: coreml swift, core ml swift, watson swift core, create model swift, image classification swift, version parameter swift, swift coreml watson

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:gif: data-image-type='gif'}

# Core ML mit Watson nutzen
{: #swift-coreml}

Mit [Core ML](https://developer.apple.com/documentation/coreml){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") können Sie eine breite Palette von Modelltypen für das maschinelle Lernen in Ihre App integrieren. Es werden nicht nur
umfangreiche Deep-Learning-Modelle mit über 30 Schichttypen, sondern auch
Standardmodelle wie Baumstrukturensembles, SVMs und verallgemeinerte lineare
Modelle unterstützt. Statt Daten über Fernzugriff zur Analyse zu senden, nutzt Core ML Low-Level-Technologien wie Metal und Accelerate, um CPU und GPU ohne Reibungsverluste zu nutzen und eine größtmögliche Leistung und Effizienz zu ermöglichen.

Sie können Core ML und Watson Visual Recognition zu einer Swift-Anwendung hinzufügen, indem Sie die folgenden Schritte ausführen, um ein Modell zu trainieren und zu erstellen, Abhängigkeiten herunterzuladen und zu erstellen sowie eine Bildklassifizierung hinzuzufügen.
{: shortdesc}

## Vorbereitende Schritte
{: #prereqs-coreml}

Zur Verwendung von Core ML und Watson Visualization Visual Recognition mit Swift benötigen Sie die folgenden Komponenten:

* iOS 11.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage oder Swift Package Manager

Sie können das [Swift-SDK für Watson](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") installieren, indem Sie [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") oder den [Swift-Paketmanager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") verwenden. Bei Verwendung von CocoaPods für die Verwaltung von Abhängigkeiten erhalten Sie nur die erforderlichen Frameworks und nicht das gesamte Swift-SDK für Watson. Wenn Sie noch nicht mit CocoaPods gearbeitet haben, können Sie es problemlos installieren:

```console
sudo gem install cocoapods
```
{: codeblock}

## Schritt 1. Modell mit
{{site.data.keyword.watson}}
{{site.data.keyword.visualrecognitionshort}} trainieren
{: #train-module-coreml}

Falls kein aktuelles Modell vorhanden ist, wird das erste über Fernzugriff gefundene oder ein lokal vorhandenes Modell verwendet. Die folgende Abbildung zeigt Ihnen zusammen mit den zugehörigen Anweisungen, wie Sie den Service mit Watson Studio verknüpfen und Ihr Modell trainieren.

![Modelldurchlauf bei Core ML](images/CoreMLWalkthrough.gif)

### Service über das App-Dashboard von Core ML konfigurieren
{: #service-coreml}

1. Starten Sie das Visual Recognition-Tool aus dem Dashboard Ihres Starter-Kits, indem Sie die Option **Tool starten** auswählen.
2. Beginnen Sie mit der Erstellung des Modells, indem Sie **Modell erstellen** auswählen.
2. Falls der von Ihnen erstellten Visual Recognition-Instanz noch kein
Projekt zugeordnet ist, wird ein Projekt erstellt. Fahren Sie andernfalls
direkt mit dem Abschnitt **Modell erstellen** fort.
3. Benennen Sie Ihr Projekt und klicken Sie auf **Erstellen**.
  
  Wenn kein Speicher definiert ist, klicken Sie auf 'Aktualisieren'.
  {: tip}

### Service an ein Projekt binden
{: #bind-service-coreml}

1. Nachdem Sie Ihr Projekt erstellt haben, wird das Projektdashboard angezeigt.
2. Wechseln Sie auf die Registerkarte "Einstellungen", blättern Sie
vor bis zu **Zugehörige
Services** und wählen Sie **Service hinzufügen** >
**Watson** aus.
3. Wählen Sie auf der Seite "Watson-Services" den Service
**Visual
Recognition** aus.
4. Wählen Sie auf der Seite mit den Serviceinformationen die Registerkarte **Vorhanden** aus und wählen Sie dann Ihre Serviceinstanz im Dashboard aus.
5. Nachdem Sie Ihren Service gebunden haben, können Sie nun mit der
Erstellung Ihres Modells beginnen. Hierzu wählen Sie im Projektdashboard
die Option **Assets** aus und klicken Sie dann auf
**Visual Recognition-Modell hinzufügen**.

### Modell erstellen
{: #create-model-coreml}

1. Über das Tool zum Erstellen von Modellen können Sie den Namen des Klassifikationsmerkmals aktualisieren. Wenn Sie ein
bestimmtes Modell verwenden wollen, denken Sie daran,
das Feld `defaultClassifierID` im Hauptansichtencontroller zu
ändern.

2. Laden Sie die Modelltrainingskurse, die als ZIP-Dateien vorliegen, über die Seitenleiste hoch. Wählen Sie anschließend jeden Datenbestand aus und fügen Sie ihn mithilfe des Dropdown-Menüs zu Ihrem Modell hinzu. Sie können
ohne Weiteres zusätzliche Klassen hinzufügen, die Ihre eigenen Bildbestände
enthalten, um das Klassifikationsmerkmal zu erweitern.

![Klassen hinzufügen](images/add_classes.png "Service mit Watson Studio verknüpfen")

3. Wählen Sie **Modell trainieren** aus und warten
Sie, bis das Modell vollständig trainiert worden ist.

Hiermit ist alles vorbereitet. Nun können Sie Ihr Core ML-Modell herunterladen und es mithilfe des [Swift-SDKs für Watson Developer Cloud](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") in Ihre App integrieren.

## Schritt 2. Abhängigkeiten herunterladen und erstellen
{: #install-depend-coreml}

Erstellen Sie mit einem Texteditor Ihrer Wahl eine neue `Podfile` im Stammverzeichnis Ihres Projekts (in dem sich die Datei `.xcodeproj` befindet), indem Sie `pod init` ausführen. Fügen Sie anschließend eine Zeile hinzu, um das {{site.data.keyword.visualrecognitionshort}}-Framework des Swift-SDK für Watson als Abhängigkeit anzugeben:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

Für eine Produktions-App sollten Sie eine bestimmte [Versionsvoraussetzung](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") angeben, um unerwartete Änderungen aus neuen Releases des Swift-SDKs für Watson zu vermeiden.

Da die Datei `Podfile` jetzt vorhanden ist, können Sie die Abhängigkeiten herunterladen. Navigieren Sie mithilfe eines Terminals zum Stammverzeichnis Ihres Projekts und führen Sie dann CocoaPods aus:

```console
pod install
```
{: codeblock}

Mit Cocoapods wird das {{site.data.keyword.visualrecognitionshort}}-Framework heruntergeladen und im Ordner `Pods/` Ihres Projekts erstellt.

Zur Vermeidung von Pod-Erstellungsfehlern müssen Sie die Datei mit der Endung `.xcworkspace` und nicht mit der Endung `.xcodeproj` öffnen, wenn Sie das Projekt in Xcode öffnen.
{: tip}

## Schritt 3. App mit Bildklassifizierung ausstatten
{: #add-image-coreml}

Die folgenden Beispiele unterstützen Sie beim Hinzufügen von {{site.data.keyword.visualrecognitionshort}} Core ML-Funktionen zu Ihrer Anwendung, in der Regel in der Schnittstelle `ViewController.swift`. Mithilfe der folgenden Beispiele können Sie die Aufrufe des lokalen Modells für Ihren Anwendungsfall erweitern.

1. Fügen Sie eine Importanweisung für Visual Recognition hinzu:
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. Übergeben Sie den API-Schlüssel und die Version, um das SDK zu initialisieren:
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  Ziehen Sie die [Dokumentation zu Versionsparametern](https://{DomainName}/apidocs/visual-recognition#versioning){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") zu Rate oder verwenden Sie das Datum, an dem der Service {site.data.keyword.visualrecognitionshort}} erstellt wurde.
  {: tip}

3. Fügen Sie den folgenden Code hinzu, um das lokale Core ML-Modell mit Ihrem Watson-Klassifikationsmerkmal herunterzuladen oder zu aktualisieren:
  ```swift
  // Name of the classifier to use
  let classifierID = "your-classifier-ID-here"

  // Minimum confidence threshold for image recognition
        let threshold = 0.5

  // Update or download your model
  visualRecognition.updateLocalModel(classifierID: classifierID) { _, error in
      if let error = error {
          print(error)
      } else {
          print("model successfully updated")
      }                
  }
  ```
  {: codeblock}

4. Fügen Sie den folgenden Code hinzu, um ein Bild mithilfe Ihres lokalen Core ML-Modells zu klassifizieren:
  ```swift
  let image = UIImage(named: "your-image-filename")
  let classifierID = "your-classifier-ID-here"
  let threshold = 0.5

  // Classify your image using your model                                         
  visualRecognition.classifyWithLocalModel(image: image, classifierIDs: [classifierID], threshold: threshold) { classifiedImages, error in
      if let error = error {
          print(error)
    }
      guard let classifiedImages = classifiedImages? else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

5. Erkunden Sie die anderen [Core ML-Klassifizierungsfunktionen](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link"), die vom Watson-SDK unterstützt werden.

## Schritt 4. Starter-Kits verwenden
{: #coreml_starterkits}

Mithilfe von Starter-Kits können Sie das Leistungsspektrum von {{site.data.keyword.cloud_notm}} und Core ML unverzüglich und ohne großen Aufwand nutzen. Sie können den
{{site.data.keyword.visualrecognitionshort}}-Service mit den
Starter-Kits zu einer beliebigen iOS-Client-App oder einem beliebigen
serverseitigen Back-End-Programm
hinzufügen. Das Startet-Kit "Custom Vision Model for Core ML with
{{site.data.keyword.watson}}" veranschaulicht, wie Sie ein
angepasstes {{site.data.keyword.visualrecognitionshort}}-Modell
erstellen
und es als Core ML-Modell instanziieren, das lokal auf Ihrem Gerät ausgeführt
werden kann.

Gehen Sie zum Hinzufügen von
{{site.data.keyword.visualrecognitionshort}} zu einem Starter-Kit wie
folgt vor:

1. Wählen Sie das [Starter-Kit](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link"), mit dem Sie arbeiten möchten.
2. Erstellen Sie die App mit den Standardservices.
3. Klicken Sie auf **Service hinzufügen > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Laden Sie das Projekt herunter, indem Sie auf **Code herunterladen** klicken. Bei iOS-Projekten werden die
Berechtigungsnachweise in der Datei
`BMSCredentials.plist` in den entsprechenden Schlüsselfeldern
eingefügt. Bei serverseitigen Swift-Projekten finden Sie diese
Berechtigungsnachweise in der Datei `config/local-dev.json`.

## Nächste Schritte
{: #coreml_next}

Jetzt können Sie Bilder mithilfe von benutzerdefiniert generierten Core ML-Modellen analysieren. Nutzen Sie diesen Schwung und probieren
Sie gleich eine der folgenden Optionen aus:

* Sehen Sie sich das [Swift-SDK für {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") an und erkunden Sie weitere unterstützte Watson-Services.
* Fügen Sie Cloud-Logik hinzu. Beginnen Sie mit der [Entwicklung von serverunabhängigen Apps](/docs/swift/backend?topic=swift-serverless-dev-swift#serverless-dev-swift).
* Nutzen Sie alle Funktionen, die {{site.data.keyword.visualrecognitionshort}} bietet. Weitere
Details enthält die [Dokumentation](/docs/services/visual-recognition?topic=visual-recognition-index#index).
