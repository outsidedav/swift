---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: swift visual recognition, train swift, cocoapods swift, swift sdk install, starter kit watson swift, image swift classify, machine learning swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

Il servizio {{site.data.keyword.visualrecognitionfull}} consente alla tua applicazione di contrassegnare con tag, classificare ed eseguire il training di contenuto visivo utilizzando il machine learning. Il servizio può aiutare a classificare praticamente qualsiasi tipo di contenuto visivo, ad eseguire il training del tuo modello personalizzato nel giro di qualche minuto e a rilevare i volti.

## Come funziona
{: #how-it-works-recognition}

1. La tua applicazione sceglie una selezione di immagini da analizzare.
2. La tua applicazione invia l'immagine al servizio {{site.data.keyword.visualrecognitionshort}} utilizzando l'SDK Swift Watson.
3. Il servizio analizza l'immagine utilizzando l'analisi di classificazione per identificare scene, oggetti, volti e altro ancora.
4. L'analisi del servizio viene restituita alla tua applicazione dall'SDK Swift Watson.

## Prima di cominciare
{: #prereqs-recognition}

Assicurati di disporre dei seguenti prerequisiti.

* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage o Swift Package Manager

Puoi installare l'[SDK Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") utilizzando [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") o [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"). Utilizzando CocoaPods(https://cocoapods.org/) per gestire le dipendenze, ottieni solo i framework di cui hai bisogno, invece dell'intero SDK Swift Watson. Se non hai dimestichezza con CocoaPods, puoi installarlo facilmente:
```console
sudo gem install cocoapods
```
{: codeblock}

## Passo 1. Creazione di un'istanza di Visual Recognition
{: #create-instance-recognition}

Esegui il provisioning di un'istanza del servizio {{site.data.keyword.visualrecognitionshort}}:

1. Nel catalogo {{site.data.keyword.cloud_notm}}, seleziona **{{site.data.keyword.visualrecognitionshort}}**. Viene visualizzata la schermata di configurazione del servizio.
2. Dai un nome alla tua istanza del servizio oppure utilizza il nome preimpostato.
3. Seleziona un'applicazione dal menu **Connect** se vuoi eseguire il bind della tua istanza a un'applicazione.
4. Seleziona un piano di prezzi e fai clic su **Create**.
5. Seleziona la scheda **Credentials** per visualizzare le tue credenziali del servizio. Questi valori vengono utilizzati per connettersi al servizio dalla tua applicazione.

## Passo 2. Download e creazione di dipendenze
{: #download-depend-recognition}

Utilizzando il tuo editor di testo preferito, crea un nuovo `Podfile` nella directory root del tuo progetto (dove si trova il tuo file `.xcodeproj`) eseguendo `pod init`. Aggiungi quindi una riga per specificare il framework {{site.data.keyword.visualrecognitionshort}} dell'SDK Swift Watson come una dipendenza:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

Per un'applicazione di produzione, potresti voler specificare anche un determinato [requisito della versione](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") per evitare modifiche impreviste da nuove release dell'SDK Swift Watson.

Con il tuo `Podfile` debitamente collocato, puoi ora scaricare le dipendenze. Utilizza un terminale per passare alla directory root del progetto ed esegui quindi CocoaPods:

```console
pod install
```
{: codeblock}

Cocoapods scarica il framework {{site.data.keyword.visualrecognitionshort}} e lo crea nella cartella `Pods/` del tuo progetto.

Per evitare errori di creazione del pod, apri il file che termina con `.xcworkspace` invece di `.xcodeproj` quando apri il progetto in Xcode.
{: tip}

## Passo 3. Analisi delle immagini nella tua applicazione
{: #analyze-images-recognition}

I seguenti esempi ti aiutano ad aggiungere le funzionalità di {{site.data.keyword.visualrecognitionshort}} alla tua applicazione, di norma in `ViewController.swift`. Utilizzando i seguenti esempi, puoi estendere le chiamate Visual Recognition per il tuo caso d'uso.

1. Aggiungi un'istruzione import per Visual Recognition:
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. Passa la chiave API e la versione (puoi usare la data di oggi) per inizializzare l'SDK:
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  Puoi consultare la [documentazione dei parametri di versione](https://{DomainName}/apidocs/visual-recognition#versioning){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") o utilizzare la data in cui è stato creato il servizio {site.data.keyword.conversationshort}}.
  {: tip}

3. Aggiungi il seguente codice per classificare un'immagine:
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

Ci sono più metodi di classificazione che sono supportati dal framework Visual Recognition. Esplora la [documentazione di Visual Recognition](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") dell'SDK Watson per trovare quale si adatta meglio alla tua applicazione.
{: tip}

## Utilizzo dei kit starter
{: #recognition_starterkits}

I [Kit starter](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") sono uno dei modi più rapidi per utilizzare le funzionalità di {{site.data.keyword.cloud_notm}}. Puoi utilizzare il servizio {{site.data.keyword.visualrecognitionshort}} selezionando il kit starter **Visual Recognition for iOS with Watson**. Questo servizio valuta e classifica le tue immagini. Carica le immagini nuove o esistenti dal tuo dispositivo mobile; l'applicazione Visual Recognition contrassegna con tag e classifica rapidamente il contenuto delle immagini.

Per iniziare:
1. Seleziona il kit starter trovato [qui](https://{DomainName}/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").
2. Crea il progetto con i servizi predefiniti.
3. Scarica il progetto facendo clic su **Download Code**. Le credenziali del servizio vengono inserite nel file `BMSCredentials.plist` nei campi chiave corrispondenti.

## Passi successivi
{: #recognition_next notoc}

Ottimo lavoro. Visual Recognition è ora disponibile per la tua applicazione. Non fermarti ora e continua provando una delle seguenti opzioni:
* Controlla l'[SDK Swift Watson](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") ed esplora gli altri servizi Watson supportati.
* Per ulteriori informazioni, consulta [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").
