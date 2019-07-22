---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: watson swift, tone anaylzer swift, cocoapods swift, swift sdk install, starter kit watson

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

通过 {{site.data.keyword.ibmwatson}} {{site.data.keyword.toneanalyzershort}} 服务，应用程序可以理解文本中的情绪和语气。可以使用该服务来更好地了解用户的对话，或帮助用户了解他们的书面通信是如何被领会的。

## 工作原理
{: #how-it-works-tone}

1. 应用程序选择要分析的一段文本（例如，文本消息或 Twitter 订阅源）。
2. 应用程序使用 Watson Swift SDK 将文本发送到 {{site.data.keyword.toneanalyzershort}} 服务。
3. 该服务使用语言分析来分析文本，以识别情绪和语气。
4. 该服务的分析结果通过 [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk) 返回给应用程序。

## 开始之前
{: #prereqs-tone}

确保满足以下先决条件：

* OS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods、Carthage 或 Swift Package Manager

您可以使用 [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，[Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 或 [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 安装 [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。通过使用 CocoaPods 管理依赖关系，您仅获得所需框架，而不是整个 Watson Swift SDK。如果您未使用过 CocoaPods，您可以轻松进行安装：

```bash
$ sudo gem install cocoapods
```
{: codeblock}

## 步骤 1. 创建 Tone Analyzer 实例
{: #create-instance-tone}

供应 {{site.data.keyword.toneanalyzershort}} 服务的实例：

1. 在 {{site.data.keyword.cloud_notm}} 目录中，选择 {{site.data.keyword.toneanalyzershort}}。这将打开服务配置屏幕。
2. 为服务实例提供名称或使用预设名称。
3. 如果要将实例绑定到应用程序，请从“连接”菜单中选择应用程序。
4. 选择价格套餐，然后单击**创建**。
5. 选择**凭证**选项卡，以查看服务凭证。这些值用于从应用程序连接到服务。

## 步骤 2. 下载并构建依赖项
{: #download-depend-tone}

使用您最喜欢的文本编辑器，通过运行 `pod init`，在项目的根目录（`.xcodeproj` 文件所在的位置）中创建新的 `Podfile`。然后，添加行以将 Watson Swift SDK 的
{{site.data.keyword.conversationshort}} 框架指定为依赖项：

```pod
use_frameworks!target 'MyApp' do
    pod 'IBMWatsonToneAnalyzerV3'
```
{: codeblock}

对于生产应用程序，您可能还要指定特定[版本需求](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，以避免 Watson Swift SDK 新发行版中的意外更改。

`Podfile` 已就位，现在可以下载依赖项。使用终端导航至项目的根目录，然后运行 CocoaPods：

```console
pod install
```
{: codeblock}

Cocoapods 下载 {{site.data.keyword.toneanalyzershort}} 框架，在项目的 `Pods/` 文件夹中进行构建。

要防止 Pod 构建失败，在 Xcode 中打开项目时，请打开以 `.xcworkspace` 而不是 `.xcodeproj` 结尾的文件。
{: tip}

## 步骤 3. 分析应用程序中的文本
{: #analyze-text-tone}

以下示例协助您将 {{site.data.keyword.toneanalyzershort}} 功能添加到应用程序，通常在 `ViewController.swift` 中。使用以下示例，您可以扩展用例的 Tone Analyzer 调用。

1. 添加用于 Tone Analyzer 的 import 语句：
    
  ```swift
  import ToneAnalyzer
  ```
  {: codeblock}

2. 初始化 Tone Analyzer 服务：
  ```swift
  let toneAnalyzer = ToneAnalyzer(version: "yyyy-mm-dd", apiKey: "your-api-key-here")
  ```
  {: codeblock}

  查看[版本参数文档](https://{DomainName}/apidocs/tone-analyzer#versioning){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 或者使用创建 {site.data.keyword.conversationshort}} 服务的日期。
  {: tip}

3. 提供文本进行分析，并处理结果：
  ```swift
  // Text to analyze
  let review = """
      I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
          """

  // Analyze text
  let input = ToneInput(text: review)
  toneAnalyzer.tone(toneInput: .toneInput(input)) { response, error in
      if let error = error {
          print(error)
          return
      }

      guard let tones = response?.result?.documentTone.tones else {
          print("Failed to analyze the tone input")
          return
      }

      for tone in tones {
          print("\(tone.toneName): \(tone.score)")
              }
          }
  ```
  {: codeblock}

  运行代码时，在控制台中会看到以下分析：
  ```
Sadness: 0.575803
Tentative: 0.867377
```
  {: screen}

4. 探索 Watson SDK [Tone Analyzer 文档](https://watson-developer-cloud.github.io/swift-sdk/services/ToneAnalyzerV3/index.html){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 以构建您的应用程序的功能。

## 使用入门模板工具包
{: #tone_starterkits}

[入门模板工具包](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 是使用 {{site.data.keyword.cloud_notm}} 功能的最快方法之一。您可以通过选择 **Tone Analyzer for iOS with Watson** 入门模板工具包来使用 {{site.data.keyword.toneanalyzershort}} 服务。此服务利用深度学习功能来评估文本的段落。Tone Analyzer 应用程序可识别与多个类别相关的说话人语气（高兴、悲伤、自信等）。

要开始使用此入门模板工具包，请执行以下操作：

1. 选择在[此处](https://{DomainName}/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 找到的入门模板工具包。
2. 使用缺省服务创建项目。
3. 通过单击**下载代码**来下载项目。服务凭证会注入到 `BMSCredentials.plist` 文件中的相应密钥字段中。

## 后续步骤
{: #tone_next notoc}

太棒了！{{site.data.keyword.toneanalyzershort}} 已添加到应用程序。请一鼓作气尝试下列其中一个选项：

* 查看 [GitHub 上的 Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，并且探索其他受支持的 Watson 服务。
* 有关更多信息，请参阅 [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。
