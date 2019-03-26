---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip} 

# Anwendungen mit erweiterter Sicherheit erstellen
{: #security}

Swift-Entwickler können ohne großen Aufwand die von
{{site.data.keyword.cloud}} gebotenen Sicherheitsservices nutzen, um
ihre Schlüssel und ihre ruhenden, gerade genutzten oder übertragenen Daten mit
dem höchsten branchenüblichen Sicherheitsniveau zu schützen.
{: shortdesc}

Als einfache Strategie können Sie
{{site.data.keyword.cloud_notm}} HyperSecure Platform Services direkt für
alle erweiterten Sicherheitsservices in {{site.data.keyword.cloud}}
verwenden. Weitere Informationen finden Sie unter [Einführung in {{site.data.keyword.cloud_notm}} HyperSecure Platform Services](/docs/services/hypersecure-platform/index.html){:new_window}.

## {{site.data.keyword.cloud_notm}}
{{site.data.keyword.hscrypto}} verwenden
{: #security-swift}

{{site.data.keyword.hscrypto}} ist ein experimenteller Service,
der eine Verschlüsselung Ihrer Schlüssel und Daten auf dem höchsten
branchenüblichen Sicherheitsniveau bietet. Dieser Service stattet die Cloud mit
der Sicherheit und Integrität von IBM z aus. Cloudbenutzer können über
{{site.data.keyword.cloud_notm}} nun dieselbe
Verschlüsselungstechnologie nutzen, auf die sich Banken und
Finanzdienstleister verlassen.

{{site.data.keyword.hscrypto}} bietet netzadressierbare
Hardwaresicherheitsmodule (HSMs), die eine sichere und geschützte
Verschlüsselung über Anwendungsprogrammierschnittstellen (APIs) gemäß PKCS#11
bereitstellen. Sie haben hierdurch Zugang zum höchsten erreichbaren
Sicherheitsniveau für Verschlüsselungshardware, also Stufe 4 von FIPS 140-2.
{{site.data.keyword.hscrypto}} nutzt darüber hinaus die Lösung
"IBM Advanced Crypto Service Provider" (ACSP), die den Fernzugriff auf die kryptografischen
Koprozessoren von IBM ermöglicht.

{{site.data.keyword.hscrypto}} ist auch der Keystore für den
{{site.data.keyword.keymanagementserviceshort}}-Service.

Weitere Informationen zu {{site.data.keyword.hscrypto}} finden Sie unter [Einführung in {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}](/docs/services/hs-crypto/index.html){:new_window}.

## {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}} verwenden
{: #swift-key-management}

{{site.data.keyword.keymanagementserviceshort}} hilft Ihnen bei
der Erstellung verschlüsselter Schlüssel für Apps für
{{site.data.keyword.cloud_notm}}-Services. Bei der Verwaltung des
Lebenszyklus Ihrer Schlüssel profitieren Sie von der Gewissheit, dass Ihre
Schlüssel durch cloudbasierte Hardwaresicherheitsmodule (HSMs) vor
Datendiebstahl geschützt werden. Zusammen mit
{{site.data.keyword.hscrypto}} erreichen Sie für Ihre Schlüssel das
höchste Sicherheitsniveau des Zertifikats für Ebene 4 von FIPS 140-2.

Weitere Informationen zu {{site.data.keyword.keymanagementserviceshort}} finden Sie unter [Einführung in {{site.data.keyword.keymanagementserviceshort}}](/docs/services/keymgmt/index.html){:new_window}.

## {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS verwenden
{: #hypersecure-dbaas}

{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS ist ein
experimenteller {{site.data.keyword.cloud_notm}}-Service, der sichere
Datenbanken bedarfsgesteuert erstellt. Es bietet eine erweiterbare
Plattform für die schnelle und einfache Bereitstellung und
Verwaltung Ihrer MongoDB-Datenbank.

Mit diesem Service können Sie Datenbankcluster in der
{{site.data.keyword.cloud_notm}} erstellen. Jeder Datenbankcluster, den
Sie erstellen, verfügt über eine primäre Datenbankinstanz und zwei sekundäre
Datenbankinstanzen, die als Replikate für die primäre Datenbank dienen. Die
Logik von
{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS erstellt
MongoDB-Datenbanken in Ihren Datenbankclustern und greift darauf zu.

### Daten und Vertraulichkeit schützen
{: #swift-securing-data}

{{site.data.keyword.IBM_notm}} hostet Ihre Datenbanken in einer
hoch verfügbaren und sicheren Umgebung:
 * Die Daten sind sowohl im Ruhezustand als auch während der
Ausführung verschlüsselt.
 * Systemhardware, Systemkonfiguration und Datenbankkonfiguration
gewährleisten eine hohe Verfügbarkeit.
 * Die zugrunde liegenden Technologien verhindern, dass
{{site.data.keyword.IBM_notm}} oder ein anderer Anbieter auf Ihre
Daten zugreifen kann.

### Logik von {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS
zu eigener Anwendung hinzufügen
{: #swift-hyperprotect}

Informationen zur Verwendung einer MongoDB-Datenbank in Ihrer Anwendung finden Sie unter [Einführung in die Verwendung einer Datenbank](/docs/hypersecure_dbaas/database-cluster.html).  

### Weitere Informationen zu {{site.data.keyword.cloud_notm}} Hyper
Protect DBaaS
{: #swift-learnmore}

Mehr über {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS erfahren Sie unter [Einführung in IBM Cloud Hyper Protect DBaaS](/docs/services/hyper-protect-dbaas/index.html).

## {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscontainers}} verwenden
{: #swift-hscontainers}

{{site.data.keyword.hscontainers}} bietet durch die Kombination
der Docker- und Kubernetes-Technologie leistungsfähige Tools, eine intuitive
Funktionalität für den Benutzer sowie eine integrierte Sicherheit und
Isolation, um die Bereitstellung, den Betrieb, die Skalierung und die
Überwachung von containerbasierten Apps in einem Cluster von Berechnungshosts
zu automatisieren.

{{site.data.keyword.hscontainers}} sind nur für Sponsorbenutzer verfügbar. Falls Sie eine dedizierte Unterstützung von
Sicherheitsfunktionen erwarten, registrieren Sie sich als Sponsorbenutzer bei [IBM Z Client Feedback Program](https://www-01.ibm.com/marketing/iwm/iwmdocs/web/cc/earlyprograms/zcustomer.shtml), um Ihre Anwendung im {{site.data.keyword.hscontainers}}-Cluster bereitstellen zu können.
{: tip}

Solange Sie kein Sponsorbenutzer sind, können Sie Ihre Anwendung mit {{site.data.keyword.containershort_notm}} schützen. Weitere
Informationen finden Sie unter [Einführung in {{site.data.keyword.containershort_notm}}](/docs/containers/container_index.html).
