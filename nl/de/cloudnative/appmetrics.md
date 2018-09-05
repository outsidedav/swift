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

# Anwendungsmetriken bei Swift-Apps verwenden
{: #metrics}

Anwendungsmetriken sind wichtig, um die Leistung Ihrer Anwendung zu
überwachen. Die Überwachung der Leistung in der Umgebung (wie CPU,
Hauptspeicher, Latenzzeit und HTTP) scheint möglicherweise mit einem
erheblichen Aufwand verbunden zu sein, ist jedoch essenziell, damit gewährleistet
wird, dass Ihre Swift-Anwendung jederzeit effizient ausgeführt wird. Gestützt
auf solche Metriken können cloudnative
Services wie beispielsweise die automatische Skalierung ein Scale-up
für Ihre App durchführen, um die Ausführung bei einer Spitzenbelastung
sicherzustellen, oder auch ein Scale-down, um die Kosten gering zu halten.

Anwendungmetriken können Sie bei der Erkennung allgemeiner
Leistungsprobleme unterstützen. Beispiele:

* Lange HTTP-Antwortzeiten bei einigen oder allen Routen
* Schlechter Durchsatz in der Anwendung
* Leistungsminderung infolge von Nachfragespitzen
* CPU-Auslastung in unerwarteter Höhe auf Durchsatz-/Lastebene
* Hohe und/oder zunehmende Hauptspeicherbelegung (potenzieller
Speicherverlust)

## Anwendungsmetriken zu einer vorhandenen Swift-Anwendung hinzufügen
{: #add-appmetrics-existing}

Mit
[Application
Metrics for Swift](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/) können Sie die Leistungsüberwachung zu Ihrer
Swift-Anwendung hinzufügen. Application Metrics for Swift besteht aus zwei
Bibliotheken namens `SwiftMetrics` und `SwiftMetricsDash`.

* Die Bibliothek `SwiftMetrics` ist eine umfassende
Instrumentierungsbibliothek, in der Metriken für Ihre Anwendung
zusammengestellt und aggregiert werden. Sie besitzt mehrere Erweiterungen,
unter anderem ein
Kitura-Modul für HTTP-Metriken,
eine [Prometheus-Unterstützung](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support)
und einen eigenständigen
[Emitter](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent).

* Die Bibliothek `SwiftMetricsDash` verarbeitet die von
`SwiftMetrics` erzeugten Metriken und bietet ein integriertes
Dashboard für ihre Darstellung.


Um die Basis-API für die Überwachung zu
aktivieren, fügen Sie `SwiftMetrics` zum Abschnitt
**dependencies:** in der Datei
`Package.swift` hinzu (achten Sie darauf, sie zum gewünschten
Ziel hinzuzufügen):
```swift
.package(url: "https://github.com/RuntimeTools/SwiftMetrics.git", from: "2.4.0")
```
{: codeblock}

Fügen Sie den folgenden Instrumentierungscode hinzu, um den Code von
`SwiftMetrics` zu initialisieren:
```swift
import SwiftMetrics
import SwiftMetricsKitura

let metrics = try SwiftMetrics()
SwiftMetricsKitura(swiftMetricsInstance: metrics)
let monitoring = metrics.monitor()
```
{: codeblock}

Fügen Sie den folgenden Code zum ursprünglichen Beispiel hinzu, damit
Metriken im Überwachungsdashboard zur Verfügung gestellt werden:
```swift
import SwiftMetricsDash

let smd = try SwiftMetricsDash(swiftMetricsInstance : metrics)
```  
{: codeblock}

`SwiftMetricsDash` startet standardmäßig einen eigenen
Kitura-Server und stellt die Seite unter
der Adresse `http://<hostname>:<port>/swiftmetrics-dash`
bereit. Wenn Sie auf das Dashboard zugreifen, werden Ihre neuen
Anwendungsmetriken inklusive HTTP-Anforderungen und Ereignisschleifenlatenz
angezeigt.

## Anwendungsmetriken in Starter-Kits verwenden

Die serverseitigen Swift-Anwendungen, die ausgehend von Starter-Kits
erstellt werden, beinhalten den Code für `SwiftMetrics`,
`SwiftMetricsDash` und
`SwiftMetricsPrometheus` und können daher sofort in
Kubernetes-Umgebungen eingesetzt werden, die Prometheus-Endpunkte zur Erfassung
von Metriken verwenden.

Der Code für `SwiftMetrics` ist in der Datei
`/Sources/Application/Metrics.swift` zu finden:
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

Sobald Ihre Anwendung ausgeführt wird, können Sie über den Endpunkt
`/swiftmetrics-dash` auf das Dashboard zugreifen.

Der Code für `SwiftMetricsPrometheus` stellt
standardmäßig den
[Prometheus-Endpunkt](https://prometheus.io/) unter der
Adresse
`http://<hostname>:<port>/metrics` bereit.
