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

# 機械学習機能をアプリに追加する
{: #overview}

[Core ML](https://developer.apple.com/documentation/coreml){:new_window} を使用すると、さまざまなタイプの機械学習モデルをアプリに統合できます。30 を超えるタイプのレイヤーによる高度なディープ・ラーニングのサポートに加え、ツリー・アンサンブル、SVM、一般化線形モデルなどの標準的なモデルもサポートされています。Core ML では、分析するデータをリモート送信するのではなく、Metal や Accelerate などの低レベルのテクノロジーを使用し、CPU や GPU のメリットをシームレスに活用することにより最大限のパフォーマンスや効率を提供します。

## 始めに
{: #before-you-begin}

Core ML と Watson Visualization を使用するには、次のコンポーネントが必要です。

  * iOS 11.0 以上
  * Xcode 9.0 以上
  * Swift 4.0 以上
  * Carthage

Carthage をインストールするには、次の `brew` コマンドを使用します。
```
$ brew update
$ brew install carthage
```
{: codeblock}

## ステップ 1. {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}} によるモデルのトレーニング
{: #training-your-model}

現行モデルが存在しない場合は、リモートに最初に検出されたモデル、またはローカルに存在するモデルが使用されます。以下の gif および付属する説明は、サービスを Watson Studio にリンクし、モデルをトレーニングする方法を示しています。

![Core ML モデルのウォークスルー](images/CoreMLWalkthrough.gif)

### Core ML アプリ・ダッシュボードからサービスをセットアップする

1. スターター・キットのダッシュボードから**「ツールの起動 (Launch Tool)」**を選択することにより、Visual Recognition ツールを起動します。
2. **「モデルの作成 (Create Model)」**を選択することにより、モデルの作成を開始します。
2. 作成した Visual Recognition インスタンスにまだプロジェクトが関連付けられていない場合、プロジェクトが作成されます。関連付けられている場合は、**「モデルの作成 (Create Model)」**セクションに進みます。
3. プロジェクトに名前を付け、**「作成」**をクリックします。
**ヒント**: ストレージが定義されていない場合は、「更新」をクリックします。

### サービスをプロジェクトにバインドする

1. プロジェクトを作成すると、プロジェクト・ダッシュボードが表示されます。
2. 「設定」タブに移動し、**「関連サービス (Associated Services)」**までスクロールダウンし、**「サービスの追加」**->**「Watson」**を選択します。
3. Watson サービスのページから、**「Visual Recognition」**を選択します。
4. サービス情報のページで**「既存 (Existing)」**タブを選択した後、ダッシュボードからサービス・インスタンスを選択します。
5. これでサービスがバインドされました。プロジェクトのダッシュボードから**「アセット (Assets)」**を選択し、**「Visual Recognition モデルの追加 (Add Visual Recognition Model)」**をクリックすることにより、モデルの作成を開始できます。

### モデルの作成

1. 必要に応じて、モデル作成ツールから、分類子の名前を変更します。デフォルトでは、特に指定されていない限り、利用可能な最初の分類子がアプリケーションで使用されます。特定のモデルを使用する場合は、メイン・ビュー・コントローラーの `defaultClassifierID` フィールドを必ず変更してください。

2. サイドバーから、.zip ファイル内のモデル・トレーニング・コースをアップロードします。その後、各データ・セットを選択し、それらをドロップダウン・メニューからモデルに追加します。独自の画像セットを使用するクラスを追加することにより、分類子を拡張することもできます。

![クラスの追加](images/add_classes.png)

3. **「トレーニング・モデル (Train Model)」**を選択し、モデルが十分にトレーニングされるまで待ちます。

これで、準備が完了しました。これで、[{{site.data.keyword.watson}} Developer Cloud Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window} を使用することにより、Core ML モデルをダウンロードし、アプリに統合する準備ができました。

## ステップ 2. 依存関係のダウンロードと作成
{: #installing-dependencies}

1. 任意のテキスト・エディターを使用して、プロジェクトのルート・ディレクトリー (`.xcodeproj` ファイルがある場所) に **Cartfile** という新しいファイルを作成します。

2. 依存関係として {{site.data.keyword.watson}} Developer Cloud Swift SDK を指定する行を追加します。以下に例を示します。

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: pre}

  実動アプリの場合は、特定のバージョン要件を指定することにより、SDK の新リリースでの予期しない変更を回避することができます。{: tip}

3. 端末を使用してプロジェクトのルート・ディレクトリーにナビゲートし、次のように Carthage を実行します。

  ```
  $ carthage update --platform iOS
  ```
  {: pre}

  次に {{site.data.keyword.watson}} Developer Cloud Swift SDK がダウンロードされ、そのフレームワークが、プロジェクトの `Carthage/Build/iOS` フォルダー内に作成されます。

## ステップ 3. アプリへの画像分類機能の追加
{: #adding-image-classification}

```Swift
//
//  ViewController.swift
//  CoreMLImageClassificationExample
//
//  Created by Aaron Liberatore on 3/1/18.
//  Copyright © 2018 Aaron Liberatore. All rights reserved.
//

import UIKit
import VisualRecognitionV3

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        initializeCoreMLExample()
    }

    func failureHandler(_ error: Error) {
        print(error)
    }

    func initializeCoreMLExample() {
        // Configuration

        // Your Watson Visual Recognition service api key
        let apiKey = "your-apiKey-here"

        // Name of the classifier to use
        let classifierID = "your-classifier-here"

        // Minimum confidence threshold for image recognition
        let threshold = 0.5

        // Initialize Service
        let visualRecognition = VisualRecognition(apiKey: apiKey, version: "03-01-2018")

        // Update or download your model
        visualRecognition.updateLocalModel(classifierID: classifierID,
                                           failure: failureHandler) {

            // Classify your image using your model                                         
            visualRecognition.classifyWithLocalModel(image: image,
                                                     classifierIDs: [classifierID],
                                                     threshold: threshold,
                                                     failure: self.failureHandler) { classifiedImages in

                 print(classifiedImages)
             }            
        }
    }
}
```
{: codeblock}

## ステップ 4. スターター・キットの使用
{: #coreml_starterkits}

スターター・キットにより、{{site.data.keyword.cloud_notm}} および Core ML のさまざまな機能を素早く容易に活用できます。スターター・キットを使用することにより、{{site.data.keyword.visualrecognitionshort}} を任意のクライアント iOS アプリまたはサーバー・サイドのバックエンドに追加できます。{{site.data.keyword.watson}} スターター・キットによる Core ML の Custom Vision Model は、カスタム {{site.data.keyword.visualrecognitionshort}} モデルを作成して、デバイス上でローカルに実行できる Core ML のモデルとしてそのインスタンスを生成する方法を示しています。

{{site.data.keyword.visualrecognitionshort}} をスターター・キットに追加するには、次の手順を実行します。

1. 使用する[スターター・キット](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window}を選択します。
2. デフォルト・サービスを使用してプロジェクトを作成します。
3. **「リソースの追加」>「Watson」>「{{site.data.keyword.visualrecognitionshort}}」**をクリックします。
4. **「コードのダウンロード (Download Code)」**をクリックしてプロジェクトをダウンロードします。iOS プロジェクトでは、対応するキー・フィールドの `BMSCredentials.plist` ファイルに資格情報が挿入されます。サーバー・サイドの Swift プロジェクトでは、`config/local-dev.json` ファイルの中にそれらの資格情報が含まれています。

## 次のステップ
{: #coreml_next}

これで、カスタム生成された Core ML のモデルを使用することにより画像の分析をすることができるようになりました。この調子で、以下のいずれかのオプションを試してみてください。

* クラウド・ロジックを追加する。[サーバーレス・アプリの開発](/docs/swift/backend/functions.html)を開始します。
* {{site.data.keyword.visualrecognitionshort}} の提供するすべての機能を利用する。詳しくは、[ドキュメンテーション](/docs/services/visual-recognition/index.html)を参照してください。
