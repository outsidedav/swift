---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: swift visual recognition, train swift, cocoapods swift, swift sdk install, starter kit watson swift, image swift classify, machine learning swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

通过 {{site.data.keyword.visualrecognitionfull}} 服务，应用程序可以使用机器学习来快速、准确地对可视内容进行标记、分类和训练。该服务可以帮助您对几乎任何可视内容进行分类，在几分钟内训练自己的定制模型，并检测人脸。

## 工作原理
{: #how-it-works-recognition}

1. 应用程序选择要分析的图像。
2. 应用程序使用 Watson Swift SDK 将图像发送到 {{site.data.keyword.visualrecognitionshort}} 服务。
3. 该服务使用分类分析对图像进行分析，以识别场景、对象、人脸等。
4. 该服务的分析结果由 Watson Swift SDK 返回给应用程序。

## 开始之前
{: #prereqs-recognition}

确保满足以下先决条件：

* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods、Carthage 或 Swift Package Manager

您可以使用 [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，[Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 或 [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 安装 [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。通过使用 CocoaPods(https://cocoapods.org/) 管理依赖关系，您仅获得所需框架，而不是整个 Watson Swift SDK。如果您未使用过 CocoaPods，您可以轻松进行安装：
```console
sudo gem install cocoapods
```
{: codeblock}

## 步骤 1. 创建 Visual Recognition 实例
{: #create-instance-recognition}

供应 {{site.data.keyword.visualrecognitionshort}} 服务的实例：

1. 在 {{site.data.keyword.cloud_notm}} 目录中，选择 **{{site.data.keyword.visualrecognitionshort}}**。这将打开服务配置屏幕。
2. 为服务实例提供名称或使用预设名称。
3. 如果要将实例绑定到应用程序，请从**连接**菜单中选择应用程序。
4. 选择价格套餐，然后单击**创建**。
5. 选择**凭证**选项卡，以查看服务凭证。这些值用于从应用程序连接到服务。

## 步骤 2. 下载并构建依赖项
{: #download-depend-recognition}

使用您最喜欢的文本编辑器，通过运行 `pod init`，在项目的根目录（`.xcodeproj` 文件所在的位置）中创建新的 `Podfile`。然后，添加行以将 Watson Swift SDK 的 {{site.data.keyword.visualrecognitionshort}} 框架指定为依赖项：

```pod
use_frameworks!target 'MyApp' do
    pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

对于生产应用程序，您可能还要指定特定[版本需求](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，以避免 Watson Swift SDK 新发行版中的意外更改。

`Podfile` 已就位，现在可以下载依赖项。使用终端导航至项目的根目录，然后运行 CocoaPods：

```console
pod install
```
{: codeblock}

Cocoapods 下载 {{site.data.keyword.visualrecognitionshort}} 框架，并在项目的 `Pods/` 文件夹中进行构建。

要防止 Pod 构建失败，在 Xcode 中打开项目时，请打开以 `.xcworkspace` 而不是 `.xcodeproj` 结尾的文件。
{: tip}

## 步骤 3. 分析应用程序中的图像
{: #analyze-images-recognition}

以下样本帮助您将 {{site.data.keyword.visualrecognitionshort}} 功能添加到应用程序，通常在 `ViewController.swift` 中。使用以下示例，您可以扩展用例的 Visual Recognition 调用。

1. 添加用于 Visual Recognition 的 import 语句：
    
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. 传递 API 密钥和版本（可以使用当天的日期）来初始化 SDK：
    
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  您可以查看[版本参数文档 ](https://{DomainName}/apidocs/visual-recognition#versioning){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 或者使用创建 {site.data.keyword.conversationshort}} 服务的日期。
  {: tip}

3. 添加以下代码来对图像进行分类：
    
  ```swift
  let url = "your-image-url"
  visualRecognition.classify(url: url) { response, error in
      if let error = error {
          print(error)
    }

    guard let classifiedImages = response?.result else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

Visual Recognition 框架支持多个分类方法。查看 Watson SDK [Visual Recognition 文档 ](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 以找到最适合应用程序的方法。
{: tip}

## 使用入门模板工具包
{: #recognition_starterkits}

[入门模板工具包](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 是使用 {{site.data.keyword.cloud_notm}} 功能的最快方法之一。您可以通过选择 **Visual Recognition for iOS with Watson** 入门模板工具包来使用 {{site.data.keyword.visualrecognitionshort}} 服务。此服务可对图像进行评估和分类。从移动设备上传新图像或现有图像，然后 Visual Recognition 应用程序会对图像内容快速标记和分类。

要开始使用，请执行以下操作：
1. 选择在[此处](https://{DomainName}/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 找到的入门模板工具包。
2. 使用缺省服务创建项目。
3. 通过单击**下载代码**来下载项目。服务凭证会注入到 `BMSCredentials.plist` 文件中的相应密钥字段中。

## 后续步骤
{: #recognition_next notoc}

太棒了！现在，Visual Recognition 可用于应用程序。请一鼓作气尝试下列其中一个选项：
* 查看 [Watson Swift SDK ](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，并且探索其他受支持的 Watson 服务。
* 有关更多信息，请参阅 [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。
