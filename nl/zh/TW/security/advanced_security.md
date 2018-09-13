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

# 使用進階安全建置
{: #security}

Swift 開發人員可以輕鬆地運用 {{site.data.keyword.cloud}} 提供的進階安全服務，以業界最高的安全層次保護靜態、使用中或傳輸中的金鑰及資料。
{:shortdesc}

因為方法很簡單，您可以直接對 {{site.data.keyword.cloud}} 中的所有進階安全服務，使用 {{site.data.keyword.cloud_notm}} HyperSecure Platform Services。如需相關資訊，請參閱[開始使用 {{site.data.keyword.cloud_notm}} HyperSecure Platform Services](/docs/services/hypersecure-platform/index.html){:new_window}。

## 使用 {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}

{{site.data.keyword.hscrypto}} 是一個實驗性服務，對您的金鑰及資料提供業界最高安全層次的加密法。它將 System z 的安全與完整性帶至雲端。現在則透過 {{site.data.keyword.cloud_notm}}，將銀行及金融服務所根據的相同最先進加密技術，提供給雲端使用者。

{{site.data.keyword.hscrypto}} 提供可網路定址的硬體安全模組 (HSM)，透過 PKCS#11 應用程式設計介面 (API)，提供安全且受保護的加密法。對於加密硬體，您可以存取可實現的最高安全層次，也就是「FIPS 140-2 層次 4」。{{site.data.keyword.hscrypto}} 也運用可遠端存取 IBM 加密輔助處理器的 IBM Advanced Crypto Service Provider (ACSP) 解決方案。

{{site.data.keyword.hscrypto}} 也是 {{site.data.keyword.keymanagementserviceshort}} 服務的金鑰儲存庫。

如需 {{site.data.keyword.hscrypto}} 的相關資訊，請參閱[開始使用 {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}](/docs/services/hs-crypto/index.html){:new_window}。

## 使用 {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}}

{{site.data.keyword.keymanagementserviceshort}} 可協助您在 {{site.data.keyword.cloud_notm}} 服務之間為應用程式佈建加密金鑰。管理金鑰的生命週期時，即可知道您的金鑰是由防止資訊竊取的雲端型硬體安全模組 (HSM) 所保護。搭配使用 {{site.data.keyword.hscrypto}}，您的金鑰即受到最高安全層次「FIPS 140-2 層次 4」憑證的保護。

如需 {{site.data.keyword.keymanagementserviceshort}} 的相關資訊，請參閱[開始使用 {{site.data.keyword.keymanagementserviceshort}}](/docs/services/keymgmt/index.html){:new_window}。

## 使用 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS
{: #hypersecure-dbaas}

{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 是一個實驗性 {{site.data.keyword.cloud_notm}} 服務，隨需提供安全的資料庫。其提供彈性且可擴充的平台，可快速且輕鬆地佈建及管理您的 MongoDB 資料庫。

使用此服務，您可以在 {{site.data.keyword.cloud_notm}} 中建立資料庫叢集。您建立的每個資料庫叢集都包含一個主要資料庫實例以及兩個次要資料庫實例，次要實例作為主要實例的抄本。{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 邏輯會在您的資料庫叢集中，建立及存取 MongoDB 資料庫。

### 保護資料及隱私權

{{site.data.keyword.IBM_notm}} 在可用性高且安全的環境中，管理您的資料庫：
 * 不論資料是否在使用中，都會予以加密。
 * 系統硬體、系統配置及資料庫設定都確保高可用性。
 * 基礎技術可防止 {{site.data.keyword.IBM_notm}} 或協力廠商存取您的資料。

### 將 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 邏輯新增至應用程式

若要在您的應用程式中使用 MongoDB 資料庫，請參閱[開始使用資料庫](../hypersecure_dbaas/database-cluster.html)。  

### 進一步瞭解 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS

若要進一步瞭解 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS，請參閱[開始使用 IBM Cloud Hyper Protect DBaaS](/docs/services/hyper-protect-dbaas/index.html)。

## 使用 {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscontainers}}

{{site.data.keyword.hscontainers}} 藉由結合 Docker 和 Kubernetes 技術、直覺式使用者體驗以及內建安全和隔離來提供功能強大的工具，以在運算主機的叢集中自動部署、操作、擴充及監視容器化應用程式。


請注意，{{site.data.keyword.hscontainers}} 僅供贊助者使用者使用。如果您預期專用的安全支援，請使用 [IBM Z Client Feedback Program](https://www-01.ibm.com/marketing/iwm/iwmdocs/web/cc/earlyprograms/zcustomer.shtml) 登錄為贊助者使用者，以將您的應用程式部署至 {{site.data.keyword.hscontainers}} 叢集。
{: tip}

在變成贊助者使用者之前，您可以使用 {{site.data.keyword.containershort_notm}} 來保護您的應用程式。如需相關資訊，請參閱[開始使用 {{site.data.keyword.containershort_notm}}](/docs/containers/container_index.html#container_index)。
