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

# 使用 Swift 进行日志记录
{: #logging_swift}

日志消息字符串包含微服务在日志条目创建时的状态和活动的上下文相关信息。使用日志，可以诊断服务失败的方式和原因，将[应用程序度量值](appmetrics.html)与日志配置使用后，可以更好地监视应用程序运行状况。

云环境中的进程具有瞬态性质，因此必须将日志收集并发送到其他位置，通常是一个便于分析的集中位置。在云环境中，最常用的日志记录方式是将日志条目发送到标准输出和错误流，然后由基础架构来处理其余工作。


## 向 Swift 应用程序添加日志记录

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) 是 Swift 的常用轻量级日志记录框架，提供了许多本机优点（例如，日志记录到标准输出和不同日志级别）。

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) 是用于为 Swift 中不同类型的记录器提供公共日志记录接口的记录器协议。Kitura 在其整个实现中会使用 `LoggerAPI`。

要使用 `HeliumLogger`，请在 `Package.swift` 的 **dependencies:** 部分中为所有适当的目标添加以下代码：
```swift
.package(url: "https://github.com/IBM-Swift/HeliumLogger.git", from: "1.7.1")
```
{: codeblock}

添加以下检测代码以初始化 `HeliumLogger`，并将其设置为 `LoggerAPI` 使用的记录器：
```swift
import HeliumLogger
import LoggerAPI

let logger = HeliumLogger(.verbose)
Log.logger = logger

Log.info("This is an informational log message.")
```
{: codeblock}

在提供的示例中，[日志级别](http://ibm-swift.github.io/HeliumLogger/)显式设置为 `.verbose`，这是缺省值。

有关定制日志消息的更多信息，请参阅官方 [HeliumLogger API 参考文档](http://ibm-swift.github.io/HeliumLogger/)。

## 使用入门模板工具包进行日志记录
{: #monitoring}

缺省情况下，使用 {{site.data.keyword.cloud_notm}} App Service 创建的 Swift 应用程序随附 `HeliumLogger`。以本机方式或在云环境中运行应用程序会生成以下输出：
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

这些消息可在本地的 `stdout` 或 [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) 和 [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/) 部署的日志中找到，您可以通过运行以下命令来访问这些日志：`ibmcloud app logs --recent <APP_NAME>` 和 `kubectl logs <deployment name>`。

在 `/Sources/AppName/main.swift` 文件中，可以看到以下代码：
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

将日志级别显式设置为 `.info`，可记录参考级别的消息，如应用程序启动日志。
{: tip}

## 后续步骤
{: #next_steps}

了解有关查看每个部署环境中日志的更多信息：
* [Kubernetes 日志](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [Cloud Foundry 日志](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [{{site.data.keyword.openwhisk}} 日志和监视](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

了解如何实施和使用日志聚集器：
* [{{site.data.keyword.cloud_notm}} Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Private ELK 堆栈](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
