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

# Swift アプリでのヘルス・チェックの使用
{: #healthcheck}

ヘルス・チェックには、サーバー・サイド・アプリケーションが正常に動作しているかどうかを判別するための単純なメカニズムが備わっています。[Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) や [Kubernetes](https://www.ibm.com/cloud/container-service) などの多くのデプロイメント環境は、サービスのインスタンスがトラフィックを受け入れる準備ができているかどうかを判別するために、ヘルス・エンドポイントを定期的にポーリングするように構成できます。

ヘルス・チェックは、通常 HTTP を介して使用され、`UP` または `DOWN` の状況を示すために標準の戻りコードを使用します。戻りコードの例としては、`UP` の場合の `200`、`DOWN` の場合の `5xx` があります。具体的には、アプリケーションで要求を処理できないか、またはアプリケーションがまだ開始されていない (そのサービスが利用できない) 場合に `503` が使用され、サーバーがエラー状態になった場合に `500` が使用されます。ヘルス・チェックの戻り値は変数ですが、`{“status”: “UP”}` のような最小の JSON 応答は一貫性を提供しています。

Cloud Foundry では、1 つのヘルス・エンドポイントを使用して、サービス・インスタンスが要求を処理できるかどうかを示します。Kubernetes では、ヘルス・チェック・エンドポイントは、`readiness` (作動可能) のエンドポイントと同等です。自動ルーティングの決定に役立てるには、このエンドポイントをアプリケーションで定義する必要があります。このエンドポイントの成功/失敗については、万一許容できるフォールバックがなにもない場合の、必要な下流の各サービスに対する考慮事項を含めることができます。下流の各サービスをチェックする場合、ヘルス・チェックによるシステム全体の負荷を最小限に抑えるために、結果のキャッシングが役立つことがあります。

Kubernetes では、余分の [liveness](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (活性) のエンドポイントを定義します。これにより、アプリケーションでプロセスを再始動する予定であるかどうかを示すことができます。ここでは、考慮事項のサブセットが当てはまります。例えば、特定のメモリー使用率のしきい値に達すると、`liveness` のチェックが失敗する可能性があります。アプリが Kubernetes で実行されている場合、必要な場合にプロセスが確実に再始動されるように、`liveness` のエンドポイントを追加することを検討してください。

## 既存の Swift アプリへのヘルス・チェックの追加
{: #add-healthcheck-existing}

[Health](https://github.com/IBM-Swift/Health) ライブラリーを利用すれば、Swift アプリケーションにヘルス・チェックを簡単に追加できます。ヘルス・チェックは拡張できます。DoS 攻撃を防止するための[キャッシング](https://github.com/IBM-Swift/Health#caching)、または[カスタム・チェック](https://github.com/IBM-Swift/Health#implementing-a-health-check)の追加について詳しくは、[Health](https://github.com/IBM-Swift/Health) ライブラリーを参照してください。

既存の Swift アプリに Health ライブラリーを追加するには、次に示すように、このライブラリーを `Package.swift` ファイルの *dependencies:* セクションに指定し、このライブラリーが使用されるすべてのターゲット (target) にこのライブラリーを必ず追加してください。
```swift
  .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
```
{: codeblock}

次に、アプリケーションに以下の初期化コードを追加します。
```swift
import Health

let health = Health()
```
{: codeblock}

その後、次のように経路定義を追加して、ヘルス・チェック・エンドポイントを定義します。
```
router.get("/health") { request, response, next in
  // let status = health.status.toDictionary()
  let status = health.status.toSimpleDictionary()
  if health.status.state == .UP {
    try response.send(json: status).end()
  } else {
    try response.status(.serviceUnavailable).send(json: status).end()
  }
}
```
{: codeblock}

ブラウザーで `/health` エンドポイントにアクセスして、アプリの状況をチェックします。コードは、簡単な辞書 (simpleDictionary) で定義されている、ペイロードの `{"status": "UP"}` を返します。

 **Codable** や標準の辞書の使用などの代替実装については、[Health ライブラリーの例](https://github.com/IBM-Swift/Health#usage)を参照してください。

## サーバー・サイドの Swift Starter Kit アプリからのヘルス・チェックへのアクセス
{: #healthcheck-starterkit}

Starter Kit を使用して Kitura ベースの Swift アプリを生成する場合、基本のヘルス・チェック・エンドポイントの `/health` がデフォルトで組み込まれます。このエンドポイントでは、Swift 4 で使用できる Codable プロトコルを活用します。
このプロトコルは、[Health](https://github.com/IBM-Swift/Health) ライブラリーでサポートされています。

基本の初期化コード (Health オブジェクトの初期化など) は、`Sources/Application.swift` で実行されますが、ヘルス・チェック・エンドポイントは、`/Sources/Application/Routes/HealthRoutes.swift` ファイル (次のコードが含まれています) によって提供されます。
```swift
import LoggerAPI
import Health
import KituraContracts

func initializeHealthRoutes(app: App) {

    app.router.get("/health") { (respondWith: (Status?, RequestError?) -> Void) -> Void in
        if health.status.state == .UP {
            respondWith(health.status, nil)
        } else {
            respondWith(nil, RequestError(.serviceUnavailable, body: health.status))
        }
    }

}
```
{: codeblock}

この例では、`/health` エンドポイントにアクセスする際に、標準の辞書を使用します。この辞書では、`{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}` などのペイロードを生成します。
