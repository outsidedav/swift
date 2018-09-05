---

copyright:
  years: 2018
lastupdated: "2018-08-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Kitura を使用したアプリケーションの作成
{: #kitura}

[Kitura](http://www.kitura.io) は、iOS バックエンドおよび Web アプリケーションを作成するためのサーバー・サイド Swift フレームワークです。このフレームワークは、Alamofire、RestKit、または Kitura 自体によって提供される [KituraKit](https://github.com/ibm-swift/kiturakit) SDK などの URLSession SDK を使用して、iOS アプリケーションから起動できる REST API を作成します。

Kitura は、{{site.data.keyword.cloud}} によって提供されるすべてのサービスおよび機能と統合できます。これには、{{site.data.keyword. appid_short}}、{{site.data.keyword.mobilepushshort}}、{{site.data.keyword.mobileanalytics_short}} の他、データベース、機械学習、およびその他のサービスが含まれます。その後、{{site.data.keyword.cloud}} で Cloud Foundry または Docker (Kubernetes ベース) のいずれかのプラットフォームを使用して、Kitura をデプロイし、自動的にスケーリングできます。

Kitura には、Kitura アプリケーションの作成、ビルド、テスト、およびデプロイを簡素化する `kitura` [コマンド・ライン・インターフェース (CLI)](http://www.kitura.io/en/starter/gettingstarted.html) があります。Kitura CLI を使用して作成されたアプリケーションには、Cloud Foundry、Docker、および Kubernetes テクノロジーをサポートするあらゆるクラウドにデプロイするための完全なサポートが組み込まれています。ただし、{{site.data.keyword.cloud}} 用に特別に作成している場合は、ブラウザーで IBM Apple Development Console を使用するか、{{site.data.keyword.dev_cli_notm}} を使用することをお勧めします。また、この両方の方法では基礎となるテクノロジーが共有されていると同時に、Apple Development Console および IBM Developer Tools は、ホストされたプロジェクトとデプロイメント・パイプラインを自動的に作成するとともに、アプリケーションで必要なサービスをプロビジョニングします。

## 始めに

まず、以下の前提条件が整っていることを確認してください。  

* iOS 11.0+  
* Xcode 9.0  
* Swift 4.0+  
* Cocoapods  

## ステップ 1. ブラウザーを使用した Kitura プロジェクトの作成

1. Apple Development Console の「スターター・キット (Starter Kits)」セクションに移動します。定義済みのスターター (「Swift for Backend for Frontend API」など) を選択するか、 **「プロジェクトの作成 (Create Project)」**タイルを使用してカスタム・プロジェクトを作成します。**「プロジェクトの作成 (Create Project)」**をクリックします。
2. プロジェクトに名前を付けて、プロジェクトをデプロイする場所を選択します。アプリケーションをデプロイする場所が不確かな場合は、デフォルト値を使用します。この値は後で変更できます。
3. Swift 言語を選択します。サーバー・サイド Swift プロジェクトが作成されます。また、アプリケーションの URL を形成する「ホストおよびドメインのオプション (Host and Domain options)」も表示されます。値が分からない場合は、デフォルト値を使用し、アプリケーションを常駐させるドメイン・プロバイダーからの独自のカスタム・ドメインを提供することもできます。
4. 作成する REST API の OpenAPI (Swagger とも呼ばれる) 定義を指定できます。このような定義がある場合は、対応するハンドラー関数を含む REST API が Kitura 内に作成されます。OpenAPI 定義がない場合でも、Kitura では、ルーター API を使用して手動で REST API を容易に作成できます。
5. **「プロジェクトの作成 (Create Project)」**をクリックします。

プロジェクトが作成されますが、そのプロジェクトでは追加のサービスや機能はまだ使用されていません。**「リソースの追加」**ボタンを使用するか、**「コードのダウンロード (Download Code)」**ボタンをクリックしてプロジェクトのコードを取得することによって、サービスを追加できます。既存のプロジェクトにサービスを簡単に追加することもできます。

## ステップ 2. サービスの追加

1. **「リソースの追加 (Add Resource)」**ボタンをクリックして、サービスを追加します。サービス・カテゴリーのパネルが表示されます。例えば、使用可能なデータベースを参照するには、**「データ (Data)」**を選択してから、**「Cloudant NoSQL DB」**を選択します。
2. サービスの価格プラン (例えば、Lite) を選択し、**「作成」**をクリックします。

アプリケーションのサービス資格情報を提供し、場合によっては、サービスへの正しい接続を含めるために必要なコードをプロジェクトに追加するサービスのインスタンスが作成されます。**「リソースの追加」**ボタンを使用するか、**「コードのダウンロード (Download Code)」**ボタンをクリックしてプロジェクトのコードを取得することにより、さらにサービスを追加できます。

プロジェクトをダウンロードすると、アプリで作業を開始できます。

## ステップ 3. Xcode を使用したアプリケーションの開発
プロジェクトをダウンロードしたら、Xcode を使用してそのプロジェクトを変更および開発し、変更したアプリケーションをアップロードしてクラウドにデプロイすることができます。

1. Xcode プロジェクトを作成します。  
  Xcode でプロジェクトを使用するには、Swift Package Manager コマンドを使用して Xcode プロジェクト・ファイルを作成しておく必要があります。ダウンロードしたプロジェクトのルート・ディレクトリー (`Package.swift` ファイルを格納) から次のコマンドを実行します。
  ```
  swift package generate-xcodeproj
  ```
  {: codeblock}

  Xcode プロジェクトが同じディレクトリーに作成され、開くことができるようになります。

2. プロジェクトの Xcode ターゲットを設定します。  
  Kitura サーバーを実行するには、ツールバーの **「project_name-Package」**セクションをクリックし、メニューから**「project_name」**ターゲットを選択して、スキームを編集する必要があります。ターゲット・デバイスが **My Mac** に設定されていることを確認します。

3. Kitura サーバーをローカルで実行します。  
  **「実行 (Run)」**ボタンをクリックするか、`⌘+R` キー・ショートカットを使用して、Kitura サーバーを始動します。サーバーが始動されると、以下の標準の Kitura URL が実行されていることを確認できます。
  * Kitura モニタリング: [http://localhost:8080/swiftmetrics-dash/]()
  * Kitura ヘルス・チェック: [http://localhost:8080/health]()

## ステップ 5. REST API の追加
スケルトン Kitura サーバーが作成されますが、iOS アプリケーションで使用できる REST API は提供されません。最小限のコーディングによって Kitura に REST API を追加します。以下のステップでは、`/meals` に対する `GET` 要求の REST API を追加する方法を示します。この `/meals` は、Kitura サーバーによって保管されている `Meal` オブジェクトを返すように設計されています。

1. `Meal` 構造体を `Sources/Application/Application.swift` ファイルに追加します。  
  App クラスの宣言の前に以下を追加して、Codable プロトコルに準拠する Meal 構造体を作成します。  
  ```swift
	struct Meal: Codable {    
	    var name: String
	    var photo: Data
	    var rating: Int
	}
  ```
  {: codeblock}

2. ディクショナリーを `Sources/Application/Application.swift` ファイルに追加して、`Meal` オブジェクトを保管します。これを行うには、`let cloudEnv = CloudEnv()` を次のコード・セクションに追加します。
 
  ```swift
  private var mealStore: [String: Meal] = [:]
  ```
  {: codeblock}

3. `/meals` に対する `GET` 要求のハンドラーを `Sources/Application/Application.swift` ファイルに追加します。これを行うには、次のコードを `postInit()` 関数に追加します。  

  ```swift
  router.get("/meals", handler: loadHandler)
  ```
  {: codeblock}

4. loadHandler 関数を `Sources/Application/Application.swift` ファイルに実装します。これを行うには、次のコードを `App` クラスの別の関数として追加します。  

  ```swift
  func loadHandler(completion: ([Meal]?, RequestError?) -> Void ) {
      let meals: [Meal] = self.mealStore.map({ $0.value })
    completion(meals, nil)
  }
  ```
  {: codeblock"}

これで、`Meals` の配列またはエラーで応答する `/meals` に対する `GET` 要求の REST API が利用できるようになりました。

5. プロジェクトを実行します。  
  **「実行 (Run)」**ボタンを押すか、`⌘+R` キー・ショートカットを使用します。プロンプトが出されたら、**「着信ネットワーク接続を許可する (Allow incoming network connections)」**を選択します。

6. REST API をテストします。`GET` 要求 (Web ブラウザーによるサーバーからのデータの要求方法と一致します) を使用して、次の URL を使用することにより REST API をテストできます。  
  ```
  * `GET /meals`:	[http://localhost:8080/meals]()
  ```

  空の配列 `[]` が返されます。これは、`Meal` オブジェクトが現在 `mealStore` に保管されていないためです。 

[FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend) チュートリアルを参照することをお勧めします。このチュートリアルは、データベースへのデータの保管を含め、iOS アプリケーションから `Meal` オブジェクトを保管、取り出し、および削除するための一連の REST API を作成する際に役立ちます。
{: tip}

## ステップ 6. iOS アプリケーションへの KituraKit のインストール
Kitura サーバーを使用して作成された REST API は、標準の Web API であり、使用しているクライアント・ライブラリーやクライアントの作成に使用する言語に関係なく、任意のアプリケーションから使用できます。このことは、Alamofire、RestKit、または URLSession を使用してサーバーに接続できることを意味します。Kitura はまた、iOS からの REST API の呼び出しを単純化するために、最適化されたカスタムのクライアント・コネクターを KituraKit の形式で提供します。 

KituraKit には、Kitura で使用されるルーター・ハンドラー API のミラー・イメージが用意されており、クライアントとサーバーの間で Swift タイプをほとんど手間をかけずに共有できます。この機能により、サーバーとの間で送受信されるデータの JSON エンコードとデコードを明示的に行う必要がなくなります。

以下のステップは、KituraKit を iOS アプリケーションにインストールし、Kitura を使用して作成された `GET /meals` REST API を KituraKit を使用して呼び出す方法を示します。

1. iOS アプリケーション・ディレクトリーのルートに Podfile を作成します (まだ存在していない場合)。
  ```
  pod init
  ```
  {: codeblock}

2. プロジェクト用の iOS 11 のグローバル・プラットフォームを設定するように Podfile を編集します。このために
  ```
  # platform :ios, '9.0'
  ```
  {: codeblock}

  上記の行を以下のコードで置き換えます。
	```
  platform :ios, '11.0'
  ```
  {: codeblock}

3. 次のコードを「# Pods for <application name>」の下に追加して、KituraKit を Podfile に追加します。
  ```
  pod 'KituraKit', :git => 'https://github.com/IBM-Swift/KituraKit.git', :branch => 'pod'
  ```
  {: codeblock}

4. Podfile を保存して、次のコマンドを使用して KituraKit をインストールします。
```
  pod install
  ```
  {: codeblock}

5. ワークスペース (プロジェクトではなく) を開くことによって、Xcode でアプリケーションを再オープンします。これで、Kitura サーバーで使用されているものと同じ「Meal」定義を iOS アプリケーションに追加できます。
```swift
  struct Meal: Codable {    
      var name: String
      var photo: Data
      var rating: Int
  }
  ```
  {: codeblock}

  次のコードを使用して、Kitura サーバーに接続します。

  ```swift
  import KituraKit
  
  // http://localhost:8080 の Kitura サーバーに接続
  guard let client = KituraKit(baseURL: "http://localhost:8080") else {
      print("Error creating KituraKit client")
      return
  }
  
  // `GET /meals` に要求を行い、[Meal] または RequestError が戻る
  client.get("/meals") { (meals: [Meal]?, error: RequestError?) in
      // エラーがないか確認する
      guard error == nil else {
          print("Error saving meal to Kitura: \(error!)")
          return
      }
      // Meal のチェック
      guard let meals = meals else {
          print("Received Meals!")
          return
      }
  }
  ```
  {: codeblock}

Kitura サーバーが、Kitura の完了ハンドラーのパラメーターと一致するコールバックで「client.get("/meals")」を使用して呼び出されます。
「FoodTrackerBackend」(https://github.com/IBM/FoodTrackerBackend) チュートリアルを参照することをお勧めします。このチュートリアルは、データベースへのデータの保管を含め、iOS アプリケーションから「Meal」オブジェクトを保管、取り出し、および削除するための一連の REST API を作成する際に役立ちます。
{: tip}

## 次のステップ
iOS アプリケーションで呼び出すことができる REST API を Kitura サーバーで提供できるようになったため、Kitura サーバーを {{site.data.keyword.cloud_notm}} にデプロイする準備ができました。デプロイメントは、Kubernetes、セキュア・コンテナー、または Cloud Foundry でコンテナーを使用して実行できます。
