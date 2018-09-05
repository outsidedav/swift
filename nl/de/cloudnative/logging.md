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

# Protokollierung in Swift
{: #logging_swift}

Protokolle werden benötigt, um zu diagnostizieren, wie und warum
Services fehlschlagen. Sie sind zwar nicht zur Überwachung der Anwendungsleistung
gedacht, wofür es Metriken gibt, aber sie können als Quelle für Alerts genutzt
werden, die dann einen größeren Umfang von Details enthalten können, als Sie
den aggregierten Metriken entnehmen können.

Einer der Vorteile bei der Arbeit mit der Cloudinfrastruktur besteht
darin, dass zahlreiche Aufgaben nicht mehr von Ihrer Anwendung selbst
übernommen werden müssen, beispielsweise die Verwaltung von Dateien. Prozesse
in Cloudumgebungen sind naturgemäß transient, weshalb erfasste Protokolle an
eine andere Position gesendet werden müssen, bei der es sich in der Regel um
eine zentrale Analysestelle handelt. Das konsistenteste Verfahren für die
Protokollierung in Cloudumgebungen besteht darin, Protokolleinträge an
Standardausgabe und Fehlerdatenströme zu senden und die weitere Verarbeitung
der Infrastruktur zu überlassen.

Wenn sich eine Anwendung im Zeitverlauf weiterentwickelt, kann sich die
Spezifik der protokollierten Elemente ändern. Bei Verwendung eines
JSON-Protokollformats erzielen Sie die folgenden Vorteile:
* Die Protokolle können indexiert werden, was Suchvorgänge für einen
aggregierten Protokollbestand vereinfacht.
* Die Protokolle sind bei Änderungen ausfallsicher, da das Parsing nicht
auf die Position von Elementen in einer Zeichenfolge angewiesen ist.

Die Verwendung der Protokollierung mit JSON-Formatierung kann es für
Sie als Benutzer schwieriger machen, die Protokolle zu lesen, wenn Sie diese
mithilfe von Befehlszeilentools abrufen. Durch den Einsatz von Umgebungsvariablen können
Sie jedoch das zu verwendende Protokollformat wechseln und auf diese Weise für
die lokale Entwicklung und das Debugging Protokolle in einfachem Text anfordern.

## Protokollierung zu einer Swift-App hinzufügen

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger)
ist ein gängiges einfaches Protokollierungsframework für Swift und bietet von
Natur aus viele Vorteile wie beispielsweise die Protokollierung in der
Standardausgabe und mit verschiedenen Protokollebenen.

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI)
ist das Protokollfunktionsprotokoll, das eine gemeinsame
Protokollierungsschnittstelle für verschiedene Arten von Protokollfunktionen in
Swift bereitstellt. Kitura verwendet `LoggerAPI`
während seiner Implementierung.

Um `HeliumLogger` zu nutzen, fügen Sie Folgendes zum
Abschnitt
**dependencies:** in Ihrer Datei
`Package.swift` hinzu; achten Sie dabei darauf, die Zeile zu
allen Zielen hinzuzufügen, bei denen diese Protokollfunktion verwendet wird.

```swift
.package(url: "https://github.com/IBM-Swift/HeliumLogger.git", from: "1.7.1")
```
{: codeblock}

Fügen Sie den folgenden Instrumentierungscode hinzu, um
`HeliumLogger` zu initialisieren und als die von
`LoggerAPI` verwendete Protokollfunktion festzulegen:
```swift
import HeliumLogger
import LoggerAPI

let logger = HeliumLogger(.verbose)
Log.logger = logger

Log.info("This is an informational log message.")
```
{: codeblock}

Im obigen Beispiel ist als
[Protokollebene](http://ibm-swift.github.io/HeliumLogger/)
explizit `.verbose` festgelegt, was die Standardeinstellung
ist.

Weitere Informationen zum Anpassen der Protokollnachrichten finden Sie in
der offiziellen Referenzdokumentation zur API
[HeliumLogger](http://ibm-swift.github.io/HeliumLogger/).

## Protokollierung mit Starter-Kits
{: #monitoring}

Swift-Apps, die mit dem App-Service von
{{site.data.keyword.cloud_notm}} erstellt werden, enthalten
`HeliumLogger` standardmäßig. Bei der nativen Ausführung der
App oder ihrer Ausführung in einer Cloudumgebung wird die folgende Ausgabe
erzeugt:
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

Diese Nachrichten befinden sich bei der lokalen
Ausführung in `stdout` oder in den Protokollen
für
[CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)-
und
[Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)-Bereitstellungen,
auf die mit dem Befehl `ibmcloud app logs --recent <APP_NAME>`
bzw. `kubectl logs <deployment name>` zugegriffen werden kann.

In der Datei `/Sources/AppName/main.swift` ist der
folgende Code enthalten:
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

Als Protokollebene ist explizit `.info` angegeben, damit
Informationsnachrichten wie beispielsweise über den Anwendungsstart
protokolliert werden.
{: tip}

## Nächste Schritte
{: #next_steps}

Die folgenden Quellen enthalten weitere Informationen zum Anzeigen der
Protokolle in den einzelnen Bereitstellungsumgebungen:
* [Kubernetes-Protokolle](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [Cloud
Foundry-Protokolle](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [Protokolle
und Überwachung von {{site.data.keyword.openwhisk}}](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

Angaben über die Verwendung eines Protokollaggregators finden Sie in den
folgenden Abschnitten:
* [Protokollanalyse
in {{site.data.keyword.cloud_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [Privater
ELK-Stack von {{site.data.keyword.cloud_notm}}](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
