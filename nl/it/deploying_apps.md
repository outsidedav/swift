---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: deploy swift app, deploy swift, serverless swift, deploy swift cloud foundry, swift kubernetes, swift virtual server

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Distribuzione e integrazione di applicazioni Swift
{: #deploy_apps-swift}

Puoi distribuire le tue applicazioni Swift con una toolchain oppure utilizzando l'interfaccia riga di comando. Una toolchain è una serie di integrazioni degli strumenti. L'interfaccia riga di comando è un modo semplice per distribuire le tue applicazioni e le tue istanze del servizio.

Per ulteriori informazioni, consulta [Distribuzione delle applicazioni](/docs/apps?topic=creating-apps-create-deploy-app-cli#create-deploy-app-cli).

## Sviluppo di applicazioni Swift senza server
{: #serverless-swift}

Per lo sviluppo senza server, puoi utilizzare l'offerta FaaS (Functions as a Service) di IBM, {{site.data.keyword.openwhisk}}. {{site.data.keyword.openwhisk_short}} abilita l'esecuzione della logica dell'applicazione in risposta agli eventi o ai richiami diretti da applicazioni web o mobili su HTTP senza la creazione o la gestione dei server.

{{site.data.keyword.openwhisk_short}} esegue l'amministrazione del sistema, come ad esempio il ridimensionamento automatico, la gestione della disponibilità e la manutenzione, consentendo agli sviluppatori di rimanere concentrati sulla scrittura della logica dell'applicazione.

Per ulteriori informazioni, vedi [Sviluppo di applicazioni senza server](/docs/apps/deploying?topic=creating-apps-serverless#serverless).

## Integrazione dei servizi di back-end con un SDK generato
{: #backend_gensdk-swift}

Il plug-in {{site.data.keyword.IBM_notm}} SDK Generator integra facilmente i servizi di back-end alla tua applicazione con un SDK generato. Quando si verifica una modifica a un'API REST, puoi rigenerare l'SDK e sostituire quello vecchio per eseguire l'upgrade dell'SDK. Puoi anche integrare la CLI in una pipeline DevOps e assicurare che l'SDK sia sempre congruente con la specifica API ogni volta che l'applicazione viene creata.

Per ulteriori informazioni, vedi [Integrazione dei servizi di back-end con un SDK generato](/docs/swift/backend?topic=swift-sdkgen-cli#sdkgen-cli).

## Distribuzione a un cluster Kubernetes
{: #deploy_kube-swift}

Puoi imparare come utilizzare il servizio Kubernetes {{site.data.keyword.cloud_notm}} per distribuire un'applicazione inserita nel contenitore che si avvale di Watson Tone Analyzer. Nello scenario fornito, una società di PR fittizia utilizza il servizio {{site.data.keyword.cloud_notm}} per analizzare i suoi comunicati stampa e ricevere un feedback sul tono dei suoi messaggi. Per ulteriori informazioni, vedi l'esercitazione [Distribuzione di applicazioni nei cluster Kubernetes](/docs/containers?topic=containers-cs_apps_tutorial).

## Distribuzione a Cloud Foundry
{: #swift-deploy-cf}

Questa opzione distribuisce la tua applicazione nativa del cloud senza che tu debba gestire l'infrastruttura sottostante.

Se intendi distribuire la tua applicazione a [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about), devi [preparare il tuo account {{site.data.keyword.cloud_notm}}](/docs/cloud-foundry?topic=cloud-foundry-prepare).

Se il tuo account ha accesso a {{site.data.keyword.cfee_full_notm}}, puoi selezionare un tipo di deployer **[Cloud pubblico](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** o **[Ambiente aziendale](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)**, che puoi utilizzare per creare e gestire ambienti isolati per ospitare applicazioni Cloud Foundry esclusivamente per la tua azienda.

## Distribuzione a un Virtual Server
{: #virtual_deploy-swift}

Un servizio virtuale offre un elevato grado di trasparenza, prevedibilità e automazione per tutti i tipi di carichi di lavoro. Viene utilizzato Terraform per creare, modificare e controllare la versione dell'infrastruttura in modo sicuro ed efficiente.

  La distribuzione di VSI è disponibile per alcuni kit starter. Per utilizzare questa funzione, vai al [dashboard {{site.data.keyword.cloud_notm}}](https://{DomainName}) e fai clic su **Create an app** nel tile **Apps**.
  {: note}

Per ulteriori informazioni, vedi [Distribuzione a un server virtuale](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server).
