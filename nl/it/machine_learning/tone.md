---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-24"

keywords: watson swift, tone anaylzer swift, cocoapods swift, swift sdk install, starter kit watson

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

Il servizio {{site.data.keyword.ibmwatson}} {{site.data.keyword.toneanalyzershort}} consente alla tua applicazione di comprendere le emozioni e i toni nel testo. Puoi utilizzare il servizio per comprendere meglio le conversazioni dei tuoi utenti o aiutare gli utenti a comprendere come sono percepite le loro comunicazioni scritte.

## Come funziona
{: #how-it-works-tone}

1. La tua applicazione sceglie una selezione di testo da analizzare (ad esempio, un messaggio di testo o un feed di Twitter).
2. La tua applicazione invia il testo al servizio {{site.data.keyword.toneanalyzershort}} utilizzando l'SDK Swift Watson.
3. Il servizio analizza il testo utilizzando l'analisi linguistica per identificare le emozioni e i toni.
4. L'analisi del servizio viene restituita alla tua applicazione tramite l'[SDK Swift Watson](https://github.com/watson-developer-cloud/swift-sdk).

## Prima di cominciare
{: #prereqs-tone}

Assicurati di disporre dei seguenti prerequisiti.

* OS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage o Swift Package Manager

Puoi installare l'[SDK Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") utilizzando [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") o [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"). Utilizzando CocoaPods per gestire le dipendenze, ottieni solo i framework di cui hai bisogno, invece dell'intero SDK Swift Watson. Se non hai dimestichezza con CocoaPods, puoi installarlo facilmente:

```bash
$ sudo gem install cocoapods
```
{: codeblock}

## Passo 1. Creazione di un'istanza di Tone Analyzer
{: #create-instance-tone}

Esegui il provisioning di un'istanza del servizio {{site.data.keyword.toneanalyzershort}}:

1. Nel catalogo {{site.data.keyword.cloud_notm}}, seleziona {{site.data.keyword.toneanalyzershort}}. Viene visualizzata la schermata di configurazione del servizio.
2. Assegna un nome alla tua istanza del servizio oppure utilizza il nome preimpostato.
3. Seleziona un'applicazione dal menu Connect se vuoi eseguire il bind della tua istanza a un'applicazione.
4. Seleziona un piano di prezzi e fai clic su **Create**.
5. Seleziona la scheda **Credentials** per visualizzare le tue credenziali del servizio. Questi valori vengono utilizzati per connettersi al servizio dalla tua applicazione.

## Passo 2. Download e creazione di dipendenze
{: #download-depend-tone}

Utilizzando il tuo editor di testo preferito, crea un nuovo `Podfile` nella directory root del tuo progetto (dove si trova il tuo file `.xcodeproj`) eseguendo `pod init`. Aggiungi quindi una riga per specificare il framework {{site.data.keyword.conversationshort}} dell'SDK Swift Watson come una dipendenza:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonToneAnalyzerV3'
```
{: codeblock}

Per un'applicazione di produzione, potresti voler specificare anche un determinato [requisito della versione](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") per evitare modifiche impreviste da nuove release dell'SDK Swift Watson.

Con il tuo `Podfile` debitamente collocato, puoi ora scaricare le dipendenze. Utilizza un terminale per passare alla directory root del progetto ed esegui quindi CocoaPods:

```console
pod install
```
{: codeblock}

Cocoapods scarica il framework {{site.data.keyword.toneanalyzershort}} e lo crea nella cartella `Pods/` del tuo progetto.

Per evitare errori di creazione del pod, apri il file che termina con `.xcworkspace` invece di `.xcodeproj` quando apri il progetto in Xcode.
{: tip}

## Passo 3. Analisi del testo nella tua applicazione
{: #analyze-text-tone}

I seguenti esempi ti aiutano ad aggiungere le funzionalità di {{site.data.keyword.toneanalyzershort}} alla tua applicazione, di norma in `ViewController.swift`. Utilizzando i seguenti esempi, puoi estendere le chiamate Tone Analyzer per il tuo caso d'uso.

1. Aggiungi un'istruzione import per Tone Analyzer:
  ```swift
  import ToneAnalyzer
  ```
  {: codeblock}

2. Istanzia il servizio Tone Analyzer:
  ```swift
  let toneAnalyzer = ToneAnalyzer(version: "yyyy-mm-dd", apiKey: "your-api-key-here")
  ```
  {: codeblock}

  Consulta la [documentazione dei parametri di versione](https://cloud.ibm.com/apidocs/tone-analyzer#versioning){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") oppure usa la data in cui è stato creato il servizio {site.data.keyword.conversationshort}}.
  {: tip}

3. Fornisci il testo per l'analisi ed elabora i risultati.
  ```swift
  // Text to analyze
  let review = """
      I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
  """

  // Analyze text
  let input = ToneInput(text: review)
  toneAnalyzer.tone(toneInput: .toneInput(input)) { response, error in
      if let error = error {
          print(error)
          return
      }

      guard let tones = response?.result?.documentTone.tones else {
          print("Failed to analyze the tone input")
          return
      }

      for tone in tones {
          print("\(tone.toneName): \(tone.score)")
              }
  }
  ```
  {: codeblock}

  Quando esegui il codice, vedi la seguente analisi nella console:
  ```
  Sadness: 0.575803
Tentative: 0.867377
  ```
  {: screen}

4. Esplora la [documentazione di Tone Analyzer](https://watson-developer-cloud.github.io/swift-sdk/services/ToneAnalyzerV3/index.html){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") di SDK Watson per creare la funzionalità della tua applicazione.

## Utilizzo dei kit starter
{: #tone_starterkits}

I [Kit starter](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") sono uno dei modi più rapidi per utilizzare le funzionalità di {{site.data.keyword.cloud_notm}}. Puoi utilizzare il servizio {{site.data.keyword.toneanalyzershort}} selezionando il kit starter **Tone Analyzer for iOS with Watson**. Questo servizio utilizza le funzionalità di apprendimento approfondito (deep learning) per valutare dei passaggi di testo. L'applicazione Tone Analyzer identifica il tono del parlante (felice, triste, sicuro e altro) e lo correla e diverse categorie.

Per iniziare con questo kit starter:

1. Seleziona il kit starter trovato [qui](https://cloud.ibm.com/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").
2. Crea il progetto con i servizi predefiniti.
3. Scarica il progetto facendo clic su **Download Code**. Le credenziali del servizio vengono inserite nel file `BMSCredentials.plist` nei campi chiave corrispondenti.

## Passi successivi
{: #tone_next notoc}

Ottimo lavoro. {{site.data.keyword.toneanalyzershort}} è ora aggiunto alla tua applicazione. Non fermarti ora e continua provando una delle seguenti opzioni:

* Visualizza l'[SDK Swift Watson su GitHub](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") ed esplora gli altri servizi Watson supportati.
* Per ulteriori informazioni, consulta [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").
