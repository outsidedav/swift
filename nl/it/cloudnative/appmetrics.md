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

# Utilizzo delle metriche dell'applicazione con le applicazioni Swift
{: #metrics}

Le metriche dell'applicazione sono importanti per monitorare le prestazioni della tua applicazione. Monitorare le prestazioni dell'ambiente, comprese le metriche di HTTP, latenza, memoria e CPU, può sembrare uno sforzo enorme ma è essenziale per garantire che l'esecuzione della tua applicazione Swift sia efficace nel tempo. I servizi nativi cloud, quali il ridimensionamento automatico, possono basarsi su tali metriche per ingrandire la tua applicazione perché funzioni in condizioni di carico di picco e per ridurla per mantenere bassi i costi.

Le metriche dell'applicazione consentono di identificare problemi di prestazioni comuni, come ad esempio:

* Dei tempi di risposta HTTP lenti su qualcuno degli instradamenti o su tutti
* Una scarsa velocità effettiva nell'applicazione
* Dei picchi di richiesta che causano un rallentamento
* Un utilizzo della CPU più elevato del previsto per il livello di velocità effettiva/carico
* Un utilizzo della memoria elevato e/o crescente (potenziale perdita di memoria)

## Aggiunta di metriche dell'applicazione all'applicazione Swift esistente
{: #add-appmetrics-existing}

Utilizza [Application Metrics for Swift](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/) per aggiungere il monitoraggio delle prestazioni alla tua applicazione Swift. Application Metrics for Swift è formato da due librerie: `SwiftMetrics` e `SwiftMetricsDash`.

* La libreria `SwiftMetrics` è una libreria di strumentazione completa che raccoglie e aggrega le metriche per la tua applicazione. Ha diverse estensioni, tra cui un modulo Kitura per le metriche HTTP, il [supporto Prometheus](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support) e un [emettitore](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent) autonomo.

* La libreria `SwiftMetricsDash` utilizza le metriche prodotte da `SwiftMetrics` e fornisce un dashboard integrato per la visualizzazione.


Per abilitare l'API di monitoraggio di base, aggiungi `SwiftMetrics` alla sezione **dependencies:** nel tuo `Package.swift` e assicurati di aggiungerla alla destinazione desiderata:
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

Per impostazione predefinita, `SwiftMetricsPrometheus` fornisce l'[endpoint Prometheus](https://prometheus.io/) in `http://<hostname>:<port>/metrics`.
