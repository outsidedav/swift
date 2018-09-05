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

# Swift でのロギング
{: #logging_swift}

ログは、サービスがどのように、なぜ失敗したのかを診断するために必要です。ログは、アプリケーションのパフォーマンスをモニターするために使用されることにはなっていませんが (メトリックはそのモニタリング用のものですが)、アラートのソースとして使用することができます。その上、ログには、集約されたメトリックから取得できる情報よりも詳細な情報を含めることができます。

クラウド・インフラストラクチャーと連携する利点の 1 つは、アプリケーションでログ・ファイルの管理などの、非常に多くの心配が不要になることです。クラウド環境での処理の過渡的な性質を考慮すると、ログを収集してどこか別の場所に (通常は分析用の一元的な場所に) 送信する必要があります。クラウド環境にログインする最も一貫性のある方法は、ログ・エントリーを標準出力とエラー・ストリームに送信し、インフラストラクチャーに残りのものを処理させることです。

アプリケーションが時間の経過とともに進化するにつれて、ログを採取するものの性質が変化する可能性があります。JSON のログ・フォーマットを使用すると、次の利点があります。
* ログの索引付けが可能です。これにより、ログの集約された本文の検索がはるかに容易になります。
* 構文解析はストリング内の要素の位置に依存しないため、ログは変化に対して弾力性があります。

JSON でフォーマット設定されたロギングを使用すると、コマンド・ライン・ツールを使用してログをフェッチする際に、ユーザーにとって読み取りが少し難しくなる可能性があります。ローカルでの開発とデバッグ用にプレーン・テキストのログを保持できるように、環境変数を使用して、使用するログ・フォーマットを切り替えることができます。

## Swift アプリへのロギングの追加

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) は、Swift 用の一般的な軽量ロギング・フレームワークであり、標準出力へのロギングやさまざまなログ・レベルなど、ネイティブの利点を多数提供しています。

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) は、Swift での各種ロガーに共通のロギング・インターフェースを提供するロガー・プロトコルです。Kitura では、その実装全体で `LoggerAPI` を使用します。

`HeliumLogger` を活用するには、`Package.swift` 内の **dependencies:** に次のコードを追加して、このコードが使用されるすべてのターゲット (target) にこのコードを必ず追加してください。
```swift
.package(url: "https://github.com/IBM-Swift/HeliumLogger.git", from: "1.7.1")
```
{: codeblock}

次の計測機能コードを追加して、`HeliumLogger` を初期化し、`LoggerAPI` で使用されるロガーとして設定します。
```swift
import HeliumLogger
import LoggerAPI

let logger = HeliumLogger(.verbose)
Log.logger = logger

Log.info("This is an informational log message.")
```
{: codeblock}

この示した例では、[ログ・レベル](http://ibm-swift.github.io/HeliumLogger/)は `.verbose` (これがデフォルトです) に明示的に設定されています。

ログ・メッセージのカスタマイズについて詳しくは、公式の [HeliumLogger API 参照の資料](http://ibm-swift.github.io/HeliumLogger/)を参照してください。

## StarterKits を使用したロギング
{: #monitoring}

{{site.data.keyword.cloud_notm}} App Service を使用して作成された Swift アプリは、デフォルトで `HeliumLogger` が付属しています。アプリをネイティブまたはクラウド環境で実行すると、次の出力が生成されます。
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

これらのメッセージは、ローカルでの実行では `stdout`、または [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) と [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/) のデプロイメント用のログで見つかります。これらのメッセージは、`ibmcloud app logs --recent <APP_NAME>` と `kubectl logs <deployment name>` によってそれぞれアクセスされます。

`/Sources/AppName/main.swift` ファイル内で、次のコードを確認できます。
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

ログ・レベルは、アプリケーションの開始ログのような情報レベル・メッセージをログに記録するために、`.info` に明示的に設定されます。
{: tip}

## 次のステップ
{: #next_steps}

各デプロイメント環境でのログの表示について詳しくは、次の各ページを参照してください。
* [Kubernetes のログ](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [Cloud Foundry のログ](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [{{site.data.keyword.openwhisk}} のログおよびモニタリング](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

ログ統合機能の使用:
* [{{site.data.keyword.cloud_notm}} Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Private ELK スタック](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
