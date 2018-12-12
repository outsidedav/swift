---

copyright:
  years: 2018
lastupdated: "2018-11-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Swift でのロギング
{: #logging_swift}

ログ・メッセージは、ログ・エントリーが作成された時点のマイクロサービスの状態およびアクティビティーに関するコンテキスト情報を含むストリングです。 ログは、サービスで障害が発生した状況や理由を診断するために必要です。またログは、アプリケーション正常性のモニタリングにおいて[アプリケーション・メトリック](appmetrics.html)を補助する役割を果たします。

クラウド環境のプロセスには過渡的な性質があるため、ログは、収集して他の場所に送信する必要があります。通常は分析用の一元的な場所に送信します。句クラウド環境でのロギング方法として最も一貫した方法は、ログ・エントリーを標準出力とエラー・ストリームに送信し、後の処理はインフラストラクチャーに任せるという方法です。


## Swift アプリへのロギングの追加

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) は、Swift 用の一般的な軽量ロギング・フレームワークであり、標準出力へのロギングやさまざまなログ・レベルなど、ネイティブの利点を多数提供しています。

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) は、Swift での各種ロガーに対して共通のロギング・インターフェースを提供するロガー・プロトコルです。 Kitura では、その実装全体で `LoggerAPI` を使用します。

`HeliumLogger` を使用するには、該当するすべてのターゲットに対して、`Package.swift` 内の **dependencies:** セクションに次のコードを追加します。
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

## スターター・キットを使用したロギング
{: #monitoring}

{{site.data.keyword.cloud_notm}} App Service を使用して作成された Swift アプリは、デフォルトで `HeliumLogger` が付属しています。 アプリをネイティブまたはクラウド環境で実行すると、次の出力が生成されます。
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

これらのメッセージは、ローカルでは `stdout`、または [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) と [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/) のデプロイメント用のログで見つかります。これらのメッセージは、`ibmcloud app logs --recent <APP_NAME>` と `kubectl logs <deployment name>` でアクセスします。

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

ログ統合機能を実装して使用する方法について、次の各ページを参照してください。
* [{{site.data.keyword.cloud_notm}} Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Private ELK スタック](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
