---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}

# 无服务器开发
{: #serverless}

什么是无服务器？无服务器开发模式是指服务器端逻辑在无状态容器中运行的应用程序开发；无状态容器通过事件触发，属于临时容器（有效持续时间为一个执行），并完全由第三方进行管理。此范式也称为功能即服务 (FaaS)，在其中开发者可提供无状态功能，这类功能可以在不明确供应或管理服务器的情况下触发和执行。

无服务器体系结构通过抽象服务器端开发所需的基础架构和框架，使开发人员能够专注于构建自己的应用程序和编写代码，以反应性方式运行来更改数据。

IBM 的 FaaS 产品 [{{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/) 致力于提供无缝的服务器端开发体验，无需任何专业服务器端知识。通过使用无服务器技术，您可以快速开发可扩展的后端解决方案，以满足几乎所有工作负载需求，而不必提前供应资源。对于具有不可预测的负载模式或服务器停机时间长的应用程序，{{site.data.keyword.openwhisk_short}} 是出色的云解决方案，其性能经过改进，其“按使用内容付费”系统有助于降低成本。

## 体系结构更改
{: #comparison}

为了帮助您了解切换到 FaaS 的体系结构优点，将使用一个与数据库链接的简单 iOS 应用程序来比较传统体系结构和 FaaS 体系结构。

在更传统的体系结构中，iOS 应用程序会卸载网络密集型任务，或者在集中式平台对数据进行远程处理，应用程序本身通过其自己的服务或存储选项进行连接。对于传统系统，繁重的工作落在单台服务器上，该服务器要处理认证，处理密集型任务以尽可能减小客户端上的压力，以及在其用户基础中提供同步功能。

无服务器体系结构可以将此结构变更为更类似于下图。

![](./images/Architecture.png) 图 1. 无服务器体系结构

与在单台服务器内执行所有处理和认证逻辑不同，无服务器体系结构是利用高度可扩展的功能来封装大部分服务器端逻辑，并将某些逻辑卸载到客户端（和外部服务）。

从该示意图中，可以看出以下几点:

1. 客户端根据身份提供者（如应用程序标识）进行认证。
2. 调用包含访问令牌的 FaaS 后端 API。
3. 后端通过 {{site.data.keyword.openwhisk_short}} 实现。作为 Web 操作公开的无服务器操作需要令牌在请求头中发送，并验证其有效性（签名和到期日期）以提供对实际 API 的访问权。
4. 客户端提交数据后，反馈会存储在 {{site.data.keyword.cloudant_short_notm}} 中。
5. 反馈文本通过 {{site.data.keyword.toneanalyzershort}} 进行处理。
6. 根据分析结果，{{site.data.keyword.mobilepushshort}} 会将通知发回客户端。
7. 客户端收到通知。

在纯无服务器模型中，由于无法存储用户状态，客户端通常会承担额外责任。例如，在这种情况下，授权由客户端和 {{site.data.keyword.appid_short_notm}} 身份提供者服务进行处理。

虽然无服务器体系结构不一定是最理想的，但在适当的团队和使用条件下可提供大量优点。请查看一些特定示例，以了解一些最常见[用例](#use_cases)的信息。

## 无服务器优点
{: #benefits}

### 降低成本

通过外包，可减少与系统管理相关的时间和资金成本，从而降低与传统后端服务器相关的总体成本。此外，{{site.data.keyword.openwhisk_short}} 与传统计算技术不同，因为您只需根据代码为满足请求而使用的时间付费，时间会四舍五入到最接近的 100 毫秒。相对于其他技术（如 VM 和容器），Cloud Functions 可节省大量成本，因为其他技术中的利用率不可能达到 100%，并会占用云提供者系统上的内存。

### 高可用性和可扩展性

无服务器体系结构可提供即时可扩展性和几乎不变的可用性。

### 更快、更简单的开发

通过消除对系统管理的需求，并为部署提供简单的界面，无服务器范式可加速应用程序开发。开发者可以使用为了响应事件驱动型环境而执行的操作序列来快速构建应用程序。

## 示例用例
{: #use_cases}

### 移动后端
![](./images/cloud-functions-rest-api-trigger.png)

移动开发者可以轻松地访问务器端逻辑，并可将计算密集型任务外包给可扩展的云平台。您可以使用 iOS SDK 通过不同语言（如 Swift）来实现功能，并轻松使用服务器端功能，而不需要任何服务器端体验。

### 数据处理

![](./images/cloud-functions-cloudant-trigger.png)

在数据存储中更新数据时，可以通过内置触发器来执行代码。您还可以通过功能性服务器端编程模型，轻松地自动执行各种过程，如音频规范化、图像旋转、锐化、降噪、缩略图生成或视频转码。

### 认知数据处理

可以在数据变为可用时立即分析数据。支持您的功能利用强大的认知服务（如 IBM Watson）来检测图像或视频中的对象或人员。

### 已调度的任务

定期执行功能，并定义遵循类似 cron 的语法的安排，以指定应该执行操作的时间。

## API 参考
{: #openwhisk_start_api notoc}

<!-- * [REST API Documentation](./openwhisk_reference.html#openwhisk_ref_restapi)-->
* [REST API](https://console.{DomainName}/apidocs/98)

## 相关链接
{: #general notoc}

* [Discover: {{site.data.keyword.openwhisk_short}}](http://www.ibm.com/cloud-computing/bluemix/openwhisk/)
<!-- redirects to link above * [{{site.data.keyword.openwhisk_short}} on IBM developerWorks](https://developer.ibm.com/openwhisk/)-->
* [Apache OpenWhisk 项目 Web 站点](http://openwhisk.org)
* [关于无服务器的更多信息](https://martinfowler.com/articles/serverless.html)
