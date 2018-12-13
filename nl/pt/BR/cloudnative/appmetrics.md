---

copyright:
  years: 2018
lastupdated: "2018-09-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Usando Métricas do Aplicativo com aplicativos Swift
{: #metrics}

Métricas de aplicativo são importantes para monitorar o desempenho de seu aplicativo. Ter uma visualização em tempo real de métricas como CPU, Memória, Latência e HTTP é essencial para assegurar que seu aplicativo esteja sendo executado efetivamente ao longo do tempo. Os serviços do Kubernetes e do Cloud Foundry, como [autoscaling](/docs/services/Auto-Scaling/index.html), dependem de métricas para determinar quando incluir ou remover instâncias dinamicamente com base em carregamento e limpeza de instâncias que não são mais necessárias para manter os custos baixos.

As métricas do aplicativo são capturadas como dados de série temporal. A agregação e a visualização de métricas capturadas podem ajudar a identificar problemas comuns de desempenho, como:

* Tempos de resposta lentos de HTTP em algumas ou em todas as rotas
* Rendimento de rendimento no aplicativo
* Espiões na demanda que causam desaceleração
* Uso superior ao uso da CPU
* Uso de memória alta ou crescente (potencial fuga de memória)

## Incluindo métricas de aplicativos em seu aplicativo Swift existente
{: #add-appmetrics-existing}

Use o [Application Metrics for Swift](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/) para incluir monitoramento de desempenho no aplicativo Swift. O Application Metrics for Swift é composto de duas bibliotecas: `SwiftMetrics` e `SwiftMetricsDash`.

* A biblioteca `SwiftMetrics` é uma biblioteca de instrumentação abrangente que reúne e agrega métricas para seu aplicativo. Ela possui várias extensões, incluindo um módulo Kitura para as métricas HTTP, [Suporte a Prometheus](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support) e um [emissor](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent) independente.

* A biblioteca `SwiftMetricsDash` consome as métricas produzidas por `SwiftMetrics` e fornece um painel integrado para visualização.

Para ativar a API de monitoramento base, inclua `SwiftMetrics` na seção **dependências:** em seu `Package.swift` e assegure-se de incluí-la no destino apropriado:
```swift
.package (url: "https: //github.com/RuntimeTools/SwiftMetrics.git ", de:" 2.4.0 ")
```
{: codeblock}

Inclua o código de instrumentação a seguir para inicializar o código `SwiftMetrics`:
```swift
import SwiftMetrics
import SwiftMetricsKitura

let metrics = try SwiftMetrics()
SwiftMetricsKitura(swiftMetricsInstance: metrics)
let monitoring = metrics.monitor()
```
{: codeblock}

Inclua o código a seguir na amostra original para fornecer métricas no painel de monitoramento:
```swift
importar SwiftMetricsDash

let smd = try SwiftMetricsDash(swiftMetricsInstance : metrics)
```  
{: codeblock}

Por padrão, `SwiftMetricsDash` inicia seu próprio servidor Kitura e fornece a página em `http://<hostname>:<port>/swiftmetrics-dash `. Acesse o painel para ver suas novas métricas de aplicativo, incluindo solicitações de HTTP e latência de loop de eventos.

## Usando Métricas do Aplicativo em Kits de Inicialização

Os aplicativos Swift do lado do servidor que são criados por meio dos kits do iniciador incluem `SwiftMetrics`, `SwiftMetricsDash` e `SwiftMetricsPrometheus` e estão prontos para uso em ambientes do Kubernetes que usam terminais Prometheus para reunir métricas.

O código `SwiftMetrics` pode ser localizado em `/Sources/Application/Metrics.swift`:
```swift
import Kitura
import SwiftMetrics
import SwiftMetricsDash
import SwiftMetricsPrometheus
import LoggerAPI

var swiftMetrics: SwiftMetrics?
var swiftMetricsDash: SwiftMetricsDash?
var swiftMetricsPrometheus: SwiftMetricsPrometheus?

func initializeMetrics (roteador: Router) {
    fazer {
        let metrics = try SwiftMetrics()
        let dashboard = try SwiftMetricsDash(swiftMetricsInstance: metrics, endpoint: router)
        let prometheus = try SwiftMetricsPrometheus(swiftMetricsInstance: metrics, endpoint: router)

        swiftMetrics = metrics
        swiftMetricsDash = dashboard
        swiftMetricsPrometheus = prometheus
        Log.info("Initialized metrics.")
    } catch {
        Log.warning ("Falha ao inicializar as métricas: \ (erro)")
    }
}
```
{: codeblock}

Depois que seu aplicativo estiver em execução, será possível acessar o painel usando o terminal `/swiftmetrics-dash`.

Por padrão, `SwiftMetricsPrometheus` fornece o [terminal Prometheus](https://prometheus.io/) em `http://<hostname>:<port>/metrics `.
