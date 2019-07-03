---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-05"

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

ヘルス・チェックには、サーバー・サイド・アプリケーションが正常に動作しているかどうかを判別するための単純なメカニズムが備わっています。 定期的に health エンドポイントをポーリングして、サービスのインスタンスがトラフィックを受け入れる準備ができているかどうかを判断するよう、[Kubernetes](https://www.ibm.com/cloud/container-service){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") や [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") などのクラウド環境を構成することができます。
{: shortdesc}

## ヘルス・チェックの概要
{: #overview}

ヘルス・チェックには、サーバー・サイド・アプリケーションが正常に動作しているかどうかを判別するための単純なメカニズムが備わっています。 通常は HTTP を介して使用され、標準的な戻りコードで UP または DOWN 状況が示されます。 ヘルス・チェックの戻り値は変数ですが、`{"status": "UP"}` のような最小の JSON 応答が標準です。

Kubernetes には、プロセス正常性についての詳細な概念があります。 次の 2 つのプローブが定義されています。

- _**readiness**_ プローブは、プロセスが要求を扱えるかどうか (ルーティング可能かどうか) を示すために使用されます。

  Kubernetes は、readiness プローブが失敗したコンテナーには作業をルーティングしません。 サービスが初期化を完了していない場合、あるいはそれ以外でビジー状態である場合、過負荷である場合、または要求を処理できない場合、readiness プローブは失敗する可能性があります。

- _**liveness**_ プローブは、プロセスを再始動する必要があるかどうかを示すために使用されます。

  Kubernetes は、liveness プローブが失敗したコンテナーを停止して再始動します。 liveness プローブは、内部プロセスがクラッシュした後にコンテナーが適切に停止しなかった場合や、メモリーが枯渇した場合など、リカバリー不能状態のプロセスをクリーンアップするために使用します。

なお、比較として、Cloud Foundry では 1 つの health エンドポイントを使用します。 この検査が失敗するとプロセスは再始動されますが、成功した場合、要求はプロセスにルーティングされます。 この環境では、プロセスがライブ状態であればエンドポイントは最低限成功します。 再始動サイクルを回避するため、サービスの初期化が完了するまでヘルス・チェックを遅らせるように、初期待機時間を構成します。

以下の表では、readiness、liveness、単一の health の各エンドポイントで提供される応答について示します。

| 状態    | readiness                   | liveness                   | health                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | OK 以外はロードなし       | OK 以外は再始動      | OK 以外は再始動     |
| Starting | 503 - 使用不可           | 200 - OK                   | テスト回避のために遅延を使用   |
| Up       | 200 - OK                    | 200 - OK                   | 200 - OK                  |
| Stopping | 503 - 使用不可           | 200 - OK                   | 503 - 使用不可         |
| Down     | 503 - 使用不可           | 503 - 使用不可          | 503 - 使用不可         |
| Errored  | 500 - サーバー・エラー          | 500 - サーバー・エラー         | 500 - サーバー・エラー        |
{: caption="表 1. HTTP 状況コード。" caption-side="bottom"}

## 既存の Swift アプリへのヘルス・チェックの追加
{: #existing-app}

[Health](https://github.com/IBM-Swift/Health){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") ライブラリーを利用すれば、Swift アプリケーションにヘルス・チェックを簡単に追加できます。 ヘルス・チェックは拡張できます。 DoS 攻撃を回避するための [キャッシング](https://github.com/IBM-Swift/Health#caching){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") や、[カスタム・チェック](https://github.com/IBM-Swift/Health#implementing-a-health-check){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") の追加について詳しくは、[Health](https://github.com/IBM-Swift/Health){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") ライブラリーを参照してください。

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

スターター・キットを使用して Kitura ベースの Swift アプリを生成する場合、基本のヘルス・チェック・エンドポイントの `/health` がデフォルトで組み込まれます。 このエンドポイントでは、Swift 4 で使用できる Codable プロトコルを利用します。このプロトコルは、[Health](https://github.com/IBM-Swift/Health){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") ライブラリーでサポートされています。

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

Readiness チェックの結果には、ダウンストリームのサービスが使用不可の際に受け入れ可能なフォールバックが存在しない場合、ダウンストリーム・サービスへの接続の存続性が含まれていなければなりません。 これは、ダウンストリーム・サービスが備えているヘルス・チェックを直接呼び出すということではありません。それはインフラストラクチャーが検査することになります。 代わりに、アプリケーションにおけるダウンストリーム・サービスへの既存の参照の正常性を検査することを検討してください。これに該当するものとして、WebSphere MQ への JMS 接続、あるいは初期化された Kafka コンシューマーまたはプロデューサーが考えられます。 ダウンストリーム・サービスへの内部参照の実行可能性を実際に検査する場合は、ヘルス・チェックがアプリケーションに与える影響を最小限に抑えるために、結果をキャッシュしてください。

一方、liveness プローブでは、失敗するとプロセスが即時に終了するため、チェック対象について注意を要します。 状況コードが `200` の `{"status": "UP"}` を常に返す単純な HTTP エンドポイントが妥当な選択です。

### Kubernetes の Readiness および Liveness のサポートを Swift アプリに追加する
{: #kube-readiness-swift}

**Codable** や標準の辞書の使用などの代替実装については、[Health ライブラリーの例](https://github.com/IBM-Swift/Health#usage){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。 これらの実装の中には、バッキング・サービスに対して実行されるチェックをキャッシングするためのサポートによって、拡張可能なヘルス・チェックの作成を簡略化できるものがあります。 このシナリオでは、単純な liveness テストを、より堅固で詳細な readiness チェックから分離することができます。

## Kubernetes の readiness プローブと liveness プローブの構成
{: #config-kube-readiness}

Kubernetes デプロイメントと併せて liveness プローブと readiness プローブを宣言します。 両方のプローブが同じ構成パラメーターを使用します。

* kubelet は最初のプローブの前に `initialDelaySeconds` 秒間待機します。

* `periodSeconds` 秒おきに kubelet がサービスをプローブします。 デフォルトは、1 です。

* `timeoutSeconds` 秒経過するとプローブがタイムアウトになります。 デフォルト値および最小値は 1 です。

* 失敗後 `successThreshold` 回成功するとプローブは成功です。 デフォルト値および最小値は 1 です。liveness プローブの場合、値は 1 でなければなりません。

* ポッドが開始され、プローブが失敗すると、Kubernetes はポッドの再始動を `failureThreshold` 回試行してから試行を停止します。 最小値は 1、デフォルト値は 3 です。
    - liveness プローブの場合、「中止する」はポッドを再始動することを意味します。
    - readiness プローブの場合、「中止する」はポッドに `Unready` のマークを付けることを意味します。

再始動サイクルを避けるために、安全を見てサービスの初期化時間よりも十分に長い時間を `livenessProbe.initialDelaySeconds` の値として設定してください。 `readinessProbe.initialDelaySeconds` にそれよりも短い値を使用することで、サービスの準備ができ次第、要求をサービスにルーティングできるようにします。

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
