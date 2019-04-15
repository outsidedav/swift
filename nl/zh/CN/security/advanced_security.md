---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: swift security, add security swift, crypto swift, hyper protect swift, ios hyper protect, dbaas swift, swift key management, swift advanced security

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip} 

# 使用高级安全性进行构建
{: #security}

Swift 开发者可以轻松地使用 {{site.data.keyword.cloud}} 提供的高级安全服务，以业界最高安全级别来保护自己的密钥以及静态数据、使用中的数据或传输中的数据。
{: shortdesc}

一种简单的方法是，可以直接将 {{site.data.keyword.cloud_notm}} HyperSecure Platform Services 用于 {{site.data.keyword.cloud}} 中的所有高级安全服务。
有关更多信息，请参阅 [{{site.data.keyword.cloud_notm}} HyperSecure Platform Services 入门](/docs/services/hypersecure-platform/index.html#getting-started-with-ibm-cloud-hyper-protect-developer-starter-kits)。

## 使用 {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}
{: #security-swift}

{{site.data.keyword.hscrypto}} 是一种试验性服务，可为密钥和数据提供业界最高安全级别的加密。它为云带来了 IBM Z 的安全性和完整性。现在，通过 {{site.data.keyword.cloud_notm}} 可以将银行和金融服务所使用的加密技术提供给云用户。

{{site.data.keyword.hscrypto}} 提供了网络可寻址硬件安全模块 (HSM)，这些模块将通过 PKCS#11 应用程序编程接口 (API) 来提供安全可靠的加密。您可以访问加密硬件可达到的最高级别的安全性，即 FIPS 140-2 4 级。{{site.data.keyword.hscrypto}} 还会使用 IBM Advanced Crypto Service Provider (ACSP) 解决方案来支持远程访问 IBM 的加密协处理器。

{{site.data.keyword.hscrypto}} 还是 {{site.data.keyword.keymanagementserviceshort}} 服务的密钥库。

有关 {{site.data.keyword.hscrypto}} 的更多信息，请参阅 [{{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} 入门](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started)。

## 使用 {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}}
{: #swift-key-management}

{{site.data.keyword.keymanagementserviceshort}} 可帮助您跨多个 {{site.data.keyword.cloud_notm}} 服务为应用程序创建加密密钥。管理密钥的生命周期时，了解密钥通过基于云的硬件安全模块 (HSM) 进行保护，用于防止信息被盗，这对您十分有益。HSM 与 {{site.data.keyword.hscrypto}} 配合使用，能以最高安全级别 FIPS 140-2 4 级证书来保护密钥。

有关 {{site.data.keyword.keymanagementserviceshort}} 的更多信息，请参阅 [{{site.data.keyword.keymanagementserviceshort}} 入门](/docs/services/key-protect?topic=key-protect-getting-started-tutorial#getting-started-tutorial)。

## 使用 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS
{: #hypersecure-dbaas}

{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 是一种试验性 {{site.data.keyword.cloud_notm}} 服务，用于按需创建安全数据库。它提供了一个可扩展的平台，让您可以快速、轻松地供应和管理 MongoDB 数据库。

通过此服务，可以在 {{site.data.keyword.cloud_notm}} 中创建数据库集群。您创建的每个数据库集群都包含一个主数据库实例和两个辅助数据库实例，辅助数据库实例充当主数据库实例的副本。通过 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 逻辑，可在数据库集群中创建和访问 MongoDB 数据库。

### 保护数据和隐私
{: #swift-securing-data}

{{site.data.keyword.IBM_notm}} 在高可用性的安全环境中托管数据库：
 * 数据采用静态加密和动态加密。
 * 系统硬件、系统配置和数据库设置可确保高可用性。
 * 底层技术可阻止 {{site.data.keyword.IBM_notm}} 或第三方访问您的数据。

### 向应用程序添加 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 逻辑
{: #swift-hyperprotect}

要在应用程序中使用 MongoDB 数据库，请参阅[数据库使用入门](/docs/swift/hypersecure_dbaas?topic=swift-create-database-cluster#creating-a-highly-available-and-secure-database)。  

### 了解有关 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 的更多信息
{: #swift-learnmore}

要了解有关 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 的更多信息，请参阅 [IBM Cloud Hyper Protect DBaaS 入门](/docs/services/hyper-protect-dbaas?topic=hyper-protect-dbaas-gettingstarted#gettingstarted)。

## 使用 {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscontainers}}
{: #swift-hscontainers}

{{site.data.keyword.hscontainers}} 通过组合 Docker 和 Kubernetes 技术、直观的用户体验以及内置安全性和隔离，提供功能强大的工具来自动对计算主机集群中的容器化应用程序进行部署、操作、缩放和监视。


{{site.data.keyword.hscontainers}} 仅供发起方用户使用。如果您需要专用安全支持，请使用 [IBM Z Client Feedback Program](https://www-01.ibm.com/marketing/iwm/iwmdocs/web/cc/earlyprograms/zcustomer.shtml){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 注册为发起方用户，以将应用程序部署到 {{site.data.keyword.hscontainers}} 集群。
{: tip}

在成为发起方用户之前，可以使用 {{site.data.keyword.containershort_notm}} 来保护应用程序。有关更多信息，请参阅 [{{site.data.keyword.containershort_notm}} 入门](/docs/containers?topic=containers-container_index#container_index)。
