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

# Back-End-Services mit einem generierten SDK zu Ihrer App hinzufügen
{: #sdk-cli}

Das Plug-in für {{site.data.keyword.IBM}} SDK Generator
kann in der
[{{site.data.keyword.cloud_notm}}-CLI](/docs/cli/reference/bluemix_cli/get_started.html)
installiert werden.

Mit dem Plug-in für {{site.data.keyword.IBM_notm}} SDK Generator
können Sie Ihre Back-End-Services unter Verwendung eines generierten SDK ohne
großen Aufwand in Ihre App integrieren. Sobald eine Änderung an einer REST-API
stattfindet, können Sie das SDK erneut generieren und das alte SDK ersetzen.
Auf diese Weise erreichen Sie ein nahtloses SDK-Upgrade. Sie können außerdem
die CLI in eine DevOps-Pipeline integrieren und sicherstellen, dass das SDK bei
jedem App-Build stets mit der API-Spezifikation konsistent ist.

Die REST-API-Definition muss gültig sein und entweder auf einem aktiven
Serverendpunkt oder in einer lokalen Datei auf Ihrem System gehostet werden.

## Vorbereitende Schritte
{: #prereqs}

Stellen Sie sicher, dass die folgenden Voraussetzungen verfügbar sind:

* [{{site.data.keyword.cloud_notm}}-Konto
![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen
Link")](http://bluemix.net){: new_window}
* Gültige und mit der Spezifikation gemäß
[Open API
![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen
Link")](https://www.openapis.org/){: new_window}
konforme API-Definition

## SDK-Plug-in installieren
{: #installation}

1. 
[Installieren
Sie die {{site.data.keyword.cloud_notm}}-CLI](/docs/cli/reference/bluemix_cli/get_started.html).

2. [Installieren Sie das SDK-Plug-in](/docs/cli/sdk/index.html).
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## SDK generieren
{: #commands}

Generieren Sie ein SDK, indem Sie den folgenden Befehl eingeben:

```
ibmcloud sdk generate [argumente...] [befehlsoptionen]
```
{: codeblock}

### Argumente
{: #gen-args}

* **APP_NAME** - Der Name der Cloud Foundry-App in Ihrem aktuellen Bereich.
* **OPENAPI_DOC_LOCATION** - Eine URL oder ein
relativer Dateipfad zur unaufbereiteten REST-API-Definition (JSON oder Yaml).
* **GENERATED_SDK_NAME** (optional) - Der Name des
generierten SDK.

### Optionen
{: #gen-options}

**Platform** (erforderlich):
  * `--ios` - Generiert ein Swift-SDK für iOS.
  * `--swift` - Generiert ein Swift-SDK für Server.
  * `--js` - Generiert ein JavaScript-SDK.

**Location** (erforderlich):

Gibt den Typ für das Argument `OPENAPI_DOC_LOCATION` an.

  * `-r` - Ferne URL.
  * `-f` - Dateiname.
  * `-a` - Unter {{site.data.keyword.coud_notm}} ausgeführte App. 
  * `-l` - URL des lokalen Hosts.

**Optional**:
  * `--output "IHR_RELATIVER_PFAD"` - Legt
das generierte SDK in dem Verzeichnis ab, das anstelle von
`IHR_RELATIVER_PFAD` angegeben ist. (Gegebenenfalls vorhandene
SDKs werden überschrieben.)
  * `--unzip` - Extrahiert das generierte
SDK. (Gegebenenfalls vorhandene SDK-Artefakte werden überschrieben.)

### Verwendung
{: #gen-usage}

Um ein SDK aus einer Cloud Foundry-App zu generieren, die unter
{{site.data.keyword.cloud_notm}} ausgeführt wird, können Sie den Namen
der App als Parameter für die CLI verwenden. Beim folgenden Befehl wird der
Name der App als Name des SDK verwendet``.

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

Mit dem folgenden Befehl wird ein SDK aus einer URL zu einer Open
API-Definitionsdatei oder einer lokalen JSON- bzw. Yaml-Datei generiert:

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}


## Open API-Definitionen validieren
{: #validating}

Führen Sie den folgenden Befehl aus: `ibmcloud sdk validate
[argument]`

### Argumente
{: #val-args}

* `APP_NAME` - Der Name der Cloud Foundry-App in Ihrem
aktuellen Bereich.
* `OPENAPI_DOC_LOCATION` - Eine URL oder ein relativer
Dateipfad zur unaufbereiteten REST-API-Definition (JSON oder Yaml).

### Verwendung
{: #val-usage}

Um die API-Spezifikation einer Cloud Foundry-App zu validieren, die unter
{{site.data.keyword.cloud_notm}} ausgeführt wird, können Sie den Namen
der App als Parameter für die CLI verwenden. 
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

Mit dem folgenden Befehl wird ein SDK aus der URL zu einem
API-Spezifikationsdokument oder aus einer lokalen JSON- bzw: Yaml-Datei
validiert:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

