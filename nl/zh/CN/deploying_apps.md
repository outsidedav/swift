---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 部署和集成 Swift 应用程序
{: #deploy_apps-swift}

可以使用工具链或命令行界面来部署 Swift 应用程序。工具链是一组工具集成。命令行界面是部署应用程序和服务实例的简单方法。


有关更多信息，请参阅[部署应用程序](/docs/apps/dep-app-tool.html#deploying-apps)。

## 开发无服务器 Swift 应用程序
{: #serverless-swift}

要进行无服务器开发，可以使用 IBM 的功能即服务 (FaaS) 产品：{{site.data.keyword.openwhisk}}。{{site.data.keyword.openwhisk_short}} 支持运行应用程序逻辑，通过 HTTP 来响应源自 Web 或移动应用程序的事件或直接调用，而无需创建或管理服务器。

{{site.data.keyword.openwhisk_short}} 可以执行系统管理，如自动缩放、可用性管理和维护。这样，开发者就可以完全专注于编写应用程序逻辑。

有关更多信息，请参阅[开发无服务器应用程序](/docs/apps/deploying/functions.html#serverless)。

## 将后端服务与生成的 SDK 集成
{: #backend_gensdk-swift}

{{site.data.keyword.IBM_notm}} SDK Generator 插件可使用生成的 SDK 轻松将后端服务集成到应用程序。对 REST API 进行更改后，您可以重新生成 SDK，并替换旧 SDK 以升级 SDK。还可以将 CLI 集成到 DevOps 管道中，并确保每次构建应用程序时 SDK 都与 API 规范一致。

有关更多信息，请参阅[将后端服务与生成的 SDK 集成](/docs/swift/backend/cli_sdkgen.html#sdk-cli)。

## 部署到 Kubernetes 集群
{: #deploy_kube-swift}

您可以了解如何使用 {{site.data.keyword.cloud_notm}} Kubernetes Service 来部署利用 Watson Tone Analyzer 的容器化 Node.js 应用程序。在提供的场景中，一家虚构的公关公司使用 {{site.data.keyword.cloud_notm}} 服务分析自己的新闻稿并接收对其消息语气的反馈。有关更多信息，请参阅[将应用程序部署到 Kubernetes 集群](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial)教程。

## 部署到 Cloud Foundry
{: #swift-deploy-cf}

借助此选项，您无需管理底层的基础架构就可以部署云本机应用程序。

如果您计划将应用程序部署到 [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry/index.html#about)，那么您必须[准备 {{site.data.keyword.cloud_notm}} 帐户](/docs/cloud-foundry/prepare-account.html#prepare)。

如果您的帐户可以访问 {{site.data.keyword.cfee_full_notm}}，那么您可以选择部署程序类型**[公共云](/docs/cloud-foundry-public/about-cf.html#about-cf)**或者**[企业环境](/docs/cloud-foundry-public/cfee.html#cfee)**，您可以将其用于创建和管理隔离环境，专门为企业托管 Cloud Foundry 应用程序。

## 部署到虚拟服务器
{: #virtual_deploy-swift}

虚拟服务为所有工作负载类型提供更高的透明度、可预测性和自动化。Terraform 用于安全、高效地对基础架构进行构建、更改和版本控制。

有关更多信息，请参阅[部署到虚拟服务器](/docs/apps/vsi-deploy.html#vsi-deploy)。
