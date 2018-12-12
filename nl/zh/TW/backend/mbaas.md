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

# 行動後端即服務特性
{: #mbaas}

您的應用程式可以透過兩種方法中的其中一種來使用外部特性：
* 從應用程式到其服務直接使用，一般都當作「行動後端即服務 (MBaaS)」平台的一部分來管理。
* 透過受管理 Backend For Frontend (BFF) 元件來使用，可提供服務的組合及控制，並可為應用程式新增後端邏輯。

一般而言，透過 MBaaS 樣式方法，直接從您的行動應用程式來新增外部特性是最直接的。少數項目及基礎架構是要新增第一組服務。不過，這個方法缺少最終的彈性，而且透過部署 BFF 可實現範圍。

{{site.data.keyword.cloud_notm}} 平台的其中一個優點是您不需要事先做出決定。使用提供的 Swift 型 SDK，即可直接從行動裝置使用提供的所有服務。{{site.data.keyword.cloud_notm}} 平台也支援透過含有 {{site.data.keyword.openwhisk_short}} 的無伺服器模型，或透過含有 Kitura 這類架構之伺服器端 Swift，以 Swift 撰寫其後端邏輯。因為這個功能，當您想要透過 BFF 模型來新增後端邏輯時，可以在 Swift 中這麼做，且可以選擇從應用程式移轉到 BFF 中使用 Swift SDK。因此，您可以用最少的額外努力，漸進式地從 MBaaS 樣式方法移至 BFF 方法。
