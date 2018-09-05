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
{:tip: .tip}

# Esercitazione introduttiva
{: #set_up}

{{site.data.keyword.cloud}} offre soluzioni e servizi per mettere a disposizione degli sviluppatori e delle applicazioni Swift la protezione, l'intelligenza artificiale e il valore richiesti dai loro clienti. Con un ampio portfolio di offerte ed SDK, puoi avvalerti di questi servizi e immettere rapidamente sul mercato applicazioni all'avanguardia.La guida alla programmazione Swift ti insegna ad aggiungere servizi a un'applicazione Swift nuova o esistente, indipendentemente dal fatto che sia per Swift lato server o lato client iOS.
{: shortdesc}

La seguente esercitazione è un punto d'ingresso per mostrarti come creare facilmente un'applicazione mobile Swift con {{site.data.keyword.mobileanalytics_full}} utilizzando un kit starter vuoto dalla [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits). Dalla console, aggiungi il servizio {{site.data.keyword.mobileanalytics_short}}, scarichi il codice, esegui l'applicazione iOS in locale in Xcode e configuri e monitori l'applicazione.

## Passo 1. Requisiti per gli sviluppatori
{: #dev-requirements}

Per iniziare con lo sviluppo iOS su {{site.data.keyword.cloud_notm}}, assicurati di soddisfare i seguenti requisiti.

### Sistema operativo

È buona prassi sviluppare applicazioni Swift utilizzando l'hardware supportato MacOS più recente e di eseguire le attività di test con le release iOS più recenti. Registrati per un account [Apple Developer](https://developer.apple.com/) per abilitare l'esecuzione di test su un dispositivo fisico.

### iOS e Xcode
{: #ios_and_xcode}

- Installa [Xcode 8+](https://developer.apple.com/xcode/) (o superiore).
- Esegui la distribuzione ai [dispositivi iOS 8](https://support.apple.com/downloads/ios) (o superiore).
- Prima dell'inoltro delle applicazioni ad Apple, leggi attentamente le [linee guida sugli inoltri all'App Store](https://developer.apple.com/app-store/guidelines/).

### Gestione di dipendenze ed SDK

I seguenti strumenti assicurano che tu possa installare gli SDK nativi per lavorare con i vari servizi {{site.data.keyword.cloud_notm}}.

1. Installa [CocoaPods](https://cocoapods.org/) per supportare gli SDK {{site.data.keyword.cloud_notm}}:
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
2. Installa [Homebrew](https://brew.sh/) per facilitare l'installazione di Carthage:
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

3. Installa [Carthage](https://github.com/Carthage/Carthage) per supportare gli SDK Watson:
  ```
  brew install carthage
  ```
  {: codeblock}

## Passo 2. Creazione di un'applicazione Swift iOS personalizzata
{: #create-ios-app}

1. Esegui l'accesso alla [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits).
2. Fai clic su **Crea applicazione**.
3. Nella pagina [Empty Starter](https://console.bluemix.net/developer/appledevelopment/create-app), puoi utilizzare la configurazione predefinita oppure aggiornare i campi come necessario. Assicurati che **iOS Swift** sia il linguaggio selezionato. Fai clic su **Create**.

## Passo 3. Aggiunta del servizio {{site.data.keyword.mobileanalytics_short}}
{: #adding-services}

1. Dalla pagina App Details, fai clic sul pulsante **Add Resource**.
2. Seleziona **Mobile** e fai clic su **Next**.
3. Seleziona **{{site.data.keyword.mobileanalytics_short}}** e fai clic su **Next**.
4. Fai clic su **Create**.

## Passo 4. Download del codice e configurazione degli SDK client
{: #run-locally}

Per scaricare il codice, fai clic su **Download Code** in `Apps` > `Your App`. Il codice scaricato viene fornito comprensivo degli SDK client **{{site.data.keyword.mobileanalytics_short}}**. Gli SDK client sono disponibili su CocoaPods e Carthage. Per questa soluzione, utilizza CocoaPods.

1. Decomprimi il codice scaricato. Quindi, utilizzando un terminale, passa alla cartella decompressa.
  ```
  cd <Name of Project>
  ```
2. La cartella include un podfile con le dipendenze richieste. Esegui il seguente comando per installare le dipendenze (SDK client):
  ```
  pod install
  ```
  {: codeblock}

## Passo 5. Configurazione dell'applicazione per utilizzare {{site.data.keyword.mobileanalytics_short}}
{: #configure-analytics}

1. Apri `.xcworkspace` in Xcode e passa a `AppDelegate.swift`.
  **Nota:** assicurati di aprire sempre il nuovo spazio di lavoro Xcode, invece del file di progetto Xcode originale: `MyApp.xcworkspace`.
   ![Apri Xcode](images/Xcode.png)

  `BMSCore` è l'SDK principale ed è la base per gli SDK client mobili. `BMSClient` è una classe di BMSCore, inizializzata in AppDelegate.swift. Insieme a BMSCore, l'SDK {{site.data.keyword.mobileanalytics_short}} SDK è già importato nel progetto.
  
2. Il codice di inizializzazione dell'analisi è già incluso, come mostrato nel seguente frammento di codice:
  ```
  // Analytics client SDK is configured to record lifecycle events.
         	Analytics.initialize(appName:dictionary["appName"] as? String,
        			     apiKey: dictionary["analyticsApiKey"] as? String,
        	        	     deviceEvents: .lifecycle)

        	// Enable Logger (disabled by default) and set level to ERROR (DEBUG by default).
        	Logger.isLogStorageEnabled = true
        	Logger.logLevelFilter = .error
  ```
  {: codeblock}

  **Nota:** le credenziali del servizio fanno parte del file `BMSCredentials.plist`.

3. Raccolta dell'analisi dell'utilizzo utilizzando `logger`. Passa al file `ViewController.swift` per visualizzare il seguente codice.
  ```
   func didBecomeActive(_ notification: Notification) {
       Analytics.send()
       Logger.send()
    }
  ```
  {: codeblock}

   Per le funzionalità di analisi e registrazione avanzate, fai riferimento ai documenti relativi alla [raccolta dell'analisi di utilizzo](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics) e alla [registrazione](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger).
   {:tip}

## Passo 6. Monitoraggio dell'applicazione con {{site.data.keyword.mobileanalytics_short}}
Il servizio {{site.data.keyword.mobileanalytics_short}} fornisce un'analisi approfondita delle prestazioni e dell'utilizzo delle applicazioni chiave per i proprietari di applicazioni e gli sviluppatori di applicazioni mobili. Utilizzando {{site.data.keyword.mobileanalytics_short}}, i proprietari e gli sviluppatori delle applicazioni possono comprendere cosa sta succedendo sul lato utente. Possono utilizzare queste analisi approfondite per creare delle applicazioni migliori, pertinenti per gli utenti, e che si distinguono nell'enorme offerta di applicazioni mobili.

Il servizio include la console {{site.data.keyword.mobileanalytics_short}}, dove gli sviluppatori e i proprietari di applicazioni possono monitorare le prestazioni delle applicazioni mobili, visualizzare le statistiche di utilizzo e cercare nei log dispositivo.

1. Apri il servizio `{{site.data.keyword.mobileanalytics_short}}` dall'applicazione mobile che hai creato oppure fai clic sui tre punti verticali accanto al servizio e seleziona `Open Dashboard`.
2. Puoi vedere sessioni e utenti LIVE e altri dati dell'applicazione disabilitando la `Demo Mode`. Puoi filtrare le informazioni di analisi in base ai seguenti criteri:
    * Data.
    * Applicazione:
    * Sistema operativo.
    * Versione dell'applicazione.
         ![{{site.data.keyword.mobileanalytics_short}}](images/mobile_analytics.png)
3. [Fai clic qui ](https://console.bluemix.net/docs/services/mobileanalytics/app-monitoring.html#monitoringapps) per impostare gli avvisi, monitorare gli arresti anomali delle applicazioni e monitorare le richieste di rete.

## Passi successivi
{: #next-steps}

### Aggiunta di altri servizi
Puoi aggiungere altri servizi alla tua applicazione iOS direttamente dalla console web, come i seguenti servizi comunemente utilizzati:

* [Aggiunta del servizio Push Notifications](/push/push_notifications.html)
* [Aggiunta dell'autenticazione utente con l'ID applicazione](/authenticate/app_id.html)

### Utilizzo degli strumenti per sviluppatori {{site.data.keyword.cloud_notm}}
Puoi anche imparare a sviluppare applicazioni Swift utilizzando gli [strumenti per gli sviluppatori {{site.data.keyword.cloud_notm}}](../cli/index.html), che offrono un approccio da riga di comando per la creazione, lo sviluppo e la distribuzione di applicazioni del microservizio, mobili e web end-to-end.

