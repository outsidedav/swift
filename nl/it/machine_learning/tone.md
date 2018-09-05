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

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

Il servizio IBM Watson Tone Analyzer consente alla tua applicazione di comprendere le emozioni e i toni nel testo. Puoi utilizzare il servizio per comprendere meglio le conversazioni dei tuoi utenti o aiutare gli utenti a comprendere come sono percepite le loro comunicazioni scritte.

## Come funziona
{: ##how-it-works}

1. La tua applicazione sceglie una selezione di testo da analizzare (ad esempio, un messaggio di testo o un feed di Twitter).
2. La tua applicazione invia il testo al servizio {{site.data.keyword.toneanalyzershort}} utilizzando l'SDK Swift Watson.
3. Il servizio analizza il testo utilizzando l'analisi linguistica per identificare le emozioni e i toni.
4. L'analisi del servizio viene restituita alla tua applicazione tramite l'SDK Swift Watson.

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
{: codeblock}

## Passo 1. Creazione di un'istanza di Tone Analyzer
{: #create-and-configure-an-instance-of-tone-analyzer}

Esegui il provisioning di un'istanza del servizio {{site.data.keyword.toneanalyzershort}}:

1. Nel catalogo {{site.data.keyword.cloud_notm}}, seleziona {{site.data.keyword.toneanalyzershort}}. Viene visualizzata la schermata di configurazione del servizio.
2. Assegna un nome alla tua istanza del servizio oppure utilizza il nome preimpostato.
3. Seleziona un'applicazione dal menu Connect se vuoi eseguire il bind della tua istanza a un'applicazione.
4. Seleziona un piano di prezzi e fai clic su **Create**.
5. Seleziona la scheda **Credentials** per visualizzare le tue credenziali del servizio. Questi valori vengono utilizzati per connettersi al servizio dalla tua applicazione.

## Passo 2. Download e creazione di dipendenze
{: ###download-and-build-dependencies}

Utilizzando il tuo editor di testo preferito, crea un nuovo file denominato `Cartfile` nella directory root del tuo progetto (dove si trova il tuo file `.xcodeproj`). Aggiungi quindi una riga per specificare l'SDK Swift Watson come una dipendenza:

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

Per un'applicazione di produzione, potresti voler specificare anche un determinato [requisito della versione](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) per evitare modifiche impreviste da nuove release dell'SDK Swift Watson.

Con il tuo `Cartfile` debitamente collocato, puoi ora scaricare e creare le dipendenze. Utilizza un terminale per passare alla directory root del progetto ed esegui quindi Carthage:
  
  ```bash
  $ carthage update --platform iOS
  ```
  {: codeblock}

Carthage scarica l'SDK Swift Watson e crea i relativi framework nella cartella `Carthage/Build/iOS` del tuo progetto.

## Passo 3. Aggiunta dei framework alla tua applicazione
{: ###add-frameworks-to-your-app}

### Passi per il collegamento di Tone Analyzer

Ora che i framework dell'SDK Swift Watson sono stati creati da Carthage, devi collegare il framework Tone Analyzer alla tua applicazione.

1. Apri la tua applicazione in Xcode, quindi seleziona il tuo progetto per aprirne le impostazioni.
2. Seleziona la destinazione della tua applicazione ed apri quindi la **scheda General**.
3. Scorri verso il basso alla sezione "Linked Frameworks and Libraries" e fai clic sull'icona `+`.
4. Nella finestra che viene visualizzata, scegli **Add Other...** e passa alla directory `Carthage/Build/iOS`. Seleziona **ToneAnalyzerV3.framework** per eseguirne il collegamento alla tua applicazione.

### Passi per copiare Tone Analyzer

Oltre a _collegare_ il framework Tone Analyzer, devi anche _copiarlo_ nell'applicazione per renderlo accessibile al runtime. Xcode ha vari modi diversi per copiare o integrare un framework ma puoi utilizzare uno script Carthage per evitare uno specifico [debug di inoltro all'App Store](http://www.openradar.me/radar?id=6409498411401216).

1. Con le impostazioni della destinazione della tua applicazione aperte in Xcode, passa alla scheda **Build Phases**.
2. Fai clic sull'icona `+` e seleziona quindi **New Run Script Phase**.
3. Aggiungi il seguente comando alla fase di esecuzione dello script: `/usr/local/bin/carthage copy-frameworks`.
4. Aggiungi il framework Tone Analyzer all'elenco "Input Files": `$(SRCROOT)/Carthage/Build/iOS/ToneAnalyzerV3.framework`.

Ora sei pronto a iniziare a lavorare con l'SDK Swift Watson nella tua applicazione.

## Passo 4. Analisi del testo nella tua applicazione
{: #analyze-text-in-your-app}

1. Apri il file `ViewController.swift` in Xcode.
2. Aggiungi un'istruzione import per Tone Analyzer:
    ```swift
    import ToneAnalyzerV3
    ```
    {: codeblock}

3. Crea una funzione vuota denominata `toneAnalyzerExample` e richiamala quindi da `viewDidLoad`.
4. Aggiungi il seguente codice alla funzione `toneAnalyzerExample`. Assicurati di aggiornare il nome utente e la password del tuo servizio.
  ```swift
  import UIKit
  import ToneAnalyzerV3

  class ViewController: UIViewController {

      override func viewDidLoad() {
          super.viewDidLoad()
          toneAnalyzerExample()
      }

      func toneAnalyzerExample() {

          // instantiate service
          let toneAnalyzer = ToneAnalyzer(
              username: "your-username-here",
              password: "your-password-here",
              version: "yyyy-mm-dd"
          )

          // text to analyze
          let review = """
              I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
          """

          // analyze text
          let toneInput = ToneInput(text: review)
          let failure = { (error: Error) in print(error) }
          toneAnalyzer.tone(toneInput: toneInput, contentType: "application/json", failure: failure) { analysis in
              for tone in analysis.documentTone.tones! {
                  print("\(tone.toneName): \(tone.score)")
              }
          }
      }
  }
  ```
  {: codeblock}

Quando esegui la tua applicazione, vedi la seguente analisi nella console:
```
Sadness: 0.575803
Tentative: 0.867377
```
{: screen}

## Utilizzo dei kit starter
{: #tone_starterkits}

I [kit starter](https://console.bluemix.net/developer/appledevelopment/starter-kits) sono uno dei modi più rapidi per avvalersi delle funzionalità di {{site.data.keyword.cloud_notm}}. Puoi utilizzare il servizio {{site.data.keyword.toneanalyzershort}} selezionando il kit starter **Tone Analyzer for iOS with Watson**. Questo servizio utilizza le funzionalità di apprendimento approfondito (deep learning) per valutare dei passaggi di testo. L'applicazione Tone Analyzer identifica il tono del parlante (felice, triste, sicuro e altro) e lo correla e diverse categorie.

Per iniziare con questo kit starter:

1. Seleziona il kit starter che si trova [qui](https://console.bluemix.net/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson).
2. Crea il progetto con i servizi predefiniti.
3. Scarica il progetto facendo clic su **Download Code**. Le credenziali del servizio vengono inserite nel file `BMSCredentials.plist` nei campi chiave corrispondenti.

## Passi successivi
{: #tone_next}

Ottimo lavoro. {{site.data.keyword.toneanalyzershort}} è ora aggiunto alla tua applicazione. Non fermarti ora e continua provando una delle seguenti opzioni:

* Visualizza l'[SDK Swift Watson su GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* Per ulteriori informazioni, vedi [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/).

