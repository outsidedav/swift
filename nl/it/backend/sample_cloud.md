---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: bluepic swift, ios bluepic, bff swift image, sample swift, swift example bff

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# Caso d'uso di iOS e logica cloud
{: #sample_cloud}

È possibile vedere un esempio di un'applicazione iOS che utilizza un BFF (Backend For Frontend) tramite l'[applicazione di esempio di condivisione di immagini BluePic](https://github.com/IBM/BluePic){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"). L'applicazione BluePic utilizza le seguenti tecnologie:

* Object Storage e Cloudant per archiviare i dati immagine.
* Watson Visual Recognition e il servizio IBM Weather Company per aggiungere ulteriori informazioni alle immagini.
* Kitura e {{site.data.keyword.openwhisk}} per fornire la logica di backend e controllare notifiche di push e autenticazione, il che è tutto scritto in Swift.

![BluePic](images/cloudlogic.png "Flusso di BluePic")

Nell'esempio BluePic, quando viene pubblicata un'immagine, i suoi dati vengono registrati in Cloudant e il file binario dell'immagine viene archiviato in Object Storage. Da lì, viene richiamata una sequenza {{site.data.keyword.openwhisk_short}} che produce il calcolo dei dati meteorologici quali la temperatura e la condizione attuale (ad esempio soleggiato, nuvoloso) sulla base della località da dove era stata caricata un'immagine. Watson Visual Recognition viene anche utilizzato nella sequenza {{site.data.keyword.openwhisk_short}} per analizzare l'immagine ed estrarre le tag di testo sulla base del contenuto dell'immagine. Infine, all'utente viene inviata una notifica di push che lo informa che l'immagine è stata elaborata e che ora include i dati meteorologici e di tag.
