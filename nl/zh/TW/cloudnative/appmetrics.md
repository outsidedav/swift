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

# 搭配使用應用程式度量值與 Swift 應用程式
{: #metrics}

應用程式度量值對監視應用程式的效能而言，非常重要。監視環境的效能，包括 CPU、記憶體、延遲及 HTTP 度量值，看似工作量很大，但正因為有這些才能確保您的 Swift 應用程式隨著時間推移都能有效率地執行。Cloud Native 服務（例如，自動調整）可以仰賴這些度量值來調整您的應用程式，以在尖峰負載下執行，並可藉由往下調整來降低成本。

應用程式度量值可協助您識別一般的效能問題，例如：

* 部分或所有路徑上的 HTTP 回應時間太慢
* 應用程式的產量太低
* 需求飆升導致速度變慢
* 產量/負載層次的 CPU 使用率比預期的還高
* 記憶體用量太高及/或持續成長中（可能記憶體洩漏）

## 新增應用程式度量值至現有的 Swift 應用程式
{: #add-appmetrics-existing}

使用 [ApplicationMetrics for Swift](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/)，將效能監視新增至您的 Swift 應用程式。Application Metrics for Swift 包含 2 個程式庫：`SwiftMetrics` 及`SwiftMetricsDash`。

* `SwiftMetrics` 程式庫是綜合性的設備測試程式庫，會收集並聚集您應用程式的度量值。它有數個延伸規格，包括適用於 HTTP 度量值的 Kitura 模組、[Prometheus 支援](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support)以及獨立式[產生器](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent)。

* `SwiftMetricsDash` 程式庫會耗用 `SwiftMetrics` 所產生的度量值，並提供內建儀表板以供視覺化顯示。


若要啟用基礎監視 API，請將 `SwiftMetrics` 新增至 `Package.swift` 中的 **dependencies:** 區段，並確定將其新增至想要的目標：
```swift
.package(url: "https://github.com/RuntimeTools/SwiftMetrics.git", from: "2.4.0")
```
{: codeblock}

新增下列設備測試代碼，以起始設定 `SwiftMetrics` 程式碼：
```swift
import SwiftMetrics
import SwiftMetricsKitura

let metrics = try SwiftMetrics()
SwiftMetricsKitura(swiftMetricsInstance: metrics)
let monitoring = metrics.monitor()
```
{: codeblock}

將下列程式碼新增至原始範例，以在監視儀表板中提供度量值：
```swift
import SwiftMetricsDash

let smd = try SwiftMetricsDash(swiftMetricsInstance : metrics)
```  
{: codeblock}

依預設，`SwiftMetricsDash` 會啟動自己的 Kitura 伺服器，並在 `http://<hostname>:<port>/swiftmetrics-dash` 之下提供頁面。存取儀表板，以查看您的新應用程式度量值，包括 HTTP 要求及事件迴圈延遲。

## 使用入門範本套件中的應用程式度量值

從「入門範本套件」建立的伺服器端 Swift 應用程式包括`SwiftMetrics`、`SwiftMetricsDash` 及 `SwiftMetricsPrometheus`，因此，它們已經準備好在使用 Prometheus端點的 Kubernetes 環境中，用來收集度量值。

`SwiftMetrics` 程式碼位於 `/Sources/Application/Metrics.swift`：
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

您的應用程式在執行中之後，便可使用 `/swiftmetrics-dash` 端點來存取儀表板。

依預設，`SwiftMetricsPrometheus` 會在 `http://<hostname>:<port>/metrics` 之下提供 [Prometheus 端點](https://prometheus.io/)。
