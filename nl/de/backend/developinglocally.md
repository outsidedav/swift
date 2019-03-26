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

# Anwendungen lokal entwickeln
{: #develop-locally}

Bei einer lokalen Entwicklung können Sie Swift-Apps ohne großen
Aufwand erstellen,
ausführen und testen. Diese Aktionen
führen Sie über {{site.data.keyword.dev_cli_short}}
mithilfe von Standardbefehlen aus. 

Mit {{site.data.keyword.dev_cli_short}} können Sie über ein
Dutzend Befehle für die Verwaltung Ihrer serverseitigen Anwendungen nutzen. Weitere
Informationen zu den verschiedenen Befehlen enthält der Abschnitt
[Befehle `ibmcloud
dev der Befehlszeilenschnittstelle von IBM Cloud Developer Tools](/docs/cli/idt/commands.html).

## Vorbereitende Schritte
{: #prereqs-local}

Für die lokale Entwicklung müssen Sie
{{site.data.keyword.dev_cli_notm}} installieren. Führen Sie das
Installationsscript mit dem folgenden Befehl aus:
```
curl -sL https://ibm.biz/idt-installer | bash
```
{: codeblock}

Weitere Informationen zum Konfigurieren und zur Verwendung der {{site.data.keyword.dev_cli_notm}} finden Sie unter [IBM Cloud Developer Tools-Befehlszeilenschnittstelle einrichten](/docs/cli/idt/setting_up_idt.html).

## Serviceberechtigungsnachweise abrufen
{: #credentials-local}

Nachdem Sie Ihre Anwendung aus Git geklont haben, müssen Sie die
Berechtigungsnachweise für Services abrufen, die an Ihre Anwendung gebunden
sind, da diese nicht im Git-Repository für Ihre Anwendung gespeichert sind. Das
Abrufen der Berechtigungsnachweise ermöglicht die Verwendung von gebundenen
Services. Die Berechtigungsnachweise können Sie ohne großen Aufwand
herunterladen, indem Sie den folgenden Befehl im Stammverzeichnis des
Anwendungsverzeichnisses ausführen:
```
ibmcloud dev get-credentials
```
{: codeblock}

## Anwendung erstellen, ausführen und bereitstellen
{: #build-deploy-local}

1. **Erstellung** - Sie können jetzt Ihre Anwendung erstellen; dies ist eine Voraussetzung
für die Ausführung der Anwendung.
  Verwenden Sie den folgenden Befehl im
Stammverzeichnis des Anwendungsverzeichnisses, um Ihre App zu erstellen:
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **Ausführung** - Nach einer erfolgreichen Erstellung können Sie Ihre Anwendung mit dem
folgenden Befehl in einem lokalen Container ausführen:
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  Wenn der Befehl erfolgreich ausgeführt wird, werden ein lokaler Host
und ein lokaler Port zum Anzeigen der Landing-Page Ihrer Anwendung angezeigt.

3. **Bereitstellung** - Stellen Sie Ihre Anwendung in {{site.data.keyword.Bluemix_notm}}
mit dem Befehl `deploy` bereit:
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}
