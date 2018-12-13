---

copyright:
  years: 2018
lastupdated: "2018-11-08"

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

Puoi utilizzare {{site.data.keyword.dev_cli_short}} per gestire le tue applicazioni lato server con più di una dozzina di comandi. Trova ulteriori informazioni sui comandi `ibmcloud dev` nel documento relativo alla [CLI IBM Cloud Developer Tools](/docs/cli/idt/commands.html).

## Passo 1. Requisiti per gli sviluppatori

Per iniziare a utilizzare {{site.data.keyword.cloud_notm}}, assicurati di soddisfare i seguenti requisiti.

### Sistema operativo

Sviluppa le applicazioni Swift con le migliori prassi utilizzando l'hardware supportato MacOS più recente ed eseguendo le attività di test con le release iOS più recenti. Registrati per un account [Apple Developer](https://developer.apple.com/) per abilitare l'esecuzione di test su un dispositivo fisico.

### iOS e Xcode
{: #ios_and_xcode}

- [Installa Xcode 8+ (o superiore)](https://developer.apple.com/xcode/)
- [Esegui la distribuzione a dispositivi iOS 8 (o superiore)](https://support.apple.com/downloads/ios)
- Prima dell'inoltro dell'applicazione ad Apple, leggi attentamente le [linee guida sugli inoltri all'App Store](https://developer.apple.com/app-store/guidelines/)

### Gestione di dipendenze ed SDK

I seguenti strumenti assicurano che tu possa installare gli SDK nativi per lavorare con i vari servizi {{site.data.keyword.cloud_notm}}.

- [Installa CocoaPods per gli SDK IBM Cloud](https://cocoapods.org/)
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Installa Homebrew per facilitare l'installazione di Carthage](https://brew.sh/)
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Installa Carthage per gli SDK Watson](https://github.com/Carthage/Carthage)
  ```
  brew install carthage
  ```
  {: codeblock}

## Passo 2. Installazione di strumenti per lo sviluppo locale

{{site.data.keyword.cloud}} fornisce strumenti CLI locali che ti aiutano a lavorare con diversi aspetti di {{site.data.keyword.cloud_notm}}. Per ulteriori informazioni, vedi [Informazioni sulla {{site.data.keyword.dev_cli_long}}](../cli/index.html). Puoi utilizzare gli strumenti per testare un backend Swift in un'immagine Docker locale prima della distribuzione cloud. 

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

1. Esegui il comando `ibmcloud dev create` dalla CLI {{site.data.keyword.dev_cli_short}} per generare uno starter preconfigurato. 
  ```
  ibmcloud dev create
  ```
  {: codeblock}

  Assicurati di eseguire l'accesso con un account {{site.data.keyword.cloud_notm}} per creare un progetto. I nuovi utenti possono eseguire la [registrazione ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/registration/?cm_sp=dw-bluemix-_-swift-_-devcenter) per un account gratuito. Utilizza il comando `ibmcloud login` per eseguire l'accesso sulla riga di comando.
  {: tip}

2. Quando richiesto, seleziona le opzioni 1, quindi 6 e infine, 2, come visualizzato nel seguente esempio:
  ```
  ? Select a resource type:
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
  ```
  {: screen}

4. Scegli di abilitare il supporto OpenAPI 2.0:
  ```
  ? Enable your application based on an OpenAPI 2.0 Specification document? [y/n]> y
  ```
  {: screen}

  Se il supporto OpenAPI 2.0 è abilitato, devi fornire un percorso per il documento OpenAPI 2.0 come un url:
  ```
  ? Path to OpenAPI 2.0 document as a url (both yaml and json formats supported)> http://hostname.domain.com/path/to/file.json

  Generating application ...
  ```
  {: screen}

## Passo 4. Creazione, esecuzione e distribuzione della tua applicazione

Puoi ora creare, eseguire e distribuire la tua applicazione utilizzando {{site.data.keyword.dev_cli_short}}.

1. **Crea**

  Puoi ora creare la tua applicazione, che è un prerequisito per eseguire la tua applicazione. Utilizza questo comando nella root della directory dell'applicazione per creare la tua applicazione:
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **Esegui**

  Dopo una creazione eseguita con esito positivo, puoi eseguire la tua applicazione in un contenitore locale con il seguente comando:
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  Se il comando viene eseguito correttamente, vengono visualizzati un host e una porta locali per visualizzare la pagina di destinazione della tua applicazione.

3. **Distribuisci**

  Distribuisci la tua applicazione a {{site.data.keyword.cloud_notm}} con il comando `deploy`:
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}

## Passi successivi

Impara a utilizzare la {{site.data.keyword.cloud_notm}} Developer Console for Apple, che consente agli sviluppatori di creare applicazioni da diversi kit starter, creare e collegare i servizi ottimizzati {{site.data.keyword.cloud_notm}} chiave e di scaricare quindi rapidamente il codice funzionante o di eseguire la configurazione per una fornitura continua. Gli utenti possono creare, visualizzare, configurare e gestire la tua applicazione, nonché scaricare il codice della tua applicazione. Utilizzando la Developer Console for Apple, puoi valutare rapidamente e testare i servizi {{site.data.keyword.cloud_notm}} con un'applicazione nuova.

Pronto a cominciare? Per un'introduzione, visita [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits).
{: tip}

Per ulteriori informazioni, vedi il documento relativo allo [sviluppo di applicazioni Swift con i kit starter](/docs/swift/starter_kit/starter_kits.html).
