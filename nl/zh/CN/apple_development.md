---

copyright:
  years: 2018
lastupdated: "2018-08-27"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:tip .tip}

# {{site.data.keyword.cloud_notm}} 上的 Apple 开发
{: #ios_dev}

{{site.data.keyword.cloud}} 提供了多种解决方案和服务，用于支持 Swift 开发者和应用程序实现客户要求的安全性、AI 和价值。通过产品与 SDK 的广泛组合，您可以利用这些服务，并快速将最新的应用程序推向市场。


## 一个开发平台：IBM Cloud
{: #platform}

 ![开发者类型](images/IBM_Cloud_icon.png "IBM Cloud")

构建到 {{site.data.keyword.cloud_notm}} 中的开发者功能与各种技能集相匹配，并提供了一个平台用于生成、交付、运行和管理应用程序。例如，在所提到的认知应用程序中，有以下相关的 {{site.data.keyword.cloud_notm}} 功能：

* [**{{site.data.keyword.cloud_notm}} Developer Experience**](https://console.bluemix.net/docs/overview/dev-journey.html#dev-journey) 不是服务，而是 {{site.data.keyword.cloud_notm}} 上的一组功能，用于帮助数字和云本机开发者开始构建生产就绪型应用程序。它包括自动供应服务和“一键”部署到 DevOps 工具链的功能。

* 通过 [**IBM Watson Data Platform**](https://dataplatform.ibm.com)，可更轻松地组合数据集合、优化数据，然后进行可视化、发现洞察并构建模型以在认知应用程序中使用。

* [**IBM Streaming Analytics**](../services/StreamingAnalytics/index.html#gettingstarted) 可帮助梳理和分析数据流。这是一个高级分析平台，可用于在来自不同类型数据源的信息到达时，实时对信息进行获取、分析和关联。

* [**{{site.data.keyword.cloud_notm}} Continuous Delivery 服务**](../services/ContinuousDelivery/index.html#cd_getting_started)可设置 DevOps 工具链，以自动执行应用程序的持续交付。您可以轻松地增强工具链功能，以包含监视、日志记录、跟踪和警报等管理功能。您甚至可以使用 [DevOps Insights 服务](../services/DevOpsInsights/index.html#gettingstarted)将高级机器学习应用到工具链。

{{site.data.keyword.cloud_notm}} 平台提供了更多丰富的功能，可用作您的综合开发平台。

## {{site.data.keyword.cloud_notm}} 功能概述
{: #capabilities}

{{site.data.keyword.cloud_notm}} Developer Experience 不是服务，而是 {{site.data.keyword.cloud_notm}} 平台中的一组功能，用于帮助在几分钟内“立即”开始构建应用程序。Developer Experience 的基本元素包括：

* 整个 {{site.data.keyword.cloud_notm}} 平台中以主题或通道为中心的一组开发者控制台。
* 特定用例入门模板工具包，可生成各种语言和体系结构模式的生产就绪型入门模板应用程序。
* 自动供应服务。
* 使用可移植应用程序结构管理应用程序组件。
* 一键创建 [DevOps 工具链](../services/ContinuousDelivery/index.html#cd_getting_started)。

要了解 {{site.data.keyword.cloud_notm}} Developer Experience 可以如何帮助您快速构建高质量的生产就绪型应用程序，请查看以下元素的更多详细信息。

## 开发者控制台
{: #developer_consoles}

{{site.data.keyword.cloud_notm}} Developer Experience 在整个 {{site.data.keyword.cloud_notm}} 平台中的各种开发者控制台中显示。每个控制台都表示一个相关区域（如 Watson、Security 或 Finance）或一个数字通道（如移动或 Web 应用程序）。[{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/dashboard) 是针对 Apple 开发者开发的，在 {{site.data.keyword.cloud_notm}} 平台的支持下，能够快速试验应用程序和服务。您可以通过单击 {{site.data.keyword.cloud_notm}} 控制台中的菜单图标来访问这些控制台。例如，查看以下开发者控制台：

* [Watson 开发者控制台](https://console.bluemix.net/developer/watson/dashboard)
* [Mobile 开发者控制台](https://console.bluemix.net/developer/mobile/dashboard)
* [Web App 开发者控制台](https://console.bluemix.net/developer/appservice/dashboard)
* [Security 开发者控制台](https://console.bluemix.net/developer/security/dashboard)
* [Finance 开发者控制台](https://console.bluemix.net/developer/finance/dashboard)

<!--Cloud native development is the process of developing apps that are optimized to leverage capabilities engendered from running on the cloud.  Flexibility, portability, scaling, rapid development, continuous delivery, and a close coupling development and operations ("devops) are characteristics of cloud applications. The {{site.data.keyword.cloud}} Developer Experience quickly gets you started building cloud native applications that are ready for team development and bound for production use.-->


<!--![Overview of elements of the {{site.data.keyword.cloud_notm}} Developer Experience](images/elements_of_devex.png "Overview of elements of the {{site.data.keyword.cloud_notm}} Developer Experience") <br> *Overview of elements of the {{site.data.keyword.cloud_notm}} Developer Experience*-->

每个开发者控制台都提供了与该控制台的焦点区域相关的入门模板工具包，以及一致、直观的工作流，可在几分钟内创建正常运行的生产就绪型应用程序。

## 使用入门模板工具包创建应用程序
{: #apps}

可以利用[入门模板工具包](starter_kit/starter_kits.html)来创建 Swift 应用程序，其中包含组成应用程序的代码、数据、服务和工具链的关联。
