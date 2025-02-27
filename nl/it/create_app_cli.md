---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-19"

keywords: server-side swift, swift cli, swift dependency, swift commands app, create app swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Creazione di un'applicazione Swift lato server con la CLI
{: #swift_cli}

{{site.data.keyword.cloud}} offre soluzioni e servizi per mettere a disposizione degli sviluppatori e delle applicazioni Swift la protezione, l'intelligenza artificiale e il valore richiesti dai loro clienti. Con un ampio portfolio di offerte ed SDK, puoi utilizzare questi servizi e immettere rapidamente sul mercato le tue applicazioni all'avanguardia.
{: shortdesc}

La seguente guida è concepita per aiutarti a creare, eseguire in locale e distribuire un'applicazione Swift lato server. Scopri come utilizzare {{site.data.keyword.dev_cli_long}} per eseguire queste azioni con i comandi standard.

Puoi utilizzare {{site.data.keyword.dev_cli_short}} per gestire le tue applicazioni lato server con più di una dozzina di comandi. Trova ulteriori informazioni sui comandi `ibmcloud dev` nel documento relativo alla [{{site.data.keyword.dev_cli_notm}}CLI](/docs/cli/idt?topic=cloud-cli-idt-cli).

## Passo 1. Requisiti per gli sviluppatori
{: #prereqs-swift-cli}

Per iniziare a utilizzare {{site.data.keyword.cloud_notm}}, assicurati di soddisfare i seguenti requisiti.

### Sistema operativo
{: #swift-cli-os-reqs}

Sviluppa le applicazioni Swift con le migliori prassi utilizzando l'hardware supportato MacOS più recente ed eseguendo le attività di test con le release iOS più recenti. Registrati per un account [Apple Developer](https://developer.apple.com/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") per abilitare l'esecuzione di test su un dispositivo fisico.

### iOS e Xcode
{: #swift-cli-ios_xcode}

- [Installa Xcode 8+ (o superiore)](https://developer.apple.com/xcode/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")
- [Distribuisci ai dispositivi iOS 8 (o superiore)](https://support.apple.com/downloads/ios){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")
- Prima dell'inoltro dell'applicazione ad Apple, leggi attentamente le [linee guida sugli inoltri all'App Store](https://developer.apple.com/app-store/resources/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")

### Gestione di dipendenze ed SDK
{: #swift-cli-sdk-dependency}

I seguenti strumenti assicurano che tu possa installare gli SDK nativi per lavorare con i vari servizi {{site.data.keyword.cloud_notm}}.

- [Installa CocoaPods per gli SDK {{site.data.keyword.cloud_notm}}](https://cocoapods.org/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Installa Homebrew per assistenza nell'installazione di Carthage](https://brew.sh/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Installa Carthage per gli SDK Watson](https://github.com/Carthage/Carthage){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")
  ```
  brew install carthage
  ```
  {: codeblock}

## Passo 2. Installazione di strumenti per lo sviluppo locale
{: #swift-cli-install-tools}

{{site.data.keyword.cloud}} fornisce strumenti CLI locali che ti aiutano a lavorare con diversi aspetti di {{site.data.keyword.cloud_notm}}. Per ulteriori informazioni, vedi [Informazioni sulla {{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-getting-started). Puoi utilizzare gli strumenti per testare un backend Swift in un'immagine Docker locale prima della distribuzione cloud.

* Per MacOS e Linux, esegui questo comando:
  ```
  curl -sL http://ibm.biz/idt-installer | bash
  ```
  {: codeblock}

* Per Windows 10, esegui il seguente comando come amministratore:
  ```
  Set-ExecutionPolicy Unrestricted; iex(New-Object Net.WebClient).DownloadString('http://ibm.biz/idt-win-installer')
  ```
  {: codeblock}

  Fai clic con il tasto destro del mouse sull'icona di Windows PowerShell e seleziona **Esegui come amministratore**.
  {: tip}

## Passo 3. Creazione di un'applicazione Swift
{: #create-swift-app-cli}

1. Esegui il comando `ibmcloud dev create` dalla CLI {{site.data.keyword.dev_cli_short}} per generare uno starter preconfigurato. 
  ```
  ibmcloud dev create
  ```
  {: codeblock}

  Assicurati di eseguire l'accesso con un account {{site.data.keyword.cloud_notm}} per creare un progetto. I nuovi utenti possono eseguire la [registrazione ](https://{DomainName}/registration){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") per un account gratuito. Utilizza il comando `ibmcloud login` per eseguire l'accesso sulla riga di comando.
  {: tip}

2. Quando richiesto, seleziona le opzioni 1, quindi 6 e infine, 2, come visualizzato nel seguente esempio:
  ```
  ? Select a service type:                  
  1. Backend Service / Web App
  2. Mobile Client
  Enter a number> 1

  ? Select a language:
  1. Java - MicroProfile / Java EE
  2. Java - Spring
  3. Node
  4. Python - Django
  5. Python - Flask
  6. Swift
  Enter a number> 6

  ? Select a Starter Kit:
  1. Backend for Frontend - Swift Kitura - A starter for building
  backend-for-frontend APIs in Swift, using the Kitura framework.
  2. Web App - Create Project
  3. Web App - Swift Kitura Basic - A starter that provides a basic web serving
  application in Swift, using the Kitura framework.
  Enter a number> 2
  ```
  {: screen}

3. Fornisci un nome per la tua applicazione:
  ```
  ? Enter a name for your application> swift_project

  Generating application ...
  ```
  {: screen}

## Passo 4. Creazione, esecuzione e distribuzione della tua applicazione
{: #swift-cli-deploy}

Puoi ora creare, eseguire e distribuire la tua applicazione utilizzando {{site.data.keyword.dev_cli_short}}.

### Creazione della tua applicazione
{: #swift-build-cli}

Puoi ora creare la tua applicazione, che è un prerequisito per eseguire la tua applicazione. Utilizza questo comando nella root della directory dell'applicazione per creare la tua applicazione:
```
ibmcloud dev build
```
{: codeblock}

### Esecuzione della tua applicazione
{: #swift-run-cli}

Dopo una creazione eseguita con esito positivo, puoi eseguire la tua applicazione in un contenitore locale con il seguente comando:
```
ibmcloud dev run
```
{: codeblock}

Se il comando viene eseguito correttamente, vengono visualizzati un host e una porta locali per visualizzare la pagina di destinazione della tua applicazione.

### Distribuzione della tua applicazione
{: #swift-deploy-cli}

Distribuisci la tua applicazione a {{site.data.keyword.cloud_notm}} con il comando `deploy`:
```
ibmcloud dev deploy
```
{: codeblock}

## Passi successivi
{: #swift-cli-next notoc}

Impara a utilizzare la {{site.data.keyword.cloud_notm}} Developer Console for Apple, che consente agli sviluppatori di creare applicazioni da diversi kit starter, creare e collegare i servizi ottimizzati {{site.data.keyword.cloud_notm}} chiave e di scaricare quindi rapidamente il codice funzionante o di eseguire la configurazione per una fornitura continua. Gli utenti possono creare, visualizzare, configurare e gestire la tua applicazione, nonché scaricare il codice della tua applicazione. Utilizzando la Developer Console for Apple, puoi valutare rapidamente e testare i servizi {{site.data.keyword.cloud_notm}} con un'applicazione nuova.

Pronto a cominciare? Per un'introduzione, visita [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").
{: tip}

Per ulteriori informazioni, vedi il documento relativo allo [sviluppo di applicazioni Swift con i kit starter](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro).
