---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-28"

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
[{{site.data.keyword.cloud_notm}}-CLI](/docs/cli/index.html)
installiert werden.

Mit dem Plug-in für {{site.data.keyword.IBM_notm}} SDK Generator
können Sie Ihre Back-End-Services unter Verwendung eines generierten SDK in Ihre App integrieren. Wenn eine Änderung bei einer REST-API auftritt, können Sie das SDK neu generieren und das alte ersetzen, um das SDK ohne großen Aufwand zu aktualisieren. Sie können außerdem
die CLI zu einer DevOps-Pipeline hinzufügen und sicherstellen, dass das SDK bei
jedem App-Build stets mit der API-Spezifikation konsistent ist.

Die REST-API-Definition muss gültig sein und entweder auf einem aktiven
Serverendpunkt oder in einer lokalen Datei auf Ihrem System gehostet werden.

## Vorbereitende Schritte
{: #prereqs-sdk-cli}

Stellen Sie sicher, dass die folgenden Voraussetzungen verfügbar sind:

* [{{site.data.keyword.cloud_notm}}-Konto ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://cloud.ibm.com){: new_window}
* Gültige und mit der Spezifikation gemäß
[Open API
![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen
Link")](https://www.openapis.org/){: new_window}
konforme API-Definition

## SDK-Plug-in installieren
{: #install-sdk-cli}

1. [Installieren
Sie die {{site.data.keyword.cloud_notm}}-CLI](/docs/cli/index.html).

2. [Installieren Sie das SDK-Plug-in](/docs/cli/sdk/index.html).
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## SDK generieren
{: #commands-sdk-cli}

Generieren Sie ein SDK, indem Sie den folgenden Befehl eingeben:
```
ibmcloud sdk generate [argumente...] [befehlsoptionen]
```
{: codeblock}

### Argumente
{: #gen-args-sdk-cli}

* **APP_NAME** - Der Name der Cloud Foundry-App in Ihrem aktuellen Bereich.
* **OPENAPI_DOC_LOCATION** - Eine URL oder ein relativer Dateipfad zur unformatierten REST-API-Definition (JSON oder yaml).
* **GENERATED_SDK_NAME** (optional) - Der Name des
generierten SDK.

### Optionen
{: #gen-options-sdk-cli}

**Platform** (erforderlich):
  * `--ios` - Generiert ein Swift-SDK für iOS.
  * `--swift` - Generiert ein Swift-SDK für Server.
  * `--js` - Generiert ein JavaScript-SDK.

**Location** (erforderlich):

Gibt den Typ für das Argument `OPENAPI_DOC_LOCATION` an.

  * `-r` - Ferne URL.
  * `-f` - Dateiname.
  * `-a` - Unter {{site.data.keyword.cloud_notm}} ausgeführte App.
  * `-l` - URL des lokalen Hosts.

**Optional**:
  * `--output "YOUR_RELATIVE_PATH"` - Legt
das generierte SDK in dem Verzeichnis ab, das anstelle von
`YOUR_RELATIVE_PATH` angegeben ist. (Gegebenenfalls vorhandene
SDKs werden überschrieben.)
  * `--unzip` - Extrahiert das generierte SDK (überschreibt vorhandene Daten falls SDK-Artefakte vorhanden sind).

### Verwendung
{: #gen-usage-sdk-cli}

Um ein SDK aus einer Cloud Foundry-App zu generieren, die unter
{{site.data.keyword.cloud_notm}} ausgeführt wird, können Sie den Namen
der App als Parameter für die CLI verwenden. Beim folgenden Befehl wird der
Name der App als Name des SDK verwendet``.

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
{: #validating-sdk-cli}

Führen Sie den folgenden Befehl aus: `ibmcloud sdk validate
[argument]`

### Argumente
{: #val-args-sdk-cli}

* `APP_NAME` - Der Name der Cloud Foundry-App in Ihrem aktuellen Bereich.
* `OPENAPI_DOC_LOCATION` - Eine URL oder ein relativer Dateipfad zur unformatierten REST-API-Definition (JSON oder yaml).

### Verwendung
{: #val-usage-sdk-cli}

Um die API-Spezifikation einer Cloud Foundry-App zu validieren, die unter
{{site.data.keyword.cloud_notm}} ausgeführt wird, können Sie den Namen
der App als Parameter für die CLI verwenden.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

Führen Sie den folgenden Befehl aus, um ein SDK aus der URL für ein API-Spezifikationsdokument oder eine lokale JSON- oder yaml-Datei zu prüfen:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

