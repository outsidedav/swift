---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-21"

keywords: foodtrackerbackend, kitura swift, urlsession sdk, alamofire, restkit, kiturakit, kitura, xcode kitura, meals swift, rest api kitura, rest api swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Creazione di un'applicazione con Kitura
{: #kitura}

[Kitura](http://www.kitura.io){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") è un framework Swift lato server per creare applicazioni web e backend iOS. Questo framework crea delle API REST che possono essere richiamate dall'applicazione iOS utilizzando gli SDK URLSession quali Alamofire, RestKit oppure l'SDK [KituraKit](https://github.com/ibm-swift/kiturakit){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") fornito da Kitura stesso.

Kitura è in grado di integrarsi con tutti i servizi e tutte le funzioni forniti da {{site.data.keyword.cloud}}, compresi {{site.data.keyword.appid_short}}, {{site.data.keyword.mobilepushshort}} e {{site.data.keyword.mobileanalytics_short}}, così come con database, machine learning e altri servizi. Kitura può quindi essere distribuito e ridimensionato automaticamente utilizzando le piattaforme Cloud Foundry o Docker (basata su Kubernetes) in {{site.data.keyword.cloud}}.

Kitura fornisce una `kitura` [CLI (command line interface)](https://www.kitura.io/guides/kituracli/gettingstarted.html){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") che semplifica la creazione, lo sviluppo, la verifica e la distribuzione di applicazioni Kitura. Le applicazioni sviluppate utilizzando la CLI Kitura includono il pieno supporto per la distribuzione a qualsiasi cloud che supporta le tecnologie Cloud Foundry, Docker e Kubernetes. Tuttavia, se stai eseguendo attività di creazione specificamente per {{site.data.keyword.cloud_notm}}, ti consigliamo di utilizzare la IBM Apple Development Console nel browser o di utilizzare la {{site.data.keyword.dev_cli_notm}}. Inoltre, mentre entrambi i metodi condividono la tecnologia sottostante, la Apple Development Console e IBM Developer Tools creano per tuo conto un'applicazione ospitata e una pipeline di distribuzione, oltre ad eseguire il provisioning dei servizi di cui la tua applicazione ha bisogno.

## Prima di cominciare
{: #prereqs-kitura}

Assicurati innanzitutto di disporre dei seguenti prerequisiti pronti a essere utilizzati:  

* iOS 11.0+  
* Xcode 9.0  
* Swift 4.0+  
* CocoaPods  

## Passo 1. Creazione di un'applicazione Kitura utilizzando il browser
{: #create_kitura}

1. Vai alla sezione Kit starter della Apple Development Console. Seleziona uno starter predefinito, come ad esempio "Swift for Backend for Frontend API" oppure crea un'applicazione personalizzata utilizzando il tile **Create app** . Fai clic su **Create app**.

2. Dai alla tua applicazione un nome e seleziona dove vuoi che venga distribuita la tua applicazione. Se non sei sicuro di dove deve essere distribuita l'applicazione, utilizza i valori predefiniti poiché è possibile modificarli in un secondo momento.

3. Seleziona il linguaggio Swift. Viene creata un'applicazione Swift lato server. Sono visualizzate anche le opzioni Host e Domain, che formano l'URL per l'applicazione. Se non sei sicuro, utilizza i valori predefiniti; puoi anche fornire un tuo dominio personalizzato da un provider di domini dove l'applicazione deve risiedere.

4. Puoi fornire una definizione OpenAPI (nota anche come Swagger) per l'API REST che vuoi eseguire l'attività di creazione. Se disponi di una tale definizione, viene creata una API REST che include le funzioni di gestore corrispondenti in Kitura. Se non hai una definizione OpenAPI, non preoccuparti poiché Kitura ti aiuta a creare facilmente in modo manuale le API REST utilizzando le sue API Router.

5. Fai clic su **Create app**.

Viene creata un'applicazione che però non utilizza ancora alcun servizio aggiuntivo. Puoi aggiungere dei servizi facendo clic sul pulsante **Add service** o **Download code** per ottenere il codice per l'applicazione. Puoi anche aggiungere facilmente dei servizi a un'applicazione esistente.

## Passo 2. Aggiunta di servizi
{: #add_services-kitura}

1. Fai clic su **Add service** per aggiungere i servizi. Vengono visualizzate le categorie del servizio. Ad esempio, seleziona **Data** per visualizzare i database disponibili e seleziona **Cloudant NoSQL DB**.
2. Seleziona un piano di prezzi per il servizio, ad esempio Lite, e fai clic su **Create**.

Viene creata un'istanza del servizio che fornisce le credenziali per l'applicazione e, in alcuni casi, aggiunge il codice necessario alla tua applicazione per includere la corretta connessione al servizio. Puoi ora aggiungere ulteriori servizi utilizzando il pulsante **Add service** o facendo clic su **Download code** per ottenere il codice per l'applicazione.

Dopo che hai scaricato la tua applicazione, puoi iniziare a utilizzarla.

## Passo 3. Sviluppo della tua applicazione con Xcode
{: #develop_xcode-kitura}

Dopo che hai scaricato il tuo codice dell'applicazione, puoi modificarlo e svilupparlo utilizzando Xcode e caricare quindi l'applicazione modificata per la distribuzione al cloud.

1. Crea un progetto Xcode.  
  Prima di poter utilizzare il tuo progetto in Xcode, devi creare un file di progetto Xcode utilizzando il comando del gestore pacchetti Swift. Esegui il seguente comando dalla directory root del tuo progetto scaricato, dove risiede il file `Package.swift`:
  ```
  swift package generate-xcodeproj
  ```
  {: codeblock}

  Un progetto Xcode viene creato nella stessa directory che puoi quindi aprire.

2. Imposta la destinazione Xcode per il progetto.  
  Per eseguire il server Kitura, devi modificare lo schema facendo clic sulla sezione **project_name-Package** sulla barra degli strumenti e selezionando la destinazione **project_name** dal menu. Controlla che il dispositivo di destinazione sia impostato su **My Mac**.

3. Esegui il server Kitura localmente. 
  Fai clic su **Run** oppure utilizza la scelta rapida da tastiera `⌘+R` per avviare il server Kitura. Una volta avviato il server, puoi controllare che i seguenti URL Kitura standard siano in esecuzione.
  * Kitura Monitoring: [http://localhost:8080/swiftmetrics-dash/]()
  * Kitura Health check: [http://localhost:8080/health]()

## Passo 4. Aggiunta di API REST
{: #add_restapi-kitura}

Viene creato un server Kitura di base che però non fornisce alcuna API REST che può essere utilizzata da un'applicazione iOS. Aggiungi le API REST in Kitura con una codifica minima. Utilizza la seguente procedura per aggiungere un'API REST per le richieste `GET` su `/meals`, che è progettata per restituire gli oggetti `Meal` archiviati dal server Kitura.

1. Aggiungi uno struct `Meal` al file `Sources/Application/Application.swift`:  
  Prima della dichiarazione della classe `App`, aggiungi quanto segue per creare uno struct Meal conforme al protocollo Codable:  
  ```swift
	struct Meal: Codable {    
	    var name: String
	    var photo: Data
	    var rating: Int
	}
  ```
  {: codeblock}

2. Aggiungi un dizionario al file `Sources/Application/Application.swift` per archiviare gli oggetti `Meal` aggiungendo `let cloudEnv = CloudEnv()` alla seguente sezione di codice:
  ```swift
  private var mealStore: [String: Meal] = [:]
  ```
  {: codeblock}

3. Aggiungi un gestore per le richieste `GET` su `/meals` al file `Sources/Application/Application.swift` aggiungendo il seguente codice nella funzione `postInit()`:  
  ```swift
  router.get("/meals", handler: loadHandler)
  ```
  {: codeblock}

4. Implementa la funzione loadHandler al file `Sources/Application/Application.swift` aggiungendo il seguente codice come un'altra funzione nella classe `App`:
  ```swift
  func loadHandler(completion: ([Meal]?, RequestError?) -> Void ) {
      let meals: [Meal] = self.mealStore.map({ $0.value })
    completion(meals, nil)
  }
  ```
  {: codeblock"}

Ora hai una API REST per le richieste `GET` su `/meals` che risponde con un array di `Meals` o un errore.

5. Esegui il progetto.  
  Fai clic su **Run** oppure utilizza la scelta rapida da tastiera `⌘+R`. Se ti viene richiesto, seleziona **Allow incoming network connections**.

6. Verifica l'API REST utilizzando una richiesta `GET`, che corrisponde al modo in cui i browser web richiedono dati da un server. 

  Puoi verificare l'API REST utilizzando il seguente URL:  
  ```swift
  * `GET /meals`:	[http://localhost:8080/meals]()
  ```
  {: codeblock}

  Viene restituito un array vuoto `[]` perché attualmente non è archiviato alcun oggetto `Meal` nel `mealStore`. 

Puoi utilizzare l'esercitazione [FoodTracker Backend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"), che ti aiuta a creare una serie di API REST per l'archiviazione, il recupero e l'eliminazione di oggetti `Meal` da un'applicazione iOS, compresa l'archiviazione dei dati in un database.
{: tip}

## Passo 5. Installazione di KituraKit nella tua applicazione iOS
{: #install-kiturakit}

Le API REST sviluppate utilizzando il server Kitura sono API web standard, utilizzabili da qualsiasi applicazione indipendentemente dalla libreria client utilizzata o dal linguaggio in cui è scritto il client. Questo significa che puoi utilizzare Alamofire, RestKit o URLSession per stabilire delle connessioni al server. Kitura fornisce anche un connettore client ottimizzato e personalizzato per semplificare il richiamo delle sue API REST da iOS, sotto forma di KituraKit. 

KituraKit fornisce un'immagine di mirror delle API del gestore del router utilizzate in Kitura, rendendo possibile la condivisione di tipi Swift tra il client e il server con un minimo sforzo. Questa funzione rimuove l'esigenza di eseguire esplicitamente la codifica e la decodifica JSON dei dati inviati al, o ricevuti dal, server.

I seguenti passi mostrano come installare KituraKit nella tua applicazione iOS e come utilizzarla per richiamare l'API REST `GET /meals` creata utilizzando Kitura:

1. Crea un Podfile nella root della tua directory dell'applicazione iOS, se non ne hai già uno:
  ```
  pod init
  ```
  {: codeblock}

2. Modifica il Podfile per impostare una piattaforma globale di iOS 11 per il tuo progetto sostituendo la seguente riga:
  ```pod
  # platform :ios, '9.0'
  ```
  {: codeblock}

  Con il seguente codice:
	```pod
  platform :ios, '11.0'
  ```
  {: codeblock}

3. Aggiungi KituraKit al Podfile aggiungendo quanto segue sotto `# Pods for <application name>`:
  ```pod
  pod 'KituraKit', :git => 'https://github.com/IBM-Swift/KituraKit.git', :branch => 'pod'
  ```
  {: codeblock}

4. Salva il Podfile e installa KituraKit utilizzando il seguente comando:
  ```
  pod install
  ```
  {: codeblock}

5. Riapri l'applicazione in Xcode aprendo lo spazio di lavoro (non il progetto). Puoi ora aggiungere la stessa definizione di `Meal` alla tua applicazione iOS come viene utilizzata nel server Kitura:
  ```swift
  struct Meal: Codable {    
      var name: String
      var photo: Data
      var rating: Int
  }
  ```
  {: codeblock}

  E utilizza il seguente codice per stabilire una connessione al server Kitura:
  
  ```swift
  import KituraKit
  
  /* Connect to the Kitura server on http://localhost:8080 */
  guard let client = KituraKit(baseURL: "http://localhost:8080") else {
      print("Error creating KituraKit client")
      return
  }
  
  /* Make a request to `GET /meals`, expecting a [Meal] or a RequestError */
  client.get("/meals") { (meals: [Meal]?, error: RequestError?) in
      /* Check for an error */
      guard error == nil else {
          print("Error saving meal to Kitura: \(error!)")
          return
      }
      /* Check for meals */
      guard let meals = meals else {
          print("Received Meals!")
          return
      }
  }
  ```
  {: codeblock}

Il server Kitura viene richiamato utilizzando `client.get("/meals")` con un callback che corrisponde ai parametri dal gestore di completamento di Kitura.

Puoi utilizzare l'esercitazione [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"), che ti aiuta a creare una serie di API REST per l'archiviazione, il recupero e l'eliminazione di oggetti `Meal` da un'applicazione iOS, compresa l'archiviazione dei dati in un database.
{: tip}

## Passi successivi
{: #next-kitura notoc}

Ora che hai un server Kitura che fornisce un'API REST che può essere richiamata dalla tua applicazione iOS, sei pronto a distribuire il tuo server a {{site.data.keyword.cloud_notm}}. [Le distribuzioni](/docs/swift?topic=swift-deploy_apps-swift#deploy_apps-swift) possono essere eseguite utilizzando i contenitori con Kubernetes, i contenitori protetti, Cloud Foundry, Cloud Foundry Enterprise Environment o le istanze virtuali.
