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

# Aggiunta di una chatbot
{: #assistant}

Puoi utilizzare il servizio {{site.data.keyword.conversationshort}} per creare applicazioni che comprendono l'input in linguaggio naturale e rispondono agli utenti con una conversazione simile a quella umana.

L'elenco seguente illustra il flusso della modalità di funzionamento dell'integrazione:

  1. Gli utenti interagiscono con l'interfaccia che è implementata nella tua applicazione.
  2. La tua applicazione invia l'input utente a {{site.data.keyword.conversationshort}} utilizzando l'SDK Swift {{site.data.keyword.watson}}.
  3. L'SDK Swift {{site.data.keyword.watson}} stabilisce una connessione a uno spazio di lavoro, che è un contenitore per il tuo flusso di dialogo e i tuoi dati di training.
  4. Lo spazio di lavoro interpreta l'input utente e indirizza il flusso della conversazione, inviando una risposta alla tua applicazione.
  5. La tua applicazione visualizza la risposta per l'utente.

## Prima di cominciare
{: #before-you-begin}

Assicurati di disporre dei seguenti prerequisiti.

  * Un'istanza del [servizio {{site.data.keyword.conversationshort}}](/docs/services/conversation/getting-started.html)
  * iOS 8.0+
  * Xcode 9.0+
  * Swift 3.2+ o Swift 4.0+
  * Carthage

Utilizza [Carthage](https://github.com/Carthage/Carthage){:new_window} per gestire le dipendenze e crea l'SDK Swift {{site.data.keyword.watson}} per la tua applicazione. Se non hai dimestichezza con Carthage, puoi installarlo con [Homebrew](http://brew.sh/){:new_window}:

```bash
$ brew update
$ brew install carthage
```

## Download e creazione di dipendenze
{: #download-and-build-dependencies}

1. Utilizzando il tuo editor di testo preferito, crea un nuovo file denominato `Cartfile` nella directory root del tuo progetto (dove si trova il tuo file `.xcodeproj`). 

2. Aggiungi una riga per specificare l'SDK Swift Watson come una dipendenza:
  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

  Per un'applicazione di produzione, puoi specificare un [requisito di versione](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement){:new_window} per evitare modifiche impreviste da nuove release dell'SDK Swift {{site.data.keyword.watson}}.
  {: tip}

3. Utilizza un terminale per passare alla directory root del progetto ed esegui quindi Carthage:
  ```bash
  carthage update --platform iOS
  ```
  {: codeblock}

  L'SDK Swift {{site.data.keyword.watson}} viene quindi scaricato e il suo framework viene creato nella cartella `Carthage/Build/iOS` del tuo progetto.

## Aggiunta di framework alla tua applicazione
{: #add-frameworks-to-your-app}

Ora che il framework dell'SDK Swift {{site.data.keyword.watson}} è stato creato, devi collegare e copiare il framework {{site.data.keyword.conversationshort}} nella tua applicazione.

1. Apri la tua applicazione in Xcode e seleziona il tuo progetto in Navigator per aprirne le impostazioni.
2. Seleziona la destinazione della tua applicazione e apri la scheda **General**.
3. Scorri alla sezione Linked Frameworks and Libraries e fai clic sull'icona `+`.
4. Nella nuova finestra che viene visualizzata, fai clic su **Add Other** e passa alla directory `Carthage/Build/iOS`.
5. Seleziona `AssistantV1.framework` per eseguirne il collegamento alla tua applicazione.

Oltre a collegare il framework {{site.data.keyword.conversationshort}}, devi anche copiarlo nella tua applicazione per renderlo accessibile al runtime. Uno script Carthage viene quindi utilizzato per evitare uno specifico [bug di inoltro all'App Store](http://www.openradar.me/radar?id=6409498411401216){:new_window}.

1. Con le impostazioni della destinazione della tua applicazione aperte in Xcode, passa alla scheda **Build Phases**.
2. Fai clic sull'icona `+` e seleziona **New Run Script Phase**.
3. Aggiungi il comando `/usr/local/bin/carthage copy-frameworks` alla fase di esecuzione dello script.
4. Aggiungi il framework {{site.data.keyword.conversationshort}} all'elenco **Input Files**:
  ```
  $(SRCROOT)/Carthage/Build/iOS/AssistantV1.framework
  ```
  {: codeblock}

## Aggiunta di un assistente virtuale alla tua applicazione
{: #add-a-virtual-assistant-to-your-app}

1. Apri il tuo `ViewController.swift` in Xcode.
2. Aggiungi un'istruzione di importazione per {{site.data.keyword.conversationshort}}, ad esempio `import AssistantV1`.
3. Crea una funzione vuota denominata `assistantExample` e quindi richiamala da `viewDidLoad`.
4. Aggiungi il seguente codice per la funzione `assistantExample`. Assicurati di aggiornare i tuoi nome utente, password e ID spazio di lavoro.

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

Quando esegui la tua applicazione, nella console vengono visualizzati i seguenti messaggi:
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hi. It looks like a nice drive today. What would you like me to do?
Request: turn the radio on
Response: Sure thing! Which genre would you prefer? Jazz is my personal favorite..
```
{: screen}

## Utilizzo dei kit starter
{: #conversation_starterkits}

Con i kit starter, puoi sfruttare rapidamente e facilmente le funzionalità di {{site.data.keyword.cloud_notm}}. Puoi aggiungere {{site.data.keyword.conversationshort}} a qualsiasi back-end lato server utilizzando i kit starter. Il kit starter Watson Chatbot for iOS illustra come utilizzare le funzionalità di apprendimento approfondito di {{site.data.keyword.conversationshort}} aggiungendo un'interfaccia di linguaggio naturale alla tua applicazione che automatizza le interazioni con i tuoi utenti finali.

1. Seleziona il [kit starter](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} con cui vuoi lavorare.
2. Crea il progetto con i servizi predefiniti.
3. Fai clic su **Add Resources > Watson > {{site.data.keyword.conversationshort}}**.
4. Scarica il progetto facendo clic su **Download Code**. Puoi trovare le credenziali del servizio nel file `config/local-dev.json`.

## Passi successivi
{: #assistant_next}

Ottimo lavoro. Hai aggiunto un assistente di intelligenza artificiale alla tua applicazione. Non fermarti ora e continua provando una delle seguenti opzioni:

* Controlla l'[SDK Swift {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.
* Avvaliti di tutte le funzioni che [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) ha da offrire.
* Visualizza il codice sorgente per l'[applicazione di esempio Simple Chat](https://github.com/watson-developer-cloud/simple-chat-swift){:new_window}, che illustra l'SDK Swift {{site.data.keyword.watson}} su GitHub.
