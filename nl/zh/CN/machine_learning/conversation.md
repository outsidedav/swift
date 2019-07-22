---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: chatbot swift, virtual assistant swift, assistant swift, watson swift starter, assistantv2 swift, watson sdk swift, add chatbot swift, add assistant swift

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 添加聊天机器人
{: #assistant}

可以使用 {{site.data.keyword.conversationshort}} 服务来构建可理解自然语言输入的应用程序，并使用类似真人的对话来响应用户。

以下列表概述了该集成的工作流程：

1. 用户与应用程序中实现的界面进行交互。
2. 应用程序使用 {{site.data.keyword.watson}} Swift SDK 将用户输入发送到 {{site.data.keyword.conversationshort}}。
3. {{site.data.keyword.watson}} Swift SDK 连接到工作空间，此工作空间是用于对话流和训练数据的容器。
4. 工作空间解读用户输入并定向对话流，以向应用程序发送响应。
5. 应用程序为用户显示响应。

## 开始之前
{: #prereqs-chatbot}

确保满足以下先决条件：

* [{{site.data.keyword.conversationshort}} 服务](/docs/services/assistant?topic=assistant-getting-started#getting-started)的实例
* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods、Carthage 或 Swift Package Manager

您可以使用 [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，[Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 或 [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 安装 [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。通过使用 CocoaPods 管理依赖关系，您仅获得所需框架，而不是整个 Watson Swift SDK。如果您未使用过 CocoaPods，您可以轻松进行安装：

```console
sudo gem install cocoapods
```
{: codeblock}

## 步骤 1. 创建 Watson Assistant 的实例 
{: #instance-watson-chatbot}

供应 {{site.data.keyword.conversationshort}} 服务的实例：

1. 在 {{site.data.keyword.cloud_notm}} 目录中，选择 **{{site.data.keyword.conversationshort}}**。这将打开服务配置屏幕。
2. 为服务实例提供名称或使用预设名称。
3. 如果要将实例绑定到应用程序，请从**连接**菜单中选择应用程序。
4. 选择价格套餐，然后单击**创建**。
5. 选择**凭证**选项卡，以查看服务凭证。这些值用于从应用程序连接到服务。
6. 单击**启动工具**以构建聊天机器人助手。遵循登录页面上的指示信息来构建您自己的聊天机器人，或者转至**技能**选项卡，并选择**新建**。在**添加对话技能**页面上，选择**使用技能样本**选项卡，并选择**客户关怀技能样本**以快速启动。您可以继续优化此技能，或者创建其他技能供应用程序以后使用。
7. 单击新技能上的菜单，并选择**查看 API 详细信息**。除了服务凭证外，您现在可以看到您需要的`工作空间标识`。

## 步骤 2. 下载并构建依赖项
{: #download-depend-chatbot}

以下示例使用 AssistantV1。有关 AssistantV2 框架的更多信息，请参阅 [Watson SDK AssistantV2 文档](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV2/index.html){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。

使用您最喜欢的文本编辑器，通过运行 `pod init`，在项目的根目录（`.xcodeproj` 文件所在的位置）中创建新的 `Podfile`。然后，添加行以将 Watson Swift SDK 的 {{site.data.keyword.conversationshort}} 框架指定为依赖项：

```pod
use_frameworks!target 'MyApp' do
    pod 'IBMWatsonAssistantV1'
```
{: codeblock}

对于生产应用程序，您可能还要指定特定[版本需求](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，以避免 Watson Swift SDK 新发行版中的意外更改。

`Podfile` 已就位，现在可以下载依赖项。使用终端导航至项目的根目录，然后运行 CocoaPods：

```console
pod install
```
{: codeblock}

Cocoapods 下载 {{site.data.keyword.conversationshort}} 框架，在项目的 `Pods/` 文件夹中进行构建。

要防止 Pod 构建失败，在 Xcode 中打开项目时，请打开以 `.xcworkspace` 而不是 `.xcodeproj` 结尾的文件。
{: tip}

## 步骤 3. 向应用程序添加虚拟助手
{: #virtual-assist-chatbot}

以下示例协助您将 {{site.data.keyword.conversationshort}} 功能添加到应用程序，通常在 `ViewController.swift` 中。使用以下示例，您可以扩展用例的 Assistant 调用。

1. 添加用于 {{site.data.keyword.conversationshort}} 的 import 语句，例如 `import Assistant`。
  ```swift
  import Assistant
  ```
  {: codeblock}

2. 初始化 {{site.data.keyword.conversationshort}} 服务：
  ```swift
  let assistant = Assistant(version: "yyyy-mm-dd", apikey: "your-api-key-here")

  // save context to state to continue the conversation later
  var context: Context?
  ```
  {: codeblock}

  **提示**：此示例将上下文保存到状态。为了更好地了解此对象，以及如何修改对象以适应您的用例，请参阅[上下文变量文档](/docs/services/assistant?topic=assistant-dialog-runtime#context-variables)。查看[版本参数文档](https://{DomainName}/apidocs/assistant#versioning){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 或者使用创建 {site.data.keyword.conversationshort}} 服务的日期。

3. 初始化对话。根据助手的配置方式，可以向用户提供初始响应：
  ```swift
  // Start a conversation
  assistant.message(workspaceID: "your-workspace-ID-here) { response, error in
      if let error = error {
         print(error)
    }

    guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Set the context to state
      self.context = message.context
  }
  ```
  {: codeblock}

4. 将消息和上下文发送到助手：
  ```swift
  print("Request: When are you open?")
  let input = InputData(text: "When are you open?")

  assistant.message(workspaceID: workspace, input: input, context: self.context) { response, error in
      if let error = error {
         print(error)
    }

    guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Update the context
      self.context = message.context
  }
  ```
  {: codeblock}

使用缺省助手的这些示例运行应用程序时，在控制台中会看到以下消息：
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hello, I’m a demo customer care virtual assistant to show you the basics.  I can help with directions to my store, hours of operation and booking an in-store appointment.
Request: When are you open?
Response: Our hours are Monday to Friday 10am to 8pm and Friday and Saturday 11am to 6pm.
```
{: screen}

5. 探索 Watson SDK [Assistant 文档](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV1/index.html){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 以构建您的应用程序的功能。

## 使用入门模板工具包
{: #starterkits-chatbot}

通过入门模板工具包，可以快速、轻松地使用 {{site.data.keyword.cloud_notm}} 的功能。可以使用入门模板工具包将 {{site.data.keyword.conversationshort}} 添加到任何服务器端后端。Chatbot for iOS with Watson 入门模板工具包说明了如何通过将自动与用户进行交互的自然语言界面添加到应用程序来使用 {{site.data.keyword.conversationshort}} 的深度学习功能。

1. 选择要使用的[入门模板工具包](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。
2. 使用缺省服务创建应用程序。
3. 单击**添加服务 > Watson > {{site.data.keyword.conversationshort}}**。
4. 通过单击**下载代码**来下载项目。可以在 `config/local-dev.json` 文件中找到服务凭证。

## 后续步骤
{: #next-chatbot notoc}

太棒了！您已将 AI 助手添加到应用程序。请一鼓作气尝试下列其中一个选项：

* 查看 [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，并探索其他受支持的 Watson 服务。
* 利用 [{{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-index#index) 提供的所有功能。
* 查看 [Simple Chat 样本应用程序](https://github.com/watson-developer-cloud/simple-chat-swift){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 的源代码，并在 GitHub 上演示 {{site.data.keyword.watson}} Swift SDK。
