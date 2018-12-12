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

# チャットボットの追加
{: #assistant}

{{site.data.keyword.conversationshort}} サービスを使用して、自然言語による入力を理解して人間と会話しているかのようにユーザーに応答するアプリケーションを構築することができます。

以下のリストは、統合の処理の流れの概要を示しています。

  1. ユーザーは、アプリに実装されているインターフェースを使って対話します。
  2. アプリは、ユーザーの入力内容を {{site.data.keyword.watson}} Swift SDK を使用して {{site.data.keyword.conversationshort}} に送信します。
  3. {{site.data.keyword.watson}} Swift SDK はワークスペースに接続します。ワークスペースはダイアログ・フローとトレーニング・データのコンテナーです。
  4. ワークスペースはユーザー入力を解釈し、対話のフローを指図し、応答をアプリに送信します。
  5. アプリはユーザーに応答を表示します。

## 始めに
{: #before-you-begin}

以下の前提条件を満たしていることを確認してください。

  * [{{site.data.keyword.conversationshort}} サービスのインスタンス](/docs/services/conversation/getting-started.html)
  * iOS 8.0+
  * Xcode 9.0+
  * Swift 3.2 以上または Swift 4.0 以上
  * Carthage

[Carthage](https://github.com/Carthage/Carthage){:new_window} を使用して、依存関係を管理したり、アプリケーション用の  {{site.data.keyword.watson}} Swift SDK を作成したりします。 Carthage を使用したことがない場合は、次のようにして [Homebrew](http://brew.sh/){:new_window} でインストールできます。

```bash
$ brew update
$ brew install carthage
```

## 依存関係のダウンロードと作成
{: #download-and-build-dependencies}

1. 任意のテキスト・エディターを使用して、プロジェクトのルート・ディレクトリー (`.xcodeproj` ファイルのある場所) に `Cartfile` という名前の新規ファイルを作成します。

2. 以下のような行を追加して、Watson Swift SDK を依存関係として指定します。
  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

  実動アプリの場合は、[バージョン要件](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement){:new_window}を指定することで、{{site.data.keyword.watson}} Swift SDK の新規リリースによる予期しない変更を回避することができます。
  {: tip}

3. 端末を使用してプロジェクトのルート・ディレクトリーにナビゲートし、次のように Carthage を実行します。
  ```bash
  carthage update --platform iOS
  ```
  {: codeblock}

  {{site.data.keyword.watson}} Swift SDK がダウンロードされ、そのフレームワークが作業中のプロジェクトの `Carthage/Build/iOS` フォルダーに作成されます。

## アプリへのフレームワークの追加
{: #add-frameworks-to-your-app}

{{site.data.keyword.watson}} Swift SDK フレームワークが作成されたら、{{site.data.keyword.conversationshort}} フレームワークをアプリにリンクし、コピーする必要があります。

1. Xcode でアプリを開き、ナビゲーターでプロジェクトを選択して設定を開きます。
2. アプリのターゲットを選択して、**「General」**タブを開きます。
3. 「Linked Frameworks and Libraries」セクションまでスクロールダウンし、`+` アイコンをクリックします。
4. 新規ウィンドウが表示されたら、**「Add Other」**をクリックして、`Carthage/Build/iOS` ディレクトリーにナビゲートします。
5. アプリとリンクさせる `AssistantV1.framework` を選択します。

{{site.data.keyword.conversationshort}} フレームワークをリンクするだけでなく、実行時にアクセスできるように、それをアプリにコピーする必要もあります。 そうすることで、Carthage スクリプトが実行されて特定の [App Store 提出時のバグ](http://www.openradar.me/radar?id=6409498411401216){:new_window}を回避することができます。

1. アプリのターゲットの設定が Xcode で開いている状態で、**「Build Phases」**タブにナビゲートします。
2. `+` アイコンをクリックし、**「New Run Script Phase」**を選択します。
3. `/usr/local/bin/carthage copy-frameworks` コマンドを実行スクリプト・フェーズに追加します。
4. {{site.data.keyword.conversationshort}} フレームワークを**「Input Files」**リストに追加します。
  ```
  $(SRCROOT)/Carthage/Build/iOS/AssistantV1.framework
  ```
  {: codeblock}

## アプリへの仮想アシスタントの追加
{: #add-a-virtual-assistant-to-your-app}

1. Xcode で `ViewController.swift` を開きます。
2. {{site.data.keyword.conversationshort}} のインポート・ステートメントを追加します (例: `import AssistantV1`)。
3. `assistantExample` という空の関数を作成し、それを `viewDidLoad` から呼び出します。
4. `assistantExample` 関数に次のコードを追加します。 必ずユーザー名、パスワード、およびワークスペース ID を更新してください。

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

アプリを実行すると、次のメッセージがコンソールに表示されます。
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hi. It looks like a nice drive today. What would you like me to do?
Request: turn the radio on
Response: Sure thing! Which genre would you prefer? Jazz is my personal favorite..
```
{: screen}

## スターター・キットの使用
{: #conversation_starterkits}

スターター・キットを使用すると、{{site.data.keyword.cloud_notm}} の機能を素早く簡単に利用できます。 スターター・キットを使用して、{{site.data.keyword.conversationshort}} を任意のサーバー・サイドのバックエンドに追加できます。 Chatbot for iOS with Watson Starter Kit は、エンド・ユーザーとの対話を自動化するアプリケーションに自然言語インターフェースを追加することによって {{site.data.keyword.conversationshort}} のディープ・ラーニング機能を使用する方法について説明しています。

1. 使用する[スターター・キット](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window}を選択します。
2. デフォルト・サービスを使用してプロジェクトを作成します。
3. **「リソースの追加」>「Watson」>「{{site.data.keyword.conversationshort}}」**をクリックします。
4. **「コードのダウンロード (Download Code)」**をクリックして、プロジェクトをダウンロードします。 サービス資格情報は、`config/local-dev.json` ファイルにあります。

## 次のステップ
{: #assistant_next}

お疲れさまでした。 AI アシスタントがアプリに追加されました。 この調子で、以下のいずれかのオプションを試してみてください。

* [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window} を使ってみる。
* [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) が提供するすべての機能を利用する。
* [Simple Chat サンプル・アプリ](https://github.com/watson-developer-cloud/simple-chat-swift){:new_window}のソース・コードを表示し、GitHub 上で {{site.data.keyword.watson}} Swift SDK を実演する。
