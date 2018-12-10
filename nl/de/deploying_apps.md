---

copyright:
  years: 2018
lastupdated: "2018-11-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Swift-Apps bereitstellen und integrieren
{: #deploy_apps}

Für die Bereitstellung Ihrer Swift-Apps können Sie eine Toolchain oder
die Befehlszeilenschnittstelle verwenden. Eine Toolchain besteht aus einer
Reihe von Toolintegrationen. Die Befehlszeilenschnittstelle ist eine einfache
Möglichkeit, Ihre Apps und Serviceinstanzen bereitzustellen.

Weitere Informationen finden Sie im Abschnitt
[Apps bereitstellen](../apps/dep-app-tool.html).

## Serverunabhängige Swift-Apps entwickeln
{: #serverless}

Zur Entwicklung in einer serverunabhängigen Entwicklung können Sie
{{site.data.keyword.openwhisk}}, ein IBM Produktangebot der Kategorie
"Functions as a Service" (FaaS) nutzen. 
{{site.data.keyword.openwhisk_short}} ermöglicht die Ausführung von
Anwendungslogik als Reaktion auf Ereignisse oder direkte Aufrufe von
Web-Apps oder mobilen Apps über HTTP ohne Erstellung oder Verwaltung von
Servern.

{{site.data.keyword.openwhisk_short}} führt die Systemverwaltung
wie die automatische Skalierung, das Verfügbarkeitsmanagement und die Wartung
aus; Entwickler können sich somit weiterhin auf das Schreiben von
Anwendungslogik konzentrieren.

Weitere Informationen enthält der Abschnitt
[Serverunabhängige Apps
entwickeln](../apps/deploying/functions.html).

## Back-End-Services mit einem generierten SDK integrieren
{: #backend_gensdk}

Das Plug-in für {{site.data.keyword.IBM_notm}} SDK Generator
integriert Back-End-Services ohne großen Aufwand mit einem generierten SDK in
Ihre
App. Wenn eine Änderung bei einer REST-API auftritt, können Sie das SDK neu generieren und das alte ersetzen, um das SDK zu aktualisieren. Sie können auch die CLI in eine 'DevOps'-Pipeline integrieren und sicherstellen, dass das SDK immer mit den API-Spezifikationen übereinstimmt, wenn die App erstellt wird.

Weitere Informationen finden Sie unter
[Back-End-Services mit einem
generierten SDK integrieren](/docs/swift/backend/cli_sdkgen.html).

## Anwendung in einem Kubernetes-Cluster bereitstellen
{: #deploy_kube}

Sie können sich darüber informieren, wie eine containerisierte
Node.js-App, die Watson Tone Analyzer nutzt, mit dem Kubernetes-Service von
{{site.data.keyword.cloud_notm}} bereitgestellt wird. In dem
beschriebenen Szenario verwendet eine fiktive PR-Firma
den {{site.data.keyword.cloud_notm}}-Service, um ihre Pressemitteilungen
zu analysieren und eine Rückmeldung auf den Tonfall ihrer Nachrichten zu
erhalten. Weitere Informationen bietet das Lernprogramm
[Apps in Clustern bereitstellen](../containers/cs_tutorials_apps.html).

## Anwendung in einem virtuellen Server bereitstellen
{: #virtual_deploy}

Ein virtueller Service bietet für alle Workloadtypen ein höheres Maß an
Transparenz, Vorhersagbarkeit und Automatisierung. Mit Terraform kann Ihre
Infrastruktur sicher und effizient aufgebaut, geändert und versioniert werden.

Weitere Informationen enthält der Abschnitt
[Anwendung in einem virtuellen Server
bereitstellen](../apps/vsi-deploy.html).
