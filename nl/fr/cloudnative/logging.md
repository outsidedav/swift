---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: swift logging, ios logging, debug swift, add logging swift, heliumlogger swift, loggerapi swift, logger swift, starter kit swift logger

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Journalisation dans Swift
{: #logging_swift}

Les messages de journal sont des chaînes avec des informations contextuelles sur l'état et l'activité du microservice au moment où l'entrée de journal est créée. Les journaux sont nécessaires pour diagnostiquer comment et pourquoi les services échouent, et jouent un rôle de support pour les [métriques d'application](/docs/swift/cloudnative?topic=swift-metrics#metrics) dans la surveillance de la santé des applications.

Compte tenu de la nature transitoire des processus dans les environnements de cloud, les journaux doivent être collectés et envoyés ailleurs, généralement dans un emplacement centralisé pour analyse. Le moyen le plus cohérent de consigner dans les environnements de cloud est d'envoyer les entrées de journal dans une sortie standard et des flux d'erreurs, ce qui permet à l'infrastructure de traiter le reste.

## Ajout de la journalisation à votre application Swift
{: #logging-add}

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") est une infrastructure de journalisation légère connue pour Swift, qui présente de nombreux avantages natifs comme la journalisation dans une sortie standard et différents niveaux de journalisation.

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") est le protocole de journal d'événements qui fournit une interface de journalisation commune pour différents types de journaux d'événements dans Swift. Kitura utilise `LoggerAPI` dans ses mises en oeuvre.

Pour utiliser `HeliumLogger`, ajoutez le code suivant à la section **dependencies:** de `Package.swift` pour toutes les cibles appropriées :
```swift
.package(url: "https://github.com/IBM-Swift/HeliumLogger.git", from: "1.7.1")
```
{: codeblock}

Ajoutez le code d'instrumentation suivant pour initialiser `HeliumLogger` et définissez-le en tant que journal d'événements par `LoggerAPI` :
```swift
import HeliumLogger
import LoggerAPI

let logger = HeliumLogger(.verbose)
Log.logger = logger

Log.info("This is an informational log message.")
```
{: codeblock}

Dans l'exemple fourni, le [niveau de journalisation](http://ibm-swift.github.io/HeliumLogger/){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") a été explicitement défini sur `.verbose`, qui est la valeur par défaut.

Pour plus d'informations sur la personnalisation des messages de journal, consultez la [documentation de la référence d'API HeliumLogger](http://ibm-swift.github.io/HeliumLogger/){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") officielle.

## Journalisation avec les kits de démarrage
{: #logging-starterkits}

Les applications Swift qui sont créées à l'aide du service d'application {{site.data.keyword.cloud_notm}} sont fournies avec `HeliumLogger` par défaut. L'exécution de l'application en mode natif ou dans un environnement de cloud génère la sortie suivante :
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

Ces messages se trouvent localement dans `stdout` (sortie standard), ou dans les journaux des déploiements Cloud Foundry et Kubernetes, accessibles via `[ibmcloud app logs --recent <APP_NAME>]`(/docs/cli/reference?topic=cloud-cli-ibmcloud_commands_apps#ibmcloud_app_logs) et `[kubectl logs <deployment name>`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").

Dans le fichier `/Sources/AppName/main.swift`, vous pouvez voir le code suivant :
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

Le niveau de journalisation est défini de manière explicite sur `.info` afin de journaliser les messages de niveau informatif, comme les journaux de démarrage d'application.
{: tip}

## Etapes suivantes
{: #next-logging notoc}

En savoir plus sur l'affichage des journaux dans chacune de vos cibles de déploiement :
* [Journaux de Kubernetes](https://kubernetes.io/docs/concepts/cluster-administration/logging/#basic-logging-in-kubernetes){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")
* [Journaux de Cloud Foundry](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
* [Cloud Foundry Enterprise Environment - Contrôle et journalisation](/docs/cloud-foundry?topic=cloud-foundry-auditing-logging#auditing-logging)
* [Journaux & surveillance d'{{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-logs)

Découvrez comment implémenter et utiliser un regroupeur de journaux :
* [Analyse de journal {{site.data.keyword.cloud_notm}}](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_analysis_ov#log_analysis_ov)
* [Pile ELK privée {{site.data.keyword.cloud_notm}}](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")
