---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-24"

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

{{site.data.keyword.ibmwatson}} {{site.data.keyword.toneanalyzershort}} サービスは、テキスト内の感情やトーンをアプリが理解できるようにします。このサービスを使用すれば、ユーザーの会話をより良く理解したり、ユーザーが自分が書き込んだ内容がどのように受け取られているかを理解できるよう支援したりすることができます。

## 動作の仕組み
{: #how-it-works-tone}

1. アプリによって、分析するテキストが選択されます (テキスト・メッセージや Twitter のフィードなど)。
2. アプリにより、Watson Swift SDK を使用してそのテキストが {{site.data.keyword.toneanalyzershort}} サービスに送信されます。
3. サービスにより、言語分析機能を使用して感情やトーンを識別することによりテキストが分析されます。
4. サービスによる分析結果が、[Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk) によってアプリに返されます。

## 始める前に
{: #prereqs-tone}

以下の前提条件を満たしていることを確認してください。

* OS 10.0 以上
* Xcode 9.3 以上
* Swift 4.1 以上
* CocoaPods、Carthage、または Swift Package Manager

[CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")、[Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")、または [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")
を使用することにより、[Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") をインストールできます。CocoaPods を使用して依存関係を管理する場合、Watson Swift SDK 全体ではなく、必要なフレームワークだけを取得することになります。CocoaPods を使用したことがない場合は、次のように簡単にインストールできます。

```bash
$ sudo gem install cocoapods
```
{: codeblock}

## ステップ 1. Tone Analyzer のインスタンスの作成
{: #create-instance-tone}

{{site.data.keyword.toneanalyzershort}} サービスのインスタンスをプロビジョンします。

1. {{site.data.keyword.cloud_notm}} カタログで、{{site.data.keyword.toneanalyzershort}} を選択します。 サービス構成画面が開きます。
2. サービス・インスタンスに名前を付けます。または、事前設定された名前を使用します。
3. インスタンスをアプリにバインドする場合は、「接続」メニューからアプリを選択します。
4. 料金プランを選択し、**「作成」**をクリックします。
5. **「資格情報」**タブを選択して、サービスの資格情報を表示します。 アプリからサービスに接続するために、これらの値が使用されます。

## ステップ 2. 依存関係のダウンロードと作成
{: #download-depend-tone}

任意のテキスト・エディターを使用して、`pod init` を実行してプロジェクトのルート・ディレクトリー (`.xcodeproj` ファイルがある場所) に新しい `Podfile` を作成します。 次に、依存関係として Watson Swift SDK の {{site.data.keyword.conversationshort}} フレームワークを指定する行を追加します。

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonToneAnalyzerV3'
```
{: codeblock}

実動アプリの場合は、特定の[バージョン要件](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を指定することにより、Watson Swift SDK の新リリースでの予期しない変更を回避することができます。

`Podfile` が作成され、依存関係をダウンロードできるようになりました。 端末を使用してプロジェクトのルート・ディレクトリーにナビゲートし、次のように CocoaPods を実行します。

```console
pod install
```
{: codeblock}

Cocoapods により {{site.data.keyword.toneanalyzershort}} フレームワークがダウンロードされ、プロジェクトの `Pods/` フォルダーにこのフレームワークが作成されます。

Pod build が失敗しないようにするため、プロジェクトを Xcode で開くときには `.xcodeproj` ではなく `.xcworkspace` で終わるファイルを開きます。
{: tip}

## ステップ 3. アプリでのテキストの分析
{: #analyze-text-tone}

以下の例は、(特に `ViewController.swift` で) アプリケーションに {{site.data.keyword.toneanalyzershort}} の機能を追加する際に役立ちます。 以下の例を使用すると、ユース・ケースに合わせて Tone Analyzer 呼び出しを拡張できます。

1. Tone Analyzer の import ステートメントを追加します。
  ```swift
  import ToneAnalyzer
  ```
  {: codeblock}

2. Tone Analyzer サービスをインスタンス化します。
  ```swift
  let toneAnalyzer = ToneAnalyzer(version: "yyyy-mm-dd", apiKey: "your-api-key-here")
  ```
  {: codeblock}

  [バージョン・パラメーターの資料](https://cloud.ibm.com/apidocs/tone-analyzer#versioning){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を確認するか、{site.data.keyword.conversationshort}} サービスが作成された日付を使用してください。{: tip}

3. 分析するテキストを指定し、分析結果を処理します。
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

  コードを実行すると、コンソールに分析結果が次のように表示されます。
  ```
  Sadness: 0.575803
  Tentative: 0.867377
  ```
  {: screen}

4. Watson SDK [Tone Analyzer の資料](https://watson-developer-cloud.github.io/swift-sdk/services/ToneAnalyzerV3/index.html){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照して、アプリケーションの機能を作成します。

## スターター・キットの使用
{: #tone_starterkits}

[スターター・キット](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")は、{{site.data.keyword.cloud_notm}} の機能を素早く使用する方法の 1 つです。**Tone Analyzer for iOS with Watson** スターター・キットを選択することにより、{{site.data.keyword.toneanalyzershort}} サービスを使用できます。 このサービスは、ディープ・ラーニングの機能を利用して、一まとまりのテキストを評価します。 Tone Analyzer アプリケーションは、いくつかのカテゴリーに関連付けて、話者のトーン (幸福、悲しい、自信がある、など) を識別します。

このスターター・キットを開始するには、次のようにします。

1. [ここ](https://cloud.ibm.com/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") にあるスターター・キットを選択します。
2. デフォルト・サービスを使用してプロジェクトを作成します。
3. **「コードのダウンロード (Download Code)」**をクリックしてプロジェクトをダウンロードします。 サービスの資格情報が、対応するキー・フィールドの `BMSCredentials.plist` ファイルに注入されます。

## 次のステップ
{: #tone_next notoc}

お疲れさまでした。 これで {{site.data.keyword.toneanalyzershort}} がアプリに追加されました。 この調子で、以下のいずれかのオプションを試してみてください。

* [GitHub にある Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照し、サポートされているその他の Watson サービスについて確認する。
* 詳細については、[IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。
