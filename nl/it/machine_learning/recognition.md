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

Il servizio {{site.data.keyword.visualrecognitionfull}} consente alla tua applicazione di contrassegnare con tag, classificare ed eseguire il training di contenuto visivo utilizzando il machine learning. Il servizio può aiutare a classificare praticamente qualsiasi tipo di contenuto visivo, ad eseguire il training del tuo modello personalizzato nel giro di qualche minuto e a rilevare i volti.

## Come funziona
{: ##how-it-works}

1. La tua applicazione sceglie una selezione di immagini da analizzare.
2. La tua applicazione invia l'immagine al servizio {{site.data.keyword.visualrecognitionshort}} utilizzando l'SDK Swift Watson.
3. Il servizio analizza l'immagine utilizzando l'analisi di classificazione per identificare scene, oggetti, volti e altro ancora.
4. L'analisi del servizio viene restituita alla tua applicazione dall'SDK Swift Watson.

## Prima di cominciare
{: ###before-you-begin}

Assicurati innanzitutto di disporre dei seguenti prerequisiti pronti a essere utilizzati:
<ul>
  <li>iOS 8.0+</li>
  <li>Xcode 9.0+</li>
  <li>Swift 3.2+ o Swift 4.0+</li>
  <li>Carthage</li>
</ul>

Ti consigliamo di utilizzare [Carthage](https://github.com/Carthage/Carthage) per gestire le dipendenze e creare l'SDK Swift Watson per la tua applicazione. Se non hai dimestichezza con Carthage, puoi installarlo con [Homebrew](http://brew.sh/):

```bash
$ brew update
$ brew install carthage
```

## Passo 1. Creazione di un'istanza di Visual Recognition
{: ###create-and-configure-an-instance-of-visual-recognition}

Esegui il provisioning di un'istanza del servizio {{site.data.keyword.visualrecognitionshort}}:

1. Nel catalogo {{site.data.keyword.cloud_notm}}, seleziona **{{site.data.keyword.visualrecognitionshort}}**. Viene visualizzata la schermata di configurazione del servizio.
2. Assegna un nome alla tua istanza del servizio oppure utilizza il nome preimpostato.
3. Seleziona un'applicazione dal menu **Connect** se vuoi eseguire il bind della tua istanza a un'applicazione.
4. Seleziona un piano di prezzi e fai clic su **Create**.
5. Seleziona la scheda **Credentials** per visualizzare le tue credenziali del servizio. Questi valori vengono utilizzati per connettersi al servizio dalla tua applicazione.

## Passo 2. Download e creazione di dipendenze
{: ###download-and-build-dependencies}

Utilizzando il tuo editor di testo preferito, crea un file denominato `Cartfile` nella directory root del tuo progetto (dove si trova il tuo file `.xcodeproj`). Aggiungi quindi una riga per specificare l'SDK Swift Watson come una dipendenza:
```
github "watson-developer-cloud/swift-sdk"
```
{: pre}

Per un'applicazione di produzione, puoi specificare un determinato [requisito della versione](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) per evitare modifiche impreviste da nuove release dell'SDK Swift Watson.

Con il `Cartfile` debitamente collocato, puoi ora scaricare e creare le dipendenze. Utilizza un terminale per passare alla directory root del progetto ed esegui quindi Carthage:

```bash
carthage update --platform iOS
```
{: pre}

Carthage scarica l'SDK Swift Watson e crea i relativi framework nella cartella `Carthage/Build/iOS` del tuo progetto.

## Passo 3. Aggiunta dei framework alla tua applicazione
{: ###add-frameworks-to-your-app}

### Passi di collegamento di Visual Recognition:

Ora che i framework dell'SDK Swift Watson sono stati creati da Carthage, devi collegare il framework Visual Recognition alla tua applicazione.

1. Apri la tua applicazione in Xcode e seleziona il tuo progetto per aprirne le impostazioni.
2. Seleziona la destinazione della tua applicazione ed apri quindi la scheda **General**.
3. Scorri verso il basso alla sezione "Linked Frameworks and Libraries" e fai clic sull'icona `+`.
4. Nella finestra che viene visualizzata, scegli **Add Other...** e passa alla directory `Carthage/Build/iOS`. Seleziona **VisualRecognitionV3.framework** per eseguirne il collegamento alla tua applicazione.

### Passi di copia di Visual Recognition:

Oltre a _collegare_ il framework Visual Recognition, devi anche _copiarlo_ nell'applicazione per renderlo accessibile al runtime. Xcode ha vari modi diversi per copiare o integrare un framework ma puoi utilizzare uno script Carthage per evitare uno specifico [debug di inoltro all'App Store](http://www.openradar.me/radar?id=6409498411401216).

1. Con le impostazioni della destinazione della tua applicazione aperte in Xcode, passa alla scheda **Build Phases**.
2. Fai clic sull'icona `+` e seleziona quindi **New Run Script Phase**.
3. Aggiungi il seguente comando alla fase di esecuzione dello script: `/usr/local/bin/carthage copy-frameworks`.
4. Aggiungi il framework Visual Recognition all'elenco **Input Files**: `$(SRCROOT)/Carthage/Build/iOS/VisualRecognitionV3.framework`.

Ora sei pronto a iniziare a lavorare con l'SDK Swift Watson nella tua applicazione.

## Passo 4. Analisi delle immagini nella tua applicazione
{: #analyze-images-in-your-app}

1. Apri il file `ViewController.swift` in Xcode.

1. Aggiungi un'istruzione import per Visual Recognition:
    ```swift
    import VisualRecognitionV3
    ```
    {: pre}

1. Passa la chiave API e la versione (puoi usare la data di oggi) per inizializzare l'SDK:
    ```swift
    let visualRecognition = VisualRecognition(apiKey: "your-api-key", version: "yyyy-mm-dd")
    ```
    {: pre}

1. Aggiungi il seguente codice per classificare un'immagine:
    ```swift
    let url = "your-image-url"
    let failure = { (error: Error) in print(error) }
    visualRecognition.classify(url: url, failure: failure) { classifiedImages in
        print(classifiedImages)
    }
    ```
    {: pre}

## Utilizzo dei kit starter
{: #recognition_starterkits}

I [kit starter](https://console.bluemix.net/developer/appledevelopment/starter-kits) sono uno dei modi più rapidi per avvalersi delle funzionalità di {{site.data.keyword.cloud_notm}}. Puoi utilizzare il servizio {{site.data.keyword.visualrecognitionshort}} selezionando il kit starter **Visual Recognition for iOS with Watson**. Questo servizio valuta e classifica le tue immagini. Carica le immagini nuove o esistenti dal tuo dispositivo mobile; l'applicazione Visual Recognition contrassegna con tag e classifica rapidamente il contenuto delle immagini.

Per iniziare:
1. Seleziona il kit starter che si trova [qui](https://console.bluemix.net/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson).
2. Crea il progetto con i servizi predefiniti.
3. Scarica il progetto facendo clic su **Download Code**. Le credenziali del servizio vengono inserite nel file `BMSCredentials.plist` nei campi chiave corrispondenti.

## Passi successivi
{: #recognition_next}

Ottimo lavoro. Visual Recognition è ora disponibile per la tua applicazione. Non fermarti ora e continua provando una delle seguenti opzioni:
* Visualizza l'[SDK Swift Watson su GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* Per ulteriori informazioni, vedi [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/).

