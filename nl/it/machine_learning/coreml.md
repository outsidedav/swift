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

# Aggiunta del machine learning a un'applicazione
{: #overview}

Con [Core ML](https://developer.apple.com/documentation/coreml){:new_window}, puoi integrare un'ampia gamma di tipi di modello di machine learning nella tua applicazione. Oltre a supportare un apprendimento approfondito (deep learning) di ampio raggio con più di 30 tipi di livello, supporta anche i modelli standard quali gli insiemi di strutture ad albero, SVM e modelli lineari generalizzati. Invece di inviare i dati in remoto perché vengano analizzati, Core ML utilizza tecnologie di basso livello, quali Metal e Accelerate, per avvalersi senza soluzione di continuità della CPU e della GPU per fornire prestazioni ed efficienza massime.

## Prima di cominciare
{: #before-you-begin}

Per utilizzare Core ML e Watson Visualization, ti servono i seguenti componenti:

  * iOS 11.0+
  * Xcode 9.0+
  * Swift 4.0+
  * Carthage

Per installare Carthage, utilizza i seguenti comandi `brew`:
```
$ brew update
$ brew install carthage
```
{: codeblock}

## Passo 1. Training di un modello con {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}}
{: #training-your-model}

Se non esiste un modello corrente, viene utilizzato il primo modello trovato in remoto o qualsiasi altro che esista localmente. Il seguente gif e le istruzioni a suo corredo mostrano come puoi collegare il tuo servizio a Watson Studio ed eseguire il training del tuo modello.

![Procedura dettagliata del modello Core ML](images/CoreMLWalkthrough.gif)

### Configurazione del servizio dal tuo dashboard dell'applicazione Core ML

1. Avvia il Visual Recognition Tool dal dashboard del kit starter selezionando **Launch Tool**.
2. Inizia a creare il tuo modello selezionando **Create Model**.
2. Se un progetto non è ancora associato all'istanza Visual Recognition che hai creato, viene creato un progetto. Altrimenti, passa direttamente alla sezione **Create Model**.
3. Denomina il tuo progetto e fai clic su **Create**.
  **Suggerimento**: se non è definita alcuna archiviazione, fai clic su refresh.

### Esecuzione del bind di un servizio a un progetto

1. Dopo che hai creato il tuo progetto, viene visualizzato il dashboard del progetto.
2. Vai alla scheda settings, scorri verso il basso a **Associated Services**, seleziona **Add Service** -> **Watson**.
3. Dalla pagina dei servizi Watson, seleziona **Visual Recognition**.
4. Seleziona la scheda **Existing** nella pagina delle informazioni sui servizi e seleziona quindi la tua istanza del servizio dal dashboard.
5. Ora che il bind del tuo servizio è stato eseguito, puoi iniziare a creare il tuo modello selezionando **Assets** dal tuo dashboard del progetto e facendo quindi clic su **Add Visual Recognition Model**.

### Creazione di un modello

1. Dallo strumento di creazione dei modelli, modifica il nome del classificatore, se vuoi. Per impostazione predefinita, l'applicazione utilizza il primo disponibile, a meno che non venga specificato. Se vuoi utilizzare un modello specifico, accertati di modificare il campo `defaultClassifierID` nel controller delle viste principale.

2. Dalla barra laterale di, carica i corsi di training del modello nei file .zip. Seleziona quindi ogni dataset ed eseguine l'aggiunta al tuo modello dal menu a discesa. Puoi liberamente aggiungere ulteriori classi che usano delle tue serie di immagini per migliorare il classificatore.

![Aggiunta di classi](images/add_classes.png)

3. Seleziona **Train Model** e attendi quindi che il training del modello sia completo.

È tutto pronto. Ora sei pronto a scaricare il tuo modello Core ML e integrarlo nella tua applicazione utilizzando l'[SDK Swift di {{site.data.keyword.watson}} Developer Cloud](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.

## Passo 2. Download e creazione di dipendenze
{: #installing-dependencies}

1. Utilizzando il tuo editor di testo preferito, crea un nuovo file denominato **Cartfile** nella directory root del tuo progetto dove si trova il tuo file `.xcodeproj`.

2. Aggiungi una riga per specificare l'SDK Swift di {{site.data.keyword.watson}} Developer Cloud come una dipendenza, ad esempio:

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: pre}

  Per un'applicazione di produzione, potresti anche voler specificare un determinato requisito della versione per evitare modifiche impreviste da nuove release dell'SDK.
  {: tip}

3. Utilizza un terminale per passare alla directory root del progetto ed esegui Carthage:

  ```
  $ carthage update --platform iOS
  ```
  {: pre}

  L'SDK Swift {{site.data.keyword.watson}} Developer Cloud viene quindi scaricato e il suo framework viene creato nella cartella `Carthage/Build/iOS` del tuo progetto.

## Passo 3. Aggiunta della classificazione immagini alla tua applicazione
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

## Passo 4. Utilizzo dei kit starter
{: #coreml_starterkits}

Con i kit starter, puoi sfruttare rapidamente e facilmente le funzionalità di {{site.data.keyword.cloud_notm}} e Core ML. Puoi aggiungere {{site.data.keyword.visualrecognitionshort}} a qualsiasi applicazione iOS client o back-end lato server utilizzando i kit starter. Il kit starter Custom Vision Model for Core ML with {{site.data.keyword.watson}} illustra come creare un modello {{site.data.keyword.visualrecognitionshort}} personalizzato e istanziarlo come un modello Core ML che puoi eseguire localmente sul tuo dispositivo.

Per aggiungere {{site.data.keyword.visualrecognitionshort}} a un kit starter, completa la seguente procedura:

1. Seleziona il [kit starter](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} con cui vuoi lavorare.
2. Crea il progetto con i servizi predefiniti.
3. Fai clic su **Aggiungi risorse > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Scarica il progetto facendo clic su **Download Code**. Per i progetti iOS, le credenziali vengono inserite nel file `BMSCredentials.plist` nei campi chiave corrispondenti. Per i progetti Swift lato server, puoi trovare queste credenziali nel file `config/local-dev.json`.

## Passi successivi
{: #coreml_next}

Ora puoi analizzare le immagini utilizzando i modelli Core ML generati personalizzati. Non fermarti ora e continua provando una delle seguenti opzioni:

* Aggiungi la logica cloud. Inizia con [Sviluppo di applicazioni senza server](/docs/swift/backend/functions.html).
* Avvaliti di tutte le funzioni che {{site.data.keyword.visualrecognitionshort}} ha da offrire. Per ulteriori dettagli, vedi la [documentazione](/docs/services/visual-recognition/index.html).
