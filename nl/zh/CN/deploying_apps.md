---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: deploy swift app, deploy swift, serverless swift, deploy swift cloud foundry, swift kubernetes, swift virtual server

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# 部署和集成 Swift 应用程序
{: #deploy_apps-swift}

可以使用工具链或命令行界面来部署 Swift 应用程序。工具链是一组工具集成。命令行界面是部署应用程序和服务实例的简单方法。


有关更多信息，请参阅[部署应用程序](/docs/apps?topic=creating-apps-create-deploy-app-cli#create-deploy-app-cli)。

## 开发无服务器 Swift 应用程序
{: #serverless-swift}

要进行无服务器开发，可以使用 IBM 的功能即服务 (FaaS) 产品：{{site.data.keyword.openwhisk}}。{{site.data.keyword.openwhisk_short}} 支持运行应用程序逻辑，通过 HTTP 来响应源自 Web 或移动应用程序的事件或直接调用，而无需创建或管理服务器。

{{site.data.keyword.openwhisk_short}} 会执行系统管理（例如，自动缩放、可用性管理以及维护），使开发者能够始终专注于编写应用程序逻辑。

有关更多信息，请参阅[开发无服务器应用程序](/docs/apps/deploying?topic=creating-apps-serverless#serverless)。

## 将后端服务与生成的 SDK 集成
{: #backend_gensdk-swift}

{{site.data.keyword.IBM_notm}} SDK Generator 插件可使用生成的 SDK 轻松将后端服务集成到应用程序。对 REST API 进行更改后，您可以重新生成 SDK，并替换旧 SDK 以升级 SDK。还可以将 CLI 集成到 DevOps 管道中，并确保每次构建应用程序时 SDK 都与 API 规范一致。

有关更多信息，请参阅[将后端服务与生成的 SDK 集成](/docs/swift/backend?topic=swift-sdkgen-cli#sdkgen-cli)。

## 部署到 Kubernetes 集群
{: #deploy_kube-swift}

您可以了解如何使用 {{site.data.keyword.cloud_notm}} Kubernetes Service 来部署利用 Watson Tone Analyzer 的容器化应用程序。在提供的场景中，一家虚构的公关公司使用 {{site.data.keyword.cloud_notm}} 服务分析自己的新闻稿并接收对其消息语气的反馈。有关更多信息，请参阅[将应用程序部署到 Kubernetes 集群](/docs/containers?topic=containers-cs_apps_tutorial)教程。

## 部署到 Cloud Foundry
{: #swift-deploy-cf}

此选项可部署云本机应用程序，而无需管理底层基础架构。

如果计划将应用程序部署到 [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about)，那么必须[准备 {{site.data.keyword.cloud_notm}} 帐户](/docs/cloud-foundry?topic=cloud-foundry-prepare)。

如果您的帐户有权访问 {{site.data.keyword.cfee_full_notm}}，那么可以选择部署程序类型**[公共云](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)**或**[企业环境](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)**，可使用这些类型来创建和管理隔离的环境，以用于专门为您的企业托管 Cloud Foundry 应用程序。

## 部署到虚拟服务器
{: #virtual_deploy-swift}

虚拟服务针对所有工作负载类型提供了更高程度的透明性、可预测性和自动化。Terraform 用于安全、高效地构建和更改基础架构以及控制基础架构的版本。

  VSI 部署可用于某些入门模板工具包。要使用此功能，请转至 [{{site.data.keyword.cloud_notm}} 仪表板](https://{DomainName})，然后单击**应用程序**磁贴中的**创建应用程序**。
  {: note}

有关更多信息，请参阅[部署到虚拟服务器](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server)。
