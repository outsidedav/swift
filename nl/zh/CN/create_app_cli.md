---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-19"

keywords: server-side swift, swift cli, swift dependency, swift commands app, create app swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 使用 CLI 创建服务器端 Swift 应用程序
{: #swift_cli}

{{site.data.keyword.cloud}} 提供了多种解决方案和服务，用于支持 Swift 开发者和应用程序实现客户要求的安全性、AI 和价值。通过丰富的产品与 SDK 的组合，您可以使用这些服务，快速地将最新的应用程序推向市场。
{: shortdesc}

以下指南旨在帮助您构建、本地运行和部署服务器端 Swift 应用程序。了解如何使用 {{site.data.keyword.dev_cli_long}} 通过运行标准命令来执行这些操作。

可以使用 {{site.data.keyword.dev_cli_short}} 通过十几个命令来管理服务器端应用程序。了解有关 [{{site.data.keyword.dev_cli_notm}} CLI](/docs/cli/idt?topic=cloud-cli-idt-cli) 中 `ibmcloud dev` 命令的更多信息。

## 步骤 1. 对开发者的需求
{: #prereqs-swift-cli}

要开始使用 {{site.data.keyword.cloud_notm}}，请确保满足以下需求。

### 操作系统
{: #swift-cli-os-reqs}

开发 Swift 应用程序时，最好使用支持 MacOS 的最新硬件，并使用最新的 iOS 发行版进行测试。注册 [Apple Developer](https://developer.apple.com/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 帐户，以支持在物理设备上进行测试。

### iOS 和 Xcode
{: #swift-cli-ios_xcode}

- [安装 Xcode 8+（或更高版本）](https://developer.apple.com/xcode/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")
- [部署到 iOS 设备 8（或更高版本）](https://support.apple.com/downloads/ios){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")
- 将应用程序提交到 Apple 之前，请查看 [App Store 提交准则](https://developer.apple.com/app-store/resources/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")

### SDK 和依赖项管理
{: #swift-cli-sdk-dependency}

以下工具用于确保可以安装本机 SDK 以使用各种 {{site.data.keyword.cloud_notm}} 服务。

- [安装 CocoaPods 以用于 {{site.data.keyword.cloud_notm}} SDK](https://cocoapods.org/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")
  ```
sudo gem install cocoapods
```
  {: codeblock}
  
- [安装 Homebrew 以帮助安装 Carthage](https://brew.sh/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [安装 Carthage 以用于 Watson SDK](https://github.com/Carthage/Carthage){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")
  ```
  brew install carthage
  ```
  {: codeblock}

## 步骤 2. 安装用于本地开发的工具
{: #swift-cli-install-tools}

{{site.data.keyword.cloud}} 提供了本地 CLI 工具，可帮助您使用 {{site.data.keyword.cloud_notm}} 的各方面功能。有关更多信息，请参阅 [{{site.data.keyword.dev_cli_long}} 信息](/docs/cli?topic=cloud-cli-getting-started)。在进行云部署之前，您可以使用这些工具对本地 Docker 映像中的 Swift 后端进行测试。

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
{: #create-swift-app-cli}

1. 通过 {{site.data.keyword.dev_cli_short}} CLI 运行 `ibmcloud dev create` 命令以生成预配置的入门模板。 
  ```
ibmcloud dev create
```
  {: codeblock}

  确保使用 {{site.data.keyword.cloud_notm}} 帐户登录以创建项目。首次使用的用户可以[注册 ](https://{DomainName}/registration){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 免费帐户。在命令行上使用 `ibmcloud login` 命令登录。
  {: tip}

2. 出现提示时，选择选项 1，然后选择 6，最后选择 2，如以下示例中所示：
  ```
  ? 选择服务类型：
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
  Kitura 框架通过 Swift 编写的服务于前端的后端 API。
  2. Web 应用程序 - 创建项目
  3. Web 应用程序 - Swift Kitura Basic - 此入门模板用于提供
  使用 Kitura 框架通过 Swift 编写的服务于 Web 的基本应用程序。
  输入数字> 2
  ```
  {: screen}

3. 提供应用程序的名称：
  ```
  ? 输入应用程序的名称> swift_project
  正在生成应用程序...
  ```
  {: screen}

## 步骤 4. 构建、运行和部署应用程序
{: #swift-cli-deploy}

现在，可以使用 {{site.data.keyword.dev_cli_short}} 来构建、运行和部署应用程序。

### 构建应用程序
{: #swift-build-cli}

现在可以构建应用程序，这是运行应用程序的先决条件。在应用程序目录的根目录中使用以下命令来构建应用程序：
```
ibmcloud dev build
```
{: codeblock}

### 运行应用程序
{: #swift-run-cli}

成功构建后，可以使用以下命令在本地容器中运行应用程序：
```
ibmcloud dev run
```
{: codeblock}

如果命令成功运行，将显示用于查看应用程序登录页面的本地主机和端口。

### 部署应用程序
{: #swift-deploy-cli}

使用 `deploy` 命令将应用程序部署到 {{site.data.keyword.cloud_notm}}：
```
ibmcloud dev deploy
```
{: codeblock}

## 后续步骤
{: #swift-cli-next notoc}

了解如何使用 {{site.data.keyword.cloud_notm}} Developer Console for Apple。通过此控制台，开发者可以使用各种入门模板工具包来创建应用程序，创建和连接 {{site.data.keyword.cloud_notm}} 优化的关键服务，然后快速下载可正常运行的代码或针对持续交付进行设置。用户可以创建、查看、配置和管理应用程序，以及下载应用程序的代码。通过 Developer Console for Apple，可以使用全新的应用程序快速评估和测试 {{site.data.keyword.cloud_notm}} 服务。

准备好开始了吗？请立即访问 [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 以开始。
{: tip}

有关更多信息，请参阅[使用入门模板工具包开发 Swift 应用程序](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro)。
