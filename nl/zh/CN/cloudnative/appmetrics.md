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

# 将 Application Metrics 与 Swift 应用程序配合使用
{: #metrics}

Application Metrics 对于监视应用程序的性能非常重要。监视环境的性能（包括 CPU、内存、等待时间和 HTTP 度量值）可能看起来是非常艰巨的工作，但最基本的是确保 Swift 应用程序一直有效运行。Cloud Native 服务（例如，自动扩展）可以依赖这些度量值在峰值负载下扩展应用程序来执行工作，也可以向下扩展使成本保持在低水平。

Application Metrics 可以帮助确定常见性能问题，例如：

* 某些路径或所有路径上的 HTTP 响应时间缓慢
* 应用程序中的吞吐量差
* 导致速度缓慢的需求峰值
* 超出用于吞吐量/负载级别的预期 CPU 使用量
* 内存使用量高和/或不断增长（潜在内存泄漏）

## 向现有 Swift 应用程序添加 Application Metrics
{: #add-appmetrics-existing}

使用 [Application Metrics for Swift](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/) 将性能监视添加到 Swift 应用程序。Application Metrics for Swift 由两个库组成：`SwiftMetrics` 和 `SwiftMetricsDash`。

* `SwiftMetrics` 库是一个综合检测库，用于收集和聚集应用程序的度量值。此库有多个扩展，包括用于 HTTP 度量值的 Kitura 模块、[Prometheus 支持](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support)和独立[发射器](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent)。

* `SwiftMetricsDash` 库使用 `SwiftMetrics` 生成的度量值，并为可视化提供内置仪表板。


要启用基本监视 API，请将 `SwiftMetrics` 添加到 `Package.swift` 中的 **dependencies:** 部分，并确保将其添加到所需目标：
```swift
.package(url: "https://github.com/RuntimeTools/SwiftMetrics.git", from: "2.4.0")
```
{: codeblock}

添加以下检测代码以初始化 `SwiftMetrics` 代码：
```swift
import SwiftMetrics
import SwiftMetricsKitura

let metrics = try SwiftMetrics()
SwiftMetricsKitura(swiftMetricsInstance: metrics)
let monitoring = metrics.monitor()
```
{: codeblock}

将以下代码添加到原始样本，以在监视仪表板中提供度量值：
```swift
import SwiftMetricsDash

let smd = try SwiftMetricsDash(swiftMetricsInstance : metrics)
```  
{: codeblock}

缺省情况下，`SwiftMetricsDash` 会启动其自己的 Kitura 服务器，并在 `http://<hostname>:<port>/swiftmetrics-dash` 下提供页面。访问仪表板可查看新的应用程序度量值，包括 HTTP 请求和事件循环等待时间。

## 在入门模板工具包中使用 Application Metrics

通过入门模板工具包创建的服务器端 Swift 应用程序包括 `SwiftMetrics`、`SwiftMetricsDash` 和 `SwiftMetricsPrometheus`，因此这些应用程序可随时在使用 Prometheus 端点收集度量值的 Kubernetes 环境中使用。

`SwiftMetrics` 代码可以在 `/Sources/Application/Metrics.swift` 中找到：
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
    do {let metrics = try SwiftMetrics()
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

应用程序运行后，可以使用 `/swiftmetrics-dash` 端点来访问仪表板。

缺省情况下，`SwiftMetricsPrometheus` 会在 `http://<hostname>:<port>/metrics` 下提供 [Prometheus 端点](https://prometheus.io/)。
