---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-28"

keywords: generated sdk swift, devops pipeline swift, sdk plug-in swift, open api swift, sdkgen swift, ibmcloud sdk swift, swift backend service

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Back-End-Services mit einem generierten SDK in Ihre App integrieren
{: #sdkgen-cli}

Das Plug-in für {{site.data.keyword.IBM}} SDK Generator
kann in der [{{site.data.keyword.cloud}}-CLI ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli){: new_window} installiert werden.

Dieses Plug-in für {{site.data.keyword.IBM_notm}} SDK Generator
integriert Ihre Back-End-Services mit einem generierten SDK in Ihre App. Wenn eine Änderung bei einer REST-API auftritt, können Sie das SDK neu generieren und das alte ersetzen, um das SDK ohne großen Aufwand zu aktualisieren. Sie können auch die CLI in eine 'DevOps'-Pipeline integrieren und sicherstellen, dass das SDK immer mit den API-Spezifikationen übereinstimmt, wenn die App erstellt wird.

Die REST-API-Definition muss gültig sein und entweder auf einem aktiven
Serverendpunkt oder in einer lokalen Datei auf Ihrem System gehostet werden.

## Vorbereitende Schritte
{: #prereqs-sdkgen}

Stellen Sie sicher, dass Folgendes verfügbar ist:

* Ein [{{site.data.keyword.cloud_notm}}-Konto](http://cloud.ibm.com){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")
* Gültige und mit der Spezifikation gemäß [Open API ](https://www.openapis.org/){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") konforme API-Definition

## SDK-Plug-in installieren
{: #install-sdkgen}

1. [Installieren
Sie die {{site.data.keyword.cloud_notm}}-CLI](/docs/cli?topic=cloud-cli-install-ibmcloud-cli#install-ibmcloud-cli).

2. Installieren Sie das Plug-in `sdk-gen`:
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

Sie können die [{{site.data.keyword.dev_cli_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#install_plug-in) installieren, die die {{site.data.keyword.cloud_notm}}-Basis-CLI und auch das Plug-in `sdk-gen` enthält sowie andere nützliche lokale Tools.
{: tip}

## SDK generieren
{: #commands-sdkgen}

Generieren Sie ein SDK, indem Sie den folgenden Befehl eingeben:
`ibmcloud sdk generate
[arguments...] [command options]`

### Argumente
{: #gen-args-sdkgen}

* `APP_NAME` - Der Name der Cloud Foundry-App in
Ihrem aktuellen Bereich.
* `OPENAPI_DOC_LOCATION` - eine URL oder ein relativer Dateipfad zur unformatierten JSON- oder yaml-Datei mit der REST-API-Definition
* `GENERATED_SDK_NAME` (optional) - Der Name des
generierten SDK.

### Optionen
{: #gen-options-sdkgen}

* `PLATFORM` (erforderlich)
   * `--ios` - Generiert ein Swift-SDK für iOS.
   * `--swift` - Generiert ein Swift-SDK für Server.
   * `--js` - Generiert ein JavaScript-SDK.
* `LOCATION` (erforderlich) - Gibt den Typ für die
Option `OPENAPI_DOC_LOCATION` an.
   * `-r` - Ferne URL.
   * `-f` - Datei.
   * `-a` - Unter {{site.data.keyword.cloud_notm}} ausgeführte App.
   * `-l` - URL des lokalen Hosts.
* `--output "YOUR_RELATIVE_PATH"` (optional) - Legt
das generierte SDK in dem Verzeichnis ab, das anstelle von
`YOUR_RELATIVE_PATH` angegeben ist. (Vorhandene SDKs werden überschrieben.)
* `--unzip` (optional) - Extrahiert das generierte SDK. (Vorhandene SDKs werden überschrieben.)

### Verwendung
{: #gen-usage-sdkgen}

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
{: #validating-sdkgen-sdkgen}

Führen Sie den folgenden Befehl aus:
```
ibmcloud sdk validate [argument]
```
{: codeblock}

### Argumente
{: #val-args-sdkgen}

* `APP_NAME` - Der Name der Cloud Foundry-App in
Ihrem aktuellen Bereich.
* `OPENAPI_DOC_LOCATION` - eine URL oder ein relativer Dateipfad zur unformatierten JSON- oder yaml-Datei mit der REST-API-Definition

### Verwendung
{: #val-usage-sdkgen}

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
