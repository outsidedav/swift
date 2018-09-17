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

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

通过 {{site.data.keyword.visualrecognitionfull}} 服务，应用程序可以使用机器学习来快速、准确地对可视内容进行标记、分类和培训。该服务可以帮助您对几乎任何可视内容进行分类，在几分钟内培训自己的定制模型，并检测人脸。

## 工作原理
{: ##how-it-works}

1. 应用程序选择要分析的图像。
2. 应用程序使用 Watson Swift SDK 将图像发送到 {{site.data.keyword.visualrecognitionshort}} 服务。
3. 该服务使用分类分析对图像进行分析，以识别场景、对象、人脸等。
4. 该服务的分析结果由 Watson Swift SDK 返回给应用程序。

## 开始之前
{: ###before-you-begin}

首先，请确保您已准备好以下必备软件：
<ul>
  <li>iOS 8.0+</li>
  <li>Xcode 9.0+</li>
  <li>Swift 3.2+ 或 Swift 4.0+</li>
  <li>Carthage</li>
</ul>

建议使用 [Carthage](https://github.com/Carthage/Carthage) 来管理依赖项，并为应用程序构建 Watson Swift SDK。如果您不熟悉 Carthage，可以使用 [Homebrew](http://brew.sh/) 来安装 Carthage：

```bash
$ brew update
$ brew install carthage
```

## 步骤 1. 创建 Visual Recognition 实例
{: ###create-and-configure-an-instance-of-visual-recognition}

供应 {{site.data.keyword.visualrecognitionshort}} 服务的实例：

1. 在 {{site.data.keyword.cloud_notm}} 目录中，选择 **{{site.data.keyword.visualrecognitionshort}}**。这将打开服务配置屏幕。
2. 为服务实例提供名称或使用预设名称。
3. 如果要将实例绑定到应用程序，请从**连接**菜单中选择应用程序。
4. 选择定价套餐，然后单击**创建**。
5. 选择**凭证**选项卡，以查看服务凭证。这些值用于从应用程序连接到服务。

## 步骤 2. 下载并构建依赖项
{: ###download-and-build-dependencies}

使用您最喜欢的文本编辑器，在项目的根目录（`.xcodeproj` 文件所在的位置）中创建名为 `Cartfile` 的文件。然后，添加一行以将 Watson Swift SDK 指定为依赖项：
```
github "watson-developer-cloud/swift-sdk"
```
{: pre}

对于生产应用程序，可以指定特定的[版本需求](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement)，以避免 Watson Swift SDK 新发行版中的意外更改。

`Cartfile` 已就位，现在可以下载并构建依赖项。使用终端浏览至项目的根目录，然后运行 Carthage：

```bash
carthage update --platform iOS
```
{: pre}

Carthage 会下载 Watson Swift SDK，并在项目的 `Carthage/Build/iOS` 文件夹中构建其框架。

## 步骤 3. 向应用程序添加框架
{: ###add-frameworks-to-your-app}

### 链接 Visual Recognition 步骤：

现在，Watson Swift SDK 框架已由 Carthage 构建，您必须将 Visual Recognition 框架与应用程序相链接。

1. 在 Xcode 中打开应用程序，然后选择项目以打开其设置。
2. 选择应用程序目标，然后打开**常规**选项卡。
3. 向下滚动到“链接的框架和库”部分，然后单击 `+` 图标。
4. 在显示的窗口中，选择**添加其他项...**，然后浏览至 `Carthage/Build/iOS` 目录。选择 **VisualRecognitionV3.framework** 以将其与应用程序相链接。

### 复制 Visual Recognition 步骤：

除了_链接_ Visual Recognition 框架外，还必须将其_复制_到应用程序中，以使其在运行时可访问。Xcode 有多种不同的方法可复制或嵌入框架，但您可以使用 Carthage 脚本来避免特定的 [App Store 提交错误](http://www.openradar.me/radar?id=6409498411401216)。

1. 在 Xcode 中打开应用程序目标的设置后，浏览至**构建阶段**选项卡。
2. 单击 `+` 图标，然后选择**新建运行脚本阶段**。
3. 将以下命令添加到运行脚本阶段：`/usr/local/bin/carthage copy-frameworks`。
4. 将 Visual Recognition 框架添加到**输入文件**列表中：`$(SRCROOT)/Carthage/Build/iOS/VisualRecognitionV3.framework`。

现在，您已准备就绪，可以开始在应用程序中使用 Watson Swift SDK！

## 步骤 4. 分析应用程序中的图像
{: #analyze-images-in-your-app}

1. 在 Xcode 中打开 `ViewController.swift` 文件。

1. 添加用于 Visual Recognition 的 import 语句：
    ```swift
    import VisualRecognitionV3
    ```
    {: pre}

1. 传递 API 密钥和版本（可以使用当天的日期）来初始化 SDK：
    ```swift
    let visualRecognition = VisualRecognition(apiKey: "your-api-key", version: "yyyy-mm-dd")
    ```
    {: pre}

1. 添加以下代码来对图像进行分类：
    ```swift
    let url = "your-image-url"
    let failure = { (error: Error) in print(error) }
    visualRecognition.classify(url: url, failure: failure) { classifiedImages in
        print(classifiedImages)
    }
    ```
    {: pre}

## 使用入门模板工具包
{: #recognition_starterkits}

[入门模板工具包](https://console.bluemix.net/developer/appledevelopment/starter-kits)是利用 {{site.data.keyword.cloud_notm}} 功能的最快方法之一。您可以通过选择 **Visual Recognition for iOS with Watson** 入门模板工具包来使用 {{site.data.keyword.visualrecognitionshort}} 服务。此服务可对图像进行评估和分类。从移动设备上传新图像或现有图像，然后 Visual Recognition 应用程序会对图像内容快速标记和分类。

要开始使用，请执行以下操作：
1. 选择在[此处](https://console.bluemix.net/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson)找到的入门模板工具包。
2. 使用缺省服务创建项目。
3. 通过单击**下载代码**来下载项目。服务凭证会注入到 `BMSCredentials.plist` 文件中的相应密钥字段中。

## 后续步骤
{: #recognition_next}

太棒了！现在，Visual Recognition 可用于应用程序。请一鼓作气，尝试下列其中一个选项：
* 查看 [GitHub 上的 Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk)。
* 有关更多信息，请参阅 [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/)。

