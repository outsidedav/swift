---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: liveness probe swift, readiness probe swift, health swift, healthcheck swift, swift app status, kubernetes endpoint swift, health endpoint swift, swift health check

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Swift アプリでのヘルス・チェックの使用
{: #healthcheck}

ヘルス・チェックは、サーバー・サイド・アプリケーションが正常に動作しているかどうかを調べるための単純なメカニズムです。 定期的に health エンドポイントをポーリングして、サービスのインスタンスがトラフィックを受け入れる準備ができているかどうかを判断するよう、[Kubernetes](https://www.ibm.com/cloud/container-service){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") や [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") などのクラウド環境を構成することができます。
{: shortdesc}

## ヘルス・チェックの概要
{: #overview}

ヘルス・チェックは、サーバー・サイド・アプリケーションが正常に動作しているかどうかを調べるための単純なメカニズムです。 通常は HTTP を介して使用され、UP または DOWN の状況を示す標準の戻りコードを使用します。 ヘルス・チェックの戻り値は変数ですが、`{"status": "UP"}` といった最小限の JSON 応答が標準的です。

Kubernetes には、プロセスの正常性を詳細に示す概念があります。 次の 2 つのプローブが定義されています。

- _**readiness**_ プローブは、プロセスで要求を処理可能 (ルーティング可能) かどうかを示すために使用します。

  Kubernetes は、readiness プローブに失敗したコンテナーに処理をルーティングしません。 サービスが初期化を完了していない場合、ビジーまたは過負荷の状態にある場合、あるいは要求を処理できない場合に、readiness プローブは失敗します。

- _**liveness**_ プローブは、プロセスを再始動する必要があるかどうかを示すために使用します。

  Kubernetes は、liveness プローブに失敗したコンテナーを停止して再始動します。 liveness プローブは、内部プロセスがクラッシュした後にコンテナーが適切に停止しなかった場合や、メモリーが枯渇した場合など、リカバリー不能状態のプロセスをクリーンアップするために使用します。

なお、比較として、Cloud Foundry では 1 つの health エンドポイントを使用します。 このチェックが失敗するとプロセスは再始動しますが、成功すると要求はプロセスにルーティングされます。 この環境では、プロセスがライブ状態のとき、エンドポイントの成功は最小限になります。 再始動サイクルを回避するため、サービスの初期化が完了するまでヘルス・チェックを遅らせるように、初期待機時間を構成します。

以下の表では、readiness、liveness、単一の health の各エンドポイントで提供される応答について示します。

| 状態    | Readiness (作動可能)                   | Liveness (活性)                   | Health (正常性)                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | OK 以外ではロードなし       | OK 以外では再始動      | OK 以外では再始動     |
| Starting (開始中) | 503 - 使用不可           | 200 - OK                   | 遅延によりテストを回避   |
| Up (稼働中)       | 200 - OK                    | 200 - OK                   | 200 - OK                  |
| Stopping (停止中) | 503 - 使用不可           | 200 - OK                   | 503 - 使用不可         |
| Down (ダウン)     | 503 - 使用不可           | 503 - 使用不可          | 503 - 使用不可         |
| Errored (エラー)  | 500 - サーバー・エラー          | 500 - サーバー・エラー         | 500 - サーバー・エラー        |

## 既存の Swift アプリへのヘルス・チェックの追加
{: #existing-app}

[Health](https://github.com/IBM-Swift/Health){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") ライブラリーを利用すれば、Swift アプリケーションにヘルス・チェックを簡単に追加できます。ヘルス・チェックは拡張できます。 DoS 攻撃を回避するための [キャッシング](https://github.com/IBM-Swift/Health#caching){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") や、[カスタム・チェック](https://github.com/IBM-Swift/Health#implementing-a-health-check){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") の追加について詳しくは、[Health](https://github.com/IBM-Swift/Health){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") ライブラリーを参照してください。

Health ライブラリーを既存の Swift アプリに追加するには、以下の手順を参照してください。

1. `Package.swift` ファイルの *dependencies:* セクションで指定し、該当するすべてのターゲットに追加します。

    ```swift
    .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
    ```
    {: codeblock}

2. アプリケーションに以下の初期化コードを追加します。

    ```swift
    import Health

    let health = Health()
    ```
    {: codeblock}

3. 経路定義を追加して、ヘルス・チェック・エンドポイントを定義します。

    ```swift
    router.get("/health") { request, response, next in
        /* let status = health.status.toDictionary() */
        let status = health.status.toSimpleDictionary()
        if health.status.state == .UP {
            try response.send(json: status).end()
  } else {
            try response.status(.serviceUnavailable).send(json: status).end()
        }
    }
    ```
    {: codeblock}

4. ブラウザーで `/health` エンドポイントにアクセスして、アプリの状況をチェックします。 コードは、簡単な辞書 (simpleDictionary) で定義されている、ペイロードの `{"status": "UP"}` を返します。

## サーバー・サイド Swift スターター・キット・アプリのヘルス・チェック
{: #healthcheck-starterkit}

スターター・キットを使用して Kitura ベースの Swift アプリを生成する場合、基本のヘルス・チェック・エンドポイントの `/health` がデフォルトで組み込まれます。このエンドポイントでは、Swift 4 で使用できる Codable プロトコルを利用します。このプロトコルは、[Health](https://github.com/IBM-Swift/Health){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") ライブラリーでサポートされています。

基本の初期化コード (Health オブジェクトの初期化など) は、`Sources/Application.swift` で実行されます。 ヘルス・チェック・エンドポイント自体は、`/Sources/Application/Routes/HealthRoutes.swift` ファイルによって提供され、次のコードを使用します。

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

この例では標準の辞書を使用しています。この辞書は次のようなペイロードを生成します。
```
{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}
```

`/health` エンドポイントにアクセスすると、こうしたペイロードが生成されます。

## readiness プローブと liveness プローブに関する推奨事項
{: #recommend-probes}

Readiness チェックの結果には、ダウンストリームのサービスが使用不可の際に受け入れ可能なフォールバックが存在しない場合、ダウンストリーム・サービスへの接続の存続性が含まれていなければなりません。 ただし、ダウンストリーム・サービスによって提供されるヘルス・チェックを直接呼び出すわけではありません。それはインフラストラクチャーによってチェックされるからです。 代わりに、アプリケーションにある、ダウンストリーム・サービスに対する既存の参照の正常性を検査することを検討してください。これは、WebSphere MQ への JMS 接続、あるいは初期化されたKafka コンシューマーまたはプロデューサーが考えられます。 ダウンストリーム・サービスに対する内部参照の存続性をチェックする場合は、アプリケーションに及ぼすヘルス・チェックの影響を最小限に抑えるため、結果をキャッシュに入れてください。

一方、liveness プローブでは、失敗するとプロセスが即時に終了するため、チェック対象について注意を要します。 状況コードが `200` の `{"status": "UP"}` を常に戻す、単純な HTTP エンドポイントが妥当な選択です。

### Kubernetes の Readiness および Liveness のサポートを Swift アプリに追加する
{: #kube-readiness-swift}

**Codable** や標準の辞書の使用などの代替実装については、[Health ライブラリーの例](https://github.com/IBM-Swift/Health#usage){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。これらの実装の中には、バッキング・サービスに対して実行されるチェックをキャッシングするためのサポートによって、拡張可能なヘルス・チェックの作成を簡略化できるものがあります。 このシナリオでは、単純な liveness テストを、より堅固で詳細な readiness チェックから分離することができます。

## Kubernetes での readiness および liveness プローブの構成
{: #config-kube-readiness}

Kubernetes デプロイメントと共に liveness および readiness プローブを宣言します。 これらのプローブは両方とも、以下の同じ構成パラメーターを使用します。

* kubelet は、最初のプローブまで `initialDelaySeconds` 待機します。

* kubelet は、`periodSeconds` 秒ごとにサービスをプローブします。 デフォルトは、1 です。

* プローブは、`timeoutSeconds` 秒後にタイムアウトになります。 デフォルトおよび最小値は 1 です。

* プローブは、失敗の後に `successThreshold` 回成功すると、成功となります。 デフォルトおよび最小値は 1 です。liveness プローブの場合、この値は 1 でなければなりません。

* ポッドが開始され、プローブが失敗すると、Kubernetes はポッドの再始動を `failureThreshold` 回試行してから試行を停止します。 最小値は 1、デフォルト値は 3 です。
    - liveness プローブの場合、試行停止により、ポッドは再始動します。
    - readiness プローブの場合、試行停止により、ポッドは `Unready` とマーク付けされます。

再始動サイクルを回避するには、`livenessProbe.initialDelaySeconds` を、サービスの初期化に要する時間よりも十分長く設定してください。 その後、`readinessProbe.initialDelaySeconds` にそれより短い値を設定することで、サービスの準備ができ次第、要求をサービスにルーティングできるようにします。

次の `yaml` の例を参照してください。
```yaml
spec:
  containers:
  - name: ...
    image: ...
    readinessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 120
      timeoutSeconds: 5
    livenessProbe:
      httpGet:
        path: /liveness
        port: 8080
      initialDelaySeconds: 130
      timeoutSeconds: 10
      failureThreshold: 10
```

詳しくは、[Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")を参照してください。
