---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-31"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

{{site.data.keyword.visualrecognitionfull}} サービスを使用すると、機械学習を使用することによって、アプリでビジュアル・コンテンツを素早く正確にタグ付け、分類、およびトレーニングすることができます。 このサービスは、実質的にあらゆるビジュアル・コンテンツを分類し、数分のうちに独自のカスタム・モデルをトレーニングし、顔を検出するのに役立ちます。

## 動作の仕組み
{: #how-it-works-recognition}

1. 分析する画像がアプリによって選択されます。
2. アプリにより、Watson Swift SDK を使用して {{site.data.keyword.visualrecognitionshort}} サービスに画像が送信されます。
3. サービスにより、分類分析を使用して画像が分析され、場面、物体、顔などが識別されます。
4. サービスによる分析結果が、Watson Swift SDK によってアプリに返されます。

## 始める前に
{: #prereqs-recognition}

以下の前提条件を満たしていることを確認してください。

* iOS 10.0 以上
* Xcode 9.3 以上
* Swift 4.1 以上
* CocoaPods、Carthage、または Swift Package Manager

[Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk) をインストールするには、[CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods)、[Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage)、または[Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager) を使用できます。CocoaPods (https://cocoapods.org/) を使用して依存関係を管理する場合、Watson Swift SDK 全体ではなく、必要なフレームワークだけを取得します。CocoaPods を使用したことがない場合は、次のように簡単にインストールできます。

```console
sudo gem install cocoapods
```
{: codeblock}

## ステップ 1. Visual Recognition のインスタンスの作成
{: #create-instance-recognition}

{{site.data.keyword.visualrecognitionshort}} サービスのインスタンスをプロビジョンします。

1. {{site.data.keyword.cloud_notm}} カタログで、**{{site.data.keyword.visualrecognitionshort}}** を選択します。 サービス構成画面が開きます。
2. サービス・インスタンスに名前を付けます。または、事前設定された名前を使用します。
3. インスタンスをアプリにバインドする場合は、**「接続」**メニューからアプリを選択します。
4. 料金プランを選択し、**「作成」**をクリックします。
5. **「資格情報」**タブを選択して、サービスの資格情報を表示します。 アプリからサービスに接続するために、これらの値が使用されます。

## ステップ 2. 依存関係のダウンロードと作成
{: #download-depend-recognition}

任意のテキスト・エディターを使用して、`pod init` を実行してプロジェクトのルート・ディレクトリー (`.xcodeproj` ファイルがある場所) に新しい `Podfile` を作成します。次に、依存関係として Watson Swift SDK の {{site.data.keyword.visualrecognitionshort}} フレームワークを指定する行を追加します。

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

実動アプリの場合は、特定の[バージョン要件](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions)を指定することにより、Watson Swift SDK の新リリースでの予期しない変更を回避することができます。

`Podfile` が作成され、依存関係をダウンロードできるようになりました。端末を使用してプロジェクトのルート・ディレクトリーにナビゲートし、次のように CocoaPods を実行します。

```console
pod install
```
{: codeblock}

Cocoapods により {{site.data.keyword.visualrecognitionshort}} フレームワークがダウンロードされ、プロジェクトの `Pods/` フォルダーにこのフレームワークが作成されます。

Pod build が失敗しないようにするため、プロジェクトを Xcode で開くときには `.xcodeproj` ではなく `.xcworkspace` で終わるファイルを開きます。
{: tip}

## ステップ 3. アプリでの画像の分析
{: #analyze-images-recognition}

以下の例は、(特に `ViewController.swift` で) アプリケーションに {{site.data.keyword.visualrecognitionshort}} の機能を追加する際に役立ちます。以下の例を使用すると、ユース・ケースに合わせて Visual Recognition 呼び出しを拡張できます。

1. Visual Recognition の import ステートメントを追加します。
    
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. API 鍵とバージョン (今日の日付を使用) を渡して、SDK を初期化します。
    
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  [バージョン・パラメーターの資料](https://cloud.ibm.com/apidocs/visual-recognition#versioning)を確認するか、{site.data.keyword.conversationshort}} サービスが作成された日付を使用できます。
  {: tip}

3. 画像を分類するため、次のコードを追加します。
    
  ```swift
  let url = "your-image-url"
  visualRecognition.classify(url: url) { response, error in
      if let error = error {
          print(error)
      }
      guard let classifiedImages = response?.result else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

Visual Recognition フレームワークでは複数の分類方法がサポートされています。Watson SDK [Visual Recognition の資料](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html)を参照して、各自のアプリケーションに最適な方法を確認してください。
{: tip}

## スターター・キットの使用
{: #recognition_starterkits}

[スターター・キット](https://cloud.ibm.com/developer/appledevelopment/starter-kits)は、{{site.data.keyword.cloud_notm}} の機能を素早く使用する方法の 1 つです。**Visual Recognition for iOS with Watson** スターター・キットを選択することにより、{{site.data.keyword.visualrecognitionshort}} サービスを使用できます。 このサービスは、画像を評価して分類します。 モバイル・デバイスから新しい画像や既存の画像をアップロードすると、Visual Recognition アプリがその画像コンテンツを素早くタグ付けし、分類します。

開始するには、以下のようにします。
1. [ここ](https://cloud.ibm.com/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson)にあるスターター・キットを選択します。
2. デフォルト・サービスを使用してプロジェクトを作成します。
3. **「コードのダウンロード (Download Code)」**をクリックしてプロジェクトをダウンロードします。 サービスの資格情報が、対応するキー・フィールドの `BMSCredentials.plist` ファイルに注入されます。

## 次のステップ
{: #recognition_next}

お疲れさまでした。 これで Visual Recognition がアプリから利用可能になりました。 この調子で、以下のいずれかのオプションを試してみてください。
* [Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window} を参照し、サポートされているその他の Watson サービスについて確認する。
* 詳しくは、[IBM Watson{{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/) を参照してください。
