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

# Swift アプリでのアプリケーション・メトリックの使用
{: #metrics}

アプリケーション・メトリックは、アプリケーションのパフォーマンスをモニターするのに重要です。CPU、メモリー、待ち時間、HTTP のメトリックを含む、環境のパフォーマンスをモニターすると、非常に労力がかかる場合がありますが、Swift アプリケーションが長期にわたって効率的に稼働することを保証するのに不可欠です。自動スケーリングなどのクラウド・ネイティブ・サービスは、アプリケーションがピーク・ロードを下回って実行されるようにスケーリングしたり、低コストが維持されるようにアプリケーションを縮小したりするのに、これらのメトリックに頼ります。

アプリケーション・メトリックは、以下のような一般的なパフォーマンス上の問題を識別するのに役立つことがあります。

* 一部またはすべての経路で HTTP 応答時間が遅くなる
* アプリケーションのスループットが低下する
* 需要の急上昇によりスローダウンが発生する
* スループット/負荷のレベルで、予期される CPU 使用量を上回る
* メモリー使用量が多いか、増加しているか、またはその両方 (メモリー・リークの可能性)

## 既存の Swift アプリケーションへのアプリケーション・メトリックの追加
{: #add-appmetrics-existing}

Swift アプリケーションにパフォーマンス・モニターを追加するには、[Application Metrics for Swift](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/) を使用します。Application Metrics for Swift は、`SwiftMetrics` と `SwiftMetricsDash` の 2 つのライブラリーで構成されます。

* `SwiftMetrics` ライブラリーは、アプリケーションに関するメトリックを収集して集約する包括的な計測ライブラリーです。HTTP メトリック用の Kitura モジュール、[Prometheus サポート](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support)、スタンドアロンの[エミッター](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent)などのいくつかの拡張機能があります。

* `SwiftMetricsDash` ライブラリーは、`SwiftMetrics` で作成されるメトリックを利用し、視覚化用の組み込みのダッシュボードを提供します。


基本のモニター API を有効にするには、`Package.swift` 内の **dependencies:** セクションに `SwiftMetrics` を追加し、ご希望のターゲットに追加されていることを確認してください。
```swift
.package(url: "https://github.com/RuntimeTools/SwiftMetrics.git", from: "2.4.0")
```
{: codeblock}

以下の計測コードを追加して `SwiftMetrics` コードを初期化します。
```swift
import SwiftMetrics
import SwiftMetricsKitura

let metrics = try SwiftMetrics()
SwiftMetricsKitura(swiftMetricsInstance: metrics)
let monitoring = metrics.monitor()
```
{: codeblock}

以下のコードを元のサンプルに追加して、モニタリング・ダッシュボード内にメトリックを提供します。
```swift
import SwiftMetricsDash

let smd = try SwiftMetricsDash(swiftMetricsInstance : metrics)
```  
{: codeblock}

デフォルトで、`SwiftMetricsDash` は、独自の Kitura サーバーを開始し、`http://<hostname>:<port>/swiftmetrics-dash` ページで要求を listen します。
HTTP 要求やイベント・ループの待ち時間などの新しいアプリケーション・メトリックを参照するには、ダッシュボードにアクセスしてください。

## スターター・キットでのアプリケーション・メトリックの使用

スターター・キットから作成されるサーバー・サイドの Swift アプリケーションには `SwiftMetrics`、`SwiftMetricsDash`、`SwiftMetricsPrometheus` が含まれており、Prometheus エンドポイントを使用する Kubernetes 環境でメトリックの収集に使用する準備ができています。

`SwiftMetrics` コードは、`/Sources/Application/Metrics.swift` にあります。
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

アプリケーションが稼働中になると、`/swiftmetrics-dash` エンドポイントを使用してダッシュボードにアクセスできます。

デフォルトでは、`SwiftMetricsPrometheus` は `http://<hostname>:<port>/metrics` で [Prometheus エンドポイント](https://prometheus.io/)を提供します。
