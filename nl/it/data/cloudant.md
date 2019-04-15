---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-14"

keywords: cloudant swift, store data swift, dbaas swift, cloudant instance swift, initialize sdk swift, create document swift, read document swift, delete document swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Archiviazione di documenti in {{site.data.keyword.cloud_notm}}
{: #cloudant}

{{site.data.keyword.cloudantfull}} è un DBaaS (DataBase as a Service) orientato ai documenti. Archivia i dati come documenti in formato JSON. È stato messo a punto tenendo presenti scalabilità, alta disponibilità e durabilità ed è facile da configurare per l'utilizzo nelle applicazioni Swift. Viene fornito con un'ampia gamma di opzioni di indicizzazione che includono MapReduce, {{site.data.keyword.cloudant_short_notm}} Query, indicizzazione full-text e indicizzazione geospaziale. Le funzionalità di replica rendono più semplice mantenere i dati
sincronizzati tra i cluster di database, i PC desktop e i dispositivi mobili. 
{: shortdesc}

Per tutti i modi in cui puoi utilizzare {{site.data.keyword.cloudant_short_notm}}, vedi [Principi di base di {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/basics?topic=cloudant-ibm-cloudant-basics#cloudant-nosql-db-basics).

## Prima di cominciare
{: #prereqs-cloudant}

Assicurati innanzitutto di disporre dei seguenti prerequisiti pronti a essere utilizzati:
 * CocoaPods (versione 1.1.0 o superiore)
 * iOS (versione 9 o superiore)
 * MacOS (versione 10.11.5 o superiore)
 * Xcode (versione 9.0.1 o superiore)

L'[SDK {{site.data.keyword.cloudant_short_notm}} per Swift](https://github.com/cloudant/swift-cloudant){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") viene creato con Swift 3.2. Se intendi utilizzare {{site.data.keyword.cloudant_short_notm}} con Kitura, controlla la [libreria Kitura-CouchDB ](https://github.com/IBM-Swift/Kitura-CouchDB){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"), che è creata con Swift 4.0.
{: tip}

## Passo 1: Creazione di un'istanza di {{site.data.keyword.cloudant_short_notm}}
{: #create-instance-cloudant}

Vedi l'esercitazione [Creazione di un'istanza IBM Cloudant su {{site.data.keyword.cloud_notm}}](/docs/services/Cloudant/tutorials?topic=cloudant-creating-an-ibm-cloudant-instance-on-ibm-cloud#creating-an-ibm-cloudant-instance-on-ibm-cloud){:new_window} per creare un'istanza del servizio.

## Passo 2. Installazione dell'SDK
{: #install-sdk-cloudant}

### Installazione dell'SDK Swift iOS
{: #install-swift-sdk-cloudant}

Utilizza l'SDK Cloudant Swift per semplificare la codifica della tua applicazione. L'SDK deve essere installato nel tuo codice dell'applicazione.

1. Apri la tua directory del progetto Xcode esistente nel file `Podfile`.
2. Nella destinazione dei tuoi progetti, aggiungi una dipendenza per il pod `SwiftCloudant`. Assicurati che anche il comando `use_frameworks!` si trovi nella tua destinazione, come mostrato nel seguente esempio.
    ```
    target '<yourTarget>' do
      use_frameworks!
        pod 'SwiftCloudant', :git => 'https://github.com/cloudant/swift-cloudant.git'
    end
    ```
    {: screen}

3. Scarica la dipendenza `SwiftCloudant`.
    ```
    pod install
    ```
    {: codeblock}

### Installazione dell'SDK Swift lato server
{: #install-serverside-cloudant}

Per l'utilizzo con il gestore pacchetti Swift per lo sviluppo lato server, aggiungi la seguente riga alle tue dipendenze nel tuo `Package.swift`:
```swift
.Package(url: "https://github.com/cloudant/swift-cloudant.git")
```
{: codeblock}

## Passo 3. Inizializzazione dell'SDK
{: #initialize-cloudant}

Dopo che hai inizializzato l'SDK nella tua applicazione, puoi cominciare ad utilizzare {{site.data.keyword.cloudant_short_notm}} per archiviare i dati.

1.  Aggiungi la seguente istruzione import al tuo `AppDelegate.swift` o al file swift lato server.
    ```swift
    import SwiftCloudant
    ```
    {: codeblock}

2. Inizializza la connessione al database.
    ```swift
    let cloudantURL = NSURL(string: "https://username.cloudant.com")!
    let client = CouchDBClient(url: cloudantURL, username: "username", password: "password")
    let dbName = "database"
    ```
    {: codeblock}

### Operazioni di base
{: #basic-operations-cloudant}

Queste operazioni di base illustrano le azioni fondamentali per creare, leggere ed eliminare i tuoi documenti utilizzando il client inizializzato.

#### Crea un documento
```swift
let create = PutDocumentOperation(id: "doc1", body: ["hello": "world"], databaseName: dbName) {(response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while creating a document. Error: \(error)")
    } else {
        print("Created document \(response?["id"]) with revision id \(response?["rev"])")
    }
}
client.add(operation: create)
```
{: codeblock}

#### Leggi un documento
```swift
let read = GetDocumentOperation(id: "doc1", databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while reading a document. Error: \(error)")
    } else {
        print("Read document: \(response)")
    }
}
client.add(operation: read)
```
{: codeblock}

#### Elimina un documento
```swift
let delete = DeleteDocumentOperation(id: "doc1",
    revision: "1-revisionidhere",
    databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while deleting a document. Error: \(error)")
    } else {
        print("Document deleted")
    }
}
client.add(operation: delete)
```
{: codeblock}

## Passo 4. Verifica della tua applicazione
{: #cloudant_testing}

È tutto configurato correttamente? Verificalo.

1. Esegui la tua applicazione, assicurandoti di avviare l'inizializzazione e le rispettive operazioni, come ad esempio la creazione di un documento.
2. Ritorna all'istanza del servizio {{site.data.keyword.cloudant_short_notm}} creata precedentemente nel tuo browser web e apri il dashboard del servizio.
3. Seleziona il database utilizzato; puoi vedere i documenti nel dashboard.

Problemi? Controlla la [Guida di riferimento API {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/api?topic=cloudant-ibm-cloudant-basics#api-reference-overview).

## Passi successivi
{: #cloudant_next notoc}

Ottimo lavoro. Hai aggiunto un livello di persistenza sicura alla tua applicazione. Non fermarti ora e continua provando una delle seguenti opzioni:

* Visualizza il codice sorgente dell'[SDK {{site.data.keyword.cloudant_short_notm}} per Swift](https://github.com/cloudant/swift-cloudant){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").
* I kit starter sono uno dei modi più rapidi per utilizzare le funzionalità di {{site.data.keyword.cloud_notm}}. Il kit starter **Infinite Scrolling with Cloudant NoSQL for iOS** illustra come estendere un `ViewController` per visualizzare i dati utilizzando la paginazione. Questo modello di applicazione è comune per gli sviluppatori iOS ed è un ottimo esempio per illustrare le funzionalità di {{site.data.keyword.cloudant_short_notm}}. Visualizza i kit starter disponibili nel [Dashboard dello sviluppatore mobile ](https://cloud.ibm.com/developer/mobile/dashboard){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"). Scarica il codice. Esegui l'applicazione!
* Scopri di più e avvaliti di tutte le funzioni offerte da {{site.data.keyword.cloudant_short_notm}}, [controlla la documentazione](/docs/services/Cloudant?topic=cloudant-ibm-cloudant-basics#ibm-cloudant-basics)!
