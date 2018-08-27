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

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

通过 IBM Watson Tone Analyzer 服务，应用程序可以理解文本中的情绪和语气。可以使用该服务来更好地了解用户的对话，或帮助用户了解他们的书面通信是如何被领会的。

## 工作原理
{: ##how-it-works}

1. 应用程序选择要分析的精选文本（例如，文本消息或 Twitter 订阅源）。
2. 应用程序使用 Watson Swift SDK 将文本发送到 {{site.data.keyword.toneanalyzershort}} 服务。
3. 该服务使用语言分析来分析文本，以识别情绪和语气。
4. 该服务的分析结果通过 Watson Swift SDK 返回给应用程序。

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
{: codeblock}

## 步骤 1. 创建 Tone Analyzer 实例
{: #create-and-configure-an-instance-of-tone-analyzer}

供应 {{site.data.keyword.toneanalyzershort}} 服务的实例：

1. 在 {{site.data.keyword.cloud_notm}} 目录中，选择 {{site.data.keyword.toneanalyzershort}}。这将打开服务配置屏幕。
2. 为服务实例提供名称或使用预设名称。
3. 如果要将实例绑定到应用程序，请从“连接”菜单中选择应用程序。
4. 选择定价套餐，然后单击**创建**。
5. 选择**凭证**选项卡，以查看服务凭证。这些值用于从应用程序连接到服务。

## 步骤 2. 下载并构建依赖项
{: ###download-and-build-dependencies}

使用您最喜欢的文本编辑器，在项目的根目录（`.xcodeproj` 文件所在的位置）中创建名为 `Cartfile` 的新文件。然后，添加一行以将 Watson Swift SDK 指定为依赖项：

  ```
github "watson-developer-cloud/swift-sdk"
```
  {: codeblock}

对于生产应用程序，可指定特定的[版本需求](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement)，以避免 Watson Swift SDK 新发行版中的意外更改。

`Cartfile` 已就位，现在可以下载并构建依赖项。使用终端浏览至项目的根目录，然后运行 Carthage：
  
  ```bash
  $ carthage update --platform iOS
  ```
  {: codeblock}

Carthage 会下载 Watson Swift SDK，并在项目的 `Carthage/Build/iOS` 文件夹中构建其框架。

## 步骤 3. 向应用程序添加框架
{: ###add-frameworks-to-your-app}

### 链接 Tone Analyzer 的步骤

现在，Watson Swift SDK 框架已由 Carthage 构建，您必须将 Tone Analyzer 框架与应用程序相链接。

1. 在 Xcode 中打开应用程序，然后选择项目以打开其设置。
2. 选择应用程序目标，然后打开**常规**选项卡。
3. 向下滚动到“链接的框架和库”部分，然后单击 `+` 图标。
4. 在显示的窗口中，选择**添加其他项...**，然后浏览至 `Carthage/Build/iOS` 目录。选择 **ToneAnalyzerV3.framework** 以将其与应用程序相链接。

### 复制 Tone Analyzer 的步骤

除了_链接_ Tone Analyzer 框架外，还必须将其_复制_到应用程序中，以使其在运行时可访问。Xcode 具有若干种不同的方法可复制或嵌入框架，但您可以使用 Carthage 脚本来避免特定的 [App Store 提交错误](http://www.openradar.me/radar?id=6409498411401216)。

1. 在 Xcode 中打开应用程序目标的设置后，浏览至**构建阶段**选项卡。
2. 单击 `+` 图标，然后选择**新建运行脚本阶段**。
3. 将以下命令添加到运行脚本阶段：`/usr/local/bin/carthage copy-frameworks`。
4. 将 Tone Analyzer 框架添加到“输入文件”列表中：`$(SRCROOT)/Carthage/Build/iOS/ToneAnalyzerV3.framework`。

现在，您已准备就绪，可以开始在应用程序中使用 Watson Swift SDK！

## 步骤 4. 分析应用程序中的文本
{: #analyze-text-in-your-app}

1. 在 Xcode 中打开 `ViewController.swift` 文件。
2. 添加用于 Tone Analyzer 的 import 语句：
    ```swift
    import ToneAnalyzerV3
    ```
    {: codeblock}

3. 创建名为 `toneAnalyzerExample` 的空函数，然后通过 `viewDidLoad` 调用该函数。
4. 将以下代码添加到 `toneAnalyzerExample` 函数。确保更新服务的用户名和密码。
  ```swift
  import UIKit
  import ToneAnalyzerV3

  class ViewController: UIViewController {

      override func viewDidLoad() {
          super.viewDidLoad()
          toneAnalyzerExample()
      }

      func toneAnalyzerExample() {

          // 实例化服务
          let toneAnalyzer = ToneAnalyzer(
              username: "your-username-here",
              password: "your-password-here",
              version: "yyyy-mm-dd"
          )

          // 要分析的文本
          let review = """
              I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
          """

          // 分析文本
          let toneInput = ToneInput(text: review)
          let failure = { (error: Error) in print(error) }
          toneAnalyzer.tone(toneInput: toneInput, contentType: "application/json", failure: failure) { analysis in
              for tone in analysis.documentTone.tones! {
                  print("\(tone.toneName): \(tone.score)")
              }
          }
      }
  }
  ```
  {: codeblock}

运行应用程序时，在控制台中会看到以下分析：
```
Sadness: 0.575803
Tentative: 0.867377
```
{: screen}

## 使用入门模板工具包
{: #tone_starterkits}

[入门模板工具包](https://console.bluemix.net/developer/appledevelopment/starter-kits)是最快利用 {{site.data.keyword.cloud_notm}} 功能的其中一种方法。您可以通过选择 **Tone Analyzer for iOS with Watson** 入门模板工具包来使用 {{site.data.keyword.toneanalyzershort}} 服务。此服务利用深度学习功能来评估文本的段落。Tone Analyzer 应用程序可识别说话人的语气（高兴、悲伤、自信等），因为会与多个类别相关。

要开始使用此入门模板工具包，请执行以下操作：

1. 选择在[此处](https://console.bluemix.net/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson)找到的入门模板工具包。
2. 使用缺省服务创建项目。
3. 通过单击**下载代码**来下载项目。服务凭证会注入到 `BMSCredentials.plist` 文件中的相应密钥字段中。

## 后续步骤
{: #tone_next}

太棒了！现在，{{site.data.keyword.toneanalyzershort}} 已添加到应用程序。请一鼓作气，尝试下列其中一个选项：

* 查看 [GitHub 上的 Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk)。
* 有关更多信息，请参阅 [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/)。

