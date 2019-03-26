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

# 发送 {{site.data.keyword.mobilepushshort}}
{: #push_notifications}

通过使用 {{site.data.keyword.cloud}} 上的 {{site.data.keyword.mobilepushshort}} 服务向移动设备和 Web 应用程序发送实时通知，从而增强 Swift 应用程序。

 - 可以将通知传递给所有应用程序用户，也可以传递给一组所选用户或设备。
 - 支持交互式通知和静默通知。
 - 客户可以选择预订通知的特定标记或主题。
 - 支持应用程序所有者分析注册以接收通知的设备数和发送的通知数。

可以选择将 {{site.data.keyword.mobilepushshort}} 服务用作 MobileFirst Services 入门样本或作为 {{site.data.keyword.cloud_notm}} [Dedicated 服务](/docs/dedicated/index.html)的一部分。您还可以使用 SDK（软件开发包）和 [REST API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobile.{DomainName}/imfpush/){: new_window} 来进一步开发您的客户端应用程序。

![Push 概览图](images/push_notification_lifecycle.jpg) 图 1. {{site.data.keyword.mobilepushshort}} 服务生命周期概览图

## 开始之前
{: #prereqs-push}

首先，请确保您已准备好以下必备软件：

 - iOS 8.0+
 - Xcode 7.3 和 8.0
 - Swift 2.3 - 4.0
 - CocoaPods 或 Carthage

## 步骤 1. 创建 {{site.data.keyword.mobilepushshort}} 的实例
{: #push_create}

1. 在 {{site.data.keyword.cloud_notm}}“目录”中，单击**移动** > **{{site.data.keyword.mobilepushshort}}**。这将打开服务配置屏幕。
2. 为服务实例提供名称或使用预设名称。
3. 单击**创建**。
4. 在导航窗格中，单击**连接**以选择应用程序并将其绑定到服务。如果在创建期间未绑定服务实例，那么可以日后将其绑定到应用程序。


## 步骤 2. 获取通知提供程序凭证
{: #get_creds-push}

要设置 Push Notifications 服务，您需要从 Apple 推送通知服务 (APNs) 获取必需的凭证。执行此处的步骤来[获取并配置 APNs 凭证 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/mobilepush/push_step_1.html#push_step_1_ios){: new_window}。


## 步骤 3. 配置服务实例
{: #config-resource-push}

要使用 {{site.data.keyword.mobilepushshort}} 服务来发送通知，请上传您创建的 `.p12` 密钥库，其中包含构建和发布应用程序所需的专用密钥和 SSL 证书。此外，也可以使用 REST API 来上传 APNs 证书。

在 ` .cer` 文件位于密钥链访问中之后，请将其导出到计算机以创建 `.p12` 证书。

有关使用 APNs 的更多信息，请参阅 [iOS Developer Library: Local and Push Notification Programming Guide ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1){: new_window}。

要在 Push Notifications 服务控制台上设置 APNs，请完成以下步骤：

1. 在 {{site.data.keyword.mobilepushshort}} 服务控制台上，选择**配置**。
2. 选择**移动**选项，以更新 **APNs 推送凭证**表单上的信息。
3. 选择以下任一选项：
	- 对于**移动**选项
		1. 选择**沙箱**（开发）或**生产**（分发），然后上传您创建的 `p.12` 证书。
		  ![设置 {{site.data.keyword.mobilepushshort}} 控制台](images/wizard.jpg)

		2. 在**密码**字段中，输入与 `.p12` 证书文件相关联的密码，然后单击**保存**。
	- 对于 **Web** 选项
		- 在“Safari 推送”部分中，使用必要信息更新表单。
		- **Web 站点名称**：通知中心内提供的 Web 站点名称。
		- **Web 站点推送标识**：使用 Web 站点推送标识的反向域字符串进行更新。例如，web.com.acmebanks.www。
		- **Web 站点 URL**：提供您预订推送通知的 Web 站点的 URL。例如，https://www.acmebanks.com。
		- **允许的域**：（可选参数）向用户请求许可权的 Web 站点的列表。请确保 URL 是以逗号分隔的值。如果未提供信息，将使用“Web 站点 URL”中的值。
		- **URL 格式字符串**：单击通知时解析的 URL。例如，["https://www.acmebanks.com"]。确保 URL 使用 HTTP 或 HTTPS 模式。
		-**Safari Web 推送证书**：上传 `.p12` 证书并提供密码。
4. 单击**保存**。
	![{{site.data.keyword.mobilepushshort}} 控制台](images/push_configure_safari.jpg)

## 步骤 4. 设置服务客户端 SDK
{: #service-client-push}

要使 iOS 应用程序能够在设备上接收推送通知，您需要为 {{site.data.keyword.mobilepushshort}} 服务配置 iOS SDK。

{{site.data.keyword.cloud_notm}} Mobile Services Swift SDK 可通过 CocoaPods 或 Carthage 来进行安装。有关更多信息，请参阅 [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application)。

## 步骤 5. 发送通知
{: #send-notify-push}

开发应用程序后，可以发送基本推送通知。

要发送基本推送通知，请完成以下步骤：

1. 选择**发送通知**，并通过选择**发送至**选项编辑消息。支持的选项为**设备（按标记）**、**设备标识**、**用户标识**、**iOS 设备**、**Web 通知**和**所有设备**。**注**：选择**所有设备**选项时，所有预订 {{site.data.keyword.mobilepushshort}} 的设备都会收到通知。

	![“通知”屏幕](images/tag_notification.jpg)

2. 在**消息**字段中，编辑您的消息。根据需要，选择配置可选设置。
3. 单击**发送**。
3. 验证设备或浏览器是否已收到通知。

以下截屏显示的是在设备的前台处理推送通知的警报框。
	![Android 上的前台推送通知](images/Android_Screenshot.jpg)

以下截屏显示的是后台推送通知。
	![Android 上的后台推送通知](images/background.png)

### 可选设置
{: #optional-push}

可以定制用于将通知发送至 iOS 设备的 {{site.data.keyword.mobilepushshort}} 设置。支持以下可选的定制选项。

- **角标**：指示在应用程序角标上显示的数字。缺省值为零 (0)，并且不显示角标。
- **声音**：指示在接收通知时播放声音片段。支持缺省值或应用程序中捆绑的声音资源的名称。
- **其他有效内容**：为您的通知指定定制的有效内容值。

您还可以选择启用[交互式通知](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#interactive-notifications)和[富媒体通知](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enabling-rich-media-notifications)。

## 步骤 6. 监视传递的通知
{: #monitor-push}

{{site.data.keyword.mobilepushshort}} 服务提供监视实用程序，以帮助您检查已发送消息的状态。要配置监视实用程序，请参阅[启用 iOS 应用程序的监视](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enable-monitoring)。

## 后续步骤
{: #next-push}

 - 要了解有关服务的更多信息并利用所有功能，请通读[文档](/docs/services/mobilepush/c_overview_push.html#overview-push)。

 - 有关使用移动服务和 {{site.data.keyword.cloud_notm}} 的简介，请参阅 [{{site.data.keyword.cloud_notm}} 上的移动应用程序入门](/docs/services/mobile/index.html)。

 - 入门模板工具包是使用 {{site.data.keyword.cloud_notm}} 功能的最快方法之一。请在 [Mobile 开发者仪表板](https://cloud.ibm.com/developer/mobile/dashboard)中查看可用的入门模板工具包。下载代码。运行应用程序！

 - 可以使用 [Swagger UI](https://mobile.ng.bluemix.net/imfpush/) 来快速查看 REST API 文档。
