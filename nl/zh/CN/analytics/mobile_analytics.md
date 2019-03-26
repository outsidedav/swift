---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 收集移动分析
{: #mobile_analytics}

{{site.data.keyword.mobileanalytics_short}} on {{site.data.keyword.cloud_notm}} 可让开发者、管理员和业务利益相关方深入了解其移动应用程序的性能和使用情况。使用 {{site.data.keyword.mobileanalytics_short}} 服务，可以：

 - **立即获得洞察** - 查看实时性能和使用情况度量值。

 - **在几分钟后实施** - 在 {{site.data.keyword.cloud_notm}} 中创建服务实例，将 SDK 添加到项目，将两行代码粘贴到应用程序中，并准备好收集数十个预定义的度量值。

 - **收集所需数据** - 使用定制事件来检测应用程序，查看用户如何与应用程序交互，跟踪开销动向以及监视应用程序活动。

 - **查看度量值概览** - {{site.data.keyword.mobileanalytics_short}} 控制台提供了现成可用的图表，无需编写查询。

 - **关注对您重要的内容** - 按时间、适配器、应用程序、应用程序版本、操作系统、操作系统版本或设备模型来过滤度量值。

 - **快速发现问题** - 监视崩溃状态。对于重要度量设置警报触发器，并将警报路由到任何 REST 端点。

 - **故障诊断以发现根本原因** - 在应用程序中使用定制日志记录，自动上传日志并在控制台中搜索这些日志。向下钻取崩溃事件，以查看堆栈跟踪。

## 开始之前
{: #prereqs-analytics}

首先，请确保您已准备好以下必备软件：

 - iOS 8.0+ / watchOS 2.0+
 - Xcode 7.3 和 8.0
 - Swift 2.2 - 3.0
 - CocoaPods 或 Carthage

## 步骤 1. 创建 {{site.data.keyword.mobileanalytics_short}} 的实例
{: #create-analytics}

1. 在 {{site.data.keyword.cloud_notm}}“目录”中，单击**移动** > **{{site.data.keyword.mobileanalytics_short}}**。这将打开服务配置屏幕。
2. 为服务实例提供名称或使用预设名称。
3. 单击**创建**。
4. 在导航窗格中，单击**连接**以选择应用程序并将其绑定到服务。如果在创建期间未绑定服务实例，那么可以日后将其绑定到应用程序。

## 步骤 2. 安装 iOS Swift SDK
{: #install-analytics-swift}

服务提供了特定于平台的 SDK 来简化应用程序开发。{{site.data.keyword.cloud_notm}} Mobile Services Swift SDK 可通过 CocoaPods 或 Carthage 来进行安装。有关更多信息，请参阅 [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics)。

您可以使用 {{site.data.keyword.mobileanalytics_full}} SDK 来检测移动应用程序。iOS 和 watchOS 可以使用 Swift SDK。

1. 确保正确设置 Xcode。要了解如何设置 iOS 开发环境，请参阅 [Apple Developer Web 站点 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/support/xcode/){: new_window}。请阅读有关客户端 SDK Swift Analytics 的 [Xcode 要求 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#requirements){: new_window}。

2. 通过在应用程序的项目文件夹中的 `Info.plist` 文件中添加属性来启用位置 API。例如，添加 `Privacy - Location Usage Description`，并为添加位置 API 提供相应的理由，例如“应用程序需要启用位置服务”。

{{site.data.keyword.mobileanalytics_short}} SDK 通过 [CocoaPods ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cocoapods.org/){: new_window} 和 [Carthage ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/Carthage/Carthage#getting-started){: new_window} 进行分发，它们是 Cocoa 项目的依赖关系管理器。CocoaPods 和 Carthage 会自动从存储库下载工件，以使其可供应用程序使用。选择 CocoaPods 或 Carthage：

### CocoaPods
{: #cocoapods-analytics}

1. 按照 GitHub 上的 [{{site.data.keyword.Bluemix_notm}}Mobile Services Swift SDK 指示信息 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#cocoapods){: new_window}，以使用 CocoaPods 安装 `BMSAnalytics`，并将其添加到 Podfile 中。

2. 安装 iOS 客户端 SDK 后，[导入并初始化](sdk.html#initalize-ma-sdk) Analytics 客户端 SDK。   

### Carthage
{: #carthage-analytics}

如果使用的不是 CocoaPods，那么可以使用 [Carthage ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos){: new_window} 将框架添加到项目中。

1. 遵循 GitHub 上的 [Carthage 安装指示信息 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#carthage){: new_window} 以安装 `BMSAnalytics`。

2. 安装 iOS 客户端 SDK 之后，导入并初始化 Analytics 客户端 SDK。

## 步骤 3. 初始化 SDK
{: #initialize-analytics}

通过使用 {{site.data.keyword.mobileanalytics_short}}，可以收集以下类别的数据，每种类别的数据需要不同程度的检测：

**预定义数据**：此类别包含适用于所有应用程序的通用使用情况和设备信息。这类信息指示应用程序使用的卷、频率或持续时间。在应用程序中初始化 {{site.data.keyword.mobileanalytics_short}} SDK 后，会自动收集预定义数据。

**应用程序日志消息**：此类别支持开发者在整个应用程序中添加代码行来记录定制消息，从而更好地进行开发和调试。开发者可以为每条日志消息分配严重性/详细程度级别。

**定制事件**：此类别包含您自己定义的数据以及特定于应用程序的数据。这些数据显示的是您应用程序中发生的事件，如查看页面、点击按钮或应用程序内采购。除了在您的应用程序中初始化 {{site.data.keyword.mobileanalytics_short}} SDK 之外，您还必须对要跟踪的每一个定制事件，添加一行代码。

## 步骤 4. 识别服务凭证 API 密钥值
{: #analytics-clientkey}

在设置客户端 SDK 之前，先识别您的 **API 密钥**值。初始化客户端 SDK 需要 API 密钥。

1. 打开 {{site.data.keyword.mobileanalytics_short}} 服务仪表板。
2. 展开**查看凭证**以显示 API 密钥值。初始化 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK 时，需要 API 密钥值。

## 步骤 5. 初始化应用程序以收集分析
{: #initalize-ma-sdk}

初始化应用程序以启用发送日志到 {{site.data.keyword.mobileanalytics_short}} 服务。

1. 导入客户端 SDK。

    iOS 和 watchOS 可以使用 Swift SDK。通过将以下 `import` 语句添加到 `AppDelegate.swift` 项目文件的开头，导入 `BMSCore` 和 `BMSAnalytics` 框架：
	```swift
	import BMSCore
	import BMSAnalytics
	```
	{: codeblock}  

2. 初始化您应用程序中的 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK。

	首先，通过将初始化代码放入应用程序代表的 `application(_:didFinishLaunchingWithOptions:)` 方法中，或者放入最适用于项目的位置中，对 `BMSClient` 类进行初始化。
	```swift
	BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Make sure that you point to your region
	```
	{: codeblock}

	必须使用 **bluemixRegion** 参数来初始化 `BMSClient`。在初始化程序中，**bluemixRegion** 值指定的是您要使用的 {{site.data.keyword.Bluemix_notm}} 部署。

3. 使用应用程序对象并为其提供应用程序名称，来初始化 Analytics。

	您针对应用程序选择的名称 (`your_app_name_here`) 会在 {{site.data.keyword.mobileanalytics_short}} Console 中显示为应用程序名称。应用程序名称用作过滤器，在仪表板中搜索应用程序日志。在跨平台（例如，iOS）使用相同的应用程序名称时，可以在同一名称下查看该应用程序的所有日志，而不管日志是从哪个平台发送的。

	您还需要 [API 密钥](#analytics-clientkey)值。
	```swift
	Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, collectLocation: true, deviceEvents: .lifecycle, .network)
	```
    {: codeblock}

4. 应用程序现在已初始化并准备就绪，可收集分析。接下来，您可以将分析数据发送到 {{site.data.keyword.mobileanalytics_short}} 服务。		

## 步骤 6. 收集使用情况分析
{: #usage-analytics}

您可以配置 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK，以记录使用情况分析，并将记录的数据发送到 {{site.data.keyword.mobileanalytics_short}} 服务。

使用以下 API 开始记录和发送使用情况分析：
```swift
// 禁用使用情况分析的记录（例如：为了节省磁盘空间）
// 缺省情况下记录已启用

Analytics.isEnabled = false

// 启用使用情况分析的记录

Analytics.isEnabled = true

// 将记录的使用情况分析发送到 Mobile Analytics Service

Analytics.send(completionHandler: { (response: Response?, error: Error?) in
    if let response = response {

	  // Handle Analytics send success here.
    }
    if let error = error {
           // Handle Analytics send failure here.
    }
})
```
{: codeblock}

记录事件的使用情况分析示例：
```swift
// 记录定制分析事件
let eventObject = ["customProperty": "propertyValue"]
Analytics.log(metadata: eventObject)
```
{: codeblock}

## 步骤 7. 使用记录器
{: #analytics-logger}

{{site.data.keyword.mobileanalytics_full}} 客户端 SDK 提供了一种日志记录框架，与您可能熟悉的其他日志框架（如 `java.util.logging` 或 `log4j`）类似。此日志记录框架支持每个数据包多个记录器实例，支持不同的日志级别，支持捕获对应用程序崩溃的堆栈跟踪，等等。

您还可以将记录的数据配置为存储在运行应用程序的设备上，并稍后将设备日志发送到 {{site.data.keyword.mobileanalytics_short}} 服务。

{{site.data.keyword.mobileanalytics_short}} 客户端 SDK 日志记录框架支持以下日志级别，按详细程度从低到高的顺序与其建议的使用准则一起列出：

  * `FATAL` - 用于不可恢复的崩溃或挂起。`FATAL` 级别保留用于记录不可恢复的错误，这些错误对于用户显示为应用程序崩溃
  * `ERROR` - 用于意外异常或意外网络协议错误
  * `WARN` - 用于记录不视为严重错误的使用情况警告，例如使用了不推荐的 API 或网络响应速度慢
  * `INFO` - 用于报告初始化事件以及其他可能重要但不紧急的数据
  * `DEBUG` - 用于报告调试语句，以帮助开发者解决应用程序缺陷

### 日志级别方案
{: #log-level-scenario}

当记录器级别配置为 `FATAL` 时，记录器会捕获未捕获的异常，但并不会捕获导致崩溃事件的任何日志。您可以设置更详细的记录器级别，以确保同时捕获可能导致 `FATAL` 记录器条目的日志，如 `WARN` 和 `ERROR`。

记录器级别设置为 `DEBUG` 时，您还可以获取 Mobile Analytics 客户端 SDK 记录，在您发送日志时该记录也包含在内。

### 样本记录器用法
{: #sample-logger-usage}

**注：**在使用日志记录框架之前，请确保应用程序已配置为使用 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK。

以下代码片段显示了样本记录器用法：
```swift
// 配置记录器。缺省情况下已禁用；

Logger.isLogStorageEnabled = true

// 更改最低日志级别（可选）。缺省值为 LogLevel.debug

Logger.logLevelFilter = LogLevel.info

// 可以创建多个日志实例来组织日志

let logger1 = Logger.logger(name: "feature1Logger")
let logger2 = Logger.logger(name: "feature2Logger")

// 记录不同级别的消息

logger1.debug(message: "debug message for feature 1")

// 未记录 logger1.debug 消息，因为 logLevelFilter 设置为 info

logger2.info(message: "info message for feature 2")

// 将日志发送到 Mobile Analytics Service

Logger.send(completionHandler: { (response: Response?, error: Error?) in
        if let response = response {
        logger.debug(message: "Status code: \(response.statusCode)")
        logger.debug(message: "Response: \(response.responseText)")
    }
    if let error = error {
           logger.error(message: "Error: \(error)")
    }
})
```
{: codeblock}

出于隐私考虑，可以禁用以发布方式构建的应用程序的记录器输出。缺省情况下，记录器类会将日志打印到 Xcode 控制台。在目标的构建设置中，将 `-D RELEASE_BUILD` 标记添加到发布构建配置的**其他 Swift 标记**部分。
{: tip}

## 步骤 8. 日志记录位置数据
{: #location-logging}

移动设备的位置可通过提供的此 API 从应用程序进行记录：
```swift
Analytics.logLocation();
```

通过此 API，应用程序可以在应用程序会话之间将其位置（以纬度和经度形式）发送到服务器。除了对 `location-logging` API 的显式调用外，该 SDK 还会发送每个应用程序会话的设备位置。对于初始应用程序会话上下文，会发送位置，并且用户可切换应用程序会话（即，在应用程序会话之间切换用户）上下文。位置 API 必须已在初始化 SDK 时启用。

要调用 `location-logging` API，请在 SDK 初始化中将 `collectLocation` 参数设置为 `true`：
```swift
Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
		
```

## 步骤 9. 发出网络请求
{: #network-requests}

您可以配置 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK 以[发起网络请求](/docs/mobile/sdk_network_request.html)。确保已经初始化 `BMSClient` 和 `BMSAnalytics`，并已导入客户端 SDK。

## 步骤 10. 报告崩溃分析
{: #report-crash-analytics}

您可以通过将分析和日志信息发送到 {{site.data.keyword.mobileanalytics_short}} 来查看[应用程序崩溃数据](app-monitoring.html#monitor-app-crash)。

`Analytics.send()` 方法会在**崩溃**页面填充**崩溃概述**和**崩溃**表。您可以通过初始化和发送过程来启用图表以进行分析。

`Logger.send()` 方法会填充**故障诊断**页面上的**崩溃摘要**和**崩溃详细信息**表。您必须通过在应用程序代码中添加语句，使应用程序能够存储和发送日志以填充图表：
```swift
Logger.isLogStorageEnabled = true
Logger.logLevelFilter = LogLevel.Fatal // 或更高级别
```
{: codeblock}

查看 iOS [样本记录器用法](sdk.html##sample-logger-usage)。

## 步骤 11. 跟踪活动用户
{: #trackingusers}

如果应用程序可以识别设备上的唯一用户，那么可以通过将活动用户的用户名传递到 {{site.data.keyword.mobileanalytics_short}} 来跟踪有多少用户正在使用该应用程序。

通过使用 `hasUserContext=true` 初始化 {{site.data.keyword.mobileanalytics_short}} 来启用用户跟踪。否则，{{site.data.keyword.mobileanalytics_short}} 仅会从每个设备捕获一个用户。

添加下列代码，以在用户登录时进行跟踪：
```swift
Analytics.userIdentity = "username"
```
{: codeblock}

## 步骤 12. 测试应用程序
{: #appid_testing}

一切都配置正确吗？是时候进行测试了！

1. 打开应用程序。如果是 Web 应用程序，请使用浏览器打开。如果是 iOS 客户端应用程序，请使用 Xcode 仿真器打开。
2. 在移动仿真器或设备上编译并运行应用程序。
3. 使用 GUI 逐步完成登录到应用程序的过程。
4. 转至 {{site.data.keyword.mobileanalytics_short}} 控制台，以查看应用程序的使用情况分析。您还可以通过[设置警报](/docs/services/mobileanalytics/app-monitoring.html#alerts)和[监视应用程序崩溃](/docs/services/mobileanalytics/app-monitoring.html#monitor-app-crash)来监视应用程序。

## 后续操作
{: #next-analytics notoc}

 - 要了解有关该服务的更多信息，请通读[文档](/docs/services/mobileanalytics/index.html#getting-started-tutorial)。

 - 有关使用移动服务和 {{site.data.keyword.Bluemix_notm}} 的简介，请参阅 [IBM Cloud 上的移动应用程序入门](/docs/services/mobile/index.html)。

 - 入门模板工具包是利用 {{site.data.keyword.cloud_notm}} 功能的最快方法之一。请在 [Mobile 开发者仪表板](https://cloud.ibm.com/developer/mobile/dashboard)中查看所有可用的入门模板工具包。下载代码。运行应用程序！

 - 可以使用 [Swagger UI](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/) 来快速查看 REST API 文档。
