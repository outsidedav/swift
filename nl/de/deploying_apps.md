---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: deploy swift app, deploy swift, serverless swift, deploy swift cloud foundry, swift kubernetes, swift virtual server

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Swift-Apps bereitstellen und integrieren
{: #deploy_apps-swift}

Für die Bereitstellung Ihrer Swift-Apps können Sie eine Toolchain oder
die Befehlszeilenschnittstelle verwenden. Eine Toolchain besteht aus einer
Reihe von Toolintegrationen. Die Befehlszeilenschnittstelle ist eine einfache
Möglichkeit, Ihre Apps und Serviceinstanzen bereitzustellen.

Weitere Informationen finden Sie im Abschnitt
[Apps bereitstellen](/docs/apps?topic=creating-apps-create-deploy-app-cli#create-deploy-app-cli).

## Serverunabhängige Swift-Apps entwickeln
{: #serverless-swift}

Zur Entwicklung in einer serverunabhängigen Entwicklung können Sie
{{site.data.keyword.openwhisk}}, ein IBM Produktangebot der Kategorie
"Functions as a Service" (FaaS) nutzen. {{site.data.keyword.openwhisk_short}} ermöglicht die Ausführung von
Anwendungslogik als Reaktion auf Ereignisse oder direkte Aufrufe von
Web-Apps oder mobilen Apps über HTTP ohne Erstellung oder Verwaltung von
Servern.

{{site.data.keyword.openwhisk_short}} führt die Systemverwaltung
wie die automatische Skalierung, das Verfügbarkeitsmanagement und die Wartung
aus; Entwickler können sich somit weiterhin auf das Schreiben von
Anwendungslogik konzentrieren.

Weitere Informationen enthält der Abschnitt
[Serverunabhängige Apps
entwickeln](/docs/apps/deploying?topic=creating-apps-serverless#serverless).

## Back-End-Services mit einem generierten SDK integrieren
{: #backend_gensdk-swift}

Das Plug-in für {{site.data.keyword.IBM_notm}} SDK Generator
integriert Back-End-Services ohne großen Aufwand mit einem generierten SDK in
Ihre
App. Wenn eine Änderung bei einer REST-API auftritt, können Sie das SDK neu generieren und das alte ersetzen, um das SDK zu aktualisieren. Sie können auch die CLI in eine 'DevOps'-Pipeline integrieren und sicherstellen, dass das SDK immer mit den API-Spezifikationen übereinstimmt, wenn die App erstellt wird.

Weitere Informationen finden Sie unter
[Back-End-Services mit einem
generierten SDK integrieren](/docs/swift/backend?topic=swift-sdkgen-cli#sdkgen-cli).

## Anwendung in einem Kubernetes-Cluster bereitstellen
{: #deploy_kube-swift}

Sie können erlernen, wie Sie mit dem Kubernetes-Service von {{site.data.keyword.cloud_notm}} eine containerisierte App bereitstellen können, die Watson Tone Analyzer nutzt. In dem
beschriebenen Szenario verwendet eine fiktive PR-Firma
den {{site.data.keyword.cloud_notm}}-Service, um ihre Pressemitteilungen
zu analysieren und eine Rückmeldung auf den Tonfall ihrer Nachrichten zu
erhalten. Weitere Informationen bietet das Lernprogramm [Apps in Kubernetes-Clustern bereitstellen](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial).

## Anwendung in Cloud Foundry bereitstellen
{: #swift-deploy-cf}

Mit dieser Option wird die cloudnative App bereitgestellt, ohne dass Sie die zugrunde liegende Infrastruktur verwalten müssen.

Wenn Sie die App in [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about#about) bereitstellen möchten, müssen Sie [Ihr {{site.data.keyword.cloud_notm}}-Konto vorbereiten](/docs/cloud-foundry?topic=cloud-foundry-prepare#prepare).

Wenn Ihr Konto über Zugriff auf {{site.data.keyword.cfee_full_notm}} verfügt, können Sie als Bereitstellertyp entweder **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf#about-cf)** oder **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee#cfee)** auswählen, das zum Erstellen und Verwalten isolierter Umgebungen für das exklusive Hosting von Cloud Foundry-Anwendungen für Ihr Unternehmen verwendet werden kann.

## Anwendung in einem virtuellen Server bereitstellen
{: #virtual_deploy-swift}

Ein virtueller Service bietet für alle Workloadtypen ein höheres Maß an
Transparenz, Vorhersagbarkeit und Automatisierung. Mit Terraform kann Ihre
Infrastruktur sicher und effizient aufgebaut, geändert und versioniert werden.

Weitere Informationen enthält der Abschnitt
[Anwendung in einem virtuellen Server
bereitstellen](/docs/apps?topic=creating-apps-vsi-deploy#vsi-deploy).
