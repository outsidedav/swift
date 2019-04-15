---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: watson swift, swift developer, apple development, ios oveview, developer consoles swift, apple console

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:tip .tip}

# Sviluppo Apple su {{site.data.keyword.cloud_notm}}
{: #ios_dev}

{{site.data.keyword.cloud}} offre soluzioni e servizi per permettere agli sviluppatori e alle applicazioni Swift di utilizzare la protezione, l'intelligenza artificiale e il valore richiesti dai loro clienti. Con un ampio portfolio di offerte ed SDK, puoi avvalerti di questi servizi e immettere rapidamente sul mercato le tue applicazioni all'avanguardia.

## Una singola piattaforma di sviluppo: IBM Cloud
{: #platform}

 ![Tipi di sviluppatori](images/IBM_Cloud_icon.png "IBM Cloud")

Le funzionalità per gli sviluppatori integrate in {{site.data.keyword.cloud_notm}} si allineano a diverse gamme di competenze e forniscono una singola piattaforma per produrre, distribuire, eseguire e gestire la tua applicazione. Ad esempio, dall'applicazione cognitiva menzionata, sono d'interesse le seguenti funzionalità di {{site.data.keyword.cloud_notm}}:

* La [**{{site.data.keyword.cloud_notm}} Developer Experience**](/docs/overview?topic=overview-dev-journey#dev-journey) non è un servizio ma piuttosto un insieme di funzionalità su {{site.data.keyword.cloud_notm}} che aiuta gli sviluppatori nativi cloud e digitali a iniziare a creare delle applicazioni pronte per la produzione. Include il provisioning automatico di servizi e una distribuzione "in un clic" a una toolchain DevOps.

* [**IBM Watson Data Platform**](https://dataplatform.ibm.com){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") rende facile assemblare raccolte di dati, affinare i dati e quindi eseguire visualizzazioni, rilevamento di informazioni approfondite e creazione di modelli per l'utilizzo nella tua applicazione cognitiva.

* [**IBM Streaming Analytics**](/docs/services/StreamingAnalytics?topic=StreamingAnalytics-gettingstarted#gettingstarted) aiuta ad eseguire il wrangling e l'analisi dei flussi di dati. Si tratta di una piattaforma di analisi avanzata che puoi utilizzare per inserire, analizzare e correlare le informazioni man che arrivano da diversi tipi di origini dati in tempo reale.

* Il [servizio Continuous Delivery di **{{site.data.keyword.cloud_notm}}**](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-cd_getting_started#cd_getting_started) configura una toolchain devops per automatizzare la fornitura continua della tua applicazione. Puoi migliorare facilmente la toolchain per includere funzioni di gestione come il monitoraggio, la registrazione, la traccia e la segnalazione. Puoi anche applicare il machine learning avanzato alla tua toolchain utilizzando il [servizio DevOps Insights](/docs/services/DevOpsInsights?topic=DevOpsInsights-getting-started#getting-started).

La piattaforma {{site.data.keyword.cloud_notm}} offre molte più funzionalità e può essere utilizzata come tua piattaforma di sviluppo completa.

## Panoramica delle funzionalità di {{site.data.keyword.cloud_notm}}
{: #capabilities}

{{site.data.keyword.cloud_notm}} Developer Experience non è un servizio ma piuttosto un insieme di funzionalità nella piattaforma {{site.data.keyword.cloud_notm}} che ti aiutano a creare applicazioni nel modo "giusto" in pochi minuti. Gli elementi essenziali di Developer Experience sono:

* Una serie di console per gli sviluppatori basate sui canali o sugli argomenti disponibili nell'ambito della piattaforma {{site.data.keyword.cloud_notm}}.
* Specifici kit starter per i casi d'uso che producono applicazioni starter pronte per la produzione in diversi linguaggi e modelli di architettura.
* Il provisioning automatico dei servizi.
* La gestione dei componenti dell'applicazione utilizzando una struttura dell'applicazione portatile.
* La creazione con un clic di una [toolchain devops](/docs/services/DevOpsInsights?topic=DevOpsInsights-getting-started#getting-started).

Per comprendere il modo in cui {{site.data.keyword.cloud_notm}} Developer Experience può aiutarti a creare rapidamente applicazioni pronte per la produzione di elevata qualità, consulta i seguenti elementi in modo più dettagliato.

## Console per gli sviluppatori
{: #developer_consoles}

{{site.data.keyword.cloud_notm}} Developer Experience si presenta in diverse console per gli sviluppatori nell'ambito della piattaforma {{site.data.keyword.cloud_notm}}. Ogni console rappresenta un'area di interesse (come Watson, sicurezza o finanza) o un canale digitale (come applicazioni mobili o web). La [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/dashboard){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") è stata messa a punto per gli sviluppatori Apple per consentire di fare rapidamente prove con applicazioni e servizi, con il supporto della piattaforma {{site.data.keyword.cloud_notm}}. Puoi accedere a queste console facendo clic sull'icona del menu nella console {{site.data.keyword.cloud_notm}}. Ad esempio, vedi le seguenti console per gli sviluppatori:

* [Console per gli sviluppatori Watson](https://cloud.ibm.com/developer/watson/dashboard){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")
* [Console per gli sviluppatori di applicazioni mobili](https://cloud.ibm.com/developer/mobile/dashboard){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")
* [Console per gli sviluppatori di applicazioni web](https://cloud.ibm.com/developer/appservice/dashboard){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")
* [Console per gli sviluppatori nel settore sicurezza](https://cloud.ibm.com/developer/security/dashboard){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")
* [Console per gli sviluppatori nel settore finanziario](https://cloud.ibm.com/developer/finance/dashboard){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")

<!--Cloud native development is the process of developing apps that are optimized to leverage capabilities engendered from running on the cloud.  Flexibility, portability, scaling, rapid development, continuous delivery, and a close coupling development and operations ("devops) are characteristics of cloud applications. The {{site.data.keyword.cloud}} Developer Experience quickly gets you started building cloud native applications that are ready for team development and bound for production use.-->


<!--![Overview of elements of the {{site.data.keyword.cloud_notm}} Developer Experience](images/elements_of_devex.png "Overview of elements of the {{site.data.keyword.cloud_notm}} Developer Experience") <br> *Overview of elements of the {{site.data.keyword.cloud_notm}} Developer Experience*-->

Ogni console per gli sviluppatori fornisce dei kit starter pertinenti all'area a cui si dedica tale console, offrendo un flusso di lavoro congruente e intuitivo per creare un'applicazione funzionante pronta per la produzione in pochi minuti.

## Creazione di applicazioni con i kit starter
{: #starterkit-apps}

Puoi avvalerti dei [kit starter](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro) per creare applicazioni Swift, che contengono l'associazione di codice, dati, servizi e toolchain che formano la tua applicazione.
