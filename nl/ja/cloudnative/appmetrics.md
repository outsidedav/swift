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

# Swift アプリでのアプリケーション・メトリックの使用
{: #metrics}

アプリケーション・メトリックは、アプリケーションのパフォーマンスをモニターするのに重要です。 時間が経ってもアプリケーションが効果的に実行されていることを確認するには、CPU、メモリー、待ち時間、HTTP などのメトリックのライブ・ビューが不可欠です。 [Auto-Scaling](/docs/services/Auto-Scaling?topic=Auto-Scaling-get-started) などの Kubernetes や Cloud Foundry のサービスは、負荷に応じてインスタンスを動的に追加/削除するタイミングや、コストを抑えるために不要になったインスタンスをクリーンアップするタイミングを見極めるために、メトリックに依存しています。

アプリケーション・メトリックは、時系列データとして収集されます。 収集されたメトリックを集約して視覚化することは、以下のような一般的なパフォーマンス上の問題を特定するのに役立ちます。

* 一部またはすべての経路で HTTP 応答時間が遅くなる
* アプリケーションのスループットが低下する
* 需要の急上昇によりスローダウンが発生する
* 予想より CPU 使用量が多い
* メモリー使用量が多いか、増加している (メモリー・リークの可能性)

## 既存の Swift アプリケーションへのアプリケーション・メトリックの追加
{: #add-appmetrics-existing}

Swift アプリケーションにパフォーマンス・モニターを追加するには、[Application Metrics for Swift](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を使用します。 Application Metrics for Swift は、`SwiftMetrics` と `SwiftMetricsDash` の 2 つのライブラリーで構成されます。

* `SwiftMetrics` ライブラリーは、アプリケーションに関するメトリックを収集して集約する包括的な計測ライブラリーです。 ここには、HTTP メトリック用の Kitura モジュール、[Prometheus サポート](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")、スタンドアロンの[エミッター](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") などのいくつかの拡張機能があります。

* `SwiftMetricsDash` ライブラリーは、`SwiftMetrics` で作成されるメトリックを利用し、視覚化用の組み込みのダッシュボードを提供します。

基本のモニター API を有効にするには、`Package.swift` 内の **dependencies:** セクションに `SwiftMetrics` を追加します。適切なターゲットに追加するようにしてください。
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

デフォルトで、`SwiftMetricsDash` は、独自の Kitura サーバーを開始し、`http://<hostname>:<port>/swiftmetrics-dash` ページで要求を listen します。 HTTP 要求やイベント・ループの待ち時間などの新しいアプリケーション・メトリックを参照するには、ダッシュボードにアクセスしてください。

## スターター・キットでのアプリケーション・メトリックの使用
{: #appmetrics-starterkits}

スターター・キットから作成されるサーバー・サイドの Swift アプリケーションには `SwiftMetrics`、`SwiftMetricsDash`、`SwiftMetricsPrometheus` が含まれており、これらは Prometheus エンドポイントを使用する Kubernetes 環境でメトリックの収集にそのまま使用できます。

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

デフォルトでは、`SwiftMetricsPrometheus` は `http://<hostname>:<port>/metrics` で [Prometheus エンドポイント](https://prometheus.io/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を提供します。
