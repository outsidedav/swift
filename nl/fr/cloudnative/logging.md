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

# Journalisation dans Swift
{: #logging_swift}

Les journaux sont nécessaires pour diagnostiquer comment et pourquoi des services échouent. Les journaux ne sont pas censés être utilisés pour surveiller les performances d'une application car c'est le rôle des métriques, mais ils peuvent faire office de source pour les alertes, et peuvent donc comporter davantage de détails que ce que vous pourriez obtenir de mesures globales.

L'un des avantages de travailler avec une infrastructure de cloud est que votre application n'a plus à se soucier de nombreuses choses, notamment la gestion des fichiers journaux. Compte tenu de la nature transitoire des processus dans les environnements de cloud, les journaux doivent être collectés et envoyés ailleurs, généralement dans un emplacement centralisé pour analyse. La manière la plus cohérente de journaliser des environnements de cloud est d'envoyer des entrées de journal dans des flux de sortie et d'erreur standard, puis de laisser l'infrastructure s'occuper du reste.

Comme votre application évolue avec le temps, la nature de ce que vous consignez peut changer. L'utilisation d'un format JSON présente les avantages suivants :
* Les journaux sont indexables, ce qui simplifie grandement la recherche d'un corps agrégé de journaux.
* Les journaux sont plus résistants aux changements, car l'analyse syntaxique ne repose pas sur la position des éléments dans une chaîne.

L'utilisation d'une journalisation au format JSON peut rendre la lecture des journaux légèrement plus compliqués pour vous, en tant qu'humain, lorsque vous utilisez des outils de ligne de commande pour l'extraction de journaux. Vous pouvez utiliser des variables d'environnement pour basculer entre les formats de journaux à utiliser de manière à disposer de journaux de texte en clair pour le développement local et le débogage.

## Ajout de la journalisation à votre appli Swift

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) est une infrastructure de journalisation légère connue pour Swift ; elle présente de nombreux avantages natifs comme la journalisation dans une sortie standard et différents niveaux de journalisation.

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) est le protocole de journal d'événements qui fournit une interface de journalisation commune pour différents types de journaux d'événements dans Swift. Kitura utilise the `LoggerAPI` dans ses mises en oeuvre.

Pour optimiser `HeliumLogger`, ajoutez ce qui suit à **dependencies:** dans votre `Package.swift`, afin de garantir son ajout aux cibles où il est utilisé.
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

Dans l'exemple fourni, le [niveau de journalisation](http://ibm-swift.github.io/HeliumLogger/) a été explicitement défini sur `.verbose`, qui est la valeur par défaut.

Pour plus d'informations sur la personnalisation des messages de journal, consultez la [documentation de référence d'API HeliumLogger API](http://ibm-swift.github.io/HeliumLogger/) officielle.

## Journalisation avec des kits de démarrage
{: #monitoring}

Les applis Swift qui sont créées à l'aide du service d'appli {{site.data.keyword.cloud_notm}} sont fournies avec `HeliumLogger` par défaut. L'exécution de l'application en mode natif ou dans un environnement de cloud génère la sortie suivante :
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

Ces messages se trouvent dans `stdout` à partir d'une exécution en local, ou dans les journaux des déploiements [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) et [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/), lesquels sont accessibles par les journaux `ibmcloud app --recent <APP_NAME>` et `kubectl<deployment name>`, respectivement.

Dans le fichier `/Sources/AppName/main.swift`, vous pouvez voir le code suivant :
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

Le niveau de journalisation est défini de manière explicite sur `.info` afin de journaliser les messages de niveau informatif, comme les journaux de démarrage d'application.
{: tip}

## Etapes suivantes
{: #next_steps}

En savoir plus sur l'affichage des journaux dans chacun des environnements de déploiement :
* [Journaux Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [Journaux de Cloud Foundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [Journaux & surveillance d'{{site.data.keyword.openwhisk}}](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

Utilisation d'un regroupeur de journaux
* [Analyse de journal {{site.data.keyword.cloud_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [Pile ELK privée {{site.data.keyword.cloud_notm}} ](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
