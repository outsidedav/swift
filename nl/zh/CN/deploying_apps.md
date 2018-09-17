---

copyright:
  years: 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 部署和集成 Swift 应用程序
{: #deploy_apps}

可以使用工具链或命令行界面来部署 Swift 应用程序。工具链是一组工具集成。命令行界面是部署应用程序和服务实例的简单方法。


有关更多信息，请参阅[部署应用程序](../apps/dep-app-tool.html)。

## 开发无服务器 Swift 应用程序
{: #serverless}

为了方便无服务器开发，您可以使用 IBM 的功能即服务 (FaaS) 产品 {{site.data.keyword.openwhisk}}。{{site.data.keyword.openwhisk_short}} 支持运行应用程序逻辑，以通过 HTTP 响应来自 Web 或移动应用程序的事件或直接调用，而无需供应或管理服务器。

{{site.data.keyword.openwhisk_short}} 会执行系统管理，如自动扩展、可用性管理和维护，允许开发者始终专注于编写应用程序逻辑。

有关更多信息，请参阅[开发无服务器应用程序](../apps/deploying/functions.html)。

## 将后端服务与生成的 SDK 集成
{: #backend_gensdk}

{{site.data.keyword.IBM_notm}} SDK Generator 插件可使用生成的 SDK 轻松将后端服务集成到应用程序。发生对 REST API 的更改时，可以重新生成 SDK，并将旧的 SDK 替换为无缝 SDK 升级。您还可以将 CLI 集成到 DevOps 管道中，并确保每次构建应用程序时，SDK 都始终与 API 规范一致。

有关更多信息，请参阅[将后端服务与生成的 SDK 集成](/docs/swift/backend/cli_sdkgen.html)。

## 部署到 Kubernetes 集群
{: #deploy_kube}

您可以了解如何使用 {{site.data.keyword.cloud_notm}} Kubernetes 服务来部署利用 Watson Tone Analyzer 的容器化 Node.js 应用程序。在提供的场景中，一家虚构的公关公司使用 {{site.data.keyword.cloud_notm}} 服务分析自己的新闻稿并接收对其消息语气的反馈。有关更多信息，请参阅[将用程序部署到集群](../containers/cs_tutorials_apps.html)教程。

## 部署到虚拟服务器
{: #virtual_deploy}

虚拟服务为所有工作负载类型提供更高的透明度、可预测性和自动化。Terraform 用于安全、高效地对基础架构进行构建、更改和版本控制。

有关更多信息，请参阅[部署到虚拟服务器](../apps/vsi-deploy.html)。
