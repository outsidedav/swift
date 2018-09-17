---

copyright:
  years: 2018
lastupdated: "2018-08-07"

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
  3. {{site.data.keyword.watson}} Swift SDK 连接到工作空间，此工作空间是用于对话流和培训数据的容器。
  4. 工作空间解读用户输入并定向对话流，以向应用程序发送响应。
  5. 应用程序为用户显示响应。

## 开始之前
{: #before-you-begin}

确保满足以下先决条件：

  * [{{site.data.keyword.conversationshort}} 服务](/docs/services/conversation/getting-started.html)的实例
  * iOS 8.0+
  * Xcode 9.0+
  * Swift 3.2+ 或 Swift 4.0+
  * Carthage

使用 [Carthage](https://github.com/Carthage/Carthage){:new_window} 来管理依赖项，并为应用程序构建 {{site.data.keyword.watson}} Swift SDK。如果您不熟悉 Carthage，可以使用 [Homebrew](http://brew.sh/){:new_window} 来安装 Carthage：

```bash
$ brew update
$ brew install carthage
```

## 下载并构建依赖项
{: #download-and-build-dependencies}

1. 使用您最喜欢的文本编辑器，在项目的根目录（`.xcodeproj` 文件所在的位置）中创建名为 `Cartfile` 的新文件。

2. 添加一行以将 Watson Swift SDK 指定为依赖项：
  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

  对于生产应用程序，可指定[版本需求](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement){:new_window}，以避免 {{site.data.keyword.watson}} Swift SDK 新发行版中的意外更改。
  {: tip}

3. 使用终端浏览至项目的根目录，然后运行 Carthage：
  ```bash
  carthage update --platform iOS
  ```
  {: codeblock}

  随后会下载 {{site.data.keyword.watson}} Swift SDK，并在项目的 `Carthage/Build/iOS` 文件夹中构建其框架。

## 向应用程序添加框架
{: #add-frameworks-to-your-app}

现在，{{site.data.keyword.watson}} Swift SDK 框架已构建，您必须将 {{site.data.keyword.conversationshort}} 框架链接并复制到应用程序中。

1. 在 Xcode 中打开应用程序，然后在导航器中选择项目以打开其设置。
2. 选择应用程序目标，然后打开**常规**选项卡。
3. 滚动到“链接的框架和库”部分，然后单击 `+` 图标。
4. 在显示的新窗口中，选择**添加其他项**，然后浏览至 `Carthage/Build/iOS` 目录。
5. 选择 `AssistantV1.framework` 以将其与应用程序相链接。

除了链接 {{site.data.keyword.conversationshort}} 框架外，还必须将其复制到应用程序中，以使其在运行时可访问。然后，使用 Carthage 脚本来避免特定 [App Store 提交错误](http://www.openradar.me/radar?id=6409498411401216){:new_window}。

1. 在 Xcode 中打开应用程序目标的设置后，浏览至**构建阶段**选项卡。
2. 单击 `+` 图标，然后选择**新建运行脚本阶段**。
3. 将 `/usr/local/bin/carthage copy-frameworks` 命令添加到运行脚本阶段。
4. 将 {{site.data.keyword.conversationshort}} 框架添加到**输入文件**列表中：
  ```
  $(SRCROOT)/Carthage/Build/iOS/AssistantV1.framework
  ```
  {: codeblock}

## 向应用程序添加虚拟助手
{: #add-a-virtual-assistant-to-your-app}

1. 在 Xcode 中打开 `ViewController.swift`。
2. 添加用于 {{site.data.keyword.conversationshort}} 的 import 语句，例如 `import AssistantV1`。
3. 创建名为 `assistantExample` 的空函数，然后通过 `viewDidLoad` 调用该函数。
4. 为 `assistantExample` 函数添加以下代码。确保更新用户名、密码和工作空间标识。

```swift
//
//  ViewController.swift
//  WatsonAssistantExample
//
//  Created by Glenn R. Fisher on 3/1/18.
//  Copyright © 2018 Glenn R. Fisher. All rights reserved.
//

import UIKit
import AssistantV1

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        assistantExample()
    }

    func assistantExample() {
        // Assistant 凭证
        let username = "your-username-here"
        let password = "your-password-here"
        let workspace = "your-workspace-id-here"

        // 实例化服务
        let assistant = Assistant(username: username, password: password, version: "2018-03-01")

        // 开始对话
        assistant.message(workspaceID: workspace) { response in
            print("Conversation ID: \(response.context.conversationID!)")
            print("Response: \(response.output.text.joined())")

            // 继续执行 assistant
            print("Request: turn the radio on")
            let input = InputData(text: "turn the radio on")
            let request = MessageRequest(input: input, context: response.context)
            assistant.message(workspaceID: workspace, request: request) { response in
                print("Response: \(response.output.text.joined())")
            }
        }
    }
}
```
{: codeblock}

运行应用程序时，在控制台中会看到以下消息：
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hi. It looks like a nice drive today. What would you like me to do?
Request: turn the radio on
Response: Sure thing! Which genre would you prefer? Jazz is my personal favorite..
```
{: screen}

## 使用入门模板工具包
{: #conversation_starterkits}

通过入门模板工具包，可以快速、轻松地利用 {{site.data.keyword.cloud_notm}} 的功能。可以使用入门模板工具包将 {{site.data.keyword.conversationshort}} 添加到任何服务器端后端。Chatbot for iOS with Watson 入门模板工具包说明了如何通过将自然语言界面添加到应用程序来使用 {{site.data.keyword.conversationshort}} 的深度学习功能，以自动与最终用户进行交互。

1. 选择要使用的[入门模板工具包](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window}。
2. 使用缺省服务创建项目。
3. 单击**添加资源 > Watson > {{site.data.keyword.conversationshort}}**。
4. 通过单击**下载代码**来下载项目。可以在 `config/local-dev.json` 文件中找到服务凭证。

## 后续步骤
{: #assistant_next}

太棒了！您已将 AI 助手添加到应用程序。请一鼓作气，尝试下列其中一个选项：

* 查看 [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window}。
* 利用 [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) 可以提供的所有功能。
* 查看 [Simple Chat 样本应用程序](https://github.com/watson-developer-cloud/simple-chat-swift){:new_window}的源代码，并在 GitHub 上演示 {{site.data.keyword.watson}} Swift SDK。
