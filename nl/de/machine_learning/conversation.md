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

# Chatbot hinzufügen
{: #assistant}

Mit dem {{site.data.keyword.conversationshort}}-Service können
Sie Anwendungen erstellen, die Eingabe in natürlicher Sprache verstehen und
Benutzern wie in einem normalen Gespräch antworten.

In der folgenden Liste ist der Ablauf für die Integration dargestellt:

  1. Benutzer interagieren mit der Schnittstelle, die in Ihrer App implementiert ist.
  2. Ihre App sendet die Benutzereingabe an den
{{site.data.keyword.conversationshort}}-Service und verwendet hierzu das Swift-SDK für {{site.data.keyword.watson}}.
  3. Das Swift-SDK für {{site.data.keyword.watson}} stellt eine
Verbindung zu einem Arbeitsbereich her, der als Container für Ihren
Dialogablauf und für die Trainingsdaten dient.
  4. Der Arbeitsbereich interpretiert die Benutzereingabe und führt den
Dialogablauf fort, indem er eine Antwort an Ihre App sendet.
  5. Ihre App zeigt die Antwort für den Benutzer an.

## Vorbereitende Schritte
{: #before-you-begin}

Stellen Sie sicher, dass die folgenden Voraussetzungen gegeben sind:

  * Instanz des
[{{site.data.keyword.conversationshort}}-Service](/docs/services/conversation/getting-started.html)
  * iOS 8.0+
  * Xcode 9.0+
  * Swift 3.2+ bzw. Swift 4.0+
  * Carthage

Mit
[Carthage](https://github.com/Carthage/Carthage){:new_window}
verwalten Sie Abhängigkeiten und erstellen Sie das Swift-SDK für
{{site.data.keyword.watson}} für Ihre Anwendung. Wenn Sie noch nicht
mit Carthage gearbeitet haben, können Sie es mit
[Homebrew](http://brew.sh/){:new_window} installieren:

```bash
$ brew update
$ brew install carthage
```

## Abhängigkeiten herunterladen und erstellen
{: #download-and-build-dependencies}

1. Erstellen Sie mit einem Texteditor Ihrer Wahl eine neue Datei namens
`Cartfile` im Stammverzeichnis Ihres Projekts (in dem sich die
Datei `.xcodeproj` befindet).

2. Fügen Sie eine Zeile hinzu, um das Swift-SDK für Watson als
Abhängigkeit anzugeben:
  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

  Für eine Produktions-App können Sie eine
[Versionsvoraussetzung](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement){:new_window}
angeben, um unerwartete Änderungen aus neuen Releases des Swift-SDK für
{{site.data.keyword.watson}} zu verhindern.
  {: tip}

3. Navigieren Sie mithilfe eines Terminals zum Stammverzeichnis Ihres
Projekts und führen Sie dann Carthage aus:
  ```bash
  carthage update --platform iOS
  ```
  {: codeblock}

  Das Swift-SDK für {{site.data.keyword.watson}} wird nun
heruntergeladen und sein Framework im Ordner
`Carthage/Build/iOS` Ihres Projekts erstellt.

## Frameworks zu einer App hinzufügen
{: #add-frameworks-to-your-app}

Nachdem nun das Framework des
Swift-SDK für {{site.data.keyword.watson}}
erstellt worden ist, müssen Sie das
{{site.data.keyword.conversationshort}}-Framework verknüpfen und in Ihre
App kopieren.

1. Öffnen Sie Ihre App in Xcode und wählen Sie Ihr Projekt im Navigator aus, um die Projekteinstellungen zu öffnen.
2. Wählen Sie Ihr App-Ziel aus und öffnen Sie die Registerkarte **General**.
3. Blättern Sie zum Abschnitt "Linked Frameworks and Libraries" vor und klicken Sie auf das Symbol `+`.
4. Klicken Sie in dem neu angezeigten Fenster auf **Add
Other** und navigieren Sie zum Verzeichnis
`Carthage/Build/iOS`.
5. Wählen Sie `AssistantV1.framework` aus, um das
Framework mit Ihrer App zu verknüpfen.

Zusätzliche zum Verknüpfen des
{{site.data.keyword.conversationshort}}-Frameworks müssen Sie das
Framework auch in die App kopieren, damit es zur Laufzeit zugänglich ist. Dann
wird ein Carthage-Script verwendet, um einen speziellen
[Fehler bei der
Übergabe an den App Store](http://www.openradar.me/radar?id=6409498411401216){:new_window} zu verhindern.

1. Navigieren Sie, während die Einstellungen Ihres App-Ziels in Xcode
geöffnet sind, zur Registerkarte **Build Phases**.
2. Klicken Sie auf das Symbol `+` und wählen Sie **New Run
Script Phase** aus.
3. Fügen Sie den Befehl `/usr/local/bin/carthage
copy-frameworks` zur Scriptausführungsphase (Run Script Phase) hinzu.
4. Fügen Sie das
{{site.data.keyword.conversationshort}}-Framework
zur Liste **Input Files** hinzu:
  ```
  $(SRCROOT)/Carthage/Build/iOS/AssistantV1.framework
  ```
  {: codeblock}

## App mit virtuellem Assistenten ausstatten
{: #add-a-virtual-assistant-to-your-app}

1. Öffnen Sie `ViewController.swift` in Xcode.
2. Fügen Sie eine Importanweisung für {{site.data.keyword.conversationshort}} hinzu, z. B. `import AssistantV1`.
3. Erstellen Sie eine leere Funktion namens
`assistantExample` und rufen Sie diese Funktion aus `viewDidLoad` heraus auf.
4. Fügen Sie den folgenden Code für die Funktion `assistantExample` hinzu. Achten
Sie darauf, Ihren Benutzernamen, Ihr Kennwort und Ihre Arbeitsbereichs-ID zu
aktualisieren.

```swift
//
//  ViewController.swift
//  WatsonAssistantExample
//
//  Created by Glenn R. Fisher on 3/1/18.
//  Copyright © 2018 Glenn R. Fisher. All rights reserved.
//

import UIKit
import AssistantV1

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        assistantExample()
    }

    func assistantExample() {
        // Assistant credentials
        let username = "your-username-here"
        let password = "your-password-here"
        let workspace = "your-workspace-id-here"

        // instantiate service
        let assistant = Assistant(username: username, password: password, version: "2018-03-01")

        // start a conversation
        assistant.message(workspaceID: workspace) { response in
            print("Conversation ID: \(response.context.conversationID!)")
            print("Response: \(response.output.text.joined())")

            // continue assistant
            print("Request: turn the radio on")
            let input = InputData(text: "turn the radio on")
            let request = MessageRequest(input: input, context: response.context)
            assistant.message(workspaceID: workspace, request: request) { response in
                print("Response: \(response.output.text.joined())")
            }
        }
    }
}
```
{: codeblock}

Wenn Sie Ihre App ausführen, werden die folgenden Nachrichten in der
Konsole angezeigt:
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hi. It looks like a nice drive today. What would you like me to do?
Request: turn the radio on
Response: Sure thing! Which genre would you prefer? Jazz is my personal favorite..
```
{: screen}

## Starter-Kits verwenden
{: #conversation_starterkits}

Mithilfe von Starter-Kits können Sie das Leistungsspektrum von
{{site.data.keyword.cloud_notm}} unverzüglich und ohne großen Aufwand
nutzen. Sie können den {{site.data.keyword.conversationshort}}-Service
mit den Starter-Kits zu einem beliebigen serverseitigen Back-End-Programm
hinzufügen. Das Starter-Kit "Chatbot for iOS with Watson" veranschaulicht, wie
die Deep-Learning-Funktionalität von
{{site.data.keyword.conversationshort}} genutzt wird, indem zu Ihrer
Anwendung eine Schnittstelle in natürlicher Sprache hinzugefügt wird, die
Interaktionen mit den Benutzern automatisiert.

1. Wählen Sie das [Starter-Kit](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} aus, mit dem Sie arbeiten möchten.
2. Erstellen Sie das Projekt mit den Standardservices.
3. Klicken Sie auf ** Ressourcen hinzufügen > Watson > {{site.data.keyword.conversationshort}}**.
4. Laden Sie das Projekt herunter, indem Sie der
Projektseite auf **Code
herunterladen** klicken. Die Serviceberechtigungsnachweise finden Sie
in der Datei `config/local-dev.json`.

## Nächste Schritte
{: #assistant_next}

Hervorragend! Sie haben einen KI-Assistenten zu Ihrer App hinzugefügt. 
Nutzen Sie diesen Schwung und probieren Sie gleich eine der folgenden
Optionen aus:

* Informieren Sie sich über das
[Swift-SDK
für {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.
* Nutzen Sie alle Funktionen, die
[{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) zu bieten hat.
* Zeigen Sie den Quellcode der
[Beispiel-App
für einen einfachen Chat](https://github.com/watson-developer-cloud/simple-chat-swift){:new_window} an, die das Swift-SDK für
{{site.data.keyword.watson}} unter GitHub demonstriert.
