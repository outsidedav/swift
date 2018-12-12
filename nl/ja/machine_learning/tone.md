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

IBM Watson Tone Analyzer サービスは、アプリがテキスト内の感情やトーンを理解できるようにします。 このサービスを使用すれば、ユーザーの会話をより良く理解したり、ユーザーが自分が書き込んだ内容がどのように受け取られているかを理解できるよう支援したりすることができます。

## 動作の仕組み
{: ##how-it-works}

1. アプリによって、分析するテキストが選択されます (テキスト・メッセージや Twitter のフィードなど)。
2. アプリにより、Watson Swift SDK を使用してそのテキストが {{site.data.keyword.toneanalyzershort}} サービスに送信されます。
3. サービスにより、言語分析機能を使用して感情やトーンを識別することによりテキストが分析されます。
4. サービスによる分析結果が、Watson Swift SDK によってアプリに返されます。

## 始めに
{: ###before-you-begin}

まず、以下の前提条件が整っていることを確認してください。
<ul>
  <li>iOS 8.0 以上</li>
  <li>Xcode 9.0 以上</li>
  <li>Swift 3.2 以上または Swift 4.0 以上</li>
  <li>Carthage</li>
</ul>

[Carthage](https://github.com/Carthage/Carthage) を使用して依存関係を管理し、アプリケーション用の Watson Swift SDK を構築することをお勧めします。 Carthage を使用したことがない場合は、次のようにして [Homebrew](http://brew.sh/) でインストールできます。

```bash
$ brew update
$ brew install carthage
```
{: codeblock}

## ステップ 1. Tone Analyzer のインスタンスの作成
{: #create-and-configure-an-instance-of-tone-analyzer}

{{site.data.keyword.toneanalyzershort}} サービスのインスタンスをプロビジョンします。

1. {{site.data.keyword.cloud_notm}} カタログで、{{site.data.keyword.toneanalyzershort}} を選択します。 サービス構成画面が開きます。
2. サービス・インスタンスに名前を付けます。または、事前設定された名前を使用します。
3. インスタンスをアプリにバインドする場合は、「接続」メニューからアプリを選択します。
4. 料金プランを選択し、**「作成」**をクリックします。
5. **「資格情報」**タブを選択して、サービスの資格情報を表示します。 アプリからサービスに接続するために、これらの値が使用されます。

## ステップ 2. 依存関係のダウンロードと作成
{: ###download-and-build-dependencies}

任意のテキスト・エディターを使用して、プロジェクトのルート・ディレクトリー (`.xcodeproj` ファイルがある場所) に `Cartfile` という新しいファイルを作成します。 次に、依存関係として Watson Swift SDK を指定する行を以下のように追加します。

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

実動アプリの場合は、特定の[バージョン要件](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement)を指定することにより、Watson Swift SDK の新リリースでの予期しない変更を回避することができます。

`Cartfile` が作成され、依存関係をダウンロードして作成できるようになりました。 端末を使用してプロジェクトのルート・ディレクトリーにナビゲートし、次のように Carthage を実行します。
  
  ```bash
  $ carthage update --platform iOS
  ```
  {: codeblock}

Carthage により、Watson Swift SDK がダウンロードされ、そのフレームワークがプロジェクトの `Carthage/Build/iOS` フォルダー内に作成されます。

## ステップ 3. アプリへのフレームワークの追加
{: ###add-frameworks-to-your-app}

### Tone Analyzer のリンク手順

Watson Swift SDK フレームワークが Carthage によって作成されたので、Tone Analyzer フレームワークをアプリにリンクする必要があります。

1. Xcode でアプリを開き、プロジェクトを選択して設定を開きます。
2. アプリ・ターゲットを選択してから、**「General」**タブを開きます。
3. 「Linked Frameworks and Libraries」セクションまでスクロールダウンし、`+` アイコンをクリックします。
4. 表示されるウィンドウで、**「Add Other...」**を選択し、`Carthage/Build/iOS` ディレクトリーにナビゲートします。 **ToneAnalyzerV3.framework** を選択して、アプリとリンクします。

### Tone Analyzer のコピー手順

Tone Analyzer フレームワークを_リンク_ することに加えて、それをアプリに_コピー_ して、実行時にアクセス可能になるようにすることも必要です。 Xcode には、フレームワークをコピーまたは埋め込む方法がいくつかありますが、Carthage スクリプトを使用して特定の [App Store への提出時のバグ](http://www.openradar.me/radar?id=6409498411401216)を回避することができます。

1. アプリのターゲットの設定が Xcode で開いている状態で、**「Build Phases」**タブにナビゲートします。
2. `+` アイコンをクリックし、**「New Run Script Phase」**を選択します。
3. 実行スクリプト・フェーズにコマンド `/usr/local/bin/carthage copy-frameworks` を追加します。
4. Tone Analyzer フレームワークを「Input Files」リストに次のように追加します: `$(SRCROOT)/Carthage/Build/iOS/ToneAnalyzerV3.framework`

これで、アプリで Watson Swift SDK を使用できるようになりました。

## ステップ 4. アプリでのテキストの分析
{: #analyze-text-in-your-app}

1. Xcode で `ViewController.swift` ファイルを開きます。
2. Tone Analyzer の import ステートメントを追加します。 
    ```swift
    import ToneAnalyzerV3
    ```
    {: codeblock}

3. `toneAnalyzerExample` という空の関数を作成し、それを `viewDidLoad` から呼び出します。
4. 次のコードを `toneAnalyzerExample` 関数に追加します。 サービスのユーザー名とパスワードを必ず更新してください。
  ```swift
  import UIKit
  import ToneAnalyzerV3

  class ViewController: UIViewController {

      override func viewDidLoad() {
          super.viewDidLoad()
          toneAnalyzerExample()
      }

      func toneAnalyzerExample() {

          // instantiate service
          let toneAnalyzer = ToneAnalyzer(
              username: "your-username-here",
              password: "your-password-here",
              version: "yyyy-mm-dd"
          )

          // text to analyze
          let review = """
              I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
          """

          // analyze text
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

アプリを実行すると、コンソールに分析結果が次のように表示されます。
```
Sadness: 0.575803
Tentative: 0.867377
```
{: screen}

## スターター・キットの使用
{: #tone_starterkits}

[スターター・キット](https://console.bluemix.net/developer/appledevelopment/starter-kits)は、{{site.data.keyword.cloud_notm}} の機能を素早く活用する方法の 1 つです。 **Tone Analyzer for iOS with Watson** スターター・キットを選択することにより、{{site.data.keyword.toneanalyzershort}} サービスを使用できます。 このサービスは、ディープ・ラーニングの機能を利用して、一まとまりのテキストを評価します。 Tone Analyzer アプリケーションは、いくつかのカテゴリーに関連付けて、話者のトーン (幸福、悲しい、自信がある、など) を識別します。

このスターター・キットを開始するには、次のようにします。

1. [ここ](https://console.bluemix.net/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson)にあるスターター・キットを選択します。
2. デフォルト・サービスを使用してプロジェクトを作成します。
3. **「コードのダウンロード (Download Code)」**をクリックしてプロジェクトをダウンロードします。 サービスの資格情報が、対応するキー・フィールドの `BMSCredentials.plist` ファイルに注入されます。

## 次のステップ
{: #tone_next}

お疲れさまでした。 これで {{site.data.keyword.toneanalyzershort}} がアプリに追加されました。 この調子で、以下のいずれかのオプションを試してみてください。

* [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk) を見る。
* 詳しくは、[IBM Watson{{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/) を参照してください。

