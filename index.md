---

copyright:
  years: 2018
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Getting started tutorial
{: #set_up}

{{site.data.keyword.cloud}} offers solutions and services to empower Swift developers and applications with the security, AI, and value demanded by your customers. With a broad portfolio of offerings and SDKs, you can leverage these services, and bring your cutting-edge applications to market quickly.
This Swift programming guide teaches you how to add services to a new or existing application, whether it is an  iOS client or server-side Swift.
{: shortdesc}

The following tutorial is a starting point that is intended to help you build, run, and deploy a server-side Swift app locally. Learn how to use the {{site.data.keyword.dev_cli_long}} to execute these actions with standard commands.

You can use the {{site.data.keyword.dev_cli_short}} to manage your server-side applications with more than a dozen commands. Learn more about the `idt` commands at [IBM Cloud Developer Tools CLI](/docs/cli/idt/commands.html).

## Step 1. Requirements for Developers

To get started with {{site.data.keyword.cloud_notm}}, make sure that you meet the following requirements.

### Operating System

It is best practice to develop Swift apps by using the latest machines running MacOS, and testing with the latest iOS releases. Sign up for an [Apple Developer](https://developer.apple.com/) account to enable testing on a physical device.

### iOS & Xcode
{: #ios_and_xcode}

- [Install Xcode 8+ (or higher)](https://developer.apple.com/xcode/)
- [Deploy to iOS devices 8 (or higher)](https://support.apple.com/downloads/ios)
- Before app submission to Apple, review the [App Store Submission Guidelines](https://developer.apple.com/app-store/guidelines/)

### SDKs and Dependency management

The following tools are required to ensure you can install the native SDKs to work with the various IBM Cloud Services.

- [Install CocoaPods for IBM Cloud SDKs](https://cocoapods.org/)
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Install Homebrew to help install Carthage](https://brew.sh/)
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Install Carthage for Watson SDKs](https://github.com/Carthage/Carthage)
  ```
  brew install carthage
  ```
  {: codeblock}

## Step 2. Installing tools for local development

The {{site.data.keyword.cloud}} provides local CLI tools that help you work with various aspects of the {{site.data.keyword.cloud_notm}}. For more information, see [{{site.data.keyword.dev_cli_long}} Information](https://www.ibm.com/cloud/cli). It is recommended to install them for testing a Swift backend in a local Docker image before deploying to the cloud.

-  Install {{site.data.keyword.dev_cli_notm}}:
  ```
  curl -sL https://ibm.biz/idt-installer | bash
  ```
  {: codeblock}

## Step 3. Creating a Swift project

1. Use the `idt create` command from {{site.data.keyword.dev_cli_short}} CLI to generate a pre-configured starter. 
  ```
  idt create
  ```
  {: codeblock}

  Be sure to log in with an {{site.data.keyword.cloud_notm}} account to create a project. First-time users can [register ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/registration/?cm_sp=dw-bluemix-_-swift-_-devcenter) for a free account. Use the `ibmcloud login` command to login on the command line.
  {: tip}

2. When prompted, choose options 1, then 6, and lastly 2, as displayed in the following example:
  ```
  ? Select a resource type:                  
  1. Backend Service / Web App
  2. Mobile Client
  Enter a number> 1

  ? Select a language:
  1. Java - MicroProfile / Java EE
  2. Java - Spring
  3. Node
  4. Python - Django
  5. Python - Flask
  6. Swift
  Enter a number> 6

  ? Select a Starter Kit:
  1. Backend for Frontend - Swift Kitura - A starter for building 
  backend-for-frontend APIs in Swift, using the Kitura framework.
  2. Web App - Create Project
  3. Web App - Swift Kitura Basic - A starter that provides a basic web serving 
  application in Swift, using the Kitura framework.
  Enter a number> 2
  ```
  {: screen}

3. Provide a name for your application:
  ```
  ? Enter a name for your application> swift_project
  ```
  {: screen}

4. Choose to enable OpenAPI 2.0 support:
  ```
  ? Enable your application based on an OpenAPI 2.0 Specification document? [y/n]> y
  ```
  {: screen}

  If OpenAPI 2.0 support is enabled, you must provide a path to the OpenAPI 2.0 document as a url:
  ```
  ? Path to OpenAPI 2.0 document as a url (both yaml and json formats supported)> http://hostname.domain.com/path/to/file.json

  Generating application ...
  ```
  {: screen}

## Step 4. Build, Run, and Deploy your application

You can now build, run, and deploy your application by using the {{site.data.keyword.dev_cli_short}}.

1. **Build**

  You can now `build` your application, which is a prerequisite to `run` your application. Use the following command in the root of the application directory to build your app:
  ```
  idt build
  ```
  {:codeblock}

2. **Run**

  After a successful `build`, you can `run` your application in a local container with the following command:
  ```
  idt run
  ```
  {:codeblock}

  A local host and port to view your application's landing page is displayed if the command runs successfully.

3. **Deploy**

  Deploy your application to the {{site.data.keyword.cloud_notm}} with the `deploy` command:
  ```
  idt deploy
  ```
  {:codeblock}

## Next steps

Learn to use the {{site.data.keyword.cloud_notm}} Developer Console for Apple that enables developers to create apps from a variety of starter kits, provision and connect key {{site.data.keyword.cloud_notm}} optimized services, and then quickly download working code or set up for continuous delivery. Users are able to create, view, configure, and manage your app, as well as download your app's code. This allows you to quickly evaluate and test {{site.data.keyword.cloud_notm}} services with a brand new app.

Ready to jump in?  Visit the [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits) now to get started.
{: tip}

For more information, see [Developing Swift apps with Starter Kits](/docs/swift/starter_kit/starter_kits.html).
