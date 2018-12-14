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

# Utilización de métricas de aplicación con apps Swift
{: #metrics}

Las métricas de aplicación son importantes para supervisar el rendimiento de la aplicación. Tener una vista en directo de métricas como CPU, memoria, latencia y métricas HTTP es esencial para asegurarse de que la aplicación se ejecuta de forma efectiva a lo largo del tiempo. Los servicios de Kubernetes y Cloud Foundry como [Autoscaling](/docs/services/Auto-Scaling/index.html) se basan en las métricas para determinar cuándo se deben añadir o eliminar instancias de forma dinámica en función de la carga y limpiar las instancias que ya no son necesarias para mantener los costes bajos.

Las métricas de aplicación se capturan como datos de serie temporal. La agregación y visualización de métricas capturadas puede ayudar a identificar problemas de rendimiento comunes como, por ejemplo:

* Tiempos de respuesta HTTP lentos en algunas o en todas las rutas
* Rendimiento bajo en la aplicación
* Aumentos considerables en la demanda que provocan la desaceleración
* Uso de CPU mayor del esperado
* Uso de memoria alto o creciente (fuga de memoria potencial)

## Adición de métricas de aplicación a la aplicación Swift existente
{: #add-appmetrics-existing}

Utilice [Métricas de aplicación para Swift](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/) para añadir supervisión de rendimiento a la aplicación Swift. Las métricas de aplicación para Swift se componen de dos bibliotecas: `SwiftMetrics` y `SwiftMetricsDash`.

* La biblioteca `SwiftMetrics` es una biblioteca de instrumentación completa que recopila y agrega métricas para la aplicación. Tiene varias extensiones, entre ellas un módulo Kitura para métricas HTTP, [soporte de Prometheus](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support) y un [emisor](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent) autónomo.

* La biblioteca `SwiftMetricsDash` consume las métricas producidas por `SwiftMetrics` y proporciona un panel de control incorporado para la visualización.

Para habilitar la API de supervisión base, añada `SwiftMetrics` a la sección **dependencies:** del `Package.swift`, y asegúrese de añadirlo al destino adecuado: 
```swift
.package(url: "https://github.com/RuntimeTools/SwiftMetrics.git", from: "2.4.0")
```
{: codeblock}

Añada el código de instrumentación siguiente para inicializar el código `SwiftMetrics`:
```swift
import SwiftMetrics
import SwiftMetricsKitura

let metrics = try SwiftMetrics()
SwiftMetricsKitura(swiftMetricsInstance: metrics)
let monitoring = metrics.monitor()
```
{: codeblock}

Añada el código siguiente al ejemplo original para proporcionar métricas en el panel de control de supervisión:
```swift
import SwiftMetricsDash

let smd = try SwiftMetricsDash(swiftMetricsInstance : metrics)
```  
{: codeblock}

De forma predeterminada, `SwiftMetricsDash` inicia su propio servidor Kitura, y sirve la página en `http://<hostname>:<port>/swiftmetrics-dash`. Acceda al panel de control para ver las nuevas métricas de aplicación, incluidas las solicitudes HTTP y la latencia de bucle de suceso.

## Utilización de métricas de aplicación en kits de iniciación

Las aplicaciones Swift del lado del servidor que se crean a partir de kits de iniciación incluyen `SwiftMetrics`, `SwiftMetricsDash` y `SwiftMetricsPrometheus`, de modo que están listas para utilizarse en entornos de Kubernetes que utilizan puntos finales de Prometheus para la recopilación de métricas.

El código `SwiftMetrics` se puede encontrar en `/Sources/Application/Metrics.swift`:
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

Una vez que se esté ejecutando la aplicación, puede acceder al panel de control utilizando el punto final `/swiftmetrics-dash`.

De forma predeterminada, `SwiftMetricsPrometheus` proporciona el [punto final Prometheus](https://prometheus.io/) en `http://<hostname>:<port>/metrics`.
