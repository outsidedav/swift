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

# 使用 Swift 进行日志记录
{: #logging_swift}

需要日志来诊断服务故障的方式和原因。日志并不用于监视应用程序性能，这是度量值的工作，而是用作警报源，这可使包含的详细信息多于从聚集度量值中获得的信息。

使用云基础架构的其中一个优点是，应用程序不必再操心许多工作，例如管理日志文件。由于云环境中进程具有瞬态性质，因此必须收集日志并将其发送到其他位置，通常会发送到集中位置进行分析。云环境最一致的日志记录方式是将日志条目发送到标准输出和错误流，其余工作则交由基础架构处理。

在应用程序随时间推移而变化时，所记录内容的性质可能会更改。通过使用 JSON 日志格式，您将获得以下优点：
* 日志可建立索引，这使得搜索日志的聚集主体容易得多。
* 日志具有变化弹性，因为解析不依赖于字符串中元素的位置。

使用 JSON 格式的日志记录时，在您使用命令行工具访存日志时，可能对于您（人类）来说，阅读日志的难度会略大。您可以使用环境变量来切换使用的日志格式，以便可以使用纯文本日志进行本地开发和调试。

## 向 Swift 应用程序添加日志记录

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) 是 Swift 的常用轻量级日志记录框架，提供了许多本机优点（例如，日志记录到标准输出和不同日志级别）。

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) 是用于为 Swift 中不同类型的记录器提供公共日志记录接口的记录器协议。Kitura 在其整个实施过程中会使用 `LoggerAPI`。

要利用 `HeliumLogger`，请将以下内容添加到 `Package.swift` 中的 **dependencies:**，并确保将其添加到使用它的任何目标。
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

缺省情况下，使用 {{site.data.keyword.cloud_notm}} App Service 创建的 Swift 应用程序随附 `HeliumLogger`。本机或在云环境中运行应用程序会生成以下输出：
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

这些消息可在 `stdout` 中（对于本地运行）或者在日志中（对于 [ CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) 和 [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/) 部署）找到，可分别通过 `ibmcloud app logs --recent <APP_NAME>` 和 `kubectl logs <deployment name>` 进行访问。

在 `/Sources/AppName/main.swift` 文件中，可以看到以下代码：
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

日志级别显式设置为 `.info`，以记录参考级别的消息，如应用程序启动日志。
{: tip}

## 后续步骤
{: #next_steps}

了解有关查看每个部署环境中日志的更多信息：
* [Kubernetes 日志](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [Cloud Foundry 日志](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [{{site.data.keyword.openwhisk}} 日志和监视](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

使用日志聚集器：
* [{{site.data.keyword.cloud_notm}} Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Private ELK 堆栈](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
