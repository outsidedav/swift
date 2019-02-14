---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Integrating back-end services to your app with a generated SDK
{: #sdkgen-cli}

The {{site.data.keyword.IBM}} SDK Generator plug-in can be installed in the [{{site.data.keyword.cloud_notm}} CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/cli/reference/bluemix_cli/index.html){: new_window}.

This {{site.data.keyword.IBM_notm}} SDK Generator plug-in integrates your back-end services to your app with a generated SDK. When a change to a REST API occurs, you can regenerate the SDK and replace the old one to easily upgrade the SDK. You can also integrate the CLI into a DevOps pipeline and ensure that the SDK is always consistent with the API spec each time the app is built.

The REST API definition must be valid and either hosted on a live server endpoint or a local file on your system.

## Before you begin
{: #prereqs-sdkgen}

Ensure that you have:

* An [{{site.data.keyword.Bluemix_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://cloud.ibm.com){: new_window} account
* A valid API definition that conforms to the [Open API ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.openapis.org/){: new_window} specification

## Installing the SDK plug-in
{: #install-sdkgen}

1. [Install the {{site.data.keyword.Bluemix}} CLI](/docs/cli/reference/bluemix_cli/get_started.html).

2. [Install the plug-in ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/cli/reference/bluemix_cli/index.html#install_plug-in){: new_window}.

  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## Generating the SDK
{: #commands-sdkgen}

Generate an SDK by entering: `ibmcloud sdk generate [arguments...] [command options]`

### Arguments
{: #gen-args}

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
   * `-a` - app that runs on {{site.data.keyword.Bluemix_notm}}
   * `-l` - localhost URL
* `--output "YOUR_RELATIVE_PATH"` (optional) - places the generated SDK in the directory that is specified by `YOUR_RELATIVE_PATH` (Existing SDKs are overwritten).
* `--unzip` (optional) - extracts the generated SDK (Existing SDKs are overwritten).

### Usage
{: #gen-usage-sdkgen}

To generate an SDK from a Cloud Foundry app that is running in {{site.data.keyword.Bluemix_notm}}, you can use the app's name as a parameter to the CLI. The following command uses the app's name as the `SDK_Name`.

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

To validate a Cloud Foundry app's API spec that is running in {{site.data.keyword.Bluemix_notm}}, you can use the app's name as a parameter to the CLI.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

To validate an SDK from the URL to an API spec document or a local JSON or yaml file, use the following command:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}
