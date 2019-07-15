---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: swift-cfenv, service bindings swift, environment swift, swift configuration, cloudenvironment swift, VCAP_SERVICES swift, swift credentials

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Configurazione dell'ambiente Swift
{: #configuration}

Lo sviluppo nativo cloud ha due prassi strettamente correlate che si intersecano nel modo in cui gestisci i dati di configurazione. La prima è che devi creare delle risorse immutabili per ridurre al minimo la probabilità di introdurre errori nel graduale passaggio della tua applicazione dallo sviluppo alla produzione. La seconda è che devi cercare la parità tra i tuoi ambienti di sviluppo e di produzione, per evitare i problemi creati dal codice specifico dell'ambiente. 

La gestione della configurazione e delle credenziali del servizio (bind dei servizi) varia tra le piattaforme. Cloud Foundry memorizza i dettagli del bind di servizio in un oggetto JSON in stringhe che viene passato all'applicazione come variabile di ambiente `VCAP_SERVICES`. Kubernetes memorizza i bind del servizio come JSON convertito in stringa o attributi semplici in `ConfigMaps` o `Secrets`, che possono essere passati all'applicazione inserita in un contenitore come variabili di ambiente o montati come un volume temporaneo. Lo sviluppo locale ha una propria configurazione poiché la verifica locale è spesso un derivato semplificato di qualsiasi cosa sia in esecuzione nel cloud. Lavorare su queste variazioni in modo trasportabile senza avere percorsi di codice specifici per l'ambiente può essere una sfida.

Puoi attenerti a semplici linee guida per aiutarti a scrivere delle applicazioni portatili e i programmi di utilità che puoi usare per incapsulare i bind di servizio di ricerca (o altra configurazione) in ubicazioni specifiche per l'ambiente.Sia che tu debba aggiungere il supporto cloud alle applicazioni esistenti oppure creare delle applicazioni con i kit starter, l'obiettivo è fornire la portabilità alle applicazioni Swift da utilizzare in molte piattaforme di distribuzione.

## Aggiunta di {{site.data.keyword.cloud_notm}} alle applicazioni Swift esistenti
{: #addcloud-env}

Il percorso per astrarre i valori di ambiente può differire da un ambiente cloud ad un altro. La libreria [CloudEnvironment](https://github.com/IBM-Swift/CloudEnvironment){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") astrae le credenziali e la configurazione dell'ambiente da vari provider cloud in modo che la tua applicazione Swift possa accedere in modo congruente alle informazioni mediante l'esecuzione in locale o in Cloud Foundry, Cloud Foundry Enterprise Environment, Kubernetes, {{site.data.keyword.openwhisk}} o nelle istanze virtuali. L'astrazione delle credenziali è fornita dalla libreria `CloudEnvironment`, che utilizza internamente [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") per la configurazione Cloud Foundry e [Configuration](https://github.com/IBM-Swift/Configuration){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") come un gestore della configurazione.

Con `CloudEnvironment`, puoi astrarre i dettagli di basso livello dal codice sorgente della tua applicazione definendo una chiave di ricerca che la tua applicazione Swift può utilizzare per cercare il suo valore corrispondente.

La libreria `CloudEnvironment` fornisce una chiave di ricerca congruente che può essere utilizzata nel tuo codice sorgente. La libreria effettua quindi la ricerca in un array di modelli di ricerca per trovare un oggetto JSON con gli attributi di configurazione o le credenziali del servizio. 

### Aggiunta del pacchetto CloudEnvironment alla tua applicazione Swift
{: #add-cloudenv}

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
{: #access-credentials}

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

Questo esempio fornisce l'accesso alla serie di credenziali per i servizi, che è ora possibile utilizzare per inizializzare le connessioni a questi [servizi supportati o a un dizionario generico](https://github.com/IBM-Swift/CloudEnvironment#supported-services){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"). Consulta [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") per la configurazione specifica di Cloud Foundry e i [dettagli della configurazione](https://github.com/IBM-Swift/Configuration){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") sul caricamento dei dati di configurazione.

## Descrizione delle credenziali del servizio
{: #service_creds}

La libreria `CloudEnvironment` utilizza un file denominato `mappings.json`, disponibile nella directory `config`, per comunicare dove sono memorizzate le credenziali per ciascun servizio. Il file `mappings.json` supporta la ricerca dei valori che utilizzano i seguenti tre tipi di modello di ricerca:
- **`cloudfoundry`** - Un tipo di modello utilizzato per ricercare un valore nella variabile di ambiente dei servizi di Cloud Foundry (`VCAP_SERVICES`). Per Cloud Foundry Enrprise Edition, vedi questa [esercitazione introduttiva](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started) per ulteriori informazioni.
- **`env`** - Un tipo di modello utilizzato per ricercare un valore associato a una variabile di ambiente, come in Kubernetes o Functions.
- **`file`** - Un tipo di modello utilizzato per ricercare un valore in un file JSON. Il percorso deve essere relativo alla cartella root della tua applicazione Swift.

Viene eseguita la ricerca nell'array dei valori associati a una chiave di ricerca nell'ordine in cui vengono visualizzati.

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

Per motivi di sicurezza, i file di credenziali non appartengono ai repository. Nell'esempio precedente, viene utilizzata una cartella `localdev` per memorizzare le credenziali locali per cui devi aggiungere questa cartella al file `.gitignore` per evitare commit accidentale. Se stai utilizzando un'applicazione kit starter, questa cartella viene creata per tuo conto ed è presente nel file `.gitignore`.

Per ulteriori informazioni sul file `mappings.json`, controlla la sezione [Comprensione delle credenziali del servizio](#service_creds).

## Utilizzo del gestore configurazione Swift dalle applicazioni kit starter
{: #configmanager-swift}

Le applicazioni Swift create con i [Kit starter](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") vengono automaticamente fornite con le credenziali e le configurazioni necessarie per l'esecuzione in locale ed anche in molte destinazioni di distribuzione cloud, ad esempio [Kubernetes](/docs/containers?topic=containers-getting-started), [Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf), [{{site.data.keyword.cfee_full_notm}}](/docs/cloud-foundry?topic=cloud-foundry-about), [Virtual Server (VSI)](/docs/vsi?topic=virtual-servers-getting-started-tutorial) o [{{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-getting_started).

  La distribuzione di VSI è disponibile per alcuni kit starter. Per utilizzare questa funzione, vai al [dashboard {{site.data.keyword.cloud_notm}}](https://{DomainName}) e fai clic su **Create an app** nel tile **Apps**.
  {: note}

La creazione di base del gestore configurazione è disponibile in `Sources/Application/Application.swift`. Quando crei un'applicazione kit starter basata su Swift con i servizi, vengono creati per tuo conto una cartella `config` e un file `mappings.json`. Se scarichi la tua applicazione, la cartella `config` include un file `localdev-config.json` che ha tutte le credenziali per i tuoi servizi ed è presente nel file `.gitignore`.

## Passi successivi
{: #next-configß notoc}

Controlla le nostre tre librerie per aiutare le tue applicazioni ad eseguire l'astrazione dai loro ambienti:

* [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")
* [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")
* [Configuration](https://github.com/IBM-Swift/Configuration){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")
