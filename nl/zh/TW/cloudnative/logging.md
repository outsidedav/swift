---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-04"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 在 Swift 中記載
{: #logging_swift}

日誌訊息是字串，具有在製作日誌項目時有關微服務狀態及活動的環境定義資訊。需要有日誌，才能診斷服務失敗的程度和原因，日誌對於監視應用程式性能時的[應用程式度量值](/docs/swift/cloudnative/appmetrics.html)扮演了支援角色。

因為在雲端環境中，處理程序都是暫時的，因此必須收集日誌並傳送至其他地方，通常會傳送到一個集中位置，以進行分析。在雲端環境中進行日誌記載最一致的方法是，將日誌項目傳送至標準輸出及錯誤串流，然後讓基礎架構處理剩下的事宜。

## 將記載新增至 Swift 應用程式
{: #logging-add}

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) 是適用於 Swift 的熱門輕量型記載架構，提供許多好處，例如，記載成標準輸出以及不同的記載層次。

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) 是日誌程式通訊協定，針對 Swift 中不同種類的日誌程式，提供通用的記載介面。Kitura 在整個實作過程都使用 `LoggerAPI`。

若要使用 `HelloumLogger`，請針對所有適當目標，將下列程式碼新增至 `Package.swift` 中的 **dependencies:** 區段：
```swift
.package(url: "https://github.com/IBM-Swift/HeliumLogger.git", from: "1.7.1")
```
{: codeblock}

新增下列檢測代碼，以起始設定 `HeliumLogger`，並將其設為 `LoggerAPI` 所使用的日誌程式：
```swift
import HeliumLogger
import LoggerAPI

let logger = HeliumLogger(.verbose)
Log.logger = logger

Log.info("This is an informational log message.")
```
{: codeblock}

在提供的範例中，[記載層次](http://ibm-swift.github.io/HeliumLogger/)明確設為 `.verbose`，此為預設值。

如需自訂日誌訊息的相關資訊，請參閱官方的 [HeliumLogger API 參考資料文件](http://ibm-swift.github.io/HeliumLogger/)。

## 使用入門範本套件記載
{: #logging-starterkits}

依預設，使用 {{site.data.keyword.cloud_notm}} App Service 所建立的 Swift 應用程式，會隨附 `HeliumLogger`。在本機或在雲端環境中執行應用程式，會產生下列輸出：
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

這些訊息位於本端的 `stdout` 中，或 [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) 及 [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/) 部署的日誌中，其由 `ibmcloud app logs --recent <APP_NAME>` 及 `kubectl logs <deployment name>` 存取。

在 `/Sources/AppName/main.swift` 檔案中，您可以看到下列程式碼：
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

記載層次明確設為 `.info`，以記載參考資訊層次的訊息，例如，應用程式啟動日誌。
{: tip}

## 後續步驟
{: #next-logging}

進一步瞭解在每一個部署環境中檢視日誌：
* [Kubernetes 日誌](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [Cloud Foundry 日誌](/docs/cli/reference/ibmcloud/bx_cli.html)
* [Cloud Foundry Enterprise Environment - 審核及記載](docs/cloud-foundry/auditing-logging.html)
* [{{site.data.keyword.openwhisk}} 日誌及監視](/docs/openwhisk/openwhisk_logs.html)

了解如何實作及使用日誌聚集器：
* [{{site.data.keyword.cloud_notm}} Log Analysis](/docs/services/CloudLogAnalysis/log_analysis_ov.html)
* [{{site.data.keyword.cloud_notm}} 專用 ELK 堆疊](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
