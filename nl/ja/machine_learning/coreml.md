---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: coreml swift, core ml swift, watson swift core, create model swift, image classification swift, version parameter swift, swift coreml watson

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:gif: data-image-type='gif'}

# Watson での Core ML の使用
{: #swift-coreml}

[Core ML](https://developer.apple.com/documentation/coreml){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を使用すると、さまざまなタイプの機械学習モデルをアプリに統合できます。 30 を超えるタイプのレイヤーによる高度なディープ・ラーニングのサポートに加え、ツリー・アンサンブル、SVM、一般化線形モデルなどの標準的なモデルもサポートされています。 Core ML では、分析するデータをリモート送信するのではなく、Metal や Accelerate などの低レベルのテクノロジーを使用し、CPU や GPU のメリットをシームレスに活用することにより最大限のパフォーマンスや効率を提供します。

Core ML と Watson Visual Recognition を Swift アプリケーションに追加するには、以下の手順に従ってモデルをトレーニングおよび作成し、依存関係をダウンロードして作成し、画像分類機能を追加します。
{: shortdesc}

## 始める前に
{: #prereqs-coreml}

Core ML と Watson Visual Recognition を Swift で使用するには、次のコンポーネントが必要です。

* iOS 11.0 以上
* Xcode 9.3 以上
* Swift 4.1 以上
* CocoaPods、Carthage、または Swift Package Manager

[CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")、[Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")、または [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")
を使用することにより、[Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") をインストールできます。 CocoaPods を使用して依存関係を管理する場合、Watson Swift SDK 全体ではなく、必要なフレームワークだけを取得することになります。 CocoaPods を使用したことがない場合は、次のように簡単にインストールできます。

```console
sudo gem install cocoapods
```
{: codeblock}

## ステップ 1. {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}} によるモデルのトレーニング
{: #train-module-coreml}

現行モデルが存在しない場合は、リモートで最初に検出されたモデル、またはローカルに存在するモデルが使用されます。 以下の gif および付属する説明は、サービスを Watson Studio にリンクし、モデルをトレーニングする方法を示しています。

![Core ML モデルのウォークスルー](images/CoreMLWalkthrough.gif)

### Core ML アプリ・ダッシュボードからサービスをセットアップする
{: #service-coreml}

1. スターター・キットのダッシュボードから**「ツールの起動 (Launch Tool)」**を選択して、Visual Recognition ツールを開始します。
2. **「モデルの作成 (Create Model)」**を選択することにより、モデルの作成を開始します。
2. 作成した Visual Recognition インスタンスにまだプロジェクトが関連付けられていない場合、プロジェクトが作成されます。 関連付けられている場合は、**「モデルの作成 (Create Model)」**セクションに進みます。
3. プロジェクトに名前を付け、**「作成」**をクリックします。
  
  ストレージが定義されていない場合は、「更新」をクリックします。
  {: tip}

### サービスをプロジェクトにバインドする
{: #bind-service-coreml}

1. プロジェクトを作成すると、プロジェクト・ダッシュボードが表示されます。
2. 「設定」タブに移動し、**「関連サービス (Associated Services)」**までスクロールダウンし、**「サービスの追加」**->**「Watson」**を選択します。
3. Watson サービスのページから、**「Visual Recognition」**を選択します。
4. サービス情報のページで**「既存 (Existing)」**タブを選択した後、ダッシュボードからサービス・インスタンスを選択します。
5. これでサービスがバインドされました。プロジェクトのダッシュボードから**「アセット (Assets)」**を選択し、**「Visual Recognition モデルの追加 (Add Visual Recognition Model)」**をクリックすることにより、モデルの作成を開始できます。

### モデルの作成
{: #create-model-coreml}

1. モデル作成ツールから、分類子の名前を更新します。 特定のモデルを使用する場合は、メイン・ビュー・コントローラーの `defaultClassifierID` フィールドを必ず変更してください。

2. サイドバーから、圧縮 `.zip` ファイルに入れたモデル・トレーニング・コースをアップロードします。 その後、各データ・セットを選択し、ドロップダウン・メニューからモデルにそれらを追加します。 独自の画像セットを使用するクラスを追加することにより、分類子を拡張することもできます。

![クラスの追加](images/add_classes.png "Watson Studio へのサービスのリンク")

3. **「トレーニング・モデル (Train Model)」**を選択し、モデルが十分にトレーニングされるまで待ちます。

これで、準備が完了しました。 これで、[Watson Developer Cloud Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を使用して Core ML モデルをダウンロードし、アプリに統合する準備ができました。

## ステップ 2. 依存関係のダウンロードと作成
{: #install-depend-coreml}

任意のテキスト・エディターを使用して、`pod init` を実行してプロジェクトのルート・ディレクトリー (`.xcodeproj` ファイルがある場所) に新しい `Podfile` を作成します。 次に、依存関係として Watson Swift SDK の {{site.data.keyword.visualrecognitionshort}} フレームワークを指定する行を追加します。

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

実動アプリの場合は、特定の[バージョン要件](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を指定することにより、Watson Swift SDK の新リリースでの予期しない変更を回避することができます。

`Podfile` が作成され、依存関係をダウンロードできるようになりました。 端末を使用してプロジェクトのルート・ディレクトリーにナビゲートし、次のように CocoaPods を実行します。

```console
pod install
```
{: codeblock}

Cocoapods により {{site.data.keyword.visualrecognitionshort}} フレームワークがダウンロードされ、プロジェクトの `Pods/` フォルダーにこのフレームワークが作成されます。

Pod build が失敗しないようにするため、プロジェクトを Xcode で開くときには `.xcodeproj` ではなく `.xcworkspace` で終わるファイルを開きます。
{: tip}

## ステップ 3. アプリへの画像分類機能の追加
{: #add-image-coreml}

以下の例は、(特に `ViewController.swift` で) アプリケーションに {{site.data.keyword.visualrecognitionshort}} Core ML の機能を追加する際に役立ちます。 以下の例を使用すると、ユース・ケースに合わせてローカル・モデル呼び出しを拡張できます。

1. Visual Recognition の import ステートメントを追加します。
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. API 鍵とバージョンを渡して、SDK を初期化します。
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  [バージョン・パラメーターの資料](https://{DomainName}/apidocs/visual-recognition#versioning){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を確認するか、{site.data.keyword.visualrecognitionshort}} サービスが作成された日付を使用してください。
  {: tip}

3. Watson 分類子を使用してローカル Core ML モデルをダウンダウンロードまたは更新するため、次のコードを追加します。
  ```swift
  // Name of the classifier to use
  let classifierID = "your-classifier-ID-here"

  // Minimum confidence threshold for image recognition
        let threshold = 0.5

  // Update or download your model
  visualRecognition.updateLocalModel(classifierID: classifierID) { _, error in
      if let error = error {
          print(error)
      } else {
          print("model successfully updated")
      }                
  }
  ```
  {: codeblock}

4. ローカル Core ML モデルを使用して画像を分類するため、次のコードを追加します。
  ```swift
  let image = UIImage(named: "your-image-filename")
  let classifierID = "your-classifier-ID-here"
  let threshold = 0.5

  // Classify your image using your model                                         
  visualRecognition.classifyWithLocalModel(image: image, classifierIDs: [classifierID], threshold: threshold) { classifiedImages, error in
      if let error = error {
          print(error)
      }
      guard let classifiedImages = classifiedImages? else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

5. Watson SDK でサポートされているその他の [Core ML 分類機能](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を確認します。

## ステップ 4. スターター・キットの使用
{: #coreml_starterkits}

スターター・キットを使用すると、{{site.data.keyword.cloud_notm}} と Core ML の機能を素早く簡単に利用できます。 スターター・キットを使用することにより、{{site.data.keyword.visualrecognitionshort}} を任意のクライアント iOS アプリまたはサーバー・サイドのバックエンドに追加できます。 {{site.data.keyword.watson}} スターター・キットによる Core ML の Custom Vision Model は、カスタム {{site.data.keyword.visualrecognitionshort}} モデルを作成して、デバイス上でローカルに実行できる Core ML のモデルとしてそのインスタンスを生成する方法を示しています。

{{site.data.keyword.visualrecognitionshort}} をスターター・キットに追加するには、次の手順を実行します。

1. 使用する[スターター・キット](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を選択します。
2. デフォルト・サービスを使用してアプリを作成します。
3. **「サービスの追加」>「Watson」>「{{site.data.keyword.visualrecognitionshort}}」**をクリックします。
4. **「コードのダウンロード (Download code)」**をクリックしてプロジェクトをダウンロードします。 iOS プロジェクトでは、対応するキー・フィールドの `BMSCredentials.plist` ファイルに資格情報が挿入されます。 サーバー・サイドの Swift プロジェクトでは、`config/local-dev.json` ファイルの中にそれらの資格情報が含まれています。

## 次のステップ
{: #coreml_next}

これで、カスタム生成された各自のコア ML モデルを使用して画像を分析できます。 この調子で、以下のいずれかのオプションを試してみてください。

* [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照し、サポートされているその他の Watson サービスについて確認する。
* クラウド・ロジックを追加する。 [サーバーレス・アプリの開発](/docs/swift/backend?topic=swift-serverless-dev-swift#serverless-dev-swift)を開始します。
* {{site.data.keyword.visualrecognitionshort}} が提供するすべての機能を利用する。 詳しくは、[ドキュメンテーション](/docs/services/visual-recognition?topic=visual-recognition-index#index)を参照してください。
