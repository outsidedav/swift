---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: chatbot swift, virtual assistant swift, assistant swift, watson swift starter, assistantv2 swift, watson sdk swift, add chatbot swift, add assistant swift

subcollection: swift

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
{: #prereqs-chatbot}

Stellen Sie sicher, dass die folgenden Voraussetzungen verfügbar sind:

* Instanz des
[{{site.data.keyword.conversationshort}}-Service](/docs/services/assistant?topic=assistant-getting-started#getting-started)
* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage oder Swift Package Manager

Sie können das [Swift-SDK für Watson](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") installieren, indem Sie [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") oder den [Swift-Paketmanager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") verwenden. Bei Verwendung von CocoaPods für die Verwaltung von Abhängigkeiten erhalten Sie nur die erforderlichen Frameworks und nicht das gesamte Swift-SDK für Watson. Wenn Sie noch nicht mit CocoaPods gearbeitet haben, können Sie es problemlos installieren:

```console
sudo gem install cocoapods
```
{: codeblock}

## Schritt 1. Instanz von Watson Assistant erstellen
{: #instance-watson-chatbot}

Stellen Sie eine Instanz des
{{site.data.keyword.conversationshort}}-Service bereit:

1. Wählen Sie im {{site.data.keyword.cloud_notm}}-Katalog den
Eintrag für **{{site.data.keyword.conversationshort}}** aus. Die Anzeige für die Servicekonfiguration wird geöffnet.
2. Vergeben Sie für die Serviceinstanz einen Namen oder verwenden Sie den voreingestellten Namen.
3. Wählen Sie im Menü **Verbinden** eine App aus,
falls Sie Ihre Instanz an eine App binden möchten.
4. Wählen Sie einen Preistarif aus und klicken Sie auf
**Erstellen**.
5. Wählen Sie die Registerkarte
**Berechtigungsnachweise** aus, um Ihre
Serviceberechtigungsnachweise anzuzeigen. Diese Werte werden verwendet, um von
der App eine Verbindung zum Service herzustellen.
6. Klicken Sie auf **Tool starten**, um Ihren Chatbot-Assistenten zu erstellen. Befolgen Sie die Anweisungen auf der Landing-Page, um einen eigenen Chatbot zu erstellen, oder wechseln Sie zur Registerkarte **Skills** und wählen Sie **Neue erstellen** aus. Wählen Sie auf der Seite **Dialogskill hinzufügen** die Registerkarte **Beispielskill verwenden** aus und wählen Sie die Option **Beispielskill für Kundenbetreuung** aus, um rasch und unkompliziert mit der Arbeit starten zu können. Sie können diesen Skill weiter verfeinern oder andere erstellen, die später von Ihrer App verwendet werden können.
7. Klicken Sie auf das Menü für Ihren neuen Skill und wählen Sie **API-Details anzeigen** aus. Sie können jetzt die `Arbeitsbereichs-ID` sehen, die Sie zusätzlich zu den Serviceberechtigungsnachweisen benötigen.

## Schritt 2. Abhängigkeiten herunterladen und erstellen
{: #download-depend-chatbot}

Im folgenden Beispiel wird AssistantV1 verwendet. Weitere Informationen zum Framework AssistantV2 finden Sie in der Dokumentation zum [SDK für Watson 'AssistantV2'](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV2/index.html){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link").

Erstellen Sie mit einem Texteditor Ihrer Wahl eine neue `Podfile` im Stammverzeichnis Ihres Projekts (in dem sich die Datei `.xcodeproj` befindet), indem Sie `pod init` ausführen. Fügen Sie anschließend eine Zeile hinzu, um das {{site.data.keyword.conversationshort}}-Framework des Swift-SDK für Watson als Abhängigkeit anzugeben:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonAssistantV1'
```
{: codeblock}

Für eine Produktions-App sollten Sie eine bestimmte [Versionsvoraussetzung](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") angeben, um unerwartete Änderungen aus neuen Releases des Swift-SDKs für Watson zu vermeiden.

Da die Datei `Podfile` jetzt vorhanden ist, können Sie die Abhängigkeiten herunterladen. Navigieren Sie mithilfe eines Terminals zum Stammverzeichnis Ihres Projekts und führen Sie dann CocoaPods aus:

```console
pod install
```
{: codeblock}

Mit Cocoapods wird das {{site.data.keyword.conversationshort}}-Framework heruntergeladen und im Ordner `Pods/` Ihres Projekts erstellt.

Zur Vermeidung von Pod-Erstellungsfehlern müssen Sie die Datei mit der Endung `.xcworkspace` und nicht mit der Endung `.xcodeproj` öffnen, wenn Sie das Projekt in Xcode öffnen.
{: tip}

## Schritt 3. Virtuellen Assistenten zu einer App hinzufügen
{: #virtual-assist-chatbot}

Die folgenden Beispiele unterstützen Sie beim Hinzufügen von {{site.data.keyword.conversationshort}}-Funktionen zu Ihrer Anwendung, in der Regel in der Schnittstelle `ViewController.swift`. Mithilfe der folgenden Beispiele können Sie die Aufrufe des Assistenten für Ihren Anwendungsfall erweitern.

1. Fügen Sie eine Importanweisung für {{site.data.keyword.conversationshort}} hinzu, z. B. `import Assistant`.
  ```swift
  import Assistant
  ```
  {: codeblock}

2. Instanziieren Sie den {{site.data.keyword.conversationshort}}-Service:
  ```swift
  let assistant = Assistant(version: "yyyy-mm-dd", apikey: "your-api-key-here")

  // save context to state to continue the conversation later
  var context: Context?
  ```
  {: codeblock}

  **Tipp**: Bei diesem Beispiel wird der Kontext für eine spätere Fortsetzung des Dialogs gespeichert. Weitere Informationen zu diesem Objekt und zu seiner Anpassung an den Anwendungsfall finden Sie in der [Dokumentation zur Kontextvariablen](/docs/services/assistant?topic=assistant-dialog-runtime#context-variables). Ziehen Sie die [Dokumentation zu Versionsparametern](https://{DomainName}/apidocs/assistant#versioning){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") zu Rate oder verwenden Sie das Datum, an dem der Service {site.data.keyword.conversationshort}} erstellt wurde.

3. Initialisieren Sie den Dialog. Abhängig davon, wie der Assistent konfiguriert ist, kann er eine erste Antwort an den Benutzer bereitstellen:
  ```swift
  // Start a conversation
  assistant.message(workspaceID: "your-workspace-ID-here) { response, error in
      if let error = error {
         print(error)
    }

      guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Set the context to state
      self.context = message.context    
  }
  ```
  {: codeblock}

4. Senden Sie eine Nachricht und den Kontext an den Assistenten:
  ```swift
  print("Request: When are you open?")
  let input = InputData(text: "When are you open?")

  assistant.message(workspaceID: workspace, input: input, context: self.context) { response, error in
      if let error = error {
         print(error)
    }

      guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Update the context
      self.context = message.context
  }
  ```
  {: codeblock}

Wenn Sie Ihre App mit diesen Beispielen mit dem Standardassistenten ausführen, werden die folgenden Nachrichten in der Konsole angezeigt:
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hello, I’m a demo customer care virtual assistant to show you the basics.  I can help with directions to my store, hours of operation and booking an in-store appointment.
Request: When are you open?
Response: Our hours are Monday to Friday 10am to 8pm and Friday and Saturday 11am to 6pm.
```
{: screen}

5. Ziehen Sie für Informationen zur Funktionalität Ihrer Anwendung die [Assistant-Dokumentation](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV1/index.html){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") des Watson-SDKs zu Rate.

## Starter-Kits verwenden
{: #starterkits-chatbot}

Mithilfe von Starter-Kits können Sie das Leistungsspektrum von {{site.data.keyword.cloud_notm}} unverzüglich und ohne großen Aufwand nutzen. Sie können den {{site.data.keyword.conversationshort}}-Service
mit den Starter-Kits zu einem beliebigen serverseitigen Back-End-Programm
hinzufügen. Das Starter-Kit "Chatbot for iOS with Watson" veranschaulicht, wie die Deep-Learning-Funktionalität von {{site.data.keyword.conversationshort}} genutzt wird, indem zu Ihrer Anwendung eine Schnittstelle in natürlicher Sprache hinzugefügt wird, die Interaktionen mit den Benutzern automatisiert.

1. Wählen Sie das [Starter-Kit](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link"), mit dem Sie arbeiten möchten.
2. Erstellen Sie die App mit den Standardservices.
3. Klicken Sie auf **Service hinzufügen > Watson > {{site.data.keyword.conversationshort}}**.
4. Laden Sie das Projekt herunter, indem Sie auf **Code herunterladen** klicken. Die Serviceberechtigungsnachweise finden Sie
in der Datei `config/local-dev.json`.

## Nächste Schritte
{: #next-chatbot notoc}

Hervorragend! Sie haben einen KI-Assistenten zu Ihrer App hinzugefügt. Nutzen Sie diesen Schwung und probieren
Sie gleich eine der folgenden Optionen aus:

* Sehen Sie sich das [Swift-SDK für {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") an und erkunden Sie weitere unterstützte Watson-Services.
* Nutzen Sie alle Funktionen, die [{{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-index#index) bietet.
* Zeigen Sie den Quellcode der [Beispielapp 'Simple Chat'](https://github.com/watson-developer-cloud/simple-chat-swift){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") an, die das Swift-SDK für {{site.data.keyword.watson}} unter GitHub veranschaulicht.
