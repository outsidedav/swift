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

# Raccolta dell'analisi delle applicazioni mobili
{: #mobile_analytics}

{{site.data.keyword.mobileanalytics_short}} su {{site.data.keyword.cloud_notm}} fornisce agli sviluppatori, agli amministratori IT elle parti interessate alle attività dell'impresa un'analisi approfondita della qualità delle prestazioni delle applicazioni mobili e della loro modalità di utilizzo. Con il servizio {{site.data.keyword.mobileanalytics_short}} puoi:

 - **Ottenere un'analisi approfondita immediata** - vedi le metriche relative a prestazioni e utilizzo in tempo reale.

 - **Eseguire un'implementazione nel giro di qualche minuto** - crea un'istanza del servizio in {{site.data.keyword.cloud_notm}}, aggiungi l'SDK al tuo progetto, incolla due righe di codice nella tua applicazione e sei pronto a raccogliere dozzine di metriche predefinite.

 - **Raccogliere i dati che vuoi tu** - strumenta le applicazioni di eventi personalizzati, scopri in che modo gli utenti stanno interagendo con la tua applicazione, tieni traccia degli acquisti e monitora l'attività dell'applicazione.

 - **Visualizzare le metriche a colpo d'occhio** - la console {{site.data.keyword.mobileanalytics_short}} offre dei grafici già pronti, senza che occorra scrivere delle query.

 - **Concentrati su quello che è importante per te** - filtra le metriche in base a ora, adattatore, applicazione, versione dell'applicazione, sistema operativo, versione del sistema operativo o modello del dispositivo.

 - **Scoprire rapidamente eventuali problemi** - monitora lo stato degli arresti anomali. Imposta dei trigger di avviso sulle metriche critiche e instrada gli avvisi a qualsiasi endpoint REST.

 - **Risolvere i problemi alla loro causa radice** - utilizza la registrazione personalizzata nella tua applicazione e carica automaticamente i log ed esegui ricerche al loro interno dalla console. Esegui il drill-down sugli eventi di arresto anomalo per visualizzare le tracce di stack.

## Prima di cominciare

Assicurati innanzitutto di disporre dei seguenti prerequisiti pronti a essere utilizzati:

 - iOS 8.0+ / watchOS 2.0+
 - Xcode 7.3, 8.0
 - Swift 2.2 - 3.0
 - Cocoapods o Carthage

## Passo 1: Creazione di un'istanza di {{site.data.keyword.mobileanalytics_short}}
{: #mobile_analytics_create}

1. Nel catalogo {{site.data.keyword.cloud_notm}}, fai clic su **Mobile** > **{{site.data.keyword.mobileanalytics_short}}**. Viene visualizzata la schermata di configurazione del servizio.
2. Assegna un nome alla tua istanza del servizio oppure utilizza il nome preimpostato.
3. Fai clic su **Crea**.
4. Nel riquadro di navigazione, fai clic su **Connessioni** per selezionare un'applicazione ed eseguirne il bind al tuo servizio. Puoi eseguire il bind dell'istanza del servizio alla tua applicazione in un secondo momento, se la lasci senza bind durante la creazione.

## Passo 2: Installazione dell'SDK Swift iOS

Il servizio fornisce degli SDK specifici per la piattaforma per semplificare lo sviluppo delle applicazioni. Gli SDK Swift {{site.data.keyword.cloud_notm}} Mobile Services possono essere installati con Cocoapods o Carthage. Per ulteriori informazioni, vedi [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics).

Puoi strumentare la tua applicazione mobile utilizzando l'SDK {{site.data.keyword.mobileanalytics_full}}. L'SDK Swift è disponibile per iOS e watchOS.

1. Assicurati di aver configurato correttamente Xcode. Per informazioni su come configurare il tuo ambiente di sviluppo iOS, vedi il [sito web Developer di Apple![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.apple.com/support/xcode/){: new_window}. Leggi le informazioni sui [requisiti di Xcode![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#requirements){: new_window} per Client SDK Swift Analytics.

2. Abilita l'API di ubicazione aggiungendo una proprietà nel file `Info.plist` nella cartella del progetto della tua applicazione. Ad esempio, `Privacy - Location Usage Description` e dai una corretta giustificazione per l'aggiunta dell'API di ubicazione, come ad esempio "L'applicazione richiede che il servizio di ubicazione sia abilitato".

L'SDK {{site.data.keyword.mobileanalytics_short}} viene distribuito con [CocoaPods ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cocoapods.org/){: new_window} e [Carthage ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/Carthage/Carthage#getting-started){: new_window}, che sono gestori dipendenze per i progetti Cocoa. CocoaPods e Carthage scaricano automaticamente le risorse dai repository per renderle disponibili alla tua applicazione. Seleziona CocoaPods o Carthage:

### CocoaPods
{: #cocoapods}

1. Attieniti alle [istruzioni per l'SDK {{site.data.keyword.Bluemix_notm}} Mobile Services Swift ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#cocoapods){: new_window} su GitHub per installare `BMSAnalytics` utilizzando Cocoapods ed eseguine l'aggiunta al tuo Podfile.

2. Dopo l'installazione del'SDK client iOS, [importa e analizza](sdk.html#initalize-ma-sdk) l'SDK client Analytics.   

### Carthage
{: #carthage}

Se non stai utilizzando CocoaPods, puoi aggiungere dei framework al tuo progetto utilizzando [Carthage ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos){: new_window}.

1. Attieniti alle [istruzioni di installazione di Carthage![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#carthage){: new_window} su GitHub per installare `BMSAnalytics`.

2. Dopo che hai installato l'SDK client iOS, importa e quindi inizializza l'SDK client Analytics.

## Passo 3. Inizializzazione dell'SDK

Utilizzando {{site.data.keyword.mobileanalytics_short}}, puoi raccogliere le seguenti categorie di dati, ciascuna delle quali richiede un grado di strumentazione differente:

**Dati predefiniti**:
Questa categoria include le informazioni generiche su utilizzo e dispositivo valide per tutte le applicazioni. Indica il volume, la frequenza o la durata dell'uso dell'applicazione. I dati predefiniti vengono raccolti automaticamente dopo che hai inizializzato l'SDK {{site.data.keyword.mobileanalytics_short}} nella tua applicazione.

**Messaggi di log dell'applicazione**:
Questa categoria abilita lo sviluppatore ad aggiungere righe di codice in tutta l'applicazione che possono registrare messaggi personalizzati per facilitare lo sviluppo e il debug. Lo sviluppatore assegna un livello di severità/dettaglio a ogni messaggio di log.

**Eventi personalizzati**:
Questa categoria include i dati che definisci tu stesso e che sono specifici per la tua applicazione. Questi dati rappresentano gli eventi che si verificano nella tua applicazione, come le visualizzazioni di pagine, le selezioni di pulsanti o gli acquisti all'interno dell'applicazione. Oltre a inizializzare l'SDK {{site.data.keyword.mobileanalytics_short}} nella tua applicazione, devi aggiungere una riga di codice per ciascun evento personalizzato che vuoi tracciare.

## Passo 4. Identificazione del valore della tua chiave API delle credenziali del servizio
{: #analytics-clientkey}

Identifica il tuo valore **Chiave API** prima di impostare l'SDK client. La chiave API è richiesta per inizializzare l'SDK client.

1. Apri il tuo dashboard del servizio {{site.data.keyword.mobileanalytics_short}}.
2. Espandi **Visualizza credenziali** per visualizzare il tuo valore della chiave API. Ti serve il valore della chiave API quando inizializzi l'SDK client {{site.data.keyword.mobileanalytics_short}}.

## Passo 5. Inizializzazione della tua applicazione per raccogliere l'analisi 
{: #initalize-ma-sdk}

Inizializza la tua applicazione per abilitare l'invio di log al servizio {{site.data.keyword.mobileanalytics_short}}.

1. Importa l'SDK client.

    L'SDK Swift è disponibile per iOS e watchOS. Importa i framework `BMSCore` e `BMSAnalytics` aggiungendo le seguenti istruzioni `import` all'inizio del tuo file di progetto `AppDelegate.swift`:
	```
	import BMSCore
	import BMSAnalytics
	```
	{: codeblock}  

2. Inizializza l'SDK client {{site.data.keyword.mobileanalytics_short}} nella tua applicazione.

	Per prima cosa, inizializza la classe `BMSClient` inserendo il codice di inizializzazione nel metodo `application(_:didFinishLaunchingWithOptions:)` del tuo delegato dell'applicazione oppure in un'ubicazione che ritieni più adatta per il tuo progetto.
	```
	BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Make sure that you point to your region
	```
	{: codeblock}

	Devi inizializzare il `BMSClient` con il parametro **bluemixRegion**. Nell'inizializzatore, il valore **bluemixRegion** specifica quale distribuzione {{site.data.keyword.Bluemix_notm}} stai utilizzando.

3. Avvia Analytics utilizzando il tuo oggetto applicazione e fornendo ad esso il nome della tua applicazione.

	Il nome che hai selezionato per la tua applicazione (`your_app_name_here`) viene visualizzato nella console {{site.data.keyword.mobileanalytics_short}} come il nome dell'applicazione. Il nome applicazione viene utilizzato come un filtro per cercare i log dell'applicazione nel dashboard. Quando utilizzi lo stesso nome dell'applicazione su varie piattaforme (ad esempio iOS), puoi vedere tutti i log da tale applicazione sotto lo stesso nome, indipendentemente da quale sia la piattaforma da cui sono stati inviati i log.

	Ti serve anche il valore [Chiave API](#analytics-clientkey).
	```
	Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, collectLocation: true, deviceEvents: .lifecycle, .network)
	```
    {: codeblock}

4. L'applicazione è ora inizializzata e pronta a raccogliere l'analisi. Puoi quindi inviare i dati di analisi al servizio {{site.data.keyword.mobileanalytics_short}}.		

## Passo 6: Raccolta dell'analisi di utilizzo
{: #app-monitoring-gathering-analytics}

Puoi configurare l'SDK client {{site.data.keyword.mobileanalytics_short}} per registrare l'analisi dell'utilizzo e inviare i dati registrati al servizio {{site.data.keyword.mobileanalytics_short}}.

Utilizza le seguenti API per avviare la registrazione e inviare l'analisi di utilizzo:
```
// Disable recording of usage analytics (eg: to save disk space)
// Recording is enabled by default

Analytics.isEnabled = false

// Enable recording of usage analytics

Analytics.isEnabled = true

// Send recorded usage analytics to the Mobile Analytics Service

Analytics.send(completionHandler: { (response: Response?, error: Error?) in
    if let response = response {

	  // Handle Analytics send success here.
    }
    if let error = error {
        // Handle Analytics send failure here.
    }
})
```
{: codeblock}

Analisi di utilizzo di esempio per la registrazione di un evento:
```
// Log a custom analytics event
let eventObject = ["customProperty": "propertyValue"]
Analytics.log(metadata: eventObject)
```
{: codeblock}

## Passo 7. Utilizzo del logger

L'SDK client {{site.data.keyword.mobileanalytics_full}} fornisce un framework di registrazione simile ad altri framework di log con cui potresti avere dimestichezza, come `java.util.logging` o `log4j`. Il framework di registrazione supporta più istanze logger per pacchetto, diversi livelli di log, l'acquisizione di tracce di stack per l'arresto anomalo di un'applicazione e altro.

Puoi anche configurare i dati registrati perché vengano archiviati sul dispositivo dove è in esecuzione l'applicazione e inviare i log del dispositivo al servizio {{site.data.keyword.mobileanalytics_short}} successivamente.

Il framework di registrazione dell'SDK client {{site.data.keyword.mobileanalytics_short}} supporta i seguenti livelli di log, che sono elencati dal meno dettagliato al più dettagliato, con le linee guida d'uso consigliate:

  * `FATAL` - utilizzalo per i blocchi o gli arresti anomali irreversibili. Il livello `FATAL` è riservato per la registrazione degli errori irreversibili, che si presentano agli utenti come un arresto anomalo dell'applicazione
  * `ERROR` - utilizzalo per le eccezioni impreviste o gli errori di protocollo di rete imprevisti
  * `WARN` - utilizzalo per registrare le avvertenze di utilizzo che non sono considerate degli errori critici, come l'utilizzo di API ritenute obsolete o una risposta di rete lenta
  * `INFO` - utilizzalo per la notifica di eventi di inizializzazione e altri dati che potrebbero essere importanti senza però essere urgenti
  * `DEBUG` - utilizzalo per la notifica delle istruzioni di debug per aiutare gli sviluppatori a risolvere i difetti delle applicazioni

### Scenario di livello di log
{: #log-level-scenario}

Quando il livello del logger è configurato in `FATAL`, il logger acquisisce le eccezioni non rilevate, ma non acquisisce alcun log che porti all'evento di arresto anomalo. Puoi impostare un livello del logger più dettagliato per accertarti che vengano acquisiti anche log che possano portare a una voce di logger `FATAL`, come `WARN`
e `ERROR`.

Quando il livello del logger è impostato su `DEBUG`, puoi ottenere anche i log dell'SDK client Mobile Analytics, che sono inclusi quando invii i log.

### Utilizzo del logger di esempio
{: #sample-logger-usage}

**Nota:** assicurati che la tua applicazione sia configurata per utilizzare l'SDK client {{site.data.keyword.mobileanalytics_short}} prima di utilizzare il framework di registrazione.

I seguenti frammenti di codice mostrano un utilizzo di esempio del Logger:
```
// Configure Logger. Disabled by default;

Logger.isLogStorageEnabled = true

// Change the minimum log level (optional). Default is - LogLevel.debug

Logger.logLevelFilter = LogLevel.info

// Multiple log instances can be created to organize your logs

let logger1 = Logger.logger(name: "feature1Logger")
let logger2 = Logger.logger(name: "feature2Logger")

// Log messages with different levels

logger1.debug(message: "debug message for feature 1")

// logger1.debug message isn't logged as logLevelFilter is set to info

logger2.info(message: "info message for feature 2")

// Send logs to the Mobile Analytics Service

Logger.send(completionHandler: { (response: Response?, error: Error?) in
        if let response = response {
        logger.debug(message: "Status code: \(response.statusCode)")
        logger.debug(message: "Response: \(response.responseText)")
    }
    if let error = error {
        logger.error(message: "Error: \(error)")
    }
})
```
{: codeblock}

Per motivi di riservatezza, puoi disabilitare l'output del Logger per le applicazioni create in modalità di rilascio. Per impostazione predefinita, la classe Logger stampa i log sulla console Xcode. Nelle impostazioni di creazione per la tua destinazione, aggiungi un indicatore `-D RELEASE_BUILD` alla sezione **Other Swift Flags** della configurazione della build di rilascio.
{: tip}

## Passo 8. Registrazione dei dati di ubicazione
{: #location-logging}

L'ubicazione del dispositivo mobile potrebbe essere registrata dall'applicazione tramite questa API fornita.
```
Analytics.logLocation();
```

L'API abilita l'applicazione a inviare la sua ubicazione (come latitudine e longitudine) al server tra una sessione e l'altra dell'applicazione. Oltre alle chiamate esplicite all'API `location-logging`, l'SDK invia l'ubicazione del dispositivo per ogni App-Session. L'ubicazione viene inviata per il contesto della sessione dell'applicazione iniziale e il contesto della sessione dell'applicazione di switch dell'utente (ossia uno switch di un utente tra una sessione dell'applicazione). L'API di ubicazione deve essere abilitata nell'inizializzazione dell'SDK.

Per richiamare l'API `location-logging`, imposta il parametro `collectLocation` su `true` nell'inizializzazione dell'SDK:
```
Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
```

## Passo 9. Esecuzione di una richiesta di rete
{: #network-requests}

Puoi configurare l'SDK client {{site.data.keyword.mobileanalytics_short}} per [effettuare una richiesta di rete](/docs/mobile/sdk_network_request.html). Assicurati di aver inizializzato `BMSClient` e `BMSAnalytics` e importato gli SDK client.

## Passo 10. Notifica dell'analisi degli arresti anomali
{: #report-crash-analytics}

Puoi visualizzare i [dati sugli arresti anomali dell'applicazione](app-monitoring.html#monitor-app-crash) inviando le analisi e le informazioni di log a {{site.data.keyword.mobileanalytics_short}}.

Il metodo `Analytics.send()` popola le tabelle **Crash Overview** e **Crashes** nella pagina **Crashes**. I grafici sono abilitati utilizzando l'inizializzazione e inviando il processo per l'analisi; non è necessaria alcuna configurazione speciale.

Il metodo `Logger.send()` popola le tabelle **Crash Summary** e **Crash Details** nella pagina **Troubleshooting**. Devi abilitare la tua applicazione ad archiviare e inviare i log per popolare i grafici aggiungendo un'istruzione nel tuo codice dell'applicazione:
```
Logger.isLogStorageEnabled = true
Logger.logLevelFilter = LogLevel.Fatal // or greater
```
{: codeblock}

Vedi l'[utilizzo del logger di esempio](sdk.html##sample-logger-usage) di iOS.

## Passo 11. Traccia degli utenti attivi
{: #trackingusers}

Se la tua applicazione può riconoscere degli utenti univoci su un dispositivo, puoi tracciare quanti utenti stanno attivamente utilizzando l'applicazione passando il nome utente dell'utente attivo a {{site.data.keyword.mobileanalytics_short}}.

Abilita il tracciamento dell'utente inizializzando {{site.data.keyword.mobileanalytics_short}} con `hasUserContext=true`. In caso contrario, {{site.data.keyword.mobileanalytics_short}} acquisisce solo un singolo utente per dispositivo.

Aggiungi il seguente codice per tracciare quando l'utente esegue l'accesso:
```
Analytics.userIdentity = "username"
```
{: codeblock}

## Passo 12. Verifica della tua applicazione
{: #appid_testing}

È tutto configurato correttamente? È arrivato il momento di eseguire le verifiche.

1. Apri la tua applicazione. Se hai un'applicazione web, utilizza un browser. Se hai un'applicazione client iOS, aprila con l'emulatore Xcode.
2. Compila ed esegui l'applicazione sul tuo emulatore o sul tuo dispositivo.
3. Utilizzando la GUI, segui il processo di accesso alla tua applicazione.
4. Vai alla console {{site.data.keyword.mobileanalytics_short}} per visualizzare l'analisi di utilizzo per la tua applicazione. Puoi anche monitorare la tua applicazione [impostando degli avvisi](/docs/services/mobileanalytics/app-monitoring.html#alerts) e [monitorando gli arresti anomali delle applicazioni](/docs/services/mobileanalytics/app-monitoring.html#monitor-app-crash).

## Operazioni successive
{: #what-to-do-next notoc}

 - Per ulteriori informazioni sul servizio, leggi attentamente la [documentazione](/docs/services/mobileanalytics/index.html#getting-started-tutorial).

 - Per un'introduzione alla gestione dei servizi mobili e {{site.data.keyword.Bluemix_notm}}, vedi l'[introduzione alle applicazioni mobili su IBM Cloud](/docs/services/mobile/index.html).

 - I kit starter sono uno dei modi più rapidi per avvalerti delle funzionalità di {{site.data.keyword.cloud_notm}}. Visualizza tutti i kit starter disponibili nel [dashboard degli sviluppatori di applicazioni mobili](https://console.bluemix.net/developer/mobile/dashboard). Scarica il codice. Esegui l'applicazione!

 - Puoi utilizzare l'[IU Swagger](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/) per esaminare rapidamente la documentazione dell'API REST.
