---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: getting started swift, custom app, create app swift, stater kit swift, apple app swift, swift dependency, ios development

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# 入门教程
{: #getting_started_swift}

{{site.data.keyword.cloud}} 提供了多种解决方案和服务，用于支持 Swift 开发者构建具有客户所要求的安全性、AI 和价值的应用程序。通过丰富的产品与 SDK 的组合，您可以使用这些服务，快速地将最新的应用程序推向市场。此 Swift 编程教程向您介绍了如何将服务添加到新的或现有 Swift 应用程序中，而不论它是 iOS 客户端还是服务器端 Swift。
{: shortdesc}

以下教程将说明如何使用 [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 中的空入门模板工具包，通过 {{site.data.keyword.mobileanalytics_full}} 轻松地创建 Swift 移动应用程序。在控制台中，添加 {{site.data.keyword.mobileanalytics_short}} 服务，下载代码，在 Xcode 中本地运行 iOS 应用程序，以及配置和监视应用程序。

## 步骤 1. 对开发者的需求
{: #dev-requirements-swift}

要在 {{site.data.keyword.cloud_notm}} 上开始进行 iOS 开发，请确保满足以下需求。

### 操作系统
{: #swift-os-requirements}

开发 Swift 应用程序时，最好使用支持 MacOS 的最新硬件，并使用最新的 iOS 发行版进行测试。注册 [Apple Developer](https://developer.apple.com/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 帐户，以支持在物理设备上进行测试。

### iOS 和 Xcode
{: #ios_and_xcode}

- 安装 [Xcode 8+](https://developer.apple.com/xcode/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")（或更高版本）。
- 部署到 [iOS 8 设备](https://support.apple.com/downloads/ios){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")（或更高版本）。
- 在将应用程序提交到 Apple 之前，查看 [App Store 提交准则](https://developer.apple.com/app-store/guidelines/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")。

### SDK 和依赖项管理
{: #swift-sdk-management}

以下工具用于确保可以安装本机 SDK 以使用各种 {{site.data.keyword.cloud_notm}} 服务。您可以使用 CocoaPods 或 Carthage 来管理依赖关系。

* **使用 CocoaPods** - 安装 [CocoaPods](https://cocoapods.org/) 以支持 {{site.data.keyword.cloud_notm}} SDK：
  ```
sudo gem install cocoapods
```
  {: codeblock}

* **使用 Carthage** - 部分 SDK 还可通过 [Carthage](https://github.com/Carthage/Carthage){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 或 [Swift Package Manager](https://swift.org/package-manager/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 依赖关系管理器提供。要使用 Carthage 管理依赖关系，请执行以下步骤：

  安装 [Homebrew](https://brew.sh/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 以帮助安装 Carthage：
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

  安装 Carthage：
  ```
  brew install carthage
  ```
  {: codeblock}

## 步骤 2. 创建定制 iOS Swift 应用程序
{: #create-ios-app-swift}

1. 登录到 [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")。
2. 单击**创建应用程序**。
3. 在[空入门模板](https://cloud.ibm.com/developer/appledevelopment/create-app){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 页面上，可以使用缺省配置或根据需要更新字段。确保 **iOS Swift** 是所选语言。单击**创建**。

## 步骤 3. 添加 {{site.data.keyword.cloudant_short_notm}} 服务
{: #resources-swift}

现在，您可以将服务添加到 Swift 应用程序。此教程用于将 {{site.data.keyword.cloudant_short_notm}} 服务添加到 Swift 应用程序，该应用程序创建全面受管的、分发式
`JSON` 文档数据库。Cloudant 在确保数据安全和同步的轻量级框架中，赋予您的应用程序可伸缩性、高可用性和耐久性。

1. 在**应用程序详细信息**页面中，单击**添加服务**。
2. 选择**数据**，然后单击**下一步**。
3. 选择 **Cloudant**，然后单击**下一步**。
4. 单击**创建**。
5. 创建服务后，单击服务以进行启动。在此新页面上，选择**启动 Cloudant 仪表板**以开始创建数据库和 JSON 文档。或者，可以通过编程方式完成此操作。

## 步骤 4. 下载代码并设置客户端 SDK
{: #run-locally-swift}

要下载代码，请单击`应用程序` > `您的应用程序`下的**下载代码**。下载的代码随附 [SwiftCloudant SDK](https://github.com/cloudant/swift-cloudant){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 以及一些基本初始化代码。客户端 SDK 在 CocoaPods 和 Swift Package Manager 上可用。此解决方案使用 CocoaPods。

1. 解压缩已下载的代码。然后，使用终端浏览至解压缩的文件夹。
  ```
  cd <Name of Project>
  ```

2. 文件夹中包含具有必需依赖项的 Podfile。运行以下命令安装依赖项（客户端 SDK）：
  ```
  pod install
  ```
  {: codeblock}

## 步骤 5. 配置应用程序以使用新数据库
{: #config-db-swift}

1. 使用 XcodeOpen 打开以 `.xcworkspace` 结尾的文件名，并导航到 `ViewController.swift`。`SwiftCloudant` 是核心的 Cloudant SDK。`CouchDBClient` 是 `SwiftCloudant` 的类，在 `ViewController.swift` 中进行初始化。

  始终打开新的 Xcode 工作空间，而不是原始的 Xcode 项目文件：`MyApp.xcworkspace`。
  {: tip}

2. 已经包含数据库初始化代码，如下面的片段所示：
  ```swift
  /* Read the Cloudant credentials and intialize database connection */
  if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"), let dictionary = NSDictionary(contentsOfFile: contents) {
            let url = URL(string: dictionary["cloudantUrl"] as! String)
            let client = CouchDBClient(url:url!,
                                       username:dictionary["cloudantUsername"] as? String,
                                       password:dictionary["cloudantPassword"] as? String)
        }
  ```
  {: codeblock}

  服务凭证是 `BMSCredentials.plist` 文件的一部分。
  {: note}

## 步骤 6. 构建数据库操作
{: #build_ops-swift}

既然您拥有有效的数据库连接和 SDK 设置，您可以开始为您的特定用例构建基本的[创建、读取、更新和破坏操作](/docs/swift/data?topic=swift-cloudant#cloudant)。

## 后续步骤
{: #next-swift}

### 添加更多服务
{: #moreresources-swift}

可以直接在 Web 控制台中向 iOS 应用程序添加更多服务，例如以下常用服务：

* [添加 Push Notifications 服务](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate#gettingstartedtemplate)
* [添加使用应用程序标识进行的用户认证](/docs/services/appid?topic=appid-getting-started#getting-started)

### 使用 {{site.data.keyword.cloud_notm}} Developer Tools
{: #devtools-swift}

您还可以了解如何使用 [{{site.data.keyword.cloud_notm}} 开发者工具](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)来开发 Swift 应用程序。开发者工具提供了一种用于创建、开发和部署完整 Web、移动和微服务应用程序的命令行方法。
