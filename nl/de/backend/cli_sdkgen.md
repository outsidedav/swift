---
copyright:
  years: 2017, 2018
lastupdated: "2018-11-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Back-End-Services mit einem generierten SDK in Ihre App integrieren
{: #sdk-cli}

Das Plug-in für {{site.data.keyword.IBM}} SDK Generator
kann in der [{{site.data.keyword.Bluemix_notm}}-CLI ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli/reference/bluemix_cli/index.html){: new_window} installiert werden.

Dieses Plug-in für {{site.data.keyword.IBM_notm}} SDK Generator
integriert Ihre Back-End-Services mit einem generierten SDK in Ihre App. Wenn eine Änderung bei einer REST-API auftritt, können Sie das SDK neu generieren und das alte ersetzen, um das SDK ohne großen Aufwand zu aktualisieren. Sie können auch die CLI in eine 'DevOps'-Pipeline integrieren und sicherstellen, dass das SDK immer mit den API-Spezifikationen übereinstimmt, wenn die App erstellt wird.

Die REST-API-Definition muss gültig sein und entweder auf einem aktiven
Serverendpunkt oder in einer lokalen Datei auf Ihrem System gehostet werden.

## Vorbereitende Schritte
{: #prereqs}

Stellen Sie sicher, dass Folgendes verfügbar ist:

* [{{site.data.keyword.Bluemix_notm}}-Konto ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://bluemix.net){: new_window}
* Gültige und mit der Spezifikation gemäß [Open API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.openapis.org/){: new_window} konforme API-Definition

## SDK-Plug-in installieren
{: #installation}

1. [Installieren
Sie die {{site.data.keyword.Bluemix}}-CLI](/docs/cli/reference/bluemix_cli/get_started.html).

2. [Installieren Sie das Plug-in ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli/reference/bluemix_cli/index.html#install_plug-in){: new_window}.

  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## SDK generieren
{: #commands}

Generieren Sie ein SDK, indem Sie den folgenden Befehl eingeben:
`ibmcloud sdk generate
[arguments...] [command options]`

### Argumente
{: #gen-args}

* `APP_NAME` - Der Name der Cloud Foundry-App in
Ihrem aktuellen Bereich.
* `OPENAPI_DOC_LOCATION` - eine URL oder ein relativer Dateipfad zur unformatierten JSON- oder yaml-Datei mit der REST-API-Definition
* `GENERATED_SDK_NAME` (optional) - Der Name des
generierten SDK.

### Optionen
{: #gen-options}

* `PLATFORM` (erforderlich)
   * `--ios` - Generiert ein Swift-SDK für iOS.
   * `--swift` - Generiert ein Swift-SDK für Server.
   * `--js` - Generiert ein JavaScript-SDK.
* `LOCATION` (erforderlich) - Gibt den Typ für die
Option `OPENAPI_DOC_LOCATION` an.
   * `-r` - Ferne URL.
   * `-f` - Datei.
   * `-a` - Unter {{site.data.keyword.Bluemix_notm}} ausgeführte App.
   * `-l` - URL des lokalen Hosts.
* `--output "YOUR_RELATIVE_PATH"` (optional) - Legt
das generierte SDK in dem Verzeichnis ab, das anstelle von
`YOUR_RELATIVE_PATH` angegeben ist. (Vorhandene SDKs werden überschrieben.)
* `--unzip` (optional) - Extrahiert das generierte SDK. (Vorhandene SDKs werden überschrieben.)

### Verwendung
{: #gen-usage}

Um ein SDK aus einer Cloud Foundry-App zu generieren, die unter
{{site.data.keyword.Bluemix_notm}} ausgeführt wird, können Sie den
Namen der App als Parameter für die CLI verwenden. Beim folgenden Befehl wird
der Name der App als Name des SDK verwendet``.

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

Führen Sie den folgenden Befehl aus, um ein SDK aus einer URL in einer Open API-Definitionsdatei oder einer lokalen JSON- bzw. yaml-Datei zu generieren.

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}

## Open API-Definitionen validieren
{: #validating}

Führen Sie den folgenden Befehl aus:
```
ibmcloud sdk validate [argument]
```
{: codeblock}

### Argumente
{: #val-args}

* `APP_NAME` - Der Name der Cloud Foundry-App in Ihrem
aktuellen Bereich.
* `OPENAPI_DOC_LOCATION` - eine URL oder ein relativer Dateipfad zur unformatierten JSON- oder yaml-Datei mit der REST-API-Definition

### Verwendung
{: #val-usage}

Um die API-Spezifikation einer Cloud Foundry-App zu
validieren, die unter
{{site.data.keyword.Bluemix_notm}} ausgeführt wird, können Sie den
Namen der App als Parameter für die CLI verwenden.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

Führen Sie den folgenden Befehl aus, um ein SDK aus der URL für ein API-Spezifikationsdokument oder eine lokale JSON- oder yaml-Datei zu prüfen:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}
