---

copyright:
  years: 2018
lastupdated: "2018-08-29"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Creazione di applicazioni Swift con i kit starter
{: #intro}

{{site.data.keyword.cloud_notm}} Developer Console for Apple consente agli sviluppatori Apple di creare applicazioni da diversi kit starter, di eseguire il provisioning e la connessione di servizi chiave ottimizzati per {{site.data.keyword.cloud_notm}} e di scaricare quindi rapidamente il codice funzionante o di eseguire la configurazione per una fornitura continua. Gli utenti possono creare, visualizzare, configurare e gestire la tua applicazione, nonché scaricare il codice della tua applicazione. L'utilizzo dei kit starter ti aiuta a valutare e testare rapidamente i servizi {{site.data.keyword.cloud_notm}} con una nuova applicazione.

Pronto a cominciare? Per un'introduzione, visita [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits).
{: tip}

## Cos'è un kit starter?
{: #starter_kits}

Con {{site.data.keyword.cloud_notm}} Developer Experience, puoi scegliere tra diversi kit starter. I kit starter indicano a {{site.data.keyword.cloud_notm}} di assemblare dinamicamente un'applicazione di produzione di base nel linguaggio da te scelto, pronta per la distribuzione cloud. Ogni kit starter rappresenta un linguaggio, un framework e un modello per uno specifico caso d'uso del mondo reale che consente di riutilizzare il codice invece di reinventarlo.

I kit starter sono pronti per la produzione e si focalizzano sulla dimostrazione di un'implementazione del modello chiave utilizzando un runtime (ad esempio Swift). In alcuni casi, i kit starter offrono un'esperienza utente semplice per mettere in evidenza l'integrazione del servizio. In altri casi i kit starter rappresentano un'implementazione personalizzabile di un caso d'uso sofisticato.

I kit starter contengono istruzioni che consentono a {{site.data.keyword.cloud_notm}} di produrre automaticamente applicazioni di scaffolding con codice portatile e di specificare le risorse di cui eseguire il provisioning automatico quando crei un'applicazione dal kit starter.

## Utilizzo della {{site.data.keyword.cloud_notm}} Developer Console for Apple
{: #journey}

La {{site.data.keyword.cloud_notm}} Developer Console for Apple ti offre un percorso senza soluzione di continuità alla creazione di un'applicazione starter Swift per il tuo specifico caso d'uso. Vediamo le varie fasi in cui potresti articolare questo tuo percorso.

### Schermata Panoramica
{: #overview_screen}

La schermata Panoramica ti fornisce il contenuto su misura per una serie di casi d'uso come Watson, Weather e altro. Dalla schermata di panoramica, puoi visualizzare la documentazione, accedere alle risorse didattiche, esplorare i servizi, vedere i kit starter presentati oppure collegarti ad una raccolta più ampia di kit starter. Fai clic su `Kit starter` nell'area di navigazione sulla sinistra per accedere alla vista Kit starter.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - schermata Panoramica](images/overview_screen.png "Schermata Panoramica") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple - schermata Panoramica*

### Vista Kit starter
{: #starter_kits_view}

La vista Kit starter mostra la raccolta di kit starter specifica per un'area di casi d'uso. Puoi fare clic sui vari link su una scheda di kit starter per visualizzare le demo e ulteriori informazioni. Seleziona un kit starter per passare alla vista Crea applicazione.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - vista Kit starter](images/starter_kits_screen.png "Vista Kit starter") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple - vista Kit starter*

### Vista Crea applicazione
{: #create_new_app_view}

Dalla vista **Crea applicazione**, puoi denominare la tua applicazione e fornire informazioni di distribuzione e instradamento. Sulla destra, puoi anche visualizzare i servizi di cui viene eseguito automaticamente il provisioning quando crei la tua applicazione, insieme ai piani dei prezzi e ai termini per ognuno. Fai clic su `Create` per passare alla vista App Details. Se non hai eseguito l'accesso a {{site.data.keyword.cloud_notm}}, devi procedere a farlo ora.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - vista Create New App](images/create_new_project_screen.png "Vista Create New App") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple - vista Create New App*

## Vista App Details
{: #app_details_view}

La vista App Details visualizza un elenco di servizi configurati per la tua applicazione. Per ciascuna voce nell'elenco, puoi visualizzare il nome del servizio, dei link ad altre informazioni e un pulsante **actions** con tre punti allineati verticalmente. Le opzioni del pulsante **actions** sono la rimozione dei servizi da un'applicazione, l'apertura di un dashboard per il servizio e l'eliminazione del servizio. La rimozione di un'istanza del servizio rimuove l'associazione a questa applicazione e non elimina l'istanza del servizio. Inoltre, le credenziali del servizio sono accorpate in questa vista, per cui non devi visitare ogni singola vista dell'istanza del servizio per ottenerle.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - vista App Details](images/project_details_screen.png "Vista App Details") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple - vista App Details*

Utilizzando la vista App Details, puoi aggiungere servizi nuovi o esistenti alla tua applicazione che non facevano parte del kit starter originale. Fai clic sul link **Add Resource** nella casella di elenco dei servizi per aggiungere i servizi. I servizi disponibili dipendono dal tipo di applicazione e dai servizi disponibili in una regione, quindi non tutti i servizi sono disponibili per essere associati a tutte le applicazioni.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - finestra di dialogo Add Resource](images/add_resource_screen.png "Finestra di dialogo Add Resource") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple - finestra di dialogo Add Resource*

### Scaricamento del tuo codice

* Nella vista App Details, puoi accedere al tuo codice selezionando **Download Code** per generare e scaricare il codice per la tua applicazione.

### Vista App List
{: #app_list_view}

Puoi elencare tutte le tue applicazioni create dalla vista App List. Puoi rinominare o eliminare le tue applicazioni da qui. Fai clic sulla riga di un nome applicazione per ritornare alla vista App Details.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - vista App List](images/project_list_screen.png "Vista App List") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple - vista App List*

Per ulteriori informazioni, visita la pagina delle [risorse didattiche di {{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/learning-resources).
{: tip}
