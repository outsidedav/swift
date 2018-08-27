---

copyright:
  years: 2018
lastupdated: "2018-08-08"

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

Swift 开发者可以轻松利用 {{site.data.keyword.cloud}} 提供的高级安全服务，以业界最高安全级别来保护自己的密钥以及静态数据、使用中的数据或传输中的数据。
{:shortdesc}

一种简单的方法是，可以直接将 {{site.data.keyword.cloud_notm}} HyperSecure Platform Services 用于 {{site.data.keyword.cloud}} 中的所有高级安全服务。有关更多信息，请参阅 [{{site.data.keyword.cloud_notm}} HyperSecure Platform Services 入门](/docs/services/hypersecure-platform/index.html){:new_window}。

## 使用 {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}

{{site.data.keyword.hscrypto}} 是一种实验性服务，用于对密钥和数据提供业界最高安全级别的密码术。它为云带来了 System z 的安全性和完整性。现在，通过 {{site.data.keyword.cloud_notm}} 可向云用户提供银行和金融服务所依赖的最先进的加密技术。

{{site.data.keyword.hscrypto}} 提供了网络可寻址硬件安全模块(HSM)，用于通过 PKCS#11 应用程序编程接口 (API) 提供安全的密码术。您可以访问可实现的最高安全级别的加密硬件，即 FIPS 140-2 4 级。{{site.data.keyword.hscrypto}} 还利用 IBM Advanced Crypto Service Provider (ACSP) 解决方案支持远程访问 IBM 的加密协处理器。

{{site.data.keyword.hscrypto}} 也是 {{site.data.keyword.keymanagementserviceshort}} 服务的密钥库。

有关 {{site.data.keyword.hscrypto}} 的更多信息，请参阅 [{{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} 入门](/docs/services/hs-crypto/index.html){:new_window}。

## 使用 {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}}

{{site.data.keyword.keymanagementserviceshort}} 可帮助您为 {{site.data.keyword.cloud_notm}} 服务中的应用程序供应加密的密钥。管理密钥的生命周期时，了解密钥通过基于云的硬件安全模块 (HSM) 进行保护，用于防止信息被盗，这对您十分有益。HSM 与 {{site.data.keyword.hscrypto}} 配合使用，能以最高安全级别 FIPS 140-2 4 级证书来保护密钥。

有关 {{site.data.keyword.keymanagementserviceshort}} 的更多信息，请参阅 [{{site.data.keyword.keymanagementserviceshort}} 入门](/docs/services/keymgmt/index.html){:new_window}。

## 使用 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS
{: #hypersecure-dbaas}

{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 是一种试验性 {{site.data.keyword.cloud_notm}} 服务，用于按需提供安全数据库。它提供了一个灵活且可扩展的平台，可快速、轻松地供应和管理 MongoDB 数据库。

通过此服务，可以在 {{site.data.keyword.cloud_notm}} 中创建数据库集群。创建的每个数据库集群都包含一个主数据库实例和两个辅助数据库实例，辅助数据库实例充当主数据库实例的副本。通过 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 逻辑，可在数据库集群中创建和访问 MongoDB 数据库。

### 保护数据和隐私

{{site.data.keyword.IBM_notm}} 在高可用性的安全环境中托管数据库：
 * 数据采用静态和动态加密。
 * 系统硬件、系统配置和数据库设置可确保高可用性。
 * 底层技术可阻止 {{site.data.keyword.IBM_notm}} 或第三方访问您的数据。

### 向应用程序添加 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 逻辑

要在应用程序中使用 MongoDB 数据库，请参阅[数据库使用入门](../hypersecure_dbaas/database-cluster.html)。  

### 了解有关 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 的更多信息

要了解有关 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 的更多信息，请参阅 [IBM Cloud Hyper Protect DBaaS 入门](/docs/services/hyper-protect-dbaas/index.html)。

## 使用 {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscontainers}}

{{site.data.keyword.hscontainers}} 通过组合 Docker 和 Kubernetes 技术、直观的用户体验以及内置安全性和隔离，提供功能强大的工具来自动对计算主机集群中的容器化应用程序进行部署、操作、扩展和监视。


请注意，{{site.data.keyword.hscontainers}} 仅可供发起方用户使用。如果您需要专用安全支持，请使用 [IBM Z Client Feedback Program](https://www-01.ibm.com/marketing/iwm/iwmdocs/web/cc/earlyprograms/zcustomer.shtml) 注册为发起方用户，以将应用程序部署到 {{site.data.keyword.hscontainers}} 集群。
{: tip}

在成为发起方用户之前，可以使用 {{site.data.keyword.containershort_notm}} 来保护应用程序。有关更多信息，请参阅 [{{site.data.keyword.containershort_notm}} 入门](/docs/containers/container_index.html#container_index)。
