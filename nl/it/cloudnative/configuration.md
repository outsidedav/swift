---

copyright:
  years: 2018
lastupdated: "2018-11-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configurazione dell'ambiente Swift
{: #configuration}

Lo sviluppo nativo cloud ha due prassi strettamente correlate che si intersecano nel modo in cui gestisci i dati di configurazione. La prima è che devi creare delle risorse immutabili per ridurre al minimo la probabilità di introdurre errori nel graduale passaggio della tua applicazione dallo sviluppo alla produzione. La seconda è che devi sforzarti di ottenere una parità tra gli ambienti di sviluppo e produzione per evitare problemi creati da codice specifico per l'ambiente. 

La gestione della configurazione del servizio e delle credenziali (bind di servizio) varia da piattaforma a piattaforma. Cloud Foundry memorizza i dettagli del bind di servizio in un oggetto JSON in stringhe che viene passato all'applicazione come variabile di ambiente `VCAP_SERVICES`. Kubernetes memorizza i bind di servizio come attributi semplici o JSON in stringhe in `ConfigMaps` o `Secrets` che possono essere passati all'applicazione inserita nel contenitore come variabili di ambiente o montati come un volume temporaneo. Lo sviluppo locale ha una propria configurazione poiché la verifica locale è spesso un derivato semplificato di qualsiasi cosa sia in esecuzione nel cloud. Lavorare tra queste variazioni in modo portatile senza avere percorsi di codice specifico dell'ambiente può essere difficile.

Puoi attenerti a semplici linee guida per aiutarti a scrivere delle applicazioni portatili e i programmi di utilità che puoi usare per incapsulare i bind di servizio di ricerca (o altra configurazione) in ubicazioni specifiche per l'ambiente. Sia che tu debba aggiungere il supporto cloud alle applicazioni esistenti oppure creare delle applicazioni con i kit starter, l'obiettivo è fornire la portabilità alle applicazioni Swift da utilizzare in molte piattaforme di distribuzione.

## Aggiunta di {{site.data.keyword.cloud_notm}} alle applicazioni Swift esistenti
{: #addcloud-env}

Il percorso per astrarre i valori di ambiente può differire da un ambiente cloud ad un altro. La libreria [CloudEnvironment](https://github.com/IBM-Swift/CloudEnvironment.git) astrae le credenziali e la configurazione dell'ambiente da vari provider cloud in modo che la tua applicazione Swift possa accedere in modo congruente alle informazioni mediante esecuzione in locale o in Cloud Foundry, Kubernetes o {{site.data.keyword.openwhisk}}. L'astrazione delle credenziali è fornita dalla libreria `CloudEnvironment`, che utilizza internamente [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv) per la configurazione Cloud Foundry e [Configuration](https://github.com/IBM-Swift/Configuration) come gestore configurazione.

Con `CloudEnvironment`, puoi astrarre i dettagli di basso livello dal codice sorgente della tua applicazione definendo una chiave di ricerca che la tua applicazione Swift può utilizzare per cercare il suo valore corrispondente.

La libreria `CloudEnvironment` fornisce una chiave di ricerca congruente che può essere utilizzata nel tuo codice sorgente. La libreria effettua quindi la ricerca in un array di modelli di ricerca per trovare un oggetto JSON con gli attributi di configurazione o le credenziali del servizio. 

### Aggiunta del pacchetto CloudEnvironment alla tua applicazione Swift
Per utilizzare il pacchetto `CloudEnvironment` nella tua applicazione Swift, specificalo nella sezione **dependencies:** del tuo file `Package.swift`.
```swift
.package(url: "https://github.com/IBM-Swift/CloudEnvironment.git", from: "8.0.0"),
```
{: codeblock}

Aggiungi quindi il seguente codice di strumentazione alla tua applicazione.
```swift
import CloudEnvironment

let cloudEnv = CloudEnv()
```
{: codeblock}

### Accesso alle credenziali
Ora che la libreria `CloudEnvironment` è inizializzata, puoi accedere alle tue credenziali come mostrato nei seguenti esempi:
```swift
let cloudantCredentials = cloudEnv.getCloudantCredentials(name: "cloudant-credentials")
// cloudantCredentials.username, cloudantCredentials.password, cloudantCredentials.url, etc.
let objStorageCredentials = cloudEnv.getObjectStorageCredentials(name: "object-storage-credentials")
// objStorageCredentials.username, objStorageCredentials.password, objStorageCredentials.projectID, etc.

let service1Credentials = cloudEnv.getDictionary("service1-credentials")
let service1CredentialsStr = cloudEnv.getString("service1-credentials")

// Get a default PORT and URL
let port = cloudEnv.port
let url = cloudEnv.url
```
{: codeblock}

Questo esempio fornisce l'accesso alla serie di credenziali per i servizi, che è ora possibile utilizzare per inizializzare le connessioni a questi [servizi supportati o a un dizionario generico](https://github.com/IBM-Swift/CloudEnvironment#supported-services). Controlla [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api) per la configurazione specifica per Cloud Foundry e i [dettagli di configurazione](https://github.com/IBM-Swift/Configuration) sul caricamento dei dati di configurazione.

## Descrizione delle credenziali del servizio
{: #service_creds}

La libreria `CloudEnvironment` utilizza un file denominato `mappings.json`, disponibile nella directory `config`, per comunicare dove sono memorizzate le credenziali per ciascun servizio. Il file `mappings.json` supporta la ricerca di valori che utilizzano i seguenti tre tipi di pattern di ricerca:
- **`cloudfoundry`** - un tipo di pattern utilizzato per cerca un valore nella variabile di ambiente dei servizi di Cloud Foundry (`VCAP_SERVICES`).
- **`env`** - un tipo di pattern utilizzato per cercare un valore che è associato a una variabile di ambiente, come ad esempio in Kubernetes o Functions.
- **`file`** - un tipo di pattern utilizzato per cercare un valore in un file JSON. Il percorso deve essere relativo alla cartella root della tua applicazione Swift.

L'array di valori associati a una chiave di ricerca viene ricercato nell'ordine in cui tali valori vengono visualizzati.

Di seguito viene riportato un esempio di un file `mappings.json`:
```javascript
{
    "cloudant-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-cloudant-db",
                "env:my_awesome_cloudant_db_credentials",
                "file:localdev/my-awesome-cloudant-db-credentials.json"
            ]
        }
    },
    "object-storage-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-object-storage",
                "env:my_awesome_object_storage_credentials",
                "file:localdev/my-awesome-object-storage-credentials.json"
            ]
        }
    }
}
```
{: codeblock}

In questo esempio, `cloudant-credentials` e `object-storage-credentials` sono le chiavi di ricerca utilizzate dalla tua applicazione Swift per cercare le credenziali corrispondenti. In base all'array del modello di ricerca, il gestore configurazione cerca prima `my-awesome-cloudant-db` in `VCAP_SERVICES` e cerca quindi `my_awesome_cloudant_db_credentials` come una variabile di ambiente. Se non è stata definita, cerca il contenuto di `my-awesome-object-storage-credentials.json` nella cartella `localdev`. 

Quando l'applicazione viene eseguita localmente, puoi utilizzare le credenziali memorizzate in un file, come `localdev/my-awesome-object-storage-credentials.json` come mostrato nell'esempio precedente. Le informazioni di connessione per i servizi per cui desideri l'accesso in locale, quali nome utente, password e nome host, sono tutti da memorizzare in questo file. 

Per motivi di sicurezza, i file di credenziali non appartengono ai repository. Nell'esempio precedente, una cartella `localdev` viene utilizzata per memorizzare le credenziali locali, quindi devi aggiungere tale cartella al file `.gitignore` per evitare un commit accidentale. Se stai utilizzando un'applicazione kit starter, questa cartella viene creata per tuo conto ed è presente nel file `.gitignore`.

Per ulteriori informazioni sul file `mappings.json`, controlla la sezione [Descrizione delle credenziali del servizio ](configuration.html#service_creds).

## Utilizzo del gestore configurazione Swift dalle applicazioni kit starter

Le applicazioni Swift create con i [kit starter](https://console.bluemix.net/developer/appledevelopment/starter-kits/) sono automaticamente fornite con le credenziali e la configurazione necessarie per l'esecuzione in locale e anche in molti ambienti di sviluppo Cloud (CF, K8s, VSI e Functions). La creazione di base del gestore configurazione è disponibile in `Sources/Application/Application.swift`. Quando crei un'applicazione kit starter basata su Swift con i servizi, vengono creati per tuo conto una cartella `config` e un file `mappings.json`. Se scarichi la tua applicazione, la cartella `config` include un file `localdev-config.json` che ha tutte le credenziali per i tuoi servizi ed è presente nel file `.gitignore`.

## Passi successivi
{: #next notoc}

Controlla le nostre tre librerie per aiutare le tue applicazioni ad eseguire l'astrazione dai loro ambienti:

* [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env)
* [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv)
* [Configuration](https://github.com/IBM-Swift/Configuration)
