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

# 新增聊天機器人
{: #assistant}

您可以使用 {{site.data.keyword.conversationshort}} 服務來建置瞭解自然語言輸入，並使用擬人對話來回應使用者的應用程式。

下列清單概述整合的運作流程：

  1. 使用者與在您應用程式中實作的介面互動。
  2. 您的應用程式使用 {{site.data.keyword.watson}} Swift SDK，將使用者輸入傳送至 {{site.data.keyword.conversationshort}}。
  3. {{site.data.keyword.watson}} Swift SDK 連接至工作區，其為存放對話框流程及訓練資料的容器。
  4. 此工作區解譯使用者輸入並指揮對話流程，然後傳送回應至您的應用程式。
  5. 您的應用程式顯示對使用者的回應。

## 開始之前
{: #before-you-begin}

請確定您已具備下列必要條件：

  * [{{site.data.keyword.conversationshort}} 服務的實例](/docs/services/conversation/getting-started.html)
  * iOS 8.0+
  * Xcode 9.0+
  * Swift 3.2+ 或 Swift 4.0+
  * Carthage

使用 [Carthage](https://github.com/Carthage/Carthage){:new_window} 來管理相依關係，並為您的應用程式建置 {{site.data.keyword.watson}} Swift SDK。如果您是 Carthage 新手，可以使用 [Homebrew](http://brew.sh/){:new_window} 來安裝 Carthage：

```bash
$ brew update
$ brew install carthage
```

## 下載及建置相依關係
{: #download-and-build-dependencies}

1. 使用您最愛的文字編輯器，在您專案的根目錄（`.xcodeproj` 檔案所在之處）中，建立一個名為 `Cartfile` 的新檔案。

2. 新增一行，指定 Watson Swift SDK 作為相依關係：
  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

  若為正式作業應用程式，您可以指定[版本需求](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement){:new_window}，以避免新版本 {{site.data.keyword.watson}} Swift SDK 有非預期的變更。
  {: tip}

3. 使用終端機來導覽至您專案的根目錄，然後執行 Carthage：
  ```bash
  carthage update --platform iOS
  ```
  {: codeblock}

  然後會下載 {{site.data.keyword.watson}} Swift SDK，而其架構則建置在您專案的 `Carthage/Build/iOS` 資料夾中。

## 將架構新增至應用程式
{: #add-frameworks-to-your-app}

現在已建置 {{site.data.keyword.watson}} Swift SDK 架構，您必須將 {{site.data.keyword.conversationshort}} 架構鏈結並複製到您的應用程式。

1. 以 Xcode 開啟您的應用程式，然後在「導覽器」中選取您的專案以開啟其設定。
2. 選取您的應用程式目標，然後開啟**一般**標籤。
3. 捲動到「已鏈結的架構及程式庫」區段，然後按一下 `+` 圖示。
4. 在顯示的新視窗中，按一下**新增其他**，然後導覽至 `Carthage/Build/iOS` 目錄。
5. 選取 `AssistantV1.framework`，以與您的應用程式鏈結。

除了鏈結 {{site.data.keyword.conversationshort}} 架構，您也必須將其複製到應用程式中，才能在運行環境進行存取。然後，使用 Carthage Script 來避免特定的 [App Store 提交錯誤](http://www.openradar.me/radar?id=6409498411401216){:new_window}。

1. 以 Xcode 開啟您應用程式目標的設定時，導覽至**建置階段**標籤。
2. 按一下 `+` 圖示，然後選取**新建執行 Script 階段**。
3. 將 `/usr/local/bin/carthage copy-frameworks` 指令新增至執行 Script 階段。
4. 將 {{site.data.keyword.conversationshort}} 架構新增至**輸入檔**清單：
  ```
  $(SRCROOT)/Carthage/Build/iOS/AssistantV1.framework
  ```
  {: codeblock}

## 將虛擬助理新增至應用程式
{: #add-a-virtual-assistant-to-your-app}

1. 以 Xcode 開啟 `ViewController.swift`。
2. 為 {{site.data.keyword.conversationshort}} 新增一個 import 陳述式，例如，`import AssistantV1`。
3. 建立一個空函數，稱為 `assistantExample`，然後從 `viewDidLoad` 呼叫該函數。
4. 針對 `assistantExample` 函數，新增下列程式碼。請務必更新您的使用者名稱、密碼及工作區 ID。

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
        // Assistant credentials
        let username = "your-username-here"
        let password = "your-password-here"
        let workspace = "your-workspace-id-here"

        // instantiate service
        let assistant = Assistant(username: username, password: password, version: "2018-03-01")

        // start a conversation
        assistant.message(workspaceID: workspace) { response in
            print("Conversation ID: \(response.context.conversationID!)")
            print("Response: \(response.output.text.joined())")

            // continue assistant
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

當您執行應用程式時，下列訊息會顯示在主控台：
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hi. It looks like a nice drive today. What would you like me to do?
Request: turn the radio on
Response: Sure thing! Which genre would you prefer? Jazz is my personal favorite..
```
{: screen}

## 使用入門範本套件
{: #conversation_starterkits}

使用入門範本套件，您可以快速且輕鬆地運用 {{site.data.keyword.cloud_notm}} 的功能。您可以使用入門範本套件，將 {{site.data.keyword.conversationshort}} 新增至任何伺服器端後端。Chatbot for iOS with Watson Starter Kit 說明如何藉由將自動與一般使用者互動的自然語言介面新增至您的應用程式，來使用 {{site.data.keyword.conversationshort}} 的深度學習功能。

1. 選取您要使用的[入門範本套件](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window}。
2. 建立含有預設服務的專案。
3. 按一下**新增資源 > Watson > {{site.data.keyword.conversationshort}}**。
4. 按一下**下載程式碼**，以下載專案。您可以在 `config/local-dev.json` 檔案中找到服務認證。

## 後續步驟
{: #assistant_next}

做得好！您已將 AI 助理新增至您的應用程式。嘗試下列其中一個選項，以保持動力：

* 請參閱 [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window}。
* 充分運用 [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) 提供的所有特性。
* 檢視 [Simple Chat 範例應用程式](https://github.com/watson-developer-cloud/simple-chat-swift){:new_window}的原始碼，其示範 {{site.data.keyword.watson}} Swift SDK on GitHub。
