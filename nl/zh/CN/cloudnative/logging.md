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

# 使用 Swift 进行日志记录
{: #logging_swift}

日志消息字符串包含微服务在日志条目创建时的状态和活动的上下文相关信息。使用日志，可以诊断服务失败的方式和原因，将[应用程序度量值](/docs/swift/cloudnative?topic=swift-metrics#metrics)与日志配置使用后，可以更好地监视应用程序运行状况。

考虑到云环境中进程的瞬态性质，必须收集日志并将其发送到其他位置（通常是集中位置）以供分析。在云环境中进行日志记录最一致的方法是将日志条目发送到标准输出和错误流，从而使基础架构处理其余事务。

## 向 Swift 应用程序添加日志记录
{: #logging-add}

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 是 Swift 的常用轻量级日志记录框架，提供了许多本机优点（例如，日志记录到标准输出和不同日志级别）。

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 是用于为 Swift 中不同类型的记录器提供公共日志记录接口的记录器协议。Kitura 在其整个实现中会使用 `LoggerAPI`。

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

在提供的示例中，[日志级别](http://ibm-swift.github.io/HeliumLogger/){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 显式设置为 `.verbose`，这是缺省值。

有关定制日志消息的更多信息，请参阅官方 [HeliumLogger API 参考文档](http://ibm-swift.github.io/HeliumLogger/){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。

## 使用入门模板工具包进行日志记录
{: #logging-starterkits}

缺省情况下，使用 {{site.data.keyword.cloud_notm}} App Service 创建的 Swift 应用程序随附 `HeliumLogger`。以本机方式或在云环境中运行应用程序会生成以下输出：
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

这些消息可在本地的 `stdout`（标准输出）中或 Cloud Foundry 和 Kubernetes 部署的日志中找到，可通过 `[ibmcloud app logs --recent <APP_NAME>]`(/docs/cli/reference?topic=cloud-cli-ibmcloud_commands_apps#ibmcloud_app_logs) 和 `[kubectl logs <deployment name>`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 对日志进行访问。

在 `/Sources/AppName/main.swift` 文件中，可以看到以下代码：
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

将日志级别显式设置为 `.info`，可记录参考级别的消息，如应用程序启动日志。
{: tip}

## 后续步骤
{: #next-logging notoc}

了解有关查看每个部署目标中日志的更多信息：
* [Kubernetes 日志](https://kubernetes.io/docs/concepts/cluster-administration/logging/#basic-logging-in-kubernetes){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")
* [Cloud Foundry 日志](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
* [Cloud Foundry Enterprise Environment - 审计和日志记录](/docs/cloud-foundry?topic=cloud-foundry-auditing-logging#auditing-logging)
* [{{site.data.keyword.openwhisk}} 日志和监视](/docs/openwhisk?topic=cloud-functions-logs)

了解如何实施和使用日志聚集器：
* [{{site.data.keyword.cloud_notm}} Log Analysis](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_analysis_ov#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Private ELK 堆栈 ](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")
