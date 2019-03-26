---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip}

# Sviluppo senza server
{: #serverless-dev-swift}

Che cosa si intende per "senza server"? Il modello di sviluppo senza server si riferisce allo sviluppo di applicazioni in cui la logica lato server viene eseguita in contenitori senza stato. I contenitori sono attivati dagli eventi, effimeri (durano per una singola esecuzione) e pienamente gestiti da una terza parte. Questo paradigma, noto anche come FaaS (Functions as a Service), è dove lo sviluppatore fornisce una funzione senza stato che può essere attivata ed eseguita senza la creazione e la gestione in modo esplicito di un server.

Estraendo le infrastrutture e i framework necessari per lo sviluppo lato server, l'architettura senza server consente agli sviluppatori di concentrarsi sulla scrittura del codice da eseguire in modo reattivo per modificare i dati.

L'offerta FaaS di IBM, [{{site.data.keyword.openwhisk}} ](https://cloud.ibm.com/openwhisk/), si sforza di fornire un'esperienza di sviluppo lato server semplice, senza che occorra alcuna conoscenza specialistica sul lato server. Utilizzando la tecnologia senza server, puoi sviluppare rapidamente soluzioni di backend estensibili per soddisfare praticamente tutte le richieste di carico di lavoro senza la necessità di creare le risorse in anticipo. Per le applicazioni che hanno dei modelli di carico imprevedibili o un elevato tempo di inattività del server, {{site.data.keyword.openwhisk_short}} può essere un'eccellente soluzione cloud con prestazioni migliorate; inoltre, il suo sistema "paga solo ciò che utilizzi" aiuta a ridurre i costi.

## Modifiche dell'architettura
{: #comparison-serverless}

Per aiutarti a comprendere i vantaggi dal punto di vista dell'architettura derivanti dal passaggio a FaaS, un'architettura tradizionale e una FaaS sono messe a confronto utilizzando una semplice applicazione iOS collegata a un database.

In un'architettura più tradizionale, l'applicazione iOS esegue l'offload delle attività ad alta intensità di rete o elabora i dati in remoto su un server centralizzato, a cui si connette detta applicazione tramite i suoi servizi o le sue opzioni di archiviazione. Il grosso del lavoro è a carico di un singolo server che elabora le attività ad elevato consumo di risorse per ridurre al minimo lo stress sul client e fornire la sincronizzazione all'interno della sua base di utenti.

Un'architettura senza server può modificare questa struttura in modo che sia più simile alla seguente immagine.

![](./images/Architecture.png) Figura 1. Architettura senza server

Piuttosto che gestire tutta la logica di elaborazione e di autenticazione all'interno di un singolo server, un'architettura senza server utilizza funzioni che incorporano gran parte della logica lato server ed esegue l'offload di parte della logica al client (e ai servizi esterni).

Esaminando lo schema, puoi vedere i seguenti punti:

1. Il client esegue l'autenticazione rispetto a un provider di identità come ad esempio un ID applicazione.
2. Le chiamate all'API di backend FaaS che includono il token di accesso.
3. Il backend è implementato con {{site.data.keyword.openwhisk_short}}. Le azioni senza server sono esposte come azioni web e prevedono che i token vengano inviati nell'intestazione della richiesta per la verifica (firma e data di scadenza) per fornire l'accesso all'API effettiva.
4. Quando il client inoltra i dati, il feedback viene archiviato in un {{site.data.keyword.cloudant_short_notm}}.
5. Il testo di feedback viene elaborato con {{site.data.keyword.toneanalyzershort}}.
6. In base al risultato dell'analisi, {{site.data.keyword.mobilepushshort}} restituisce una notifica al client.
7. Il client riceve la notifica.

In un modello puramente senza server, il client si assume spesso delle responsabilità aggiuntive a causa dell'impossibilità di memorizzare lo stato dell'utente. L'autorizzazione viene gestita dal client e dal servizio di provider di identità {{site.data.keyword.appid_short_notm}}.

Sebbene le architetture senza server non siano sempre ideali, possono fornire vantaggi sostanziali nelle corrette condizioni di utilizzo e di team. Controlla qualche esempio specifico per informazioni su alcuni dei [casi d'uso](#use_cases) più comuni.

## Vantaggi senza server
{: #benefits-serverless}

### Costo ridotto
{: #reduced-cost-serverless}

Esternalizzare il costo monetario e di tempo associato all'amministrazione del sistema riduce il costo globale associato ai server di backend tradizionali. Inoltre {{site.data.keyword.openwhisk_short}} è diverso dalle tecnologie di elaborazione tradizionali perché paghi solo per il tempo impiegato dal tuo codice per soddisfare le richieste, arrotondato ai 100 ms più prossimi. Sono possibili dei notevoli risparmi in termini di costo in confronto ad altre tecnologie come le VM e i contenitori, che è improbabile che vengano utilizzati al 100% e che utilizzano la memoria sul sistema del tuo provider cloud.

### Elevata disponibilità e scalabilità
{: #ha-serverless}

Le architetture senza server forniscono per loro natura una scalabilità istantanea con una disponibilità pressoché costante.

### Velocità e sviluppo semplificato
{: #speed-serverless}

Eliminando la necessità di gestione del sistema e fornendo interfacce semplici per la distribuzione, il paradigma senza server accelera lo sviluppo delle applicazioni. Gli sviluppatori possono creare rapidamente applicazioni con sequenze di azioni che vengono eseguite in risposta a un mondo guidato dagli eventi.

## Casi d'uso di esempio
{: #use-cases-serverless}

### Backend mobile
{: #mobile-backend-serverless}
![](./images/cloud-functions-rest-api-trigger.png)

Gli sviluppatori di dispositivi mobili possono accedere facilmente alla logica lato server e esternare le attività ad alta intensità computazionale a una piattaforma cloud. Puoi implementare le funzioni in linguaggi come Swift e utilizzare facilmente le funzioni lato server utilizzando l'SDK iOS senza che occorra alcuna esperienza lato server.

### Elaborazione dati
{: #data-processing-serverless}

![](./images/cloud-functions-cloudant-trigger.png)

Puoi eseguire il codice ogni volta che i dati vengono aggiornati nel tuo archivio dati tramite trigger integrati. Puoi anche automatizzare facilmente i processi come la normalizzazione audio, la rotazione delle immagini, la nitidezza, la riduzione del rumore, la generazione di miniature o la transcodifica video mediante un modello di programmazione lato server funzionale.

### Elaborazione dati Cognitive
{: #cognitive-serverless}

Puoi analizzare i dati non appena diventano disponibili. La tua funzione può avvalersi di potenti servizi cognitivi, come IBM Watson, per rilevare oggetti o persone nelle immagini o nei video.

### Attività pianificate
{: #tasks-serverless}

Esegui le tue funzioni periodicamente e definisci le pianificazioni che rispettano una sintassi simile a cron per specificare quando devono essere eseguite le azioni.

## Guida di riferimento API
{: #apiref-serverless notoc}

<!-- * [REST API Documentation](./openwhisk_reference.html#openwhisk_ref_restapi)-->
* [API REST](https://cloud.ibm.com/apidocs)

## Link correlati
{: #related-serverless notoc}

* [Scopri {{site.data.keyword.openwhisk_short}}](https://www.ibm.com/cloud/functions)
<!-- redirects to link above * [{{site.data.keyword.openwhisk_short}} on IBM developerWorks](https://developer.ibm.com/openwhisk/)-->
* [Sito web del progetto Apache OpenWhisk](http://openwhisk.org)
* [Ulteriori informazioni sul concetto di "senza server"](https://martinfowler.com/articles/serverless.html)
