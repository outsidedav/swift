---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: object storage swift, static storage swift, file services swift, swift storage class, cos swift, swift data encryption, static swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilizzo di Object Storage per il contenuto statico
{: #object-storage}

Object Storage è un componente fondamentale dell'elaborazione cloud e fornisce delle potenti funzioni agli sviluppatori Apple e alle loro applicazioni. A differenza dell'archiviazione di informazioni in una gerarchia di file (come l'archiviazione blocchi o file), un'archiviazione oggetti consiste solo di file e dei relativi metadati, archiviati in raccolte note come bucket. Per definizione, questi oggetti sono immutabili, il che li rende perfetti per dati quali immagini, video e altri documenti statici. Per i dati che cambiano spesso o che sono relazionali, puoi utilizzare i servizi di database [Cloudant](/docs/swift/data?topic=swift-cloudant#cloudant) e [SQL](/docs/swift/data?topic=swift-sql_data#sql_data).

COS ({{site.data.keyword.cos_full_notm}}) è un sistema di archiviazione che può essere utilizzato per archiviare dati non strutturati flessibile, economicamente vantaggioso e scalabile. I dati sono accessibili tramite gli SDK oppure utilizzando l'interfaccia utente IBM. Puoi utilizzare {{site.data.keyword.cos_full_notm}} per accedere ai tuoi dati non strutturati tramite un portale self-service supportato da SDK e API RESTful. 

A seconda delle tue esigenze, puoi utilizzare {{site.data.keyword.cos_short}} per i seguenti servizi:

* Repository di contenuti
* Backup e ripristino
* Archiviazione dati
* Servizi file
* Trasferimento dati

Quando crei un bucket, devi selezionare un livello di resilienza (interregionale o regionale). In base alla tua selezione, i tuoi dati vengono distribuiti e archiviati in più di un'ubicazione geografica.

## API
{: #api-cos}

L'API {{site.data.keyword.cos_full}} è un'API basata su REST per la lettura e la scrittura di oggetti. Supporta un sottoinsieme dell'API S3 per una facile migrazione delle applicazioni a {{site.data.keyword.cloud_notm}}. Può essere utilizzato qualsiasi SDK S3 per utilizzare {{site.data.keyword.cos_full}}. Per ulteriori informazioni, vedi la [Guida di riferimento API {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage/api-reference?topic=cloud-object-storage-compatibility-api-about#about-the-ibm-cloud-object-storage-api) completa

## Protezione dell'archivio dell'oggetto 
{: #security-cos}

{{site.data.keyword.cos_full_notm}} è altamente sicuro. Inizialmente, solo i proprietari di bucket e oggetto hanno originariamente accesso al servizio {{site.data.keyword.cos_full_notm}} da essi creato. Supporta inoltre l'autenticazione utente per accedere ai dati. Utilizza i meccanismi di controllo degli accessi, come ad esempio le politiche di bucket, per concedere in modo selettivo le autorizzazioni agli utenti e alle applicazioni. Puoi trasferire in modo sicuro i dati a {{site.data.keyword.cos_short}} tramite gli endpoint SSL utilizzando il protocollo HTTPS. Se ti serve maggiore sicurezza, puoi utilizzare l'opzione SSE (Server-Side Encryption) oppure l'opzione SSE-C (Server-Side Encryption with Customer-Provided Keys) per crittografare i dati archiviati inattivi. {{site.data.keyword.cos_full_notm}} fornisce la tecnologia di codifica sia per SSE che per SSE-C. In alternativa, puoi utilizzare le tue librerie di crittografia per crittografare i dati prima di archiviarli in Cloud Object Storage.

Puoi utilizzare i seguenti meccanismi di controllo accessi per proteggere i tuoi dati in {{site.data.keyword.cos_full_notm}}.

### Politiche IAM (Identity and Access Management) 
{: #iam-cos}

IAM consente alle organizzazioni con più dipendenti di creare e gestire più utenti con un singolo account. Con le politiche IAM, le aziende possono concedere agli utenti IAM il controllo dei loro bucket {{site.data.keyword.cos_short}}.

### ACL (Access Control List) 
{: #acls-cos}

Con gli ACL, puoi concedere delle specifiche autorizzazioni (ad esempio READ, WRITE), a specifici utenti per un singolo bucket.

I dati inattivi vengono crittografati con la crittografia AES (Advanced Encryption Standard) a 256 bit e l'hash SHA-256 (Secure Hash Algorithm 256) lato provider automatico. I dati in movimento sono protetti utilizzando la crittografia TLS (Transport Layer Security), SSL (Secure Sockets Layer) o SNMPv3 con AES carrier-grade integrata.

### Crittografia
{: #encryption-cos}

I dati inattivi vengono crittografati con la crittografia AES (Advanced Encryption Standard) a 256 bit e l'hash SHA-256 (Secure Hash Algorithm 256) lato provider automatico. I dati in movimento vengono protetti utilizzando la crittografia TLS/SSL (Transport Layer Security/Secure Sockets Layer) o SNMPv3 con AES carrier-grade integrata.

La crittografia lato server è sempre attiva (ON) per i dati cliente. In confronto all'hash richiesto nell'autenticazione, e alla codifica di cancellazione, la crittografia non è una parte significativa del costo di elaborazione di Cloud Object Storage.

Puoi fornire una tua chiave per la crittografia poiché SSE-C è supportata in {{site.data.keyword.cos_full_notm}}. Tuttavia, è tua la responsabilità di gestire e fornire la chiave per archiviare e richiamare i dati.

## Resilienza
{: #resiliency-cos}

Quando crei un bucket, devi selezionare un livello di resilienza (interregionale o regionale). In base alla tua selezione, i tuoi dati vengono distribuiti e archiviati in più di un'ubicazione geografica.

La resilienza regionale è per la bassa latenza e i tuoi dati vengono distribuiti in tre centri all'interno di una singola regione. Utilizza la resilienza interregionale per la disponibilità di importanza critica perché i tuoi dati vengono archiviati in 3 o più regioni differenti. L'interregionalità offre una resilienza geografica ed è disponibile in più di un endpoint. Valuta le tue esigenze applicative per decidere tra le due.

### Aree geografiche
{: #geographies-cos}

Puoi utilizzare {{site.data.keyword.cos_full_notm}} da qualsiasi parte del mondo. Puoi scegliere dove vuoi archiviare i tuoi dati al momento della creazione del bucket. Le informazioni archiviate in IBM COS vengono crittografate e distribuite in più di un'ubicazione geografica utilizzando le tecnologie di archiviazione distribuita fornite dal sistema IBM Object Storage. 

Considera i seguenti fattori per selezionare l'ubicazione geografica del tuo archivio oggetti quando stai decidendo tra l'opzione regionale e quella interregionale.

Considerazioni sull'ubicazione geografica: 
* Un'ubicazione che sia remota rispetto alle tue operazioni per una ridondanza.
* Un'ubicazione per rispondere a requisiti legali e normativi.
* Riduci le latenze di accesso ai dati
* Considera i modelli di prezzo.

## Casi d'uso e classi di archiviazione
{: #usecases-cos}

A seconda del tuo caso d'uso, puoi ridurre i costi selezionando un piano di servizi che soddisfi le tue esigenze. Le operazioni di archiviazione che comportano un accesso minimo all'archivio oggetti non hanno bisogno della velocità e della durabilità di un oggetto a cui si accede frequentemente e questa distinzione si riflette nel supporto della classe di archiviazione e nel piano dei prezzi per le tue applicazioni. Le classi di archiviazione sono definite a livello di bucket e pertanto puoi utilizzare una combinazione di piani che soddisfi le tue esigenze. Crea un bucket che è impostato sulla classe di archiviazione che vuoi utilizzare.

Ulteriori informazioni sui prezzi sono disponibili dalla [documentazione relativa alla classe di archiviazione {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage/help?topic=cloud-object-storage-billing#ibm-cos-pricing).

### Classi di archiviazione di esempio
{: #samples-cos}

- Standard
  
  Questo servizio è per i dati non strutturati che richiedono un accesso frequente, come ad esempio DevOps, i repository di contenuto di azione e collaborazione. 

- Vault
  
  Questo servizio è per i carichi di lavoro con dati a cui si accede poco di frequente, come i carichi di lavoro di backup, archivio e conformità. 

- Cold Vault
  
  Questa opzione di distribuzione è ideale per i requisiti di accesso minimi, la conformità dei record cronologici e il backup a lungo termine. 

- Flex

  Distribuisci per requisiti di accesso ai dati variabili e proteggi il tuo budget da fluttuazioni dei costi imprevisti. Le classi di archiviazione sono definite a livello di bucket. Crea un bucket che è impostato sulla classe di archiviazione che vuoi utilizzare.


