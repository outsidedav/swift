---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: chatbot swift, virtual assistant swift, assistant swift, watson swift starter, assistantv2 swift, watson sdk swift, add chatbot swift, add assistant swift

subcollection: swift

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
{: #prereqs-chatbot}

請確定您具備下列必要條件：

* [{{site.data.keyword.conversationshort}} 服務的實例](/docs/services/assistant?topic=assistant-getting-started#getting-started)
* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods、Carthage 或 Swift Package Manager

您可以使用 [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")、[Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 或 [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")，來安裝 [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")。藉由使用 CocoaPods 來管理相依關係，您只會得到您需要的架構，而不是整個 Watson Swift SDK。如果您是 CocoaPods 新手，可以輕鬆地安裝它：

```console
  sudo gem install cocoapods
  ```
{: codeblock}

## 步驟 1. 建立 Watson Assistant 的實例
{: #instance-watson-chatbot}

佈建 {{site.data.keyword.conversationshort}} 服務的實例：

1. 在 {{site.data.keyword.cloud_notm}} 型錄中，選取 **{{site.data.keyword.conversationshort}}**。即會開啟服務配置畫面。
2. 提供服務實例的名稱，或使用預設名稱。
3. 如果您要將實例連結至應用程式，請從**連接**功能表中選取應用程式。
4. 選取定價方案，然後按一下**建立**。
5. 選取**認證**標籤，以檢視您的服務認證。這些值用來從您的應用程式連接至服務。
6. 按一下**啟動工具**，以建置您的聊天機器人助理。遵循登陸頁面上的指示，以建置您自己的聊天機器人，或移至**技能**標籤，再選取**建立新的項目**。在**新增對話技能**頁面上，選擇**使用範例技能**標籤，並選取**客戶管理範例技能**以便快速開始。您可以繼續修正此技能，或是建立其他技能以便應用程式之後再使用。
7. 在新技能上按一下功能表，然後選取**檢視 API 詳細資料**。您現在可以看到需要的 `Workspace ID` 以及服務認證。

## 步驟 2. 下載及建置相依關係
{: #download-depend-chatbot}

下列範例使用 AssistantV1。如需 AssistantV2 架構的相關資訊，請參閱 [Watson SDK AssistantV2 文件](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV2/index.html){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")。

使用您最愛的文字編輯器，在您專案的根目錄（`.xcodeproj` 檔案所在之處）中，執行 `pod init` 建立一個新的 `Podfile`。然後，新增一行，指定 Watson Swift SDK 的 {{site.data.keyword.conversationshort}} 架構作為相依關係：

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonAssistantV1'
```
{: codeblock}

若為正式作業應用程式，您也可能想要指定特定的[版本需求](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")，以避免新版本 Watson Swift SDK 有非預期的變更。

`Podfile` 就緒後，現在您可以下載相依關係。使用終端機來導覽至您專案的根目錄，然後執行 CocoaPods：

```console
pod install
```
{: codeblock}

Cocoapods 會下載 {{site.data.keyword.conversationshort}} 架構，並且會在您專案的 `Pods/` 資料夾中建置它。

為了避免 Pod 建置失敗，當您在 Xcode 開啟專案時，請開啟以 `.xcworkspace` 為結尾的檔案，而不是 `.xcodeproj`。
{: tip}

## 步驟 3. 將虛擬助理新增至應用程式
{: #virtual-assist-chatbot}

下列範例會協助您為應用程式新增 {{site.data.keyword.conversationshort}} 功能，一般是在 `ViewController.swift`。使用下列範例，您可以為您的使用案例延伸 Assistant 呼叫。

1. 為 {{site.data.keyword.conversationshort}} 新增一個 import 陳述式，例如，`import Assistant`。
  ```swift
  import Assistant
  ```
  {: codeblock}

2. 實例化 {{site.data.keyword.conversationshort}} 服務：
  ```swift
  let assistant = Assistant(version: "yyyy-mm-dd", apikey: "your-api-key-here")

  // save context to state to continue the conversation later
  var context: Context?
  ```
  {: codeblock}

  **提示**：此範例會將環境定義儲存至狀態。為了更加瞭解此目標，以及如何為您的使用案例調整它，請參閱[環境定義變數文件](/docs/services/assistant?topic=assistant-dialog-runtime#context-variables)。參閱[版本參數文件](https://cloud.ibm.com/apidocs/assistant#versioning){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")，或使用建立 {{site.data.keyword.conversationshort}} 服務的日期。
  

3. 起始設定交談。視您的助理配置情形而定，它可以提供起始回應給使用者：
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

4. 傳送訊息及環境定義給助理：
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

當您使用預設助理，以這些範例執行應用程式時，下列訊息會顯示在主控台：
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hello, I’m a demo customer care virtual assistant to show you the basics.  I can help with directions to my store, hours of operation and booking an in-store appointment.
Request: When are you open?
Response: Our hours are Monday to Friday 10am to 8pm and Friday and Saturday 11am to 6pm.
```
{: screen}

5. 探索 Watson SDK [Assistant 文件](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV1/index.html){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")，以建置完備應用程式的功能。

## 使用入門範本套件
{: #starterkits-chatbot}

使用入門範本套件，您可以快速且輕鬆地運用 {{site.data.keyword.cloud_notm}} 的功能。您可以使用入門範本套件，將 {{site.data.keyword.conversationshort}} 新增至任何伺服器端後端。Chatbot for iOS with Watson Starter Kit 說明如何藉由將自動與使用者互動的自然語言介面新增至您的應用程式，來使用 {{site.data.keyword.conversationshort}} 的深度學習功能。

1. 選取您要使用的[入門範本套件](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")。
2. 建立含有預設服務的應用程式。
3. 按一下**新增服務 > Watson > {{site.data.keyword.conversationshort}}**。
4. 按一下**下載程式碼**，以下載專案。您可以在 `config/local-dev.json` 檔案中找到服務認證。

## 後續步驟
{: #next-chatbot notoc}

做得好！您已將 AI 助理新增至您的應用程式。嘗試下列其中一個選項，以保持動力：

* 參閱 [{{site.data.keyword.watson}}Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 並探索其他受支援的 Watson 服務。
* 充分運用 [{{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-index#index) 提供的所有特性。
* 檢視 [Simple Chat 範例應用程式](https://github.com/watson-developer-cloud/simple-chat-swift){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 的原始碼，其示範 {{site.data.keyword.watson}} Swift SDK on GitHub。
