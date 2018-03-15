---

copyright:
  years: 2018
lastupdated: "2018-03-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Developing Locally

## Prerequisites to Developing Locally

In order to develop locally, you must install the {{site.data.keyword.dev_cli_notm}}, formerly referred to as the Bluemix CLI Developer Plug-in. To install the tool, execute the following command to run our installation script:

```
	curl -sL https://ibm.biz/idt-installer | bash
```
{:codeblock}

The installation guide can be found [here](/docs/cli/idt/setting_up_idt.md).

## How to Develop Locally

Developing locally allows you to easily build, run, and test your application. The {{site.data.keyword.dev_cli_short}} allows you to execute these actions with standard commands. 

After you clone your application from Git, you must retrieve the credentials for the services bound to your application since these are not stored in the git repo for your application. This will allow you to enable the use of bound services. You can easily download the credentials by running the following command in the root of the application directory:

```
	idt get-credentials
```
{:codeblock}


## Developing Backend/Server-side Applications

You can use the {{site.data.keyword.dev_cli_short}} to manage your server-side applications with more than a dozen commands. More information of the various commands can be found [here](/docs/cli/idt/commands.md)

You can now `build` your application, which is a prerequisite to `run` your application.

Use the following command in the root of the application directory to build your app:

```
	idt build
```
{:codeblock}

Only after a successful `build` can you `run` your application in a local container with the following command:

```
	idt run
```
{:codeblock}

Upon successful execution of the `run` command, the {{site.data.keyword.dev_cli_short}} will present you with a local host and port to view your application's landing page.

Furthermore, you can deploy your application to the {{site.data.keyword.Bluemix_notm}} with the `deploy` command:

```
	idt deploy
```
{:codeblock}

Find more information about deployments [here](/docs/cli/idt/commands.md#deploy)