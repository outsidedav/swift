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

# Swift 앱으로 애플리케이션 메트릭 사용
{: #metrics}

애플리케이션 메트릭은 애플리케이션의 성능을 모니터하는 데 중요합니다. CPU, 메모리, 대기 시간 및 HTTP 메트릭과 같은 메트릭의 실시간 보기가 있는 경우 시간 경과에 따라 효율적으로 실행 중인지 확인하는 것은 필수입니다. [Auto-Scaling](/docs/services/Auto-Scaling?topic=Auto-Scaling-get-started)과 같은 Kubernetes 및 Cloud Foundry 서비스는 메트릭에 의존하여 로드에 따라 인스턴스를 동적으로 추가하거나 제거하고 비용 절감을 위해 더 이상 필요하지 않은 인스턴스를 정리하는 시기를 결정할 수 있습니다.

애플리케이션 메트릭은 시계열 데이터로 캡처됩니다. 캡처된 메트릭의 집계 및 시각화는 다음과 같은 일반적인 성능 문제점을 식별하는 데 도움이 될 수 있습니다.

* 일부 또는 모든 라우트에서의 느린 HTTP 응답 시간
* 애플리케이션의 낮은 처리량
* 성능 저하의 원인이 되는 수요의 급증
* 예상보다 높은 CPU 사용량
* 높거나 증가하는 메모리 사용량(잠재적 메모리 누수)

## 기존 Swift 애플리케이션에 애플리케이션 메트릭 추가
{: #add-appmetrics-existing}

[Swift용 애플리케이션 메트릭](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 사용하여 성능 모니터링을 Swift 애플리케이션에 추가하십시오. Swift용 애플리케이션 메트릭은 두 가지 라이브러리 즉, `SwiftMetrics` 및 `SwiftMetricsDash`로 구성되어 있습니다.

* `SwiftMetrics` 라이브러리는 애플리케이션을 위한 메트릭을 수집하고 집계하는 종합 인스트루먼테이션 라이브러리입니다. 여기에는 HTTP 메트릭을 위한 Kitura 모듈, [Prometheus 지원](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘") 및 독립형 [이미터](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")와 같은 확장기능이 포함됩니다.

* `SwiftMetricsDash` 라이브러리는 `SwiftMetrics`로 생성되는 메트릭을 이용하고 시각화를 위한 내장 대시보드를 제공합니다.

기본 모니터링 API를 사용으로 설정하려면 `SwiftMetrics`를 `Package.swift`의 **dependencies:** 섹션에 추가하고 이를 적합한 대상에 추가해야 합니다.
```swift
.package(url: "https://github.com/RuntimeTools/SwiftMetrics.git", from: "2.4.0")
```
{: codeblock}

`SwiftMetrics` 코드를 초기화하도록 다음 인스트루먼테이션 코드를 추가하십시오.
```swift
import SwiftMetrics
import SwiftMetricsKitura

let metrics = try SwiftMetrics()
SwiftMetricsKitura(swiftMetricsInstance: metrics)
let monitoring = metrics.monitor()
```
{: codeblock}

모니터링 대시보드에서 메트릭을 제공하도록 다음 코드를 원래의 샘플에 추가하십시오.
```swift
import SwiftMetricsDash

let smd = try SwiftMetricsDash(swiftMetricsInstance : metrics)
```  
{: codeblock}

기본적으로 `SwiftMetricsDash`는 고유한 Kitura 서버를 시작하고 `http://<hostname>:<port>/swiftmetrics-dash`에서 페이지를 제공합니다. 대시보드에 액세스하여 HTTP 요청 및 이벤트 루프 대기 시간을 포함한 새 애플리케이션 메트릭을 확인하십시오.

## 스타터 킷에서 애플리케이션 메트릭 사용
{: #appmetrics-starterkits}

스타터 킷에서 작성된 서버 측 Swift 애플리케이션에는 `SwiftMetrics`, `SwiftMetricsDash` 및 `SwiftMetricsPrometheus`가 포함되며, 이에 따라 메트릭을 수집하기 위해 Prometheus 엔드포인트를 사용하는 Kubernetes 환경에서 서버 측 Swift 애플리케이션을 사용할 준비가 됩니다.

`/Sources/Application/Metrics.swift`에서 `SwiftMetrics` 코드를 찾을 수 있습니다.
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

애플리케이션이 실행되면 `/swiftmetrics-dash` 엔드포인트를 사용하여 대시보드에 액세스할 수 있습니다.

기본적으로 `SwiftMetricsPrometheus`는 `http://<hostname>:<port>/metrics` 아래에 [Prometheus 엔드포인트](https://prometheus.io/){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 제공합니다.
