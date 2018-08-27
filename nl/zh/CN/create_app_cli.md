---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 使用 CLI 创建服务器端 Swift 应用程序
{: #swift_cli}

{{site.data.keyword.cloud}} 提供了多种解决方案和服务，用于支持 Swift 开发者和应用程序实现客户要求的安全性、AI 和价值。通过产品与 SDK 的广泛组合，您可以利用这些服务，并快速将最新的应用程序推向市场。
{: shortdesc}

以下指南旨在帮助您构建、本地运行和部署服务器端 Swift 应用程序。了解如何使用 {{site.data.keyword.dev_cli_long}} 通过标准命令来执行这些操作。

可以使用 {{site.data.keyword.dev_cli_short}} 通过十几个命令来管理服务器端应用程序。了解有关 [ IBM Cloud Developer Tools CLI](/docs/cli/idt/commands.html)中 `ibmcloud dev` 命令的更多信息。

## 步骤 1. 对开发者的需求

要开始使用 {{site.data.keyword.cloud_notm}}，请确保满足以下需求。

### 操作系统

最好使用最新的 MacOS 支持的硬件来开发 Swift 应用程序，并使用最新的 iOS 发行版进行测试。注册 [Apple Developer](https://developer.apple.com/) 帐户，以支持在物理设备上进行测试。

### iOS 和 Xcode
{: #ios_and_xcode}

- [安装 Xcode 8+（或更高版本）](https://developer.apple.com/xcode/)
- [部署到 iOS 设备 8（或更高版本）](https://support.apple.com/downloads/ios)
- 将应用程序提交到 Apple 之前，请查看 [App Store 提交准则](https://developer.apple.com/app-store/guidelines/)

### SDK 和依赖项管理

以下工具用于确保可以安装本机 SDK 以使用各种 {{site.data.keyword.cloud_notm}} 服务。

- [安装 CocoaPods 以用于 IBM Cloud SDK](https://cocoapods.org/)
  ```
sudo gem install cocoapods
```
  {: codeblock}
  
- [安装 Homebrew 以帮助安装 Carthage](https://brew.sh/)
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [安装 Carthage 以用于 Watson SDK](https://github.com/Carthage/Carthage)
  ```
  brew install carthage
  ```
  {: codeblock}

## 步骤 2. 安装用于本地开发的工具

{{site.data.keyword.cloud}} 提供了本地 CLI 工具，可帮助您使用 {{site.data.keyword.cloud_notm}} 的各方面功能。有关更多信息，请参阅 [{{site.data.keyword.dev_cli_long}} 信息](../cli/index.html)。建议安装这些工具，以用于在部署云之前对本地 Docker 映像中的 Swift 后端进行测试。

* 对于 MacOS 和 Linux，运行以下命令：
  ```
  curl -sL http://ibm.biz/idt-installer | bash
  ```
  {: codeblock}

* 对于 Windows 10，请以管理员身份运行以下命令：
  ```
Set-ExecutionPolicy Unrestricted; iex(New-Object Net.WebClient).DownloadString('http://ibm.biz/idt-win-installer')
```
  {: codeblock}

  右键单击 Windows PowerShell 图标，然后选择**以管理员身份运行**。
  {: tip}

## 步骤 3. 创建 Swift 应用程序

1. 通过 {{site.data.keyword.dev_cli_short}} CLI 运行 `ibmcloud dev create` 命令以生成预配置的入门模板。 
  ```
ibmcloud dev create
```
  {: codeblock}

  确保使用 {{site.data.keyword.cloud_notm}} 帐户登录以创建项目。一开始用户可以[注册 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/registration/?cm_sp=dw-bluemix-_-swift-_-devcenter) 免费帐户。在命令行上使用 `ibmcloud login` 命令登录。
  {: tip}

2. 系统提示时，选择选项 1，然后选择 6，最后选择 2，如以下示例中所示：
  ```
  ? 选择资源类型：
  1. 后端服务 / Web 应用程序
  2. 移动式客户端
  输入数字> 1

  ? 选择语言：
  1. Java - MicroProfile / Java EE
  2. Java - Spring
  3. Node
  4. Python - Django
  5. Python - Flask
  6. Swift
  输入数字> 6

  ? 选择入门模板工具包：
  1. 服务于前端的后端 - Swift Kitura - 此入门模板用于构建使用
  Kitura 框架并用 Swift 编写的服务于前端的后端 API。
  2. Web 应用程序 - 创建项目
  3. Web 应用程序 - Swift Kitura Basic - 此入门模板用于提供服务于
  使用 Kitura 框架并用 Swift 编写的应用程序的基本 Web。
  输入数字> 2
  ```
  {: screen}

3. 提供应用程序的名称：
  ```
  ? 输入应用程序的名称> swift_project
  ```
  {: screen}

4. 选择启用 OpenAPI 2.0 支持：
  ```
  ? 根据 OpenAPI 2.0 规范文档启用应用程序吗？[y/n]> y
  ```
  {: screen}

  如果启用了 OpenAPI 2.0 支持，那么必须将 OpenAPI 2.0 文档的路径作为 URL 提供：
  ```
  ? 作为 URL 的 OpenAPI 2.0 文档路径（同时支持 YAML 和 JSON 格式）> http://hostname.domain.com/path/to/file.json

  正在生成应用程序...
  ```
  {: screen}

## 步骤 4. 构建、运行和部署应用程序

现在，可以使用 {{site.data.keyword.dev_cli_short}} 来构建、运行和部署应用程序。

1. **构建**

  现在可以构建应用程序，这是运行应用程序的先决条件。在应用程序目录的根目录中使用以下命令来构建应用程序：
  ```
  ibmcloud dev build
  ```
  {:codeblock}

2. **运行**

  成功构建后，可以使用以下命令在本地容器中运行应用程序：
  ```
  ibmcloud dev run
  ```
  {:codeblock}

  如果命令成功运行，将显示用于查看应用程序登录页面的本地主机和端口。

3. **部署**

  使用 `deploy` 命令将应用程序部署到 {{site.data.keyword.cloud_notm}}：
  ```
  ibmcloud dev deploy
  ```
  {:codeblock}

## 后续步骤

了解如何使用 {{site.data.keyword.cloud_notm}} Developer Console for Apple，此控制台支持开发者通过各种入门模板工具包来创建应用程序，供应和连接 {{site.data.keyword.cloud_notm}} 优化的关键服务，然后快速下载工作代码（或设置为持续交付）。用户可以创建、查看、配置和管理应用程序，以及下载应用程序的代码。通过 Developer Console for Apple，可以使用全新的应用程序快速评估和测试 {{site.data.keyword.cloud_notm}} 服务。

准备好开始了吗？请立即访问 [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits)。
{: tip}

有关更多信息，请参阅[使用入门模板工具包开发 Swift 应用程序](/docs/swift/starter_kit/starter_kits.html)。
