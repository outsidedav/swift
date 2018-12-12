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

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

{{site.data.keyword.visualrecognitionfull}} サービスを使用すると、機械学習を使用することによって、アプリでビジュアル・コンテンツを素早く正確にタグ付け、分類、およびトレーニングすることができます。 このサービスは、実質的にあらゆるビジュアル・コンテンツを分類し、数分のうちに独自のカスタム・モデルをトレーニングし、顔を検出するのに役立ちます。

## 動作の仕組み
{: ##how-it-works}

1. 分析する画像がアプリによって選択されます。
2. アプリにより、Watson Swift SDK を使用して {{site.data.keyword.visualrecognitionshort}} サービスに画像が送信されます。
3. サービスにより、分類分析を使用して画像が分析され、場面、物体、顔などが識別されます。
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

## ステップ 1. Visual Recognition のインスタンスの作成
{: ###create-and-configure-an-instance-of-visual-recognition}

{{site.data.keyword.visualrecognitionshort}} サービスのインスタンスをプロビジョンします。

1. {{site.data.keyword.cloud_notm}} カタログで、**{{site.data.keyword.visualrecognitionshort}}** を選択します。 サービス構成画面が開きます。
2. サービス・インスタンスに名前を付けます。または、事前設定された名前を使用します。
3. インスタンスをアプリにバインドする場合は、**「接続」**メニューからアプリを選択します。
4. 料金プランを選択し、**「作成」**をクリックします。
5. **「資格情報」**タブを選択して、サービスの資格情報を表示します。 アプリからサービスに接続するために、これらの値が使用されます。

## ステップ 2. 依存関係のダウンロードと作成
{: ###download-and-build-dependencies}

任意のテキスト・エディターを使用して、プロジェクトのルート・ディレクトリー (`.xcodeproj` ファイルがある場所) に `Cartfile` というファイルを作成します。 次に、依存関係として Watson Swift SDK を指定する行を以下のように追加します。
```
github "watson-developer-cloud/swift-sdk"
```
{: pre}

実動アプリの場合は、特定の[バージョン要件](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement)を指定することにより、Watson Swift SDK の新リリースでの予期しない変更を回避することができます。

`Cartfile` が作成され、依存関係をダウンロードして作成できるようになりました。 端末を使用してプロジェクトのルート・ディレクトリーにナビゲートし、次のように Carthage を実行します。

```bash
carthage update --platform iOS
```
{: pre}

Carthage により、Watson Swift SDK がダウンロードされ、そのフレームワークがプロジェクトの `Carthage/Build/iOS` フォルダー内に作成されます。

## ステップ 3. アプリへのフレームワークの追加
{: ###add-frameworks-to-your-app}

### Visual Recognition のリンク手順:

Watson Swift SDK フレームワークが Carthage によって作成されたので、Visual Recognition フレームワークをアプリにリンクする必要があります。

1. Xcode でアプリを開き、プロジェクトを選択して設定を開きます。
2. アプリのターゲットを選択して、**「General」**タブを開きます。
3. 「Linked Frameworks and Libraries」セクションまでスクロールダウンし、`+` アイコンをクリックします。
4. 表示されるウィンドウで、**「Add Other...」**を選択し、`Carthage/Build/iOS` ディレクトリーにナビゲートします。 **VisualRecognitionV3.framework** を選択して、アプリとリンクします。

### Visual Recognition のコピー手順:

Visual Recognition フレームワークを_リンク_ することに加えて、それをアプリに_コピー_ して、実行時にアクセス可能になるようにすることも必要です。 Xcode には、フレームワークをコピーまたは埋め込む方法がいくつかありますが、Carthage スクリプトを使用して特定の [App Store への提出時のバグ](http://www.openradar.me/radar?id=6409498411401216)を回避することができます。

1. アプリのターゲットの設定が Xcode で開いている状態で、**「Build Phases」**タブにナビゲートします。
2. `+` アイコンをクリックし、**「New Run Script Phase」**を選択します。
3. 実行スクリプト・フェーズにコマンド `/usr/local/bin/carthage copy-frameworks` を追加します。
4. Visual Recognition フレームワークを**「Input Files」**リストに次のように追加します: `$(SRCROOT)/Carthage/Build/iOS/VisualRecognitionV3.framework`

これで、アプリで Watson Swift SDK を使用できるようになりました。

## ステップ 4. アプリでの画像の分析
{: #analyze-images-in-your-app}

1. Xcode で `ViewController.swift` ファイルを開きます。

1. Visual Recognition の import ステートメントを追加します。
    ```swift
    import VisualRecognitionV3
    ```
    {: pre}

1. API 鍵とバージョン (今日の日付を使用) を渡して、SDK を初期化します。
    ```swift
    let visualRecognition = VisualRecognition(apiKey: "your-api-key", version: "yyyy-mm-dd")
    ```
    {: pre}

1. 画像を分類するため、次のコードを追加します。
    ```swift
    let url = "your-image-url"
    let failure = { (error: Error) in print(error) }
    visualRecognition.classify(url: url, failure: failure) { classifiedImages in
        print(classifiedImages)
    }
    ```
    {: pre}

## スターター・キットの使用
{: #recognition_starterkits}

[スターター・キット](https://console.bluemix.net/developer/appledevelopment/starter-kits)は、{{site.data.keyword.cloud_notm}} の機能を素早く活用する方法の 1 つです。 **Visual Recognition for iOS with Watson** スターター・キットを選択することにより、{{site.data.keyword.visualrecognitionshort}} サービスを使用できます。 このサービスは、画像を評価して分類します。 モバイル・デバイスから新しい画像や既存の画像をアップロードすると、Visual Recognition アプリがその画像コンテンツを素早くタグ付けし、分類します。

開始するには、以下のようにします。
1. [ここ](https://console.bluemix.net/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson)にあるスターター・キットを選択します。
2. デフォルト・サービスを使用してプロジェクトを作成します。
3. **「コードのダウンロード (Download Code)」**をクリックしてプロジェクトをダウンロードします。 サービスの資格情報が、対応するキー・フィールドの `BMSCredentials.plist` ファイルに注入されます。

## 次のステップ
{: #recognition_next}

お疲れさまでした。 これで Visual Recognition がアプリから利用可能になりました。 この調子で、以下のいずれかのオプションを試してみてください。
* [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk) を見る。
* 詳しくは、[IBM Watson{{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/) を参照してください。

