---

copyright:
  years: 2017-2018
lastupdated: "2018-11-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 在 {{site.data.keyword.cloud_notm}} 中存储文档
{: #cloudant}

{{site.data.keyword.cloudantfull}} 是面向文档的数据库即服务 (DBaaS)。它将数据存储为 JSON 格式的文档。它秉承可扩展性、高可用性和耐久性进行构建，可轻松配置用于 Swift 应用程序。它随附多种索引选项，包括 MapReduce、{{site.data.keyword.cloudant_short_notm}} Query、全文索引和地理空间索引。通过其复制功能，可以轻松实现数据库集群、台式 PC 和移动设备之间的数据同步。
{: shortdesc}

有关可以使用 {{site.data.keyword.cloudant_short_notm}} 的所有方法，请参阅 [{{site.data.keyword.cloudant_short_notm}} 基础知识](/docs/services/Cloudant/basics/index.html#cloudant-nosql-db-basics)。

## 开始之前

首先，请确保您已准备好以下必备软件：
 * CocoaPods（V1.1.0 或更高版本）
 * iOS（V9 或更高版本）
 * MacOS（V10.11.5 或更高版本）
 * Xcode（V9.0.1 或更高版本）

[{{site.data.keyword.cloudant_short_notm}} SDK for Swift![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/cloudant/swift-cloudant) 是使用 Swift 3.2 构建的。如果计划将 {{site.data.keyword.cloudant_short_notm}} 与 Kitura 配合使用，请查看 [Kitura-CouchDB 库 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM-Swift/Kitura-CouchDB)，该库是使用 Swift 4.0 构建的。
{: tip}

## 步骤 1. 创建 {{site.data.keyword.cloudant_short_notm}} 的实例

有关创建服务实例的信息，请参阅[在 IBM Cloud 上创建 Cloudant NoSQL DB 实例教程 ![外部链接图标](../images/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/services/Cloudant/tutorials/create_service.html#creating-a-cloudant-nosql-db-instance-on-ibm-cloud){:new_window}。

## 步骤 2. 安装 SDK

### 安装 iOS Swift SDK

使用 Swift Cloudant SDK 可帮助简化应用程序的编码。该 SDK 必须安装在应用程序代码中。

1. 打开现有 Xcode 项目目录下的 `Podfile`。
2. 在项目目标下，添加 `SwiftCloudant` pod 的依赖项。确保 `use_frameworks!` 命令也位于目标下，如以下示例中所示。
    ```
    target '<yourTarget>' do
      use_frameworks!
        pod 'SwiftCloudant', :git => 'https://github.com/cloudant/swift-cloudant.git'
    end
    ```
    {: screen}

3. 下载 `SwiftCloudant` 依赖项。
    ```
pod install
```
    {: codeblock}

### 安装服务器端 Swift SDK

要与 Swift Package Manager 一起用于服务器端开发，请将以下行添加到 `Package.swift` 中的 dependencies：
```swift
.Package(url: "https://github.com/cloudant/swift-cloudant.git")
```
{: codeblock}

## 步骤 3. 初始化 SDK

在应用程序中初始化 SDK 后，可以开始使用 {{site.data.keyword.cloudant_short_notm}} 来存储数据。

1.  将以下 import 语句添加到 `AppDelegate.swift` 或服务器端 Swift 文件中。
    ```
    import SwiftCloudant
    ```
    {: codeblock}

2. 初始化与数据库的连接。
    ```swift
    let cloudantURL = NSURL(string: "https://username.cloudant.com")!
    let client = CouchDBClient(url: cloudantURL, username: "username", password: "password")
    let dbName = "database"
    ```
    {: codeblock}

### 基本操作
这些基本操作说明了使用经过初始化的客户端来创建、读取和删除文档的基础操作。

#### 创建文档
```swift
let create = PutDocumentOperation(id: "doc1", body: ["hello": "world"], databaseName: dbName) {(response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while creating a document. Error: \(error)")
    } else {
        print("Created document \(response?["id"]) with revision id \(response?["rev"])")
    }
}
client.add(operation: create)
```
{: codeblock}

#### 读取文档
```swift
let read = GetDocumentOperation(id: "doc1", databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while reading a document. Error: \(error)")
    } else {
        print("Read document: \(response)")
    }   
}
client.add(operation: read)
```
{: codeblock}

#### 删除文档
```swift
let delete = DeleteDocumentOperation(id: "doc1",
    revision: "1-revisionidhere",
    databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while deleting a document. Error: \(error)")
    } else {
        print("Document deleted")
    }   
}
client.add(operation: delete)
```
{: codeblock}

## 步骤 4. 测试应用程序
{: #cloudant_testing}

一切都配置正确吗？请进行测试！

1. 运行应用程序，确保启动初始化和相应的操作，例如创建文档。
2. 返回到先前在 Web 浏览器中创建的 {{site.data.keyword.cloudant_short_notm}} 服务实例，然后打开服务仪表板。
3. 选择所使用的数据库，您可以在仪表板中查看相关文档。

遇到困难？请查看 [{{site.data.keyword.cloudant_short_notm}} API 参考](/docs/services/Cloudant/api/index.html#api-reference-overview)。

## 后续步骤
{: #cloudant_next notoc}

太棒了！您已将安全持久性级别添加到应用程序。请一鼓作气，尝试下列其中一个选项：

* 查看 [{{site.data.keyword.cloudant_short_notm}} SDK for Swift ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/cloudant/swift-cloudant) 源代码。
* 入门模板工具包是使用 {{site.data.keyword.cloud_notm}} 功能的最快方法之一。**Infinite Scrolling with Cloudant NoSQL for iOS** 入门模板工具包说明了如何扩展 `ViewController` 以使用分页来显示数据。此应用程序模式对于 iOS 开发者来说很常见，因此非常适合用作说明 {{site.data.keyword.cloudant_short_notm}} 功能的示例。请在 [Mobile 开发者仪表板](https://console.bluemix.net/developer/mobile/dashboard)中查看可用的入门模板工具包。下载代码。运行应用程序！
* 要了解有关 {{site.data.keyword.cloudant_short_notm}} 提供的所有功能的更多信息以及如何利用这些功能，请[查看文档](/docs/services/Cloudant/index.html)！
