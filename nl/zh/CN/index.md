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

# 入门教程
{: #set_up}

{{site.data.keyword.cloud}} 提供了多种解决方案和服务，用于支持 Swift 开发者和应用程序实现客户要求的安全性、AI 和价值。通过产品与 SDK 的广泛组合，您可以利用这些服务，并快速将最新的应用程序推向市场。Swift 编程指南指导您如何将服务添加到新的或现有 Swift 应用程序，不管它是 iOS 客户端还是服务器端 Swift。
{: shortdesc}

以下教程作为入口点，说明如何使用 [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits) 中的空入门模板工具包，通过 {{site.data.keyword.mobileanalytics_full}} 轻松创建 Swift 移动应用程序。在控制台中，添加 {{site.data.keyword.mobileanalytics_short}} 服务，下载代码，在 Xcode 中本地运行 iOS 应用程序，以及配置和监视应用程序。

## 步骤 1. 对开发者的需求
{: #dev-requirements}

要在 {{site.data.keyword.cloud_notm}} 上开始进行 iOS 开发，请确保满足以下需求。

### 操作系统

最好使用支持 MacOS 的最新硬件来开发 Swift 应用程序，并使用最新的 iOS 发行版进行测试。注册 [Apple Developer](https://developer.apple.com/) 帐户，以支持在物理设备上进行测试。

### iOS 和 Xcode
{: #ios_and_xcode}

- 安装 [Xcode 8+](https://developer.apple.com/xcode/)（或更高版本）
- 部署到 [iOS 8 设备](https://support.apple.com/downloads/ios)（或更高版本）
- 将应用程序提交到 Apple 之前，查看 [App Store 提交准则](https://developer.apple.com/app-store/guidelines/)

### SDK 和依赖项管理

以下工具用于确保可以安装本机 SDK 以使用各种 {{site.data.keyword.cloud_notm}} 服务。

1. 安装 [CocoaPods](https://cocoapods.org/) 以支持 {{site.data.keyword.cloud_notm}} SDK：
  ```
sudo gem install cocoapods
```
  {: codeblock}
  
2. 安装 [Homebrew](https://brew.sh/) 以帮助安装 Carthage：
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

3. 安装 [Carthage](https://github.com/Carthage/Carthage) 以支持 Watson SDK：
  ```
  brew install carthage
  ```
  {: codeblock}

## 步骤 2. 创建定制 iOS Swift 应用程序
{: #create-ios-app}

1. 登录到 [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits)。
2. 单击**创建应用程序**。
3. 在[空入门模板](https://console.bluemix.net/developer/appledevelopment/create-app)页面上，可以使用缺省配置或根据需要更新字段。确保 **iOS Swift** 是所选语言。单击**创建**。

## 步骤 3. 添加 {{site.data.keyword.mobileanalytics_short}} 服务
{: #adding-services}

1. 在“应用程序详细信息”页面中，单击**添加资源**按钮。
2. 选择**移动**，然后单击**下一步**。
3. 选择 **{{site.data.keyword.mobileanalytics_short}}**，然后单击**下一步**。
4. 单击**创建**。

## 步骤 4. 下载代码并设置客户端 SDK
{: #run-locally}

要下载代码，请单击`应用程序` > `您的应用程序`下的**下载代码**。下载的代码随附 **{{site.data.keyword.mobileanalytics_short}}** 客户端 SDK。客户端 SDK 在 CocoaPods 和 Carthage 上可用。对于此解决方案，请使用 CocoaPods。

1. 解压缩下载的代码。 然后，使用终端浏览至解压缩的文件夹。
  ```
  cd <Name of Project>
  ```
2. 文件夹中包含具有必需依赖项的 podfile。运行以下命令安装依赖项（客户端 SDK）：
  ```
pod install
```
  {: codeblock}

## 步骤 5. 配置应用程序以使用 {{site.data.keyword.mobileanalytics_short}}
{: #configure-analytics}

1. 在 Xcode 中打开 `.xcworkspace` 并浏览至 `AppDelegate.swift`。
  **注：**确保始终打开的是新 Xcode 工作空间，而不是原始 Xcode 项目文件：`MyApp.xcworkspace`。
     ![打开的 Xcode](images/Xcode.png)

  `BMSCore` 是核心 SDK，也是移动式客户端 SDK 的基础。`BMSClient` 是 BMSCore 类，已在 AppDelegate.swift 中初始化。{{site.data.keyword.mobileanalytics_short}} SDK 已经与 BMSCore 一起导入到项目中。
  
2. 已包含分析初始化代码，如以下片段中所示：
  ```
  // 分析客户端 SDK 配置为记录生命周期事件。
         	Analytics.initialize(appName:dictionary["appName"] as? String,
        			     apiKey: dictionary["analyticsApiKey"] as? String,
        	        	     deviceEvents: .lifecycle)

        	// 启用记录器（缺省情况下已禁用），并将级别设置为 ERROR（缺省情况下为 DEBUG）。
        	Logger.isLogStorageEnabled = true
        	Logger.logLevelFilter = .error
  ```
  {: codeblock}

  **注：**服务凭证是 `BMSCredentials.plist` 文件的一部分。

3. 使用 `logger` 收集使用情况分析。浏览至 `ViewController.swift` 文件可看到以下代码。
  ```
   func didBecomeActive(_ notification: Notification) {
       Analytics.send()
       Logger.send()
    }
  ```
  {: codeblock}

   有关高级分析和日志记录功能，请参阅[收集使用情况分析](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics)和[日志记录](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger)。
   {:tip}

## 步骤 6. 使用 {{site.data.keyword.mobileanalytics_short}} 监视应用程序
{{site.data.keyword.mobileanalytics_short}} 服务为移动应用程序开发者和应用程序所有者提供关键应用程序使用情况和性能洞察。通过使用 {{site.data.keyword.mobileanalytics_short}}，应用程序所有者和开发者可以了解用户端正在发生的情况。他们可以使用此洞察来构建与用户相关的更完善的应用程序，从而在海量的移动应用程序中脱颖而出。

该服务包括 {{site.data.keyword.mobileanalytics_short}} Console，开发人员和应用程序所有者可以在这里监视移动应用程序性能，查看使用情况统计信息，并搜索设备日志。

1. 通过创建的移动应用程序打开 `{{site.data.keyword.mobileanalytics_short}}` 服务，或者单击服务旁边的三个垂直点，然后选择`打开仪表板`。
2. 可以通过禁用`演示方式`来查看实时用户、会话和其他应用程序数据。可以按以下条件来过滤分析信息：
    * 日期。
    * 应用程序。
    * 操作系统。
    * 应用程序的版本。
         ![{{site.data.keyword.mobileanalytics_short}}](images/mobile_analytics.png)
3. [单击此处](https://console.bluemix.net/docs/services/mobileanalytics/app-monitoring.html#monitoringapps)以设置警报、监视应用程序崩溃和监视网络请求。

## 后续步骤
{: #next-steps}

### 添加更多服务
可以直接在 Web 控制台中向 iOS 应用程序添加更多服务，例如以下常用服务：

* [添加 Push Notifications 服务](/push/push_notifications.html)
* [添加使用应用程序标识进行的用户认证](/authenticate/app_id.html)

### 使用 {{site.data.keyword.cloud_notm}} Developer Tools
您还可以使用 [{{site.data.keyword.cloud_notm}} Developer Tools](../cli/index.html)（提供用于创建、开发和部署端到端 Web、移动和微服务应用程序的命令行方法）来了解如何开发 Swift 应用程序。

