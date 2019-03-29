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

# Integrating back-end services to your app with a generated SDK
{: #sdkgen-cli}

The {{site.data.keyword.IBM}} SDK Generator plug-in can be installed in the [{{site.data.keyword.cloud}} CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli){: new_window}.

This {{site.data.keyword.IBM_notm}} SDK Generator plug-in integrates your back-end services to your app with a generated SDK. When a change to a REST API occurs, you can regenerate the SDK and replace the old one to easily upgrade the SDK. You can also integrate the CLI into a DevOps pipeline and ensure that the SDK is always consistent with the API spec each time the app is built.

The REST API definition must be valid and either hosted on a live server endpoint or a local file on your system.

## Before you begin
{: #prereqs-sdkgen}

Ensure that you have:

* An [{{site.data.keyword.cloud_notm}}](http://cloud.ibm.com){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon") account
* A valid API definition that conforms to the [Open API](https://www.openapis.org/){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon") specification

## Installing the SDK plug-in
{: #install-sdkgen}

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-install-ibmcloud-cli#install-ibmcloud-cli).

2. Install the `sdk-gen` plug-in:
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

You can install the [{{site.data.keyword.dev_cli_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#install_plug-in) which include the base {{site.data.keyword.cloud_notm}} CLI, and also includes the `sdk-gen` plug-in along with other useful local tools.
{: tip}

## Generating the SDK
{: #commands-sdkgen}

Generate an SDK by entering: `ibmcloud sdk generate [arguments...] [command options]`

### Arguments
{: #gen-args-sdkgen}

* `APP_NAME` - the name of the Cloud Foundry app in your current space
* `OPENAPI_DOC_LOCATION` - a URL or a relative file path to the raw REST API definition JSON or yaml
* `GENERATED_SDK_NAME` (optional) - the name of the generated SDK

### Options
{: #gen-options-sdkgen}

* `PLATFORM` (required)
   * `--ios` - generate an iOS Swift SDK
   * `--swift` - generate a Swift server SDK
   * `--js` - generate a JavaScript SDK
* `LOCATION` (required) - specifies the type for `OPENAPI_DOC_LOCATION`
   * `-r` - remote URL
   * `-f` - file
   * `-a` - app that runs on {{site.data.keyword.cloud_notm}}
   * `-l` - localhost URL
* `--output "YOUR_RELATIVE_PATH"` (optional) - places the generated SDK in the directory that is specified by `YOUR_RELATIVE_PATH` (Existing SDKs are overwritten).
* `--unzip` (optional) - extracts the generated SDK (Existing SDKs are overwritten).

### Usage
{: #gen-usage-sdkgen}

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
{: #validating-sdkgen-sdkgen}

Run the following command:
```
ibmcloud sdk validate [argument]
```
{: codeblock}

### Arguments
{: #val-args-sdkgen}

* `APP_NAME` - the name of the Cloud Foundry app in your current space
* `OPENAPI_DOC_LOCATION` - a URL or a relative file path to the raw REST API definition JSON or yaml

### Usage
{: #val-usage-sdkgen}

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
