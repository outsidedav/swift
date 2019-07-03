---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: swift logging, ios logging, debug swift, add logging swift, heliumlogger swift, loggerapi swift, logger swift, starter kit swift logger

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Swift でのロギング
{: #logging_swift}

ログ・メッセージは、ログ・エントリーが作成された時点のマイクロサービスの状態およびアクティビティーに関するコンテキスト情報を含むストリングです。 ログは、サービスで障害が発生した状況や理由を診断するために必要です。またログは、アプリケーション正常性のモニタリングにおいて[アプリケーション・メトリック](/docs/swift/cloudnative?topic=swift-metrics#metrics)を補助する役割を果たします。

クラウド環境のプロセスの一過性の性質を考慮すると、ログを収集したら、分析のために他の場所 (通常は一元管理の場所) に送信する必要があります。 クラウド環境でロギングを行う最も一貫性のある方法は、ログ・エントリーを標準出力とエラー・ストリームに送信する方法です (こうするとインフラストラクチャーが残りを処理します)。

## Swift アプリへのロギングの追加
{: #logging-add}

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") は、Swift 用の一般的な軽量ロギング・フレームワークであり、標準出力へのロギングやさまざまなログ・レベルなど、ネイティブの利点を多数提供しています。

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") は、Swift での各種ロガーに対して共通のロギング・インターフェースを提供するロガー・プロトコルです。 Kitura では、その実装全体で `LoggerAPI` を使用します。

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

ここに示した例では、[ログ・レベル](http://ibm-swift.github.io/HeliumLogger/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") は `.verbose` (これがデフォルトです) に明示的に設定されています。

ログ・メッセージのカスタマイズについて詳しくは、公式の [HeliumLogger API 参照の資料](http://ibm-swift.github.io/HeliumLogger/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。

## スターター・キットを使用したロギング
{: #logging-starterkits}

{{site.data.keyword.cloud_notm}} App Service を使用して作成された Swift アプリは、デフォルトで `HeliumLogger` が付属しています。 アプリをネイティブまたはクラウド環境で実行すると、次の出力が生成されます。
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

これらのメッセージは、ローカルの `stdout` (標準出力)、または Cloud Foundry デプロイメントおよび Kubernetes デプロイメントのログにあります。これらのログには、`[ibmcloud app logs --recent <APP_NAME>]`(/docs/cli/reference?topic=cloud-cli-ibmcloud_commands_apps#ibmcloud_app_logs) および `[kubectl logs <deployment name>`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") でアクセスできます。

`/Sources/AppName/main.swift` ファイル内で、次のコードを確認できます。
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

ログ・レベルは、アプリケーションの開始ログのような情報レベル・メッセージをログに記録するために、`.info` に明示的に設定されます。
{: tip}

## 次のステップ
{: #next-logging notoc}

各デプロイメント・ターゲットでのログの表示について詳しくは、次の各ページを参照してください。
* [Kubernetes のログ](https://kubernetes.io/docs/concepts/cluster-administration/logging/#basic-logging-in-kubernetes){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")
* [Cloud Foundry のログ](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
* [Cloud Foundry Enterprise Environment - 監査とロギング](/docs/cloud-foundry?topic=cloud-foundry-auditing-logging#auditing-logging)
* [{{site.data.keyword.openwhisk}} のログおよびモニタリング](/docs/openwhisk?topic=cloud-functions-logs)

ログ統合機能を実装して使用する方法について、次の各ページを参照してください。
* [{{site.data.keyword.cloud_notm}} Log Analysis](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_analysis_ov#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Private ELK スタック](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")
