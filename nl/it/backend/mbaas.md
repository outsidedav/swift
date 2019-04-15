---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: mobile backend swift, mbaas swift, mbaas swift sdk, swift features, swift framework sdk

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# Funzioni MBaaS (Mobile backend as a Service)
{: #mbaas}

La tua applicazione può utilizzare le funzioni esterne tramite 1 dei due approcci indicati di seguito:
* Utilizzo diretto dall'applicazione ai suoi servizi, che sono di norma ospitati come parte di una piattaforma MBaaS (Mobile Backend as a Service).
* Utilizzo tramite un componente BFF (Backend For Frontend) ospitato, che può fornire la composizione e il controllo sui servizi e può aggiungere della logica di backend per l'applicazione.

Di norma, un approccio orientato all'aggiunta di funzioni esterne direttamente dalla tua applicazione mobile tramite un MBaaS è più diretto. Bisogna aggiungere meno elementi e infrastruttura alla prima serie di servizi. Tuttavia, a questo metodo manca la flessibilità e la portata di massimo livello possibili tramite la distribuzione di un BFF.

Uno dei vantaggi della piattaforma {{site.data.keyword.cloud_notm}} è che non devi decidere in anticipo. Tutti i servizi forniti possono essere utilizzati direttamente dal dispositivo mobile utilizzando gli SDK basati su Swift forniti. La piattaforma {{site.data.keyword.cloud_notm}} supporta anche la scrittura della sua logica di back-end in Swift, tramite un modello senza server con {{site.data.keyword.openwhisk_short}} oppure tramite Swift lato server con framework quali Kitura. Grazie a questa funzionalità, quando vuoi aggiungere della logica di back-end tramite un modello BFF, puoi farlo in Swift e scegliere di migrare il tuo utilizzo degli SDK Swift dall'applicazione a BFF. Di conseguenza, puoi passare in modo incrementale da un approccio in stile MBaaS a un approccio BFF con un minimo sforzo supplementare.
