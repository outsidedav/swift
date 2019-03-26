---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# 移动后端即服务功能
{: #mbaas}

应用程序可通过以下两种方法之一来使用外部功能：
* 从应用程序直接用于其服务，这些服务通常作为移动后端即服务 (MBaaS) 平台的一部分进行托管。
* 通过托管的服务于前端的后端 (BFF) 组件使用，此组件可以提供对服务的组合和控制，并且可以为应用程序添加后端逻辑。

通常，较为直接的方式是通过 MBaaS 样式方法直接从移动应用程序添加外部功能。需要添加第一组服务的项目和基础架构会比较少。但是，此方法缺少最终的灵活性，并且缺少可通过部署 BFF 实现的作用域。

{{site.data.keyword.cloud_notm}} 平台的其中一个优势是无需提前做决定。所有提供的服务都可通过提供的基于 Swift 的 SDK 直接从移动设备使用。{{site.data.keyword.cloud_notm}} 平台还支持使用 Swift 编写其后端逻辑，可以是使用 {{site.data.keyword.openwhisk_short}} 通过无服务器模型来编写，也可以是使用 Kitura 等框架通过服务器端 Swift 来编写。由于此功能，当您要通过 BFF 模型添加后端逻辑时，就可以在 Swift 中执行此操作，并且可以选择将 Swift SDK 的使用从应用程序迁移到 BFF。如此一来，您就可以通过递增方式从 MBaaS 样式方法移动到 BFF 方法，而所需的额外工作量最少。
