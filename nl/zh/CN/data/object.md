---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: object storage swift, static storage swift, file services swift, swift storage class, cos swift, swift data encryption, static swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 将 Object Storage 用于静态内容
{: #object-storage}

Object Storage 是云计算的基础组件，用于为 Apple 开发者及其应用程序提供强大的功能。与在文件层次结构（例如，Block Storage 或 File Storage）中存储信息不同，Object Storage 仅包含文件及其元数据，这些内容存储在称为存储区的集合中。根据定义，这些对象是不可变的，因此是用于图像、视频和其他静态文档等数据的完美存储器。对于经常更改的数据或关系数据，可以使用 [Cloudant](/docs/swift/data?topic=swift-cloudant#cloudant) 和 [SQL](/docs/swift/data?topic=swift-sql_data#sql_data) 数据库服务。

{{site.data.keyword.cos_full_notm}} (COS) 是一种存储系统，可用于存储非结构化数据，使用灵活，具有成本效益，可进行扩展。数据可通过 SDK 或使用 IBM 用户界面进行访问。您可以使用 {{site.data.keyword.cos_full_notm}} 通过 RESTful API 和 SDK 支持的自助服务门户网站来访问非结构化数据。 

根据需要，可以将 {{site.data.keyword.cos_short}} 用于以下服务：

* 内容存储库
* 备份和恢复
* 数据归档
* 文件服务
* 数据传输

创建存储区时，必须选择弹性级别（跨区域或区域性）。根据您的选择，数据可跨多个地理位置分散存储。

## API
{: #api-cos}

{{site.data.keyword.cos_full}} API 是基于 REST 的 API，用于读取和写入对象。此 API 支持 S3 API 的子集，可轻松地将应用程序迁移到 {{site.data.keyword.cloud_notm}}。在使用 {{site.data.keyword.cos_full}} 时，可以使用任何 S3 SDK。有关更多信息，请参阅完整的 [{{site.data.keyword.cos_short}} API 参考](/docs/services/cloud-object-storage/api-reference?topic=cloud-object-storage-compatibility-api-about#about-the-ibm-cloud-object-storage-api)

## 安全性
{: #security-cos}

{{site.data.keyword.cos_full_notm}} 安全性非常高。首先，只有存储区和对象所有者才有权访问自己创建的 {{site.data.keyword.cos_full_notm}} 服务。此外，还支持通过用户认证来访问数据。使用访问控制机制（如存储区策略）可有选择地向用户和应用程序授予许可权。可以使用 HTTPS 协议通过 SSL 端点将数据安全地传输到 {{site.data.keyword.cos_short}}。如果需要额外的安全性，可以使用“服务器端加密”(SSE) 选项或“使用客户提供的密钥的服务器端加密”(SSE-C) 选项来加密静态存储的数据。{{site.data.keyword.cos_full_notm}} 提供了用于 SSE 和 SSE-C 的加密技术。或者，您可以先使用自己的加密库来加密数据，然后再将其存储在 Cloud Object Storage 中。

可以使用以下访问控制机制来保护 {{site.data.keyword.cos_full_notm}} 中的数据。

**Identity and Access Management (IAM) 策略**
{: #iam-cos}

通过 IAM，拥有多名员工的组织可以在单个帐户下创建和管理多个用户。借助 IAM 策略，公司可以授予 IAM 用户对其 {{site.data.keyword.cos_short}} 存储区的控制权。

**访问控制表 (ACL)**
{: #acls-cos}

通过 ACL，可以授予特定用户对单个存储区的特定许可权（例如，读许可权、写许可权）。

静态数据使用自动提供者端高级加密标准 (AES) 256 位加密和安全散列算法 (SHA)-256 散列进行加密。动态数据使用内置运营商级别传输层安全性 (TLS)、安全套接字层 (SSL) 或采用 AES 加密的 SNMPv3 进行保护。

### 加密
{: #encryption-cos}

静态数据使用自动提供者端高级加密标准 (AES) 256 位加密和安全散列算法 (SHA)-256 散列进行加密。动态数据使用内置运营商级别传输层安全性/安全套接字层 (TLS/SSL) 或采用 AES 加密的 SNMPv3 进行保护。

服务器端加密对于客户数据始终启用。与认证所需的散列以及擦除编码相比，加密在 Cloud Object Storage 的处理成本中所占比重并不大。

您可以提供自己的密钥进行加密，因为 {{site.data.keyword.cos_full_notm}} 中支持 SSE-C。但是，管理和提供用于存储和检索数据的密钥是您的责任。

## 弹性
{: #resiliency-cos}

创建存储区时，必须选择弹性级别（跨区域或区域性）。根据您的选择，数据可跨多个地理位置分散存储。

区域性弹性适用于等待时间短的情况，并且数据会分布在单个区域内的三个中心。由于您的数据存储在 3 个或更多个不同区域中，所以请使用跨区域弹性来实现关键可用性。跨区域能够提供地理位置弹性，可跨多个端点使用。请考虑您的应用程序需求来决定使用哪种弹性。

### 地理位置
{: #geographies-cos}

可以在世界任何地方使用 {{site.data.keyword.cos_full_notm}}。您可以在创建存储区时选择存储数据的位置。IBM COS 中存储的信息是经过加密的，通过 IBM Object Storage System 提供的分布式存储技术，分散存储在多个地理位置中。 

在决定是使用区域性弹性还是跨区域弹性选项时，请考虑以下因素来选择对象存储的地理位置。

**地理位置注意事项**：
* 相对于运营处于远程的位置，以实现冗余性。
* 满足法律法规需求的位置。
* 缩短数据访问延迟。
* 考虑定价模型。

## 用例和存储类
{: #usecases-cos}

根据您的用例，可以通过选择满足您需求的服务套餐来降低成本。涉及对对象存储最低访问权的归档操作无需频繁访问的对象的速度和耐久性，此区别会反映在应用程序的存储类支持和价格套餐中。存储类在存储区级别进行定义，因此可以使用套餐组合来满足您的需求。创建一个存储区，将其设置为您要使用的存储类。

[{{site.data.keyword.cos_short}} 存储类](/docs/services/cloud-object-storage/help?topic=cloud-object-storage-billing#ibm-cos-pricing)文档中提供了有关定价的更多信息。

### 样本存储类
{: #samples-cos}

**标准**
此服务适用于需要频繁访问的非结构化数据，例如 DevOps、协作和操作内容存储库。

**保险库**
此服务适用于具有不常访问的数据的工作负载，例如备份、归档和合规性工作负载。

**冷保险库**
此部署选项是最低访问需求、历史记录合规性和长期备份的理想选择。

**灵活**
根据各种不同的数据访问需求进行部署，保护预算免受意外成本波动的影响。存储类在存储区级别进行定义。创建一个存储区，将其设置为您要使用的存储类。
