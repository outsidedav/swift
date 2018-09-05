---

copyright:
  years: 2018
lastupdated: "2018-08-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Registrazione in Swift
{: #logging_swift}

I log sono richiesti per diagnosticare come e perché i servizi hanno esito negativo. I log non sono concepiti per essere utilizzati per il monitoraggio delle prestazioni dell'applicazione, che è quello a cui servono le metriche, ma possono essere utilizzati come una origine per gli avvisi, che possono quindi includere più dettagli di quelli che puoi ottenere dalle metriche aggregate.

Uno dei vantaggi derivanti dall'utilizzare l'infrastruttura cloud è che la tua applicazione non deve più preoccuparsi di molte cose, quali ad esempio la gestione del file di log. Data la natura transitoria dei processi negli ambienti cloud, i log devono essere raccolti e inviati altrove, di norma a un'ubicazione centralizzata per l'analisi. Il modo più congruente per registrare nei log negli ambienti cloud consiste nell'inviare le voci di log a flussi di output e di errore standard e lasciare che l'infrastruttura gestisca il resto.

Man mano che la tua applicazione si evolve nel tempo, la natura di quanto registri nei log può cambiare. Utilizzando un formato di log JSON, ottieni i seguenti vantaggi:
* I log sono indicizzabili, il che rende molto più semplice la ricerca in un corpo aggregato di log.
* I log sono resilienti alla modifica, in quanto l'analisi non dipende dalla posizione degli elementi in una stringa.

L'utilizzo della registrazione con formattazione JSON puoi rendere i log un po' più difficili da leggere per te, quando utilizzi gli strumenti di riga di comando per recuperare i log. Puoi utilizzare le variabili di ambiente per alternare il formato di log che viene utilizzato in modo da poter avere dei log in testo semplice per lo sviluppo e il debug locali.

## Aggiunta della registrazione nei log alla tua applicazione Swift

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) è un leggero e molto diffuso framework di registrazione per Swift e fornisce molti vantaggi nativi quali la registrazione nell'output standard e diversi livelli di log.

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) è il protocollo logger che fornisce un'interfaccia di registrazione comune per diversi tipi di logger in Swift. Kitura utilizza `LoggerAPI` in tutta la sua implementazione.

Per avvalerti di `HeliumLogger`, aggiungi quanto segue alla sezione **dependencies:** nel tuo `Package.swift`, assicurandoti di aggiungerlo a qualsiasi destinazione dove se ne fa uso.
```swift
.package(url: "https://github.com/IBM-Swift/HeliumLogger.git", from: "1.7.1")
```
{: codeblock}

Aggiungi il seguente codice di strumentazione per inizializzare `HeliumLogger` e impostalo come il logger utilizzato dalla `LoggerAPI`:
```swift
import HeliumLogger
import LoggerAPI

let logger = HeliumLogger(.verbose)
Log.logger = logger

Log.info("This is an informational log message.")
```
{: codeblock}

Nell'esempio fornito, il [livello di log](http://ibm-swift.github.io/HeliumLogger/) era esplicitamente impostato su `.verbose`, che è il valore predefinito.

Per ulteriori informazioni sulla personalizzazione dei messaggi di log, consulta la [documentazione di riferimento della API HeliumLogger](http://ibm-swift.github.io/HeliumLogger/).

## Registrazione con StarterKits
{: #monitoring}

Le applicazioni Swift che vengono create utilizzando il servizio dell'applicazione {{site.data.keyword.cloud_notm}} vengono utilizzate con `HeliumLogger` per impostazione predefinita. L'esecuzione dell'applicazione in modo nativo oppure in un ambiente cloud produce il seguente output:
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

Questi messaggi si trovano in `stdout` dall'esecuzione in locale oppure nei log per le distribuzioni [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) e [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/), a cui si accede, rispettivamente, mediante `ibmcloud app logs --recent <APP_NAME>` e `kubectl logs <deployment name>`.

Nel file `/Sources/AppName/main.swift`, puoi vedere il seguente codice:
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

Il livello di log è esplicitamente impostato su `.info` per registrare i messaggi di livello informativo come i log di avvio applicazione.
{: tip}

## Passi successivi 
{: #next_steps}

Ulteriori informazioni sulla visualizzazione dei log in ciascuno dei nostri ambienti di distribuzione.
* [Log Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [Log Cloud Foundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [{{site.data.keyword.openwhisk}} Log e monitoraggio](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

Utilizzando un aggregatore di log:
* [Analisi dei log {{site.data.keyword.cloud_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [Stack ELK privato {{site.data.keyword.cloud_notm}}](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
