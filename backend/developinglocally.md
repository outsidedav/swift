---

copyright:
  years: 2018
lastupdated: "2018-03-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Developing Locally
Developing locally enable you to easily build, run, and test your application. You use the {{site.data.keyword.dev_cli_short}} to execute these actions with standard commands. 

You can use the {{site.data.keyword.dev_cli_short}} to manage your server-side applications with more than a dozen commands. Learn more about the various commands at [IBM Cloud Developer Tools CLI (bx dev) commands](/docs/cloudnative/idt/commands.html).

## Before you begin

To develop locally, you must install the {{site.data.keyword.dev_cli_notm}}. Execute the following command to run our installation script:

```
curl -sL https://ibm.biz/idt-installer | bash
```
{:codeblock}

See [Setting up the IBM Cloud Developer Tools CLI](/docs/cloudnative/idt/setting_up_idt.html) to learn more about the set up and use of the {{site.data.keyword.dev_cli_notm}}.

## Step 1. Retrieve the service credentials
After you clone your application from Git, you must retrieve the credentials for the services bound to your application because these are not stored in the git repo for your application. This enables the use of bound services. You can easily download the credentials by running the following command in the root of the application directory:

```
idt get-credentials
```
{:codeblock}


## Step 2. Build your application
You can now `build` your application, which is a prerequisite to `run` your application.

Use the following command in the root of the application directory to build your app:

```
idt build
```
{:codeblock}

## Step 3. Run your application
After a successful `build` you can `run` your application in a local container with the following command:

```
idt run
```
{:codeblock}

A local host and port to view your application's landing page will be displayed if the command runs successfully.

## Step 4. Deploy your application
Deploy your application to the {{site.data.keyword.Bluemix_notm}} with the `deploy` command:

```
idt deploy
```
{:codeblock}
