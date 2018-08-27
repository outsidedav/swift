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

# 在 Swift 中記載
{: #logging_swift}

一定要有日誌，才能診斷服務失敗的程度與原因。日誌並不是用來監視應用程式的效能，那是度量值的作用，而是用來作為警示的來源，且之後所包含的詳細資料，比您從聚集度量所取得的資料還多。

使用雲端基礎架構的其中一個好處是您的應用程式不需要再擔心許多事，例如，管理日誌檔。因為在雲端環境中，處理程序都是暫時的，因此日誌必須收集並傳送至其他地方，通常會傳送到一個集中位置，以進行分析。在雲端環境中進行日誌記載最一致的方法是，將日誌項目傳送至標準輸出及錯誤串流，然後讓基礎架構處理剩下的事宜。

因為您的應用程式會隨著時間有所進展，因此您所記載事物的本質也會變更。藉由使用 JSON 日誌格式，您可以獲得下列好處：
* 可以建立日誌的索引，以便更輕易地搜尋聚集的日誌內文。
* 日誌在變更後可以復原，因為剖析並非仰賴元素在字串中的位置。

當您使用指令行工具來提取日誌時，使用 JSON 格式的記載會讓您（人類）有點難閱讀日誌。您可以使用環境變數來切換要使用哪一個日誌格式，讓您有純文字日誌可以進行本端開發及除錯。

## 新增記載至 Swift 應用程式

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) 是適用於 Swift 的熱門輕量型記載架構，提供許多好處，例如，記載成標準輸出以及不同的記載層次。

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) 是日誌程式通訊協定，針對 Swift 中不同種類的日誌程式，提供通用的記載介面。Kitura 在整個實作過程都使用 `LoggerAPI`。

若要利用 `HeliumLogger`，請將下列新增至 `Package.swift` 中的 **dependencies:**，確定要將它新增至使用它的所有目標中。
```swift
.package(url: "https://github.com/IBM-Swift/HeliumLogger.git", from: "1.7.1")
```
{: codeblock}

新增下列設備測試代碼，以起始設定 `HeliumLogger`，並將其設為 `LoggerAPI` 所使用的日誌程式：
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

## 使用 StarterKits 記載
{: #monitoring}

依預設，使用 {{site.data.keyword.cloud_notm}} App Service 所建立的 Swift 應用程式，會隨附 `HeliumLogger`。在本端或在雲端環境中執行應用程式，會產生下列輸出：
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

如果在本端執行，這些訊息位於 `stdout`，如果用於 [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) 及 [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/) 部署，則位於日誌中，其分別由 `ibmcloud app logs --recent <APP_NAME>` 及 `kubectl logs <deployment name>` 所存取。

在 `/Sources/AppName/main.swift` 檔中，您可以看到下列程式碼：
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

記載層次明確設為 `.info`，以記載參考資訊層次的訊息，例如，應用程式啟動日誌。
{: tip}

## 後續步驟
{: #next_steps}

進一步瞭解在每一個部署環境中檢視日誌：
* [Kubernetes 日誌](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [Cloud Foundry 日誌](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [{{site.data.keyword.openwhisk}} 日誌 & 監視](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

使用日誌聚集器：
* [{{site.data.keyword.cloud_notm}} Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} 專用 ELK 堆疊](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
