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
{:gif: data-image-type='gif'}

# Gestione dei rilasci di funzioni dell'applicazione
{: #mobile_applaunch}

{{site.data.keyword.engage_full}} consente agli sviluppatori di creare delle applicazioni coinvolgenti controllando portata e distribuzione delle funzioni dell'applicazione che possono fornire delle metriche misurabili. Il servizio aiuta gli sviluppatori a rimuovere l'abbinamento oggi esistente tra lancio delle funzioni dell'applicazione e aggiornamenti delle applicazioni alla produzione. Ora puoi pubblicare le funzioni senza eseguirne l'esposizione alla produzione per rilasciare in modo graduale delle nuove versioni di un'applicazione in un modo controllato. Con il servizio {{site.data.keyword.engage_short}}, i proprietari dell'applicazione hanno il pieno controllo del lancio delle funzioni per un segmento di destinazione.

Il servizio {{site.data.keyword.engage_short}} definisce una funzione, crea i destinatari sulla base delle piattaforme del dispositivo (compresi gli attributi di destinatari personalizzati) e, infine, definisce un piano che coreografa la tempistica e il posizionamento della funzione. Dopo che vengono utilizzati gli SDK, insieme agli attributi di funzione e metriche incorporati nell'applicazione, il servizio inizia a misurare le esperienze dei destinatari. Puoi ora avvalerti della tua applicazione in base a queste informazioni per creare dei coinvolgimenti dei clienti personalizzati su varie categorie dei tuoi utenti dell'applicazione.

![Panoramica di Cognitive Engage](images/process_app_launch.png) Figura 1. Panoramica del ciclo di vita del servizio {{site.data.keyword.engage_short}}

Le funzioni dei servizi {{site.data.keyword.engage_short}} sono:

 - **Accelerare la distribuzione delle funzioni**

    Accelera la distribuzione di funzioni alla tua applicazione mediante un rilascio controllato riducendo i rischi. Rilascia le funzioni a un sottoinsieme di segmenti di destinatari e prendi decisioni di lancio o di ripristino di una versione valida precedente di maggiore entità in base al feedback in tempo reale. Separa i lanci di funzioni dai regolari cicli di rilascio.

 - **Segmentare i destinatari**

    I segmenti utente possono essere definiti in base ad attributi demografici, contestuali e comportamentali. In alternativa, le funzioni possono essere distribuite a una specifica percentuale dell'intera base utenti. Gli indicatori delle prestazioni chiave possono essere definiti per ogni funzione e codice lato client per misurare i risultati.

 - **Adattare l'applicazione in base al contesto**

    Il comportamento dell'applicazione, l'interfaccia utente e le notifiche possono essere personalizzati per specifici segmenti di destinatari. Ad esempio, lo sfondo dell'applicazione può essere modificato in base all'ubicazione dell'utente. Questa personalizzazione dell'utente comporta un maggiore coinvolgimento degli utenti con l'applicazione.

 - **Funzioni di test A / B**

    Acquisire sicurezza mediante sperimentazione. Due varianti delle funzioni dell'applicazione possono essere messe a confronto tra loro eseguendo il lancio di entrambe contemporaneamente. È possibile prendere decisioni sulla base di dati concreti.

 - **Aumentare il coinvolgimento dei clienti**

    Promuovi il coinvolgimento degli utenti. Le notifiche possono essere indirizzate a tutti gli utenti dell'applicazione o a una serie specifica di utenti e dispositivi. È possibile definire una pianificazione dei messaggi. L'interazione degli utenti riveste un ruolo vitale nelle relazioni con i clienti.


## Prima di cominciare

Assicurati innanzitutto di disporre dei seguenti prerequisiti pronti a essere utilizzati:

 - iOS 10+
 - Xcode 9
 - Swift 3.2 - 4
 - Cocoapods o Carthage

## Passo 1: Creazione di un'istanza di {{site.data.keyword.engage_short}}
{: ##app_launch_create}

1. Nel catalogo {{site.data.keyword.cloud_notm}}, fai clic su **Mobile** > **App Launch**. Viene visualizzata la schermata di configurazione del servizio.
2. Assegna un nome alla tua istanza del servizio oppure utilizza il nome preimpostato.
3. Fai clic su **Crea**.
4. Nel riquadro di navigazione, fai clic su **Connessioni** per selezionare un'applicazione ed eseguirne il bind al tuo servizio. Puoi eseguire il bind dell'istanza del servizio alla tua applicazione in un secondo momento, se la lasci senza bind durante la creazione.

## Passo 2. Inizializzazione della tua applicazione
{: #step2}
Il servizio fornisce degli SDK specifici per la piattaforma per semplificare lo sviluppo delle applicazioni. Gli SDK Swift {{site.data.keyword.cloud_notm}} Mobile Services possono essere installati con Cocoapods o Carthage. 

1. Fai clic su **Settings**.
2. Installa l'[SDK](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-applaunch). Per ulteriori informazioni, vedi il readme che include la procedura di installazione e i concetti tecnici.
3. Copia le chiavi di configurazione per inizializzare la tua applicazione. Utilizza il segreto dell'applicazione, il GUID dell'applicazione e il segreto del client per configurare la tua applicazione e creare coinvolgimenti.

## Passo 3. Creazione di una funzione
{: #step3}

Il servizio {{site.data.keyword.engage_short}} crea e verifica le risposte alle funzioni.

![Dettagli della funzione](images/feature_creation_animated.gif){: gif}

Per creare una funzione, completa la seguente procedura: 
1. Nel riquadro di navigazione, fai clic su **Features** > **Create New Feature**.
2. Aggiorna il modulo Create New Feature and Metrics con un nome della funzione e una descrizione. Puoi anche definire le proprietà della funzione e aggiungere delle metriche per misurare l'impatto del tuo coinvolgimento. Fai clic su **Bulk edit** per aggiungere più proprietà modificando il JSON.
3. Fai clic su **Create**.La nuova funzione viene ora visualizzata nel pannello Features.
4. Abilita la funzione dopo che è stata sviluppata.
5. Per abilitare una funzione da utilizzare come un coinvolgimento, fare clic sulla funzione che hai creato.
6. Nella finestra Feature Details, scegli di aggiornare lo stato (Update Status) della tua funzione in **Ready**.
7. Fai clic su **Update Status**.
8. Aggiorna la tua applicazione per includere gli attributi e i codici funzione appena creati nella tua applicazione iOS.
9. La funzione è ora pronta per essere utilizzata.

La finestra Feature Details può esportare la funzione come un file JSON, che può essere utilizzato nell'applicazione client per caricare i valori predefiniti.

## Passo 4. Creazione dei destinatari
{: #step4}

![Crea destinatari](images/create_audience_animated.gif){: gif}

Per creare i destinatari, completa la seguente procedura:

1. Crea un **attributo destinatari**:

	a. Fai clic su **Audience** > **Create Attribute**.
	b. Fornisci i seguenti valori:
		- **Name**: fornisci un nome appropriato per l'attributo.
		- **Description**: una breve descrizione relativa all'attributo.
		- **Type**: scegli il tipo di attributo.
		- **Allowed values**: Immetti i valori di attributo che desideri utilizzare.

  Puoi scegliere di creare più attributi di destinatari, come elencato nella seguente immagine, in base alle tue esigenze.


2. Crea dei **destinatari**:

	a. Fai clic su **Create Audience**.
	b. Fornisci un nome e una descrizione appropriati nella finestra New Audience.
	c. Seleziona un attributo e fai clic su **Add**.
	d. Scegli le opzioni richieste dagli attributi elencati.
	e. Fai clic su **Save**.


Puoi ora creare un coinvolgimento:

## Passo 5. Creazione di un coinvolgimento

Un coinvolgimento è un'istanziazione di una funzione con proprietà inizializzate e con un'associazione a uno dei destinatari predefiniti. Puoi creare un coinvolgimento utilizzando **Feature Control** o **In-App Messaging**.

### Abilitazione della funzionalità Feature Control

Tramite questo coinvolgimento, il proprietario di un'applicazione può controllare la visibilità di una funzione abilitandola o disabilitandola al runtime. Una funzione può essere abilitata o disabilitata per tutti gli utenti dell'applicazione o per una serie specifica di utenti e dispositivi.

I lanci delle funzioni possono essere pianificati e coordinati definendo delle date/ore di inizio e fine. Puoi anche scegliere un giorno specifico in cui una funzione definita deve essere abilitata o disabilitata.

![animated gif](images/create_engagement.gif){: gif}

Completa la seguente procedura per creare un coinvolgimento utilizzando Feature Control:

1. Puoi creare un coinvolgimento utilizzando l'uno o l'altro dei seguenti metodi:
	- Fai clic su **Engagements** nel riquadro di navigazione.
	- Seleziona **Create Engagements** sulla nuova funzione che hai creato.
	- Nel riquadro di navigazione, fai clic su **Overview** > **Create New Engagement**.

  Viene visualizzata la finestra New Engagement.

2. Fornisci un nome e una descrizione al tuo nuovo coinvolgimento. Assicurati di dare un nome coinvolgimento univoco e non uno già elencato in Engagements.

	a. Per **Select Engagement type**, seleziona **Feature Control**.
	b. Per fare un esperimento controllato con più varianti della funzione, seleziona **A/B testing** in **Select Experimentation Type**. Fai clic su **Next**.

3. Seleziona la funzione che hai creato. Puoi anche scegliere di aggiungere e definire le varianti che potresti voler sperimentare. Fai clic su **Next**.

4. Seleziona i destinatari. Fai clic su **Next**.

5. Definisci un trigger scegliendo Time e la data od ora di inizio (**Start**) e una data od ora di fine (**End**). Fai clic su **Save**.

  Il nuovo coinvolgimento è ora visualizzato nella finestra Engagement Details.

Puoi ora misurare le [prestazioni](/docs/services/app-launch/app_measure_performance.html#applaunch_type) del tuo coinvolgimento.

### Abilitazione della funzionalità di messaggistica interna all'applicazione

Mediante questo coinvolgimento, il proprietario di un'applicazione può inviare delle notifiche agli utenti dell'applicazione mentre la stanno utilizzando attivamente.

I messaggi possono essere indirizzati a tutti gli utenti dell'applicazione o a una serie specifica di utenti e dispositivi. Per ogni messaggio che inoltri al servizio, i destinatari previsti ricevono una notifica.

I messaggi interni all'applicazione possono essere pianificati definendo una data di inizio o di fine e un'ora. Puoi anche eseguirne la pianificazione in base a un evento. Questi messaggi sono più personalizzati poiché sono basati su informazioni approfondite derivanti da analisi relative alla scelta dell'utente, alle interazioni, al dispositivo, ai log delle applicazioni e così via.

I messaggi interni all'applicazione possono essere utilizzati per:

- Inviare messaggi personalizzati.
- Inviare messaggi agli utenti che hanno disattivato le notifiche di push.
- Richiedere il feedback o coinvolgere gli utenti in una conversazione.
- Inviare messaggi pertinenti rilevando cosa sta cercando l'utente.
- Coinvolgere clienti attivi e fedeli.
- Informare gli utenti di aggiornamenti dell'applicazione (lancio di una nuova funzione) e così via.

![animated gif](images/in-app-engagement_animated.gif){: gif}

Completa la seguente procedura per creare un coinvolgimento che utilizza l'opzione Messaging:

1. Puoi creare un coinvolgimento utilizzando l'uno o l'altro dei seguenti metodi:
	- Fai clic su **Engagements** nel riquadro di navigazione.
	- Seleziona **Create Engagement** sulla nuova funzione che hai creato.
	- Nel riquadro di navigazione, fai clic su **Overview** > **Create New Engagement**.

  Viene visualizzata la finestra New Engagement.

2. Fornisci un nome e una descrizione al tuo nuovo coinvolgimento. Assicurati di dare un nome coinvolgimento univoco e non uno già elencato in Engagements.

	a. Per **Select Engagement type**, seleziona **In-App Messaging**
	b. Per fare un esperimento controllato con più varianti della funzione di messaggistica, seleziona **A/B testing** in **Select Experimentation Type**. Fai clic su **Next**.

3. Compila le proprietà del messaggio e fai clic su **Next**.

4. Scegli i destinatari (**Select Audience**) e la percentuale di destinatari che vuoi raggiungere. Fai clic su **Next**.

5. Definisci un trigger selezionando una data e ora di inizio/fine (**Start/End Date and Time**).

6. Seleziona **Event** e fai clic su **Next**.

7. Associa gli elementi alle metriche che vuoi misurare. Seleziona l'elemento e compila i dettagli delle metriche. Fai clic su **Save**.

  Il nuovo coinvolgimento è ora visualizzato nella finestra Engagement Details.

Puoi ora misurare le [prestazioni](/docs/services/app-launch/app_measure_performance.html#applaunch_type) del tuo coinvolgimento.

### Link rapidi

Controlla i seguenti link per acquisire informazioni approfondite e comprendere le funzioni di {{site.data.keyword.engage_short}}:

 - Prova il [servizio](https://console.bluemix.net/catalog/services/app-launch)
 - [Blog e video](/docs/services/app-launch/relatedlinks.html#blogs-and-videos)
 - Per ulteriori informazioni, vedi la [documentazione](/docs/services/app-launch/index.html#gettingstartedtemplate)
