---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: chatbot swift, virtual assistant swift, assistant swift, watson swift starter, assistantv2 swift, watson sdk swift, add chatbot swift, add assistant swift

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Aggiunta di una chatbot
{: #assistant}

Puoi utilizzare il servizio {{site.data.keyword.conversationshort}} per creare applicazioni che comprendono l'input in linguaggio naturale e rispondono agli utenti con una conversazione simile a quella umana.

L'elenco seguente illustra il flusso della modalità di funzionamento dell'integrazione:

1. Gli utenti interagiscono con l'interfaccia che è implementata nella tua applicazione.
2. La tua applicazione invia l'input utente a {{site.data.keyword.conversationshort}} utilizzando l'SDK Swift {{site.data.keyword.watson}}.
3. L'SDK Swift {{site.data.keyword.watson}} stabilisce una connessione a uno spazio di lavoro, che è un contenitore per il tuo flusso di dialogo e i tuoi dati di formazione.
4. Lo spazio di lavoro interpreta l'input utente e indirizza il flusso della conversazione, inviando una risposta alla tua applicazione.
5. La tua applicazione visualizza la risposta per l'utente.

## Prima di cominciare
{: #prereqs-chatbot}

Assicurati di disporre dei seguenti prerequisiti.

* Un'istanza del [servizio {{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-getting-started#getting-started)
* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage o Swift Package Manager

Puoi installare l'[SDK Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") utilizzando [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") o [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"). Utilizzando CocoaPods per gestire le dipendenze, ottieni solo i framework di cui hai bisogno, invece dell'intero SDK Swift Watson. Se non hai dimestichezza con CocoaPods, puoi installarlo facilmente:

```console
sudo gem install cocoapods
```
{: codeblock}

## Passo 1. Creazione di un'istanza di Watson Assistant
{: #instance-watson-chatbot}

Esegui il provisioning di un'istanza del servizio {{site.data.keyword.conversationshort}}:

1. Nel catalogo {{site.data.keyword.cloud_notm}}, seleziona **{{site.data.keyword.conversationshort}}**. Viene visualizzata la schermata di configurazione del servizio.
2. Assegna un nome alla tua istanza del servizio oppure utilizza il nome preimpostato.
3. Seleziona un'applicazione dal menu **Connect** se vuoi eseguire il bind della tua istanza a un'applicazione.
4. Seleziona un piano di prezzi e fai clic su **Create**.
5. Seleziona la scheda **Credentials** per visualizzare le tue credenziali del servizio. Questi valori vengono utilizzati per connettersi al servizio dalla tua applicazione.
6. Fai clic su **Launch tool** per creare il tuo assistente chatbot. Attieniti alle istruzioni nella pagina di destinazione per creare il tuo chatbot oppure vai alla scheda **Skills** e seleziona **Create new**. Nella pagina **Add Dialog Skill**, scegli la scheda **Use sample skill** e seleziona **Customer Care Sample Skill** per iniziare a lavorare rapidamente. Puoi continuare a perfezionare questa capacità o crearne di nuove che possono essere utilizzate dalla tua applicazione in un secondo momento.
7. Fai clic sul menu sulla tua nuova capacità e seleziona **View API Details**. Puoi ora visualizzare l'ID dello spazio di lavoro (`Workspace ID`) di cui hai bisogno in aggiunta alle credenziali del servizio.

## Passo 2. Download e creazione di dipendenze
{: #download-depend-chatbot}

Il seguente esempio utilizza AssistantV1. Per ulteriori informazioni sul framework AssistantV2, vedi la [documentazione di AssistantV2 dell'SDK Watson](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV2/index.html){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").

Utilizzando il tuo editor di testo preferito, crea un nuovo `Podfile` nella directory root del tuo progetto (dove si trova il tuo file `.xcodeproj`) eseguendo `pod init`. Aggiungi quindi una riga per specificare il framework {{site.data.keyword.conversationshort}} dell'SDK Swift Watson come una dipendenza:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonAssistantV1'
```
{: codeblock}

Per un'applicazione di produzione, potresti voler specificare anche un determinato [requisito della versione](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") per evitare modifiche impreviste da nuove release dell'SDK Swift Watson.

Con il tuo `Podfile` debitamente collocato, puoi ora scaricare le dipendenze. Utilizza un terminale per passare alla directory root del progetto ed esegui quindi CocoaPods:

```console
pod install
```
{: codeblock}

Cocoapods scarica il framework {{site.data.keyword.conversationshort}} e lo crea nella cartella `Pods/` del tuo progetto.

Per evitare errori di creazione del pod, apri il file che termina con `.xcworkspace` invece di `.xcodeproj` quando apri il progetto in Xcode.
{: tip}

## Passo 3. Aggiunta di un'assistente virtuale alla tua applicazione
{: #virtual-assist-chatbot}

I seguenti esempi ti aiutano ad aggiungere le funzionalità di {{site.data.keyword.conversationshort}} alla tua applicazione, di norma in `ViewController.swift`. Utilizzando i seguenti esempi, puoi estendere le chiamate Assistant per il tuo caso d'uso.

1. Aggiungi un'istruzione di importazione per {{site.data.keyword.conversationshort}}, ad esempio `import Assistant`.
  ```swift
  import Assistant
  ```
  {: codeblock}

2. Istanzia il servizio {{site.data.keyword.conversationshort}}:
  ```swift
  let assistant = Assistant(version: "yyyy-mm-dd", apikey: "your-api-key-here")

  // save context to state to continue the conversation later
  var context: Context?
  ```
  {: codeblock}

  **Suggerimento**: questo esempio salva il contesto nello stato in cui si trova. Per una migliore comprensione di questo oggetto e di come adattarlo per il tuo caso d'uso, vedi la [documentazione delle variabili di contesto](/docs/services/assistant?topic=assistant-dialog-runtime#context-variables). Consulta la [documentazione dei parametri di versione](https://cloud.ibm.com/apidocs/assistant#versioning){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") oppure usa la data in cui è stato creato il servizio {site.data.keyword.conversationshort}}.

3. Inizializza la conversazione. A seconda di come è configurato il tuo assistente, può fornire una risposta iniziale all'utente:
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

4. Invia un messaggio e il contesto all'assistente:
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

Quando esegui la tua applicazione con questi esempi con l'assistente predefinito, nella console vengono visualizzati i seguenti messaggi:
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hello, I’m a demo customer care virtual assistant to show you the basics.  I can help with directions to my store, hours of operation and booking an in-store appointment.
Request: When are you open?
Response: Our hours are Monday to Friday 10am to 8pm and Friday and Saturday 11am to 6pm.
```
{: screen}

5. Esplora la [documentazione di Assistant](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV1/index.html){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") di SDK Watson per creare la funzionalità della tua applicazione.

## Utilizzo dei kit starter
{: #starterkits-chatbot}

Con i kit starter, puoi velocemente e facilmente utilizzare le funzionalità di {{site.data.keyword.cloud_notm}}. Puoi aggiungere {{site.data.keyword.conversationshort}} a qualsiasi back-end lato server utilizzando i kit starter. Il kit starter Watson Chatbot for iOS illustra come utilizzare le funzionalità di apprendimento approfondito di {{site.data.keyword.conversationshort}} aggiungendo un'interfaccia di linguaggio naturale alla tua applicazione che automatizza le interazioni con i tuoi utenti.

1. Seleziona il [kit starter](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") con cui vuoi lavorare.
2. Crea l'applicazione con i servizi predefiniti.
3. Fai clic su **Add service > Watson > {{site.data.keyword.conversationshort}}**.
4. Scarica il progetto facendo clic su **Download code**. Puoi trovare le credenziali del servizio nel file `config/local-dev.json`.

## Passi successivi
{: #next-chatbot notoc}

Ottimo lavoro. Hai aggiunto un assistente di intelligenza artificiale alla tua applicazione. Non fermarti ora e continua provando una delle seguenti opzioni:

* Controlla l'[SDK Swift {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") ed esplora gli altri servizi Watson supportati.
* Avvaliti di tutte le funzioni che [{{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-index#index) ha da offrire.
* Visualizza il codice sorgente per l'[applicazione di esempio Simple Chat](https://github.com/watson-developer-cloud/simple-chat-swift){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"), che illustra l'SDK Swift {{site.data.keyword.watson}} su GitHub.
