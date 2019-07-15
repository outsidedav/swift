---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-07"

keywords: develop swift, swift local, service credentials swift, developer tools swift, swift cli, ibmcloud build swift, ibmcloud swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Sviluppo in locale
{: #develop-locally}

Eseguendo uno sviluppo in locale, puoi facilmente creare, eseguire e verificare applicazioni Swift. Utilizzi {{site.data.keyword.dev_cli_short}} per eseguire tali azioni servendoti di comandi standard. 

Puoi utilizzare {{site.data.keyword.dev_cli_short}} per gestire le tue applicazioni lato server con più di una dozzina di comandi. Accedi ad ulteriori informazioni sui diversi comandi nel documento relativo ai [{{site.data.keyword.dev_cli_notm}}comandi `ibmcloud dev` della CLI](/docs/cli/idt?topic=cloud-cli-idt-cli).

## Prima di cominciare
{: #prereqs-local}

Per sviluppare in locale, devi installare la {{site.data.keyword.dev_cli_notm}}. Utilizza questo comando per eseguire lo script di installazione:
```
curl -sL https://ibm.biz/idt-installer | bash
```
{: codeblock}

Vedi il documento relativo alla [Configurazione della CLI {{site.data.keyword.dev_cli_notm}}](/docs/cli?topic=cloud-cli-getting-started) per ulteriori informazioni sulla configurazione e l'utilizzo della {{site.data.keyword.dev_cli_notm}}.

## Richiamo delle credenziali del servizio
{: #credentials-local}

Dopo che hai clonato la tua applicazione da Git, devi richiamare le credenziali per i servizi associati alla tua applicazione poiché non sono memorizzate nel repository Git per la tua applicazione. Il richiamo delle credenziali consente l'utilizzo dei servizi associati. Puoi facilmente scaricare le credenziali eseguendo questo comando nella root della directory dell'applicazione:
```
ibmcloud dev get-credentials
```
{: codeblock}

## Creazione, esecuzione e sviluppo della tua applicazione
{: #build-deploy-local}

1. **Crea** - puoi ora creare la tua applicazione, che è un prerequisito per eseguire la tua applicazione.
  Utilizza questo comando nella root della directory dell'applicazione per creare la tua applicazione:
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **Esegui** - dopo una creazione eseguita con esito positivo, puoi eseguire la tua applicazione in un contenitore locale con il seguente comando:
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  Se il comando viene eseguito correttamente, vengono visualizzati un host e una porta locali per visualizzare la pagina di destinazione della tua applicazione.

3. **Distribuisci** - distribuisci la tua applicazione a {{site.data.keyword.cloud_notm}} con il comando `deploy`:
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}
