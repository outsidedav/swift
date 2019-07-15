---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-07"

keywords: swiftmetrics-dash, swiftmetrics, prometheus swift, application metrics swift, swift performance, slow swift, swift dashboard, metris swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilizzo delle metriche dell'applicazione con le applicazioni Swift
{: #metrics}

Le metriche dell'applicazione sono importanti per il monitoraggio delle prestazioni della tua applicazione. Avere una vista in diretta delle metriche come CPU, memoria, latenza e HTTP è essenziale per garantire che l'esecuzione della tua applicazione sia efficace nel tempo. I servizi Kubernetes e Cloud Foundry come il [ridimensionamento automatico](/docs/services/Auto-Scaling?topic=Auto-Scaling-get-started) si basano sulle metriche per determinare quando aggiungere o rimuovere in modo dinamico le istanze in base al carico ed eliminare le istanze che non sono più necessarie per mantenere bassi i costi.

Le metriche dell'applicazione vengono acquisite come dati delle serie temporali. L'aggregazione e la visualizzazione delle metriche acquisite possono essere di ausilio nell'identificazione di problemi delle prestazioni comuni quali:

* Tempi di risposta HTTP lenti su qualche instradamento o su tutti gli instradamenti
* Una scarsa velocità effettiva nell'applicazione
* Dei picchi di richiesta che causano un rallentamento
* Un utilizzo della CPU più elevato del previsto
* Un utilizzo della memoria elevato o crescente (potenziale perdita di memoria)

## Aggiunta di metriche dell'applicazione all'applicazione Swift esistente
{: #add-appmetrics-existing}

Utilizza [Application Metrics for Swift](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") per aggiungere il monitoraggio delle prestazioni alla tua applicazione Swift. Application Metrics for Swift è formato da due librerie: `SwiftMetrics` e `SwiftMetricsDash`.

* La libreria `SwiftMetrics` è una libreria di strumentazione completa che raccoglie e aggrega le metriche per la tua applicazione. Ha diverse estensioni, tra cui un modulo Kitura per le metriche HTTP, il [supporto Prometheus](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") e un [emettitore](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") autonomo.

* La libreria `SwiftMetricsDash` utilizza le metriche prodotte da `SwiftMetrics` e fornisce un dashboard integrato per la visualizzazione.

Per abilitare l'API di monitoraggio di base, aggiungi `SwiftMetrics` alla sezione **dependencies:** nel tuo `Package.swift` e assicurati di aggiungerla alla destinazione appropriata:
```swift
.package(url: "https://github.com/RuntimeTools/SwiftMetrics.git", from: "2.4.0")
```
{: codeblock}

Aggiungi il seguente codice di strumentazione per inizializzare il codice `SwiftMetrics`:
```swift
import SwiftMetrics
import SwiftMetricsKitura

let metrics = try SwiftMetrics()
SwiftMetricsKitura(swiftMetricsInstance: metrics)
let monitoring = metrics.monitor()
```
{: codeblock}

Aggiungi il seguente codice all'esempio originale per fornire metriche nel dashboard di monitoraggio:
```swift
import SwiftMetricsDash

let smd = try SwiftMetricsDash(swiftMetricsInstance : metrics)
```  
{: codeblock}

Per impostazione predefinita, `SwiftMetricsDash` avvia il proprio server Kitura e fornisce la pagina in `http://<hostname>:<port>/swiftmetrics-dash`. Accedi al dashboard per visualizzare le nuove metriche dell'applicazione, incluse le richieste HTTP, e la latenza del loop di eventi.

## Utilizzo delle metriche dell'applicazione nei kit starter
{: #appmetrics-starterkits}

Le applicazioni Swift lato server create dai kit starter includono `SwiftMetrics`, `SwiftMetricsDash` e `SwiftMetricsPrometheus`, quindi sono pronte per l'utilizzo in ambienti Kubernetes che utilizzano gli endpoint Prometheus per raccogliere le metriche.

Il codice `SwiftMetrics` è disponibile in `/Sources/Application/Metrics.swift`:
```swift
import Kitura
import SwiftMetrics
import SwiftMetricsDash
import SwiftMetricsPrometheus
import LoggerAPI

var swiftMetrics: SwiftMetrics?
var swiftMetricsDash: SwiftMetricsDash?
var swiftMetricsPrometheus: SwiftMetricsPrometheus?

func initializeMetrics(router: Router) {
    do {
        let metrics = try SwiftMetrics()
        let dashboard = try SwiftMetricsDash(swiftMetricsInstance: metrics, endpoint: router)
        let prometheus = try SwiftMetricsPrometheus(swiftMetricsInstance: metrics, endpoint: router)

        swiftMetrics = metrics
        swiftMetricsDash = dashboard
        swiftMetricsPrometheus = prometheus
        Log.info("Initialized metrics.")
    } catch {
        Log.warning("Failed to initialize metrics: \(error)")
    }
}
```
{: codeblock}

Una volta che l'applicazione è in esecuzione, puoi accedere al dashboard utilizzando l'endpoint `/swiftmetrics-dash`.

Per impostazione predefinita, `SwiftMetricsPrometheus` fornisce l'[endpoint Prometheus](https://prometheus.io/){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") in `http://<hostname>:<port>/metrics`.
