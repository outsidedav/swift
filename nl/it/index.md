---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: getting started swift, custom app, create app swift, stater kit swift, apple app swift, swift dependency, ios development

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Esercitazione introduttiva
{: #getting_started_swift}

{{site.data.keyword.cloud}} offre soluzioni e servizi per abilitare gli sviluppatori Swift a creare applicazioni integrate con la protezione, l'intelligenza artificiale e il valore richiesti dai tuoi clienti. Con un ampio portfolio di offerte ed SDK, puoi utilizzare questi servizi e immettere rapidamente sul mercato applicazioni all'avanguardia. Questa programmazione Swift ti spiega come aggiungere i servizi a un'applicazione Swift nuova o esistente, indipendentemente dal fatto che sia per Swift lato server o lato client iOS.
{: shortdesc}

La seguente esercitazione ti mostra come creare facilmente un'applicazione mobile Swift con {{site.data.keyword.mobileanalytics_full}} utilizzando un kit starter vuoto dalla [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno"). Dalla console, aggiungi il servizio {{site.data.keyword.mobileanalytics_short}}, scarichi il codice, esegui l'applicazione iOS in locale in Xcode e configuri e monitori l'applicazione.

## Passo 1. Requisiti per gli sviluppatori
{: #dev-requirements-swift}

Per iniziare con lo sviluppo iOS su {{site.data.keyword.cloud_notm}}, assicurati di soddisfare i seguenti requisiti.

### Sistema operativo
{: #swift-os-requirements}

La prassi migliore per lo sviluppo delle applicazioni Swift è di utilizzare l'hardware supportato MacOS più recente e di eseguire le attività di test con le release iOS più recenti. Registrati per un account [Apple Developer](https://developer.apple.com/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") per abilitare l'esecuzione di test su un dispositivo fisico.

### iOS e Xcode
{: #ios_and_xcode}

- Installa [Xcode 8+](https://developer.apple.com/xcode/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") (o superiore).
- Distribuisci ai [dispositivi iOS 8](https://support.apple.com/downloads/ios){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") (o superiore).
- Prima di inoltrare le applicazioni ad Apple, leggi attentamente le [linee guida sugli inoltri all'App Store](https://developer.apple.com/app-store/guidelines/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").

### Gestione di dipendenze ed SDK
{: #swift-sdk-management}

I seguenti strumenti assicurano che tu possa installare gli SDK nativi per lavorare con i vari servizi {{site.data.keyword.cloud_notm}}. Puoi utilizzare CocoaPods o Carthage per la gestione delle dipendenze.

* **Utilizzando CocoaPods** - installa [CocoaPods](https://cocoapods.org/) per supportare gli SDK {{site.data.keyword.cloud_notm}}:
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}

* **Utilizzando Carthage** - alcuni SDK sono anche disponibili tramite i gestori dipendenze [Carthage](https://github.com/Carthage/Carthage){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") o [Swift Package Manager](https://swift.org/package-manager/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno"). Per utilizzare Carthage per la gestione delle dipendenze, attieniti alla seguente procedura:

  Installa [Homebrew](https://brew.sh/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") come assistente per l'installazione di Carthage:
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

  Installa Carthage:
  ```
  brew install carthage
  ```
  {: codeblock}

## Passo 2. Creazione di un'applicazione Swift iOS personalizzata
{: #create-ios-app-swift}

1. Esegui l'accesso alla [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").
2. Fai clic su **Create app**.
3. Nella pagina [Empty Starter](https://cloud.ibm.com/developer/appledevelopment/create-app){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno"), puoi utilizzare la configurazione predefinita oppure aggiornare i campi come necessario. Assicurati che **iOS Swift** sia il linguaggio selezionato. Fai clic su **Create**.

## Passo 3. Aggiunta del servizio {{site.data.keyword.cloudant_short_notm}}
{: #resources-swift}

Puoi ora aggiungere i servizi alla tua applicazione Swift. Per questa esercitazione, aggiungi il servizio {{site.data.keyword.cloudant_short_notm}} alla tua applicazione Swift, che crea un database dei documenti `JSON` distribuito e completamente gestito. Cloudant fornisce alla tua applicazione scalabilità, alta disponibilità e durabilità in un framework leggero che tiene i tuoi dati al sicuro e sincronizzati.

1. Dalla pagina **App details**, fai clic su **Add service**.
2. Seleziona **Data** e fai clic su **Next**.
3. Seleziona **Cloudant** e fai clic su **Next**.
4. Fai clic su **Create**.
5. Una volta che il servizio è stato creato, fai clic su di esso per avviarlo. In questa nuova pagina, seleziona **Launch Cloudant Dashboard** per iniziare a creare un database e i documenti JSON.  In alternativa, tale operazione può essere eseguita in modo programmatico.

## Passo 4. Download del codice e configurazione degli SDK client
{: #run-locally-swift}

Per scaricare il codice, fai clic su **Download code** in `Apps` > `Your App`. Il codice scaricato viene fornito con incluso l'[SDK SwiftCloudant](https://github.com/cloudant/swift-cloudant){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") oltre che con del codice di inizializzazione di base. Gli SDK client sono disponibili su CocoaPods e Swift Package Manager. Questa soluzione utilizza CocoaPods.

1. Estrai il codice scaricato. Quindi, utilizzando un terminale, passa alla cartella estratta.
  ```
  cd <Name of Project>
  ```

2. La cartella include un podfile con le dipendenze richieste. Esegui il seguente comando per installare le dipendenze (SDK client):
  ```
  pod install
  ```
  {: codeblock}

## Passo 5. Configura l'applicazione per utilizzare il tuo nuovo database
{: #config-db-swift}

1. Apri il nome file che termina con `.xcworkspace` con Xcode e vai a `ViewController.swift`. `SwiftCloudant` è l'SDK Cloudant principale. `CouchDBClient` è una classe di `SwiftCloudant`, inizializzata in `ViewController.swift`.

  Apri sempre il nuovo spazio di lavoro Xcode, invece del file di progetto Xcode originale: `MyApp.xcworkspace`.
  {: tip}

2. Il codice di inizializzazione del database è già incluso, come mostrato nel seguente frammento di codice:
  ```swift
  /* Read the Cloudant credentials and intialize database connection */
  if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"), let dictionary = NSDictionary(contentsOfFile: contents) {
            let url = URL(string: dictionary["cloudantUrl"] as! String)
            let client = CouchDBClient(url:url!,
                                       username:dictionary["cloudantUsername"] as? String,
                                       password:dictionary["cloudantPassword"] as? String)
        }
  ```
  {: codeblock}

  Le credenziali del servizio fanno parte del file `BMSCredentials.plist`.
  {: note}

## Passo 6. Crea le tue operazioni di database
{: #build_ops-swift}

Ora che hai una connessione al database funzionante ed SDK configurato, puoi iniziare a creare le [operazioni di creazione, lettura, aggiornamento ed eliminazione](/docs/swift/data?topic=swift-cloudant#cloudant) di base per il tuo specifico caso d'uso.

## Passi successivi
{: #next-swift}

### Aggiunta di altri servizi
{: #moreresources-swift}

Puoi aggiungere altri servizi alla tua applicazione iOS direttamente dalla console web, come i seguenti servizi comunemente utilizzati:

* [Aggiunta del servizio Push Notifications](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate#gettingstartedtemplate)
* [Aggiunta dell'autenticazione utente con l'ID applicazione](/docs/services/appid?topic=appid-getting-started#getting-started)

### Utilizzo degli strumenti per sviluppatori {{site.data.keyword.cloud_notm}}
{: #devtools-swift}

Puoi anche imparare a sviluppare applicazioni Swift utilizzando gli [strumenti per gli sviluppatori {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli), che offrono un approccio da riga di comando per la creazione, lo sviluppo e la distribuzione di applicazioni del microservizio, mobili e web complete.
