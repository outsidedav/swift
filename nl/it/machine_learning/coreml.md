---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: coreml swift, core ml swift, watson swift core, create model swift, image classification swift, version parameter swift, swift coreml watson

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilizzo di Core ML con Watson
{: #swift-coreml}

Con [Core ML](https://developer.apple.com/documentation/coreml){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"), puoi integrare un'ampia gamma di tipi di modello di machine learning nella tua applicazione. Oltre a supportare un apprendimento approfondito (deep learning) di ampio raggio con più di 30 tipi di livello, supporta anche i modelli standard quali gli insiemi di strutture ad albero, SVM e modelli lineari generalizzati. Invece di inviare i dati in remoto perché vengano analizzati, Core ML utilizza tecnologie di basso livello, quali Metal e Accelerate, per avvalersi senza soluzione di continuità della CPU e della GPU per fornire prestazioni ed efficienza massime.

Puoi aggiungere Core ML e Watson Visual Recognition a un'applicazione Swift utilizzando la seguente procedura per formare e creare un modello, scaricare e creare dipendenze e aggiungere la classificazione immagini.
{: shortdesc}

## Prima di cominciare
{: #prereqs-coreml}

Per utilizzare Core ML e Watson Visual Recognition con Swift, hai bisogno dei seguenti componenti:

* iOS 11.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage o Swift Package Manager

Puoi installare l'[SDK Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") utilizzando [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") o [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"). Utilizzando CocoaPods per gestire le dipendenze, ottieni solo i framework di cui hai bisogno, invece dell'intero SDK Swift Watson. Se non hai dimestichezza con CocoaPods, puoi installarlo facilmente:

```console
sudo gem install cocoapods
```
{: codeblock}

## Passo 1. Formazione di un modello con {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}}
{: #train-module-coreml}

Se non esiste un modello corrente, viene utilizzato il primo modello rilevato in remoto o qualsiasi altro che esista localmente. Il seguente gif e le istruzioni a suo corredo mostrano come puoi collegare il tuo servizio a Watson Studio ed eseguire la formazione del tuo modello.

![Procedura dettagliata del modello Core ML](images/CoreMLWalkthrough.gif)

### Configurazione del servizio dal tuo dashboard dell'applicazione Core ML
{: #service-coreml}

1. Avvia il Visual Recognition Tool dal dashboard del kit starter selezionando **Launch Tool**.
2. Inizia a creare il tuo modello selezionando **Create Model**.
2. Se un progetto non è ancora associato all'istanza Visual Recognition che hai creato, viene creato un progetto. Altrimenti, passa direttamente alla sezione **Create Model**.
3. Denomina il tuo progetto e fai clic su **Create**.
  
  Se non è definita alcuna archiviazione, fai clic su refresh.
  {: tip}

### Esecuzione del bind di un servizio a un progetto
{: #bind-service-coreml}

1. Dopo che hai creato il tuo progetto, viene visualizzato il dashboard del progetto.
2. Vai alla scheda settings, scorri verso il basso a **Associated Services**, seleziona **Add Service** -> **Watson**.
3. Dalla pagina dei servizi Watson, seleziona **Visual Recognition**.
4. Seleziona la scheda **Existing** nella pagina delle informazioni sui servizi e seleziona quindi la tua istanza del servizio dal dashboard.
5. Ora che il bind del tuo servizio è stato eseguito, puoi iniziare a creare il tuo modello selezionando **Assets** dal tuo dashboard del progetto e facendo quindi clic su **Add Visual Recognition Model**.

### Creazione di un modello
{: #create-model-coreml}

1. Dallo strumento di creazione dei modelli, puoi aggiornare il nome del classificatore. Se vuoi utilizzare un modello specifico, accertati di modificare il campo `defaultClassifierID` nel controller delle viste principale.

2. Dalla barra laterale, carica i corsi di formazione del modello nei file `.zip` compressi. Seleziona quindi ogni dataset ed eseguine l'aggiunta al tuo modello dal menu a discesa. Puoi liberamente aggiungere ulteriori classi che usano delle tue serie di immagini per migliorare il classificatore.

![Aggiunta di classi](images/add_classes.png)

3. Seleziona **Train Model** e attendi quindi che la formazione del modello sia completo.

È tutto pronto. Ora sei pronto a scaricare il tuo modello Core ML e integrarlo nella tua applicazione utilizzando l'[SDK Swift di Watson Developer Cloud](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").

## Passo 2. Download e creazione di dipendenze
{: #install-depend-coreml}

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

## Passo 3. Aggiunta della classificazione immagini alla tua applicazione
{: #add-image-coreml}

I seguenti esempi ti aiutano ad aggiungere le funzionalità di {{site.data.keyword.visualrecognitionshort}} Core ML alla tua applicazione, di norma in `ViewController.swift`. Utilizzando i seguenti esempi, puoi estendere le chiamate del modello locale per il tuo caso d'uso.

1. Aggiungi un'istruzione import per Visual Recognition:
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. Passa la chiave API e la versione per inizializzare l'SDK:
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  Consulta la [documentazione dei parametri di versione](https://cloud.ibm.com/apidocs/visual-recognition#versioning){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") oppure usa la data in cui è stato creato il servizio {site.data.keyword.visualrecognitionshort}}.
  {: tip}

3. Aggiungi il seguente codice per scaricare o aggiornare il modello Core ML locale con il tuo classificatore Watson:
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

4. Aggiungi il seguente codice per classificare un'immagine utilizzando il tuo modello Core ML locale:
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

5. Esplora le altre [funzionalità di classificazione di Core ML](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") supportate dall'SDK Watson.

## Passo 4. Utilizzo dei kit starter
{: #coreml_starterkits}

Con i kit starter, puoi utilizzare rapidamente e facilmente le funzionalità di {{site.data.keyword.cloud_notm}} e Core ML. Puoi aggiungere {{site.data.keyword.visualrecognitionshort}} a qualsiasi applicazione iOS client o back-end lato server utilizzando i kit starter. Il kit starter Custom Vision Model for Core ML with {{site.data.keyword.watson}} illustra come creare un modello {{site.data.keyword.visualrecognitionshort}} personalizzato e istanziarlo come un modello Core ML che puoi eseguire localmente sul tuo dispositivo.

Per aggiungere {{site.data.keyword.visualrecognitionshort}} a un kit starter, completa la seguente procedura:

1. Seleziona il [kit starter](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") con cui vuoi lavorare.
2. Crea l'applicazione con i servizi predefiniti.
3. Fai clic su **Add service > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Scarica il progetto facendo clic su **Download code**. Per i progetti iOS, le credenziali vengono inserite nel file `BMSCredentials.plist` nei campi chiave corrispondenti. Per i progetti Swift lato server, puoi trovare queste credenziali nel file `config/local-dev.json`.

## Passi successivi
{: #coreml_next}

Ora puoi analizzare le immagini utilizzando i modelli Core ML generati personalizzati. Non fermarti ora e continua provando una delle seguenti opzioni:

* Controlla l'[SDK Swift {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") ed esplora gli altri servizi Watson supportati.
* Aggiungi la logica cloud. Inizia con [Sviluppo di applicazioni senza server](/docs/swift/backend?topic=swift-serverless-dev-swift#serverless-dev-swift).
* Avvaliti di tutte le funzioni offerte da {{site.data.keyword.visualrecognitionshort}}. Per ulteriori dettagli, vedi la [documentazione](/docs/services/visual-recognition?topic=visual-recognition-index#index).
