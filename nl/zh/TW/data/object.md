---

copyright:
  years: 2018
lastupdated: "2018-11-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 對於靜態內容使用 Object Storage
{: #object}

Object Storage 是雲端運算的基本元件，提供強大的特性給 Apple 開發人員及其應用程式。不像在檔案階層（例如「區塊」或「檔案儲存空間」）中儲存資訊，物件儲存庫只包含檔案及其 meta 資料，儲存在稱為儲存區的集合之中。依據定義，這些物件都是不可變的，因此非常適用於影像、視訊及其他靜態文件等資料。對於經常變更或有關聯性的資料，您可以使用 [NoSQL](/docs/swift/data/nosql.html)、[Cloudant](/docs/swift/data/cloudant.html) 及 [SQL](/docs/swift/data/sql.html) Database 服務。

{{site.data.keyword.cos_full_notm}} (COS) 是一個儲存空間系統，可用來儲存有彈性、具成本效益且可擴充的非結構化資料。該資料可透過 SDK 存取，或使用 IBM 使用者介面進行存取。您可以使用 {{site.data.keyword.cos_full_notm}}，透過 RESTful API 及 SDK 支援的自助式入口網站，來存取您的非結構化資料。 

視需求而定，您可以針對下列服務使用 {{site.data.keyword.cos_short}}：

* 內容儲存庫
* 備份及回復
* 資料保存
* 檔案服務
* 資料傳送

建立儲存區時，您必須選取備援層次（跨地區或地區性）。根據您的選擇，資料是分散的，且儲存在多個地理位置。

## API

{{site.data.keyword.cos_full}} API 是一個 REST 型 API，用來讀寫物件。其支援一個 S3 API 子集，用來輕鬆地將應用程式移轉至 {{site.data.keyword.cloud_notm}}。任何 S3 SDK 都可以用來運用 {{site.data.keyword.cos_full}}。如需相關資訊，請參閱完整的 [{{site.data.keyword.cos_short}} API 參考資料](docs/services/cloud-object-storage/api-reference/about-compatibility-api.html#about-the-ibm-cloud-object-storage-api)

## 安全
{: #security}

{{site.data.keyword.cos_full_notm}} 極度安全。一開始，只有儲存區及物件擁有者具有其建立之 {{site.data.keyword.cos_full_notm}} 服務的原始存取權。此外，也支援使用使用者鑑別來存取資料。使用存取控制機制（例如，儲存區原則），選擇性地將許可權授與給使用者及應用程式。您可以使用 HTTPS 通訊協定，透過 SSL 端點，安全地將資料傳送至 {{site.data.keyword.cos_short}}。如果需要額外的安全，您可以使用「伺服器端加密 (SSE)」選項或「伺服器端加密與客戶自備金鑰 (SSE-C)」選項，來加密靜態儲存的資料。{{site.data.keyword.cos_full_notm}} 提供的加密技術同時適用於 SSE 及 SSE-C。或者，您也可以使用自己的加密程式庫來加密資料，然後再儲存至 Cloud Object Storage。

您可以使用下列存取控制機制，在 {{site.data.keyword.cos_full_notm}} 中保護資料的安全。

**Identity and Access Management (IAM) 原則**
IAM 可讓擁有眾多員工的組織，在單一帳戶下建立及管理多位使用者。使用 IAM 原則，公司可以授權 IAM 使用者控制其 {{site.data.keyword.cos_short}} 儲存區。

**存取控制清單 (ACL)**
使用 ACL，您可以針對個別儲存區，將特定許可權（例如，READ、WRITE）授權給特定使用者。

靜態資料會使用自動提供者端的「進階加密標準 (AES)」256 位元加密及「安全雜湊演算法 (SHA)」256 雜湊來進行加密。動態資料則是使用內建載波等級「傳輸層安全 (TLS)」、Secure Sockets Layer (SSL) 或含 AES 加密的 SNMPv3 來進行保護。

### 加密

靜態資料會使用自動提供者端的「進階加密標準 (AES)」256 位元加密及「安全雜湊演算法 (SHA)」256 雜湊來進行加密。動態資料則是使用內建載波等級「傳輸層安全/Secure Sockets Layer (TLS/SSL)」或含 AES 加密的 SNMPv3 來進行保護。

客戶資料一律「開啟」伺服器端加密。相較於鑑別所需的雜湊，以及消除編碼，加密並不是 Cloud Object Storage 處理成本中的重要部分。

因為 {{site.data.keyword.cos_full_notm}} 中支援 SSE-C，因此您可以提供自己的金鑰來進行加密。不過，您必須負責管理及提供用於儲存的金鑰，並且負責擷取資料。

## 備援

建立儲存區時，您必須選取備援層次（跨地區或地區性）。根據您的選擇，資料是分散的，且儲存在多個地理位置。

地區性備援是為了低延遲，且您的資料會分散在單一地區內的三個中心中。請使用跨地區備援以獲得重要可用性，因為您的資料會儲存在 3 個以上不同地區。跨地區提供地理備援，且可以跨多個端點使用。請考量您的應用程式需求，然後再決定要用哪一個。

### 地理位置

您可以在世界的任何角落使用 {{site.data.keyword.cos_full_notm}}。您可以在建立儲存區時，選擇資料的儲存位置。儲存在 IBM COS 中的資訊都會加密，並使用 IBM Object Storage System 所提供的分散式儲存空間技術，分散在多個地理位置。 

在您決定地區性或跨地區的選項時，請考量下列因素，以選取物件儲存庫的地理位置。

**地理位置考量**：
* 遠離您作業的位置，作為備用。
* 合法且符合法規需求的位置。
* 減少資料存取延遲。
* 考量定價模型。

## 使用案例及儲存類別

視使用案例而定，您可以選取符合所需的服務方案來降低成本。只需最低存取物件儲存庫的保存作業，不需要經常存取之物件所需的存取速度和延續性，這樣的差別會反映在您應用程式的「儲存空間類別」支援以及定價方案。儲存空間類別定義在儲存區層次，因此您可以使用方案組合來符合您的需求。建立一個儲存區，然後設為您要使用的儲存空間類別。

您可以在 [{{site.data.keyword.cos_short}} 儲存空間類別](/docs/services/cloud-object-storage/help/billing.html#ibm-cos-pricing)文件中，找到定價的相關資訊。

### 儲存空間類別範例

**Standard**
此服務適用於需要經常存取的非結構化資料，例如，DevOps、協同作業以及動作內容儲存庫。

**Vault**
此服務適用於具有不常存取之資料的工作負載，例如，備份、保存及規範工作負載。

**Cold Vault**
此部署選項非常適用於最低存取需求、歷程記錄規範以及長期備份。

**Flex** 針對不同資料存取需求進行部署，並保護您的預算免受非預期的成本變動影響。
儲存類別定義在儲存區層次。建立一個儲存區，然後設為您要使用的儲存空間類別。
