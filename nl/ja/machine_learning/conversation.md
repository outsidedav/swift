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

# チャットボットの追加
{: #assistant}

{{site.data.keyword.conversationshort}} サービスを使用して、自然言語による入力を理解して人間と会話しているかのようにユーザーに応答するアプリケーションを構築することができます。

以下のリストは、統合の処理の流れの概要を示しています。

1. ユーザーは、アプリに実装されているインターフェースを使って対話します。
2. アプリは、ユーザーの入力内容を {{site.data.keyword.watson}} Swift SDK を使用して {{site.data.keyword.conversationshort}} に送信します。
3. {{site.data.keyword.watson}} Swift SDK はワークスペースに接続します。ワークスペースはダイアログ・フローとトレーニング・データのコンテナーです。
4. ワークスペースはユーザー入力を解釈し、対話のフローを指図し、応答をアプリに送信します。
5. アプリはユーザーに応答を表示します。

## 始める前に
{: #prereqs-chatbot}

以下の前提条件を満たしていることを確認してください。

* [{{site.data.keyword.conversationshort}} サービスのインスタンス](/docs/services/assistant?topic=assistant-getting-started#getting-started)
* iOS 10.0 以上
* Xcode 9.3 以上
* Swift 4.1 以上
* CocoaPods、Carthage、または Swift Package Manager

[CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")、[Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")、または [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を使用することにより、[Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") をインストールできます。CocoaPods を使用して依存関係を管理する場合、Watson Swift SDK 全体ではなく、必要なフレームワークだけを取得することになります。CocoaPods を使用したことがない場合は、次のように簡単にインストールできます。

```console
sudo gem install cocoapods
```
{: codeblock}

## ステップ 1. Watson Assistant のインスタンスの作成
{: #instance-watson-chatbot}

{{site.data.keyword.conversationshort}} サービスのインスタンスをプロビジョンします。

1. {{site.data.keyword.cloud_notm}} カタログで、**{{site.data.keyword.conversationshort}}** を選択します。 サービス構成画面が開きます。
2. サービス・インスタンスに名前を付けます。または、事前設定された名前を使用します。
3. インスタンスをアプリにバインドする場合は、**「接続」**メニューからアプリを選択します。
4. 料金プランを選択し、**「作成」**をクリックします。
5. **「資格情報」**タブを選択して、サービスの資格情報を表示します。 アプリからサービスに接続するために、これらの値が使用されます。
6. **「ツールの起動 (Launch tool)」**をクリックしてチャットボット・アシスタントを作成します。 ランディング・ページに表示される指示に従ってチャットボットを作成するか、**「スキル (Skills)」**タブに移動して**「新規作成」**をクリックします。 作業をすぐに開始するには、**「ダイアログ・スキルの追加 (Add Dialog Skill)」**ページで**「サンプル・スキルを使用 (Use sample skill)」**タブを選択し、**「カスタマー・ケア・サンプル・スキル (Customer Care Sample Skill)」**を選択します。 引き続きこのスキルを調整するか、後でアプリで使用する他のスキルを作成できます。
7. 新しいスキルのメニューをクリックし、**「API 詳細の表示 (View API Details)」**を選択します。 サービス資格情報の他に、必要な`ワークスペース ID` が表示されます。

## ステップ 2. 依存関係のダウンロードと作成
{: #download-depend-chatbot}

以下の例では AssistantV1 を使用しています。 AssistantV2 フレームワークについて詳しくは、[Watson SDK AssistantV2 の資料](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV2/index.html){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。

任意のテキスト・エディターを使用して、`pod init` を実行してプロジェクトのルート・ディレクトリー (`.xcodeproj` ファイルがある場所) に新しい `Podfile` を作成します。 次に、依存関係として Watson Swift SDK の {{site.data.keyword.conversationshort}} フレームワークを指定する行を追加します。

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonAssistantV1'
```
{: codeblock}

実動アプリの場合は、特定の[バージョン要件](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を指定することにより、Watson Swift SDK の新リリースでの予期しない変更を回避することができます。

`Podfile` が作成され、依存関係をダウンロードできるようになりました。 端末を使用してプロジェクトのルート・ディレクトリーにナビゲートし、次のように CocoaPods を実行します。

```console
pod install
```
{: codeblock}

Cocoapods により {{site.data.keyword.conversationshort}} フレームワークがダウンロードされ、プロジェクトの `Pods/` フォルダーにこのフレームワークが作成されます。

Pod build が失敗しないようにするため、プロジェクトを Xcode で開くときには `.xcodeproj` ではなく `.xcworkspace` で終わるファイルを開きます。
{: tip}

## ステップ 3. アプリへの仮想アシスタントの追加
{: #virtual-assist-chatbot}

以下の例は、(特に `ViewController.swift` で) アプリケーションに {{site.data.keyword.conversationshort}} の機能を追加する際に役立ちます。 以下の例を使用すると、ユース・ケースに合わせてアシスタント呼び出しを拡張できます。

1. {{site.data.keyword.conversationshort}} の import ステートメント (例: `import Assistant`) を追加します。
  ```swift
  import Assistant
  ```
  {: codeblock}

2. {{site.data.keyword.conversationshort}} サービスをインスタンス化します。
  ```swift
  let assistant = Assistant(version: "yyyy-mm-dd", apikey: "your-api-key-here")

  // save context to state to continue the conversation later
  var context: Context?
  ```
  {: codeblock}

  **ヒント**: この例では、後から会話を続行するためのコンテキストが保存されます。 このオブジェクトについて、またユース・ケースにこのオブジェクトを適応させる方法について詳しくは、[コンテキスト変数の資料](/docs/services/assistant?topic=assistant-dialog-runtime#context-variables)を参照してください。 [バージョン・パラメーターの資料](https://cloud.ibm.com/apidocs/assistant#versioning){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を確認するか、{site.data.keyword.conversationshort}} サービスが作成された日付を使用してください。

3. 会話を初期化します。 アシスタントの構成方法によっては、ユーザーに対する最初の応答を提供できます。
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

4. メッセージとコンテキストをアシスタントに送信します。
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

デフォルトのアシスタントでこれらの例を使用してアプリを実行すると、次のメッセージがコンソールに表示されます。
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hello, I’m a demo customer care virtual assistant to show you the basics.  I can help with directions to my store, hours of operation and booking an in-store appointment.
Request: When are you open?
Response: Our hours are Monday to Friday 10am to 8pm and Friday and Saturday 11am to 6pm.
```
{: screen}

5. Watson SDK [Assistant の資料](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV1/index.html){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照して、アプリケーションの機能を作成します。

## スターター・キットの使用
{: #starterkits-chatbot}

スターター・キットを使用すると、{{site.data.keyword.cloud_notm}} の機能を素早く簡単に利用できます。 スターター・キットを使用して、{{site.data.keyword.conversationshort}} を任意のサーバー・サイドのバックエンドに追加できます。 Chatbot for iOS with Watson Starter Kit は、ユーザーとの対話を自動化するアプリケーションに自然言語インターフェースを追加することによって {{site.data.keyword.conversationshort}} のディープ・ラーニング機能を使用する方法について説明しています。

1. 使用する[スターター・キット](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を選択します。
2. デフォルト・サービスを使用してアプリを作成します。
3. **「サービスの追加」>「Watson」>「{{site.data.keyword.conversationshort}}」**をクリックします。
4. **「コードのダウンロード (Download code)」**をクリックしてプロジェクトをダウンロードします。サービス資格情報は、`config/local-dev.json` ファイルにあります。

## 次のステップ
{: #next-chatbot notoc}

お疲れさまでした。 AI アシスタントがアプリに追加されました。 この調子で、以下のいずれかのオプションを試してみてください。

* [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照し、サポートされているその他の Watson サービスについて確認する。
* [{{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-index#index) が提供するすべての機能を利用する。
* [Simple Chat サンプル・アプリ](https://github.com/watson-developer-cloud/simple-chat-swift){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") のソース・コードを表示し、GitHub 上で {{site.data.keyword.watson}} Swift SDK を実演する。
