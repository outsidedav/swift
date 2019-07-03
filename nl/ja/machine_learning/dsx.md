---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: watson studio swift, machine learning swift, custom model swift, data set swift, predictive swift, watson api swift, generated model swift, dataset swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# カスタム生成モデルによるデータ・セットの分析
{: #dsx-overview}

Watson Studio には、データ分析で協力することによりビジネス上の問題を解決するための環境やツールが用意されています。 データを分析、クレンジング、および編成するために必要なツールを選択できます。 ストリーミング・データの取り込み、または機械学習モデルの作成、トレーニング、およびデプロイについて説明します。 Watson Studio は、さまざまな {{site.data.keyword.cloud}} サービスおよび Watson ナレッジ・カタログに統合されています。それは、資産を制御するためのポリシー管理機能を提供し、カタログすることによりそれらを検索するためのインデックスを作成します。 [詳細はこちら](https://dataplatform.cloud.ibm.com/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")。

Watson Studio は、プロジェクト・ベースのアーキテクチャーを基に構造化されています。それにより、ビジネス上の問題を解決するようにリソースが編成されます。 リソースには、クラウドおよびオンプレミスのデータ・ストアへの接続、データ・ファイル、コラボレーター、およびモデルのなどの分析アセットが含まれます。 詳しくは、[Watson Studio の概要](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/overview-ws.html){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。

## {{site.data.keyword.DSX}} の機械学習
{: #dsx-learning}

{{site.data.keyword.DSX}} を使用することにより、モデルをトレーニングし、それらをデプロイしてから、API を使用して結果を利用することができます。 それらの API は、iOS または Swift のアプリケーションで使用できます。

IBM Watson Machine Learning を使用すると、自分の環境をセットアップした後、モデルを作成し、それらをクラウドにデプロイし、それらをトレーニングすることができます。 詳しくは、「[{{site.data.keyword.pm_full}} および {{site.data.keyword.DSX}} によるモデルの作成、デプロイ、およびトレーニング](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/wml-ai.html){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")」を参照してください。

### チュートリアル
{: #dsx-tutorials}

## iOS および Swift による {{site.data.keyword.DSX}} のセットアップ
{: #dsx_ios}

1. 資格情報の統合を容易にするため、{{site.data.keyword.pm_short}} インスタンスを iOS アプリまたはバックエンド・アプリに追加する必要があります。 アクセシビリティーを容易にするため、資格情報がプロジェクト・ダッシュボードに含められます。

![アプリの詳細](images/ios-machinelearning-app.png "アプリの詳細")

2. アプリ・コードをダウンロードします。
3. 初期化
  * iOS プロジェクトでは、{{site.data.keyword.pm_short}} サービスを iOS プロジェクトに追加するだけで、資格情報がすぐにアプリに挿入されます。
    アプリケーションから資格情報にアクセスするには、次のコード・スニペットをコピーし、貼り付けます。 また、スコアリング・エンドポイントをアプリに必ず追加するようにします。それは、モデルのデプロイメントの`「実装」`タブ内にあります。

    ```swift
    /* The url to your model's scoring endpoint */
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"

    /* Your credentials */
    var machineLearningUsername: String!
    var machineLearningPassword: String!

    /* Machine Learning initialization */
    if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"),
       let dictionary = NSDictionary(contentsOfFile: contents),
       let username = dictionary["machinelearningUsername"] as? String,
       let password = dictionary["machinelearningPassword"] as? String {

           machineLearningUsername = username
           machineLearningPassword = password

    }
    ```
    {: codeblock}

  * サーバー・サイド・アプリケーションでは、ユーザー名とパスワードをアプリケーションおよびスコアリング・エンドポイントに手動で追加します。これらは、モデルのデプロイメントの`「実装」`タブで確認できます。

    ```swift
    /* Your Machine Learning Credentials */
    let machineLearningUsername: String = "<your-ml-service-username>"
    let machineLearningPassword: String = "<your-ml-service-password>"

    /* The url to your model's scoring endpoint */
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"
    ```
    {: codeblock}

4. アクセス・トークンを取得し、シンプルなクライアント SDK によりアプリケーションからデータ・セットに対して予測分析を実行します。

  ```swift
  public class MachineLearning {

      private let username: String
      private let password: String

      public init(username: String, password: String) {
          self.username = username
          self.password = password
      }

      public static func failure(_ error: Error) {
          print("Error:", error.localizedDescription)
      }

      public func retrieveToken(failure: @escaping (Error) -> Void = MachineLearning.failure, success: @escaping (String) -> Void) {

          guard var request = createRequest(url: "https://ibm-watson-ml.mybluemix.net/v3/identity/token") else {
              failure(NSError(domain: "Could not create url", code: 1, userInfo: nil))
              return
          }

          guard let authString = (username + ":" + password).data(using: .utf8)?.base64EncodedString() else {
              failure(NSError(domain: "Could not encode credentials", code: 1, userInfo: nil))
              return
          }

          request.setValue("Basic \(authString)", forHTTPHeaderField: "Authorization")

          execute(request, failure: failure) { dict in

              guard let token = dict["token"] as? String else {
                  failure(NSError(domain: "Invalid Response", code: 1, userInfo: nil))
                  return
              }

              success(token)
          }
      }

      public func retrieveScore(_ scoringUrl: String, token: String, payload: [String: Any], failure: @escaping (Error) -> Void  = failure, success: @escaping ([String: Any]) -> Void) {

          guard let data = try? JSONSerialization.data(withJSONObject: payload, options: .prettyPrinted) else {
              failure(NSError(domain: "Could not encode data", code: 1, userInfo: nil))
              return
          }

          guard var request = createRequest(method: "POST", url: scoringUrl, body: data) else {
              failure(NSError(domain: "Could not create url", code: 1, userInfo: nil))
              return
          }

          request.setValue("Bearer " + token, forHTTPHeaderField: "Authorization")
          request.setValue("application/json", forHTTPHeaderField: "Content-Type")

          execute(request, failure: failure, success: success)
      }

      public func createRequest(method: String = "GET", url: String, body: Data? = nil) -> URLRequest? {
          guard let url = URL(string: url) else {
              return nil
          }
          var req = URLRequest(url: url)
          req.httpMethod = method
          req.httpBody = body
          return req
      }

      private func execute(_ request: URLRequest, failure: @escaping (Error) -> Void, success: @escaping ([String: Any]) -> Void) {
          URLSession.shared.dataTask(with: request) { data, _, error in

              guard let data = data, error == nil else {
                  let error = error ?? NSError(domain: "No data was returned", code: 1, userInfo: nil)
                  failure(error)
                  return
              }

              guard let body = try? JSONSerialization.jsonObject(with: data, options: .allowFragments) as? [String: Any],
                    let dict = body else {
                  failure(NSError(domain: "Could not create dictionary", code: 1, userInfo: nil))
                  return
              }

              guard let resp = response as? HTTPURLResponse , resp.statusCode == 200 else {
                 let msg = (dict["errors"] as? [[String: Any]])?.first?["message"] as? String ?? "An error occurred"
                 let code = (response as? HTTPURLResponse)?.statusCode ?? 1
                 failure(NSError(domain: msg, code: code, userInfo: nil))
                 return
             }

              success(dict)

          }.resume()
      }
  }
  ```
  {: codeblock}

### 例
{: #dsx-example}

**シナリオ名:** 製品ラインの予測

**シナリオの説明:** アウトドア用品を販売する会社が、製品ラインのうち顧客の関心を集めるものを予測するモデルを作成し、デプロイします。 開発者のタスクは、デプロイされたモデルに対してスコアリング要求を行うことです。

モデルがデプロイされたら、スコアリング・エンドポイントを使用して予測分析を実行できます。

```swift
// The data you want to have analyzed
let examplePayload: [String: Any] = [
    "fields": ["GENDER", "AGE", "MARITAL_STATUS", "PROFESSION"],
    "values": [["M", 23, "Single", "Student"],["M", 55, "Single", "Executive"]]
]

// Your service credentials
let machineLearningUsername: String = "<your-ml-service-username>"
let machineLearningPassword: String = "<your-ml-service-password>"

// Your scoring end-point
let scoringUrl: String = "<your-model-scoring-url>"

// Define your client
let client = MachineLearning(username: username, password: password)

// Retrieve your access token and score your payload!
client.retrieveToken { token in
    client.retrieveScore(scoringUrl, token: token, payload: examplePayload) { dict in
        print(dict)
    }
}
```
{: codeblock}

## 次のステップ
{: #dsx_next notoc}

お疲れさまでした。 これで、カスタム生成の機械学習モデルを使用してデータ・セットを分析できるようになりました。 この調子で {{site.data.keyword.pm_short}} が提供する機能について、[Data science and machine learning](https://www.ibm.com/analytics/machine-learning){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") でさらに学習してください。

### 関連リンク
{: #dsx-related}

* [{{site.data.keyword.pm_short}}](/docs/services/PredictiveModeling?topic=PredictiveModeling-WMLgettingstarted)
* [{{site.data.keyword.DSX}}](https://www.ibm.com/cloud/watson-studio){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")
* [{{site.data.keyword.DSX}} の資料 ](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/welcome-main.html){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")
