---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip}

# 无服务器开发
{: #serverless}

什么是无服务器？无服务器开发模式是指服务器端逻辑在无状态容器中运行的应用程序开发。这些容器是通过事件触发的临时容器（有效持续时间为单次执行的时间），并且由第三方完全管理。此范式也称为功能即服务 (FaaS)，在其中开发者可提供无状态功能，此类功能可在不明确创建或管理服务器的情况下触发和运行。

无服务器体系结构通过抽象出服务器端开发所需的基础架构和框架，使开发人员能够专注于编写代码，让代码以反应性方式运行来更改数据。

IBM 的 FaaS 产品 [{{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/) 致力于提供简单的服务器端开发体验，无需任何服务器端专业知识。通过使用无服务器技术，您可以快速开发可扩展的后端解决方案，以满足几乎所有工作负载需求，而不必提前创建资源。对于负载模式不可预测或服务器停机时间长的应用程序，{{site.data.keyword.openwhisk_short}} 会是一个出色的云解决方案，其性能已经过改进，而且其“按使用内容付费”机制还有助于降低成本。

## 体系结构更改
{: #comparison}

为了帮助您了解切换到 FaaS 的体系结构优点，将使用一个与数据库链接的简单 iOS 应用程序来比较传统体系结构和 FaaS 体系结构。

在较为传统的体系结构中，iOS 应用程序会卸载网络密集型任务，或者以远程方式在中央服务器上处理数据，应用程序可通过中央服务器的服务或存储选项与之进行连接。繁重的工作都落在单台服务器上，该服务器不仅要处理许多密集型任务以尽可能减小客户端的压力，而且还要为整个用户群同步数据。

无服务器体系结构可以将此结构变更为更类似于下图。

![](./images/Architecture.png) 图 1. 无服务器体系结构

与在单台服务器内执行所有处理和认证逻辑不同，无服务器体系结构使用功能来封装大部分服务器端逻辑，并将某些逻辑卸载到客户端（和外部服务）。

从该示意图中，可以看出以下几点:

1. 客户端根据身份提供者（如应用程序标识）进行认证。
2. 调用包含访问令牌的 FaaS 后端 API。
3. 后端通过 {{site.data.keyword.openwhisk_short}} 实现。无服务器操作会以 Web 操作形式公开，并且期望在请求头中发送令牌来进行验证（签名和到期日期），从而提供对实际 API 的访问。
4. 客户端提交数据后，反馈会存储在 {{site.data.keyword.cloudant_short_notm}} 中。
5. 反馈文本通过 {{site.data.keyword.toneanalyzershort}} 进行处理。
6. 根据分析结果，{{site.data.keyword.mobilepushshort}} 会将通知发回客户端。
7. 客户端收到通知。

在纯无服务器模型中，由于无法存储用户状态，客户端通常会承担额外责任。授权由客户端和 {{site.data.keyword.appid_short_notm}} 身份提供者服务进行处理。

虽然无服务器架构并不总是理想的，但在适当的团队和使用条件下，还是可以提供非常多的好处。请查看一些特定示例，以了解一些最常见[用例](#use_cases)的信息。

## 无服务器优点
{: #benefits}

### 降低成本

通过外包，可减少与系统管理相关的时间和资金成本，从而降低与传统后端服务器相关的总体成本。此外，{{site.data.keyword.openwhisk_short}} 与传统计算技术不同，因为您只需根据代码满足请求所用的时间付费，时间会四舍五入到最接近的 100 毫秒。相对于其他技术（如 VM 和容器），Cloud Functions 可节省大量成本，因为其他技术中的利用率不可能达到 100%，而且还会占用云提供者系统上的内存。

### 高可用性和可扩展性

无服务器体系结构本身可提供即时可扩展性和几乎不变的可用性。

### 更快、更简单的开发

通过消除对系统管理的需求，并为部署提供简单的界面，无服务器范式可加速应用程序开发。开发者可以使用为了响应事件驱动型环境而执行的操作序列来快速构建应用程序。

## 示例用例
{: #use_cases}

### 移动后端
![](./images/cloud-functions-rest-api-trigger.png)

Mobile 开发者可以轻松地访问服务器端逻辑，还可以将计算密集型任务外包给云平台。您可以使用 iOS SDK 通过不同语言（如 Swift）来实现功能，并轻松使用服务器端功能，而不需要任何服务器端经验。

### 数据处理

![](./images/cloud-functions-cloudant-trigger.png)

您可以通过内置触发器在数据存储中的数据每次更新时执行代码。您还可以通过功能性服务器端编程模型，轻松地自动执行各种过程，如音频规范化、图像旋转、锐化、降噪、缩略图生成或视频转码。

### 认知数据处理

可以在数据变为可用时立即分析数据。您的功能可以利用强大的认知服务（如 IBM Watson）来检测图像或视频中的对象或人员。

### 已调度的任务

定期执行功能，并按照类似 cron 的语法来定义计划安排，以指定何时运行操作。

## API 参考
{: #openwhisk_start_api notoc}

<!-- * [REST API Documentation](./openwhisk_reference.html#openwhisk_ref_restapi)-->
* [REST API](https://console.{DomainName}/apidocs/98)

## 相关链接
{: #general notoc}

* [发现 {{site.data.keyword.openwhisk_short}}](http://www.ibm.com/cloud-computing/bluemix/openwhisk/)
<!-- redirects to link above * [{{site.data.keyword.openwhisk_short}} on IBM developerWorks](https://developer.ibm.com/openwhisk/)-->
* [Apache OpenWhisk 项目 Web 站点](http://openwhisk.org)
* [关于无服务器的更多信息](https://martinfowler.com/articles/serverless.html)
