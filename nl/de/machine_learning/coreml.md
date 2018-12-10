---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# App mit maschinellem Lernen ausstatten
{: #overview}

Mit [Core ML](https://developer.apple.com/documentation/coreml){:new_window} können Sie eine breite Palette von Modelltypen für das maschinelle Lernen in Ihre App integrieren. Es werden nicht nur
umfangreiche Deep-Learning-Modelle mit über 30 Schichttypen, sondern auch
Standardmodelle wie Baumstrukturensembles, SVMs und verallgemeinerte lineare
Modelle unterstützt. Statt Daten über Fernzugriff zur Analyse zu senden,
nutzt Core ML
maschinennahe Technologien wie Metal und Accelerate, um CPU und GPU ohne
Reibungsverluste zu nutzen und eine größtmögliche Leistung und Effizienz zu
ermöglichen.

## Vorbereitende Schritte
{: #before-you-begin}

Zur Verwendung von Core ML und Watson Visualization benötigen Sie die
folgenden Komponenten:

  * iOS 11.0+
  * Xcode 9.0+
  * Swift 4.0+
  * Carthage

Verwenden Sie zur Installation von Carthage die folgenden Befehle
`brew`:
```
$ brew update
$ brew install carthage
```
{: codeblock}

## Schritt 1. Modell mit
{{site.data.keyword.watson}}
{{site.data.keyword.visualrecognitionshort}} trainieren
{: #training-your-model}

Falls kein aktuelles Modell vorhanden ist, wird das erste über
Fernzugriff gefundene
oder ein lokal vorhandenes Modell verwendet. Die folgende Abbildung zeigt Ihnen
zusammen mit den zugehörigen Anweisungen, wie Sie den Service mit Watson Studio
verknüpfen und Ihr Modell trainieren.

![Modelldurchlauf bei Core ML](images/CoreMLWalkthrough.gif)

### Service über das App-Dashboard von Core ML konfigurieren

1. Starten Sie das Visual Recognition-Tool aus dem  Dashboard Ihres
Starter-Kits, indem Sie die Option **Tool starten**
auswählen.
2. Beginnen Sie mit der Erstellung des Modells, indem Sie **Modell erstellen** auswählen.
2. Falls der von Ihnen erstellten Visual Recognition-Instanz noch kein
Projekt zugeordnet ist, wird ein Projekt erstellt. Fahren Sie andernfalls
direkt mit dem Abschnitt **Modell erstellen** fort.
3. Benennen Sie Ihr Projekt und klicken Sie auf **Erstellen**.
  **Tipp**: Wenn kein Speicher definiert ist, klicken Sie
auf "Aktualisieren".

### Service an ein Projekt binden

1. Nachdem Sie Ihr Projekt erstellt haben, wird das Projektdashboard angezeigt.
2. Wechseln Sie auf die Registerkarte "Einstellungen", blättern Sie
vor bis zu **Zugehörige
Services** und wählen Sie **Service hinzufügen** >
**Watson** aus.
3. Wählen Sie auf der Seite "Watson-Services" den Service
**Visual
Recognition** aus.
4. Wählen Sie auf der Seite mit den
Serviceinformationen die Registerkarte **Vorhanden**
aus und wählen Sie dann Ihre Serviceinstanz im Dashboard aus.
5. Nachdem Sie Ihren Service gebunden haben, können Sie nun mit der
Erstellung Ihres Modells beginnen. Hierzu wählen Sie im Projektdashboard
die Option **Assets** aus und klicken Sie dann auf
**Visual Recognition-Modell hinzufügen**.

### Modell erstellen

1. Ändern Sie im Modellerstellungstool bei Bedarf den Namen des
Klassifikationsmerkmals. Standardmäßig verwendet die Anwendung das erste
verfügbare Merkmal, sonfern nichts anderes angegeben wird. Wenn Sie ein
bestimmtes Modell verwenden wollen, denken Sie daran,
das Feld `defaultClassifierID` im Hauptansichtencontroller zu
ändern.

2. Laden Sie die Modelltrainingskurse, die als ZIP-Dateien vorliegen,
über die Seitenleiste hoch. Wählen Sie anschließend jeden Datenbestand aus und
fügen Sie ihn mithilfe des Dropdown-Menüs zu Ihrem Modell hinzu. Sie können
ohne Weiteres zusätzliche Klassen hinzufügen, die Ihre eigenen Bildbestände
enthalten, um das Klassifikationsmerkmal zu erweitern.

![Klassen hinzufügen](images/add_classes.png)

3. Wählen Sie **Modell trainieren** aus und warten
Sie, bis das Modell vollständig trainiert worden ist.

Hiermit ist alles vorbereitet. Nun können Sie Ihr Core ML-Modell
herunterladen und es mithilfe des [Swift-SDK für {{site.data.keyword.watson}} Developer Cloud](https://github.com/watson-developer-cloud/swift-sdk){:new_window} in Ihre App integrieren.

## Schritt 2. Abhängigkeiten herunterladen und erstellen
{: #installing-dependencies}

1. Erstellen Sie mit einem Texteditor Ihrer Wahl eine neue Datei namens
**Cartfile** im Stammverzeichnis Ihres Projekts, in dem sich
die Datei `.xcodeproj` befindet.

2. Fügen Sie eine Zeile hinzu, um das Swift-SDK für {{site.data.keyword.watson}} Developer Cloud als
Abhängigkeit anzugeben. Beispiel:

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: pre}

  Für eine Produktions-App können Sie eine spezielle Versionsvoraussetzung angeben, um unerwartete Änderungen aus neuen Releases des SDK zu verhindern.
  {: tip}

3. Navigieren Sie mithilfe eines Terminals zum Stammverzeichnis Ihres
Projekts und führen Sie Carthage aus:

  ```
  $ carthage update --platform iOS
  ```
  {: pre}

  Das Swift-SDK für {{site.data.keyword.watson}} Developer Cloud wird
nun heruntergeladen und sein Framework im Ordner
`Carthage/Build/iOS` Ihres Projekts erstellt.

## Schritt 3. App mit Imageklassifizierung ausstatten
{: #adding-image-classification}

```Swift
//
//  ViewController.swift
//  CoreMLImageClassificationExample
//
//  Created by Aaron Liberatore on 3/1/18.
//  Copyright © 2018 Aaron Liberatore. All rights reserved.
//

import UIKit
import VisualRecognitionV3

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        initializeCoreMLExample()
    }

    func failureHandler(_ error: Error) {
        print(error)
    }

    func initializeCoreMLExample() {
        // Configuration

        // Your Watson Visual Recognition service api key
        let apiKey = "your-apiKey-here"

        // Name of the classifier to use
        let classifierID = "your-classifier-here"

        // Minimum confidence threshold for image recognition
        let threshold = 0.5

        // Initialize Service
        let visualRecognition = VisualRecognition(apiKey: apiKey, version: "03-01-2018")

        // Update or download your model
        visualRecognition.updateLocalModel(classifierID: classifierID,
                                           failure: failureHandler) {

            // Classify your image using your model                                         
            visualRecognition.classifyWithLocalModel(image: image,
                                                     classifierIDs: [classifierID],
                                                     threshold: threshold,
                                                     failure: self.failureHandler) { classifiedImages in

                 print(classifiedImages)
             }            
        }
    }
}
```
{: codeblock}

## Schritt 4. Starter-Kits verwenden
{: #coreml_starterkits}

Mithilfe von Starter-Kits können Sie das Leistungsspektrum von
{{site.data.keyword.cloud_notm}} und Core ML unverzüglich und ohne großen
Aufwand
nutzen. Sie können den
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

1. Wählen Sie das
[Starter-Kit](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window}
aus, mit dem Sie arbeiten möchten.
2. Erstellen Sie das Projekt mit den Standardservices.
3. Klicken Sie auf ** Ressourcen hinzufügen > Watson >
{{site.data.keyword.visualrecognitionshort}}**.
4. Laden Sie das Projekt herunter, indem Sie der Projektseite auf
**Code herunterladen** klicken. Bei iOS-Projekten werden die
Berechtigungsnachweise in der Datei
`BMSCredentials.plist` in den entsprechenden Schlüsselfeldern
eingefügt. Bei serverseitigen Swift-Projekten finden Sie diese
Berechtigungsnachweise in der Datei `config/local-dev.json`.

## Nächste Schritte
{: #coreml_next}

Jetzt können Sie Bilder mithilfe von benutzerdefiniert generierten Core
ML-Modellen analysieren. Nutzen Sie diesen Schwung und probieren Sie
gleich eine der folgenden Optionen aus:

* Fügen Sie Cloud-Logik hinzu. Beginnen Sie mit der [Entwicklung von serverunabhängigen Apps](/docs/swift/backend/functions.html).
* Nutzen Sie alle Funktionen, die
{{site.data.keyword.visualrecognitionshort}} zu bieten hat. Weitere
Details enthält die [Dokumentation](/docs/services/visual-recognition/index.html).
