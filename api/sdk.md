---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-07"

keywords: swift sdk plug-in, sdk generator swift, generated sdk swift, devops pipeline swift, open api swift, sdkgen swift, ibmcloud sdk swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Adding back-end services to your app with a generated SDK
{: #sdk-cli}

The {{site.data.keyword.IBM}} SDK Generator plug-in can be installed in the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started).

With the {{site.data.keyword.IBM_notm}} SDK Generator plug-in, you can integrate your back-end services to your app by using a generated SDK. When a change to a REST API occurs, you can regenerate the SDK, and replace the old one to easily upgrade the SDK. You can then add the CLI into a DevOps pipeline, and ensure that the SDK is always consistent with the API spec each time the app is built.

The REST API definition must be valid and either hosted on a live server endpoint or a local file on your system.

## Before you begin
{: #prereqs-sdk-cli}

Ensure that you have the following prerequisites:

* An [{{site.data.keyword.cloud_notm}}](https://{DomainName}){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon") account.
* A valid API definition that conforms to the [Open API](https://www.openapis.org/){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon") specification.

## Installing the SDK plug-in
{: #install-sdk-cli}

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started).

2. Install the SDK plug-in:
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

You can install the [{{site.data.keyword.dev_cli_notm}}](/docs/cli?topic=cloud-cli-getting-started) which include the base {{site.data.keyword.cloud_notm}} CLI, and also includes the `sdk-gen` plug-in along with other useful local tools.
{: tip}

## Generating the SDK
{: #commands-sdk-cli}

Generate an SDK by entering:
```
ibmcloud sdk generate [arguments...] [command options]
```
{: codeblock}

### Arguments
{: #gen-args-sdk-cli}

* **APP_NAME** - The name of the Cloud Foundry app in your current space.
* **OPENAPI_DOC_LOCATION** - A URL or a relative file path to the raw REST API definition JSON or yaml.
* **GENERATED_SDK_NAME** (optional) - The name of the generated SDK.

### Options
{: #gen-options-sdk-cli}

**Platform** (required):
  * `--ios` - Generate an iOS Swift SDK.
  * `--swift` - Generate a Swift server SDK.
  * `--js` - Generate a JavaScript SDK.

**Location** (required):

Specifies the type for the `OPENAPI_DOC_LOCATION` argument.

  * `-r` - A remote URL.
  * `-f` - A file name.
  * `-a` - App that runs on {{site.data.keyword.cloud_notm}}
  * `-l` - The localhost URL.

**Optional**:
  * `--output "YOUR_RELATIVE_PATH"` - Places the generated SDK in the directory that is specified by `YOUR_RELATIVE_PATH` (Existing SDKs are overwritten if present).
  * `--unzip` - Extracts the generated SDK (overwrites existing data if SDK artifacts are present).

### Usage
{: #gen-usage-sdk-cli}

To generate an SDK from a Cloud Foundry app that is running in {{site.data.keyword.cloud_notm}}, you can use the app's name as a parameter to the CLI. The following command uses the app's name as the `SDK_Name`.

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

To generate an SDK from a URL to an Open API definition file or a local JSON or yaml file, use the following command.

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}


## Validating the Open API definitions
{: #validating-sdk-cli}

Run the following command: `ibmcloud sdk validate [argument]`

### Arguments
{: #val-args-sdk-cli}

* `APP_NAME` - The name of the Cloud Foundry app in your current space.
* `OPENAPI_DOC_LOCATION` - A URL or a relative file path to the raw REST API definition JSON or yaml.

### Usage
{: #val-usage-sdk-cli}

To validate a Cloud Foundry app's API spec that is running in {{site.data.keyword.cloud_notm}}, you can use the app's name as a parameter to the CLI.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

To validate an SDK from the URL to an API spec document or a local JSON or yaml file, use the following command:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

