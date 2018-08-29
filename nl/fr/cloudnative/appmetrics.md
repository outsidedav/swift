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

# Utilisation de métriques d'application avec les applis Swift
{: #metrics}

Les métriques d'application sont importantes pour la surveillance des performances de votre application. Il peut vous sembler que la surveillance des performances de l'environnement, et notamment l'UC, la mémoire, la latence et les métriques HTTP, vont nécessiter un effort monumental. Il est néanmoins essentiel de s'assurer que votre application Swift s'exécute de manière efficace dans le temps. Des services cloud natifs, tels que la mise à l'échelle automatique, peuvent se baser sur ces métriques pour mettre votre appli à l'échelle et lui permettre de fonctionner en cas de pic de charge et de réduire afin de maintenir un faible niveau de coût.

Les métriques d'application peuvent vous aider à identifier des problèmes de performances courants :

* Faibles temps de réponse HTTP sur tout ou partie des routes
* Débit médiocre dans l'application
* Pics de demandes entraînant des ralentissements
* Utilisation UC plus élevée que prévu pour le niveau de débit/charge
* Utilisation de la mémoire élevé et/ou en augmentation (fuite de mémoire possible)

## Ajout de métriques d'application à votre application Swift existante
{: #add-appmetrics-existing}

Utilisez [Application Metrics for Swift](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/) pour ajouter la surveillance des performances à votre application Swift. Application Metrics for Swift comporte deux bibliothèques : `SwiftMetrics` et `SwiftMetricsDash`.

* La bibliothèque `SwiftMetrics` est une bibliothèque d'instrumentation complète qui collecte et rassemble les métriques de votre application. Elle comporte plusieurs extensions, notamment un module Kitura pour les métriques HTTP, une [prise en charge Prometheus](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support) et un [émetteur](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent) autonome.

* La bibliothèque `SwiftMetricsDash` consomme les métriques qui sont produites par `SwiftMetrics` et fournit un tableau de bord intégré pour la visualisation.


Pour activer l'API de surveillance de base, ajoutez `SwiftMetrics` à la section **dependencies:** dans votre `Package.swift`, et assurez-vous de l'ajouter à la cible souhaitée :
```swift
.package(url: "https://github.com/RuntimeTools/SwiftMetrics.git", from: "2.4.0")
```
{: codeblock}

Ajoutez le code d'instrumentation suivant pour initialiser le code `SwiftMetrics` :
```swift
import SwiftMetrics
import SwiftMetricsKitura

let metrics = try SwiftMetrics()
SwiftMetricsKitura(swiftMetricsInstance: metrics)
let monitoring = metrics.monitor()
```
{: codeblock}

Ajoutez le code suivant à l'exemple d'origine afin de fournir les métriques dans le tableau de bord de surveillance de surveillance :
```swift
import SwiftMetricsDash

let smd = try SwiftMetricsDash(swiftMetricsInstance : metrics)
```  
{: codeblock}

Par défaut, `SwiftMetricsDash` démarre son propre serveur Kitura, puis présente la page sous `http://<hostname>:<port>/swiftmetrics-dash`. Accédez au tableau de bord pour voir vos nouvelles métriques d'application, y compris les demandes HTTP et la latence de boucle d'événements.

## Utilisation de métriques d'application dans les kits de démarrage

Les applications Swift côté serveur qui sont créées à partir de kits de démarrage incluent `SwiftMetrics`, `SwiftMetricsDash` et `SwiftMetricsPrometheus` ; elle sont donc prêtes pour une utilisation dans les environnements Kubernetes qui utilisent des noeuds finaux Prometheus pour la collecte de métriques.

Le code `SwiftMetrics` se trouve dans `/Sources/Application/Metrics.swift` :
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

Une fois votre application lancée, vous pouvez accéder au tableau de bord à partir du noeud final `/swiftmetrics-dash`.

Par défaut, `SwiftMetricsPrometheus` fournit le [noeud final Prometheus](https://prometheus.io/) sous `http://<hostname>:<port>/metrics`.
