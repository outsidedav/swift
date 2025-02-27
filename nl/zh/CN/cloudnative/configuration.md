---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: swift-cfenv, service bindings swift, environment swift, swift configuration, cloudenvironment swift, VCAP_SERVICES swift, swift credentials

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# 配置 Swift 环境
{: #configuration}

云本机开发有两个紧密相关的实践，贯穿于配置数据的处理方式中。第一个实践是，必须构建不可变的工件，以最大限度地减少应用程序从开发到生产过程中引入错误的可能性。第二个实践是，必须尽量使开发环境与生产环境相同，以避免特定于环境的代码产生的问题。 

服务配置和凭证（服务绑定）的管理因不同的平台而有所不同。Cloud Foundry 是将服务绑定详细信息存储在字符串化的 JSON 对象中，该对象会作为环境变量 `VCAP_SERVICES` 传递到应用程序。Kubernetes 是将服务绑定作为字符串化 JSON 或平面属性存储在 `ConfigMaps` 或 `Secrets` 中，可以将其作为环境变量传递到容器化应用程序，或作为临时卷安装。本地开发具有自己的配置，因为本地测试通常是云中运行的任何内容的一种简化的衍生方法。在不具备特定于环境的代码路径的情况下，以可移植方式应对这些变化因素可能很困难。

您可以遵循简单的准则来帮助您编写可移植的应用程序以及可用于封装应用程序的实用程序，以在特定于环境的位置中查找服务绑定（或其他配置）。无论是为现有应用程序添加云支持，还是使用入门模板工具包来创建应用程序，目标都是为 Swift 应用程序提供可移植性，使之可用于众多部署平台上。

## 向现有 Swift 应用程序添加 {{site.data.keyword.cloud_notm}}
{: #addcloud-env}

抽象出环境值的路径可能因云环境而异。[CloudEnvironment](https://github.com/IBM-Swift/CloudEnvironment){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 库从各种云提供者中抽象出环境配置和凭证，以便 Swift 应用程序可以通过在本地运行或者在 Cloud Foundry、Cloud Foundry Enterprise Environment、Kubernetes、{{site.data.keyword.openwhisk}} 或虚拟实例中运行，从而以一致的方式访问信息。凭证抽象由 `CloudEnvironment` 库提供，此库在内部将 [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 用于 Cloud Foundry 配置，将 [Configuration](https://github.com/IBM-Swift/Configuration){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 用作配置管理器。

使用 `CloudEnvironment` 时，可通过定义查询键，让 Swift 应用程序使用该键来搜索其对应的值，从而从应用程序的源代码中对低级别详细信息进行抽象化处理。

`CloudEnvironment` 库提供了可在源代码中使用的一致查询键。然后，库会在整个搜索模式数组中进行搜索，以查找具有配置属性或服务凭证的 JSON 对象。 

### 向 Swift 应用程序添加 CloudEnvironment 包
{: #add-cloudenv}

要在 Swift 应用程序中使用 `CloudEnvironment` 包，请在 `Package.swift` 文件的 **dependencies:** 部分中指定该包：
```swift
.package(url: "https://github.com/IBM-Swift/CloudEnvironment.git", from: "8.0.0"),
```
{: codeblock}

然后，将以下检测代码添加到应用程序：
```swift
import CloudEnvironment

let cloudEnv = CloudEnv()
```
{: codeblock}

### 访问凭证
{: #access-credentials}

现在，`CloudEnvironment` 库已初始化，您可以访问自己的凭证，如以下示例中所示：
```swift
let cloudantCredentials = cloudEnv.getCloudantCredentials(name: "cloudant-credentials")
// cloudantCredentials.username、cloudantCredentials.password、cloudantCredentials.url 等
let objStorageCredentials = cloudEnv.getObjectStorageCredentials(name: "object-storage-credentials")
// objStorageCredentials.username、objStorageCredentials.password、objStorageCredentials.projectID 等

let service1Credentials = cloudEnv.getDictionary("service1-credentials")
let service1CredentialsStr = cloudEnv.getString("service1-credentials")

// 获取缺省端口和 URL
let port = cloudEnv.port
let url = cloudEnv.url
```
{: codeblock}

此示例提供对服务的凭证集的访问权，这些凭证集现在可用于初始化与这些[支持的服务或通用字典](https://github.com/IBM-Swift/CloudEnvironment#supported-services){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 的连接。请查看用于特定于 Cloud Foundry 配置的 [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，以及有关装入配置数据的 [Configuration 详细信息](https://github.com/IBM-Swift/Configuration){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。

## 了解服务凭证
{: #service_creds}

`CloudEnvironment` 库将名为 `mappings.json` 的文件（位于 `config` 目录中）用于表达每个服务的凭证存储位置。`mappings.json` 文件支持搜索使用以下三种搜索模式类型的值：
- **`cloudfoundry`** - 用于在 Cloud Foundry 的服务环境变量 (`VCAP_SERVICES`) 中搜索值的模式类型。对于 Cloud Foundry 企业版，请参阅此[入门教程](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started)以获取更多信息。
- **`env`** - 用于搜索映射到环境变量的值的模式类型，如在 Kubernetes 或 Functions 中。
- **`file`** - 用于在 JSON 文件中搜索值的模式类型。路径必须相对于 Swift 应用程序的根文件夹。

搜索与查找键关联的值的数组时，将按其显示的顺序进行搜索。

下面显示了 `mappings.json` 文件的示例：
```javascript
{
    "cloudant-credentials": {
        "credentials": {
"searchPatterns": [
      "cloudfoundry:my-awesome-cloudant-db",
                "env:my_awesome_cloudant_db_credentials",
                "file:localdev/my-awesome-cloudant-db-credentials.json"
            ]
        }
    },
    "object-storage-credentials": {
        "credentials": {
"searchPatterns": [
      "cloudfoundry:my-awesome-object-storage",
                "env:my_awesome_object_storage_credentials",
                "file:localdev/my-awesome-object-storage-credentials.json"
            ]
        }
    }
}
```
{: codeblock}

在此示例中，`cloudant-credentials` 和 `object-storage-credentials` 是 Swift 应用程序用于查找相应凭证的查询键。根据搜索模式数组，配置管理器会先在 `VCAP_SERVICES` 中查找 `my-awesome-cloudant-db`，然后将 `my_awesome_cloudant_db_credentials` 作为环境变量进行查找。如果未定义该变量，那么会在 `localdev` 文件夹中查找 `my-awesome-object-storage-credentials.json` 的内容。 

应用程序在本地运行时，可以使用存储在文件中的凭证，如先前示例中的 `localdev/my-awesome-object-storage-credentials.json` 文件。要在本地访问的服务的连接信息（例如，用户名、密码和主机名）都会存储在此文件中。 

出于安全原因，凭证文件不属于存储库。在先前的示例中，`localdev` 文件夹用于存储本地凭证，因此必须将此文件夹添加到 `.gitignore` 文件以避免意外落实。如果使用的是入门模板工具包应用程序，那么将为您创建此文件夹，并使其出现在 `.gitignore` 文件中。

有关 `mappings.json` 文件的更多信息，请查看[了解服务凭证](#service_creds)部分。

## 通过入门模板工具包应用程序使用 Swift 配置管理器
{: #configmanager-swift}

使用[入门模板工具包](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 创建的 Swift 应用程序会自动随附在本地以及许多云部署目标中运行所需的凭证和配置，例如 [Kubernetes](/docs/containers?topic=containers-getting-started)、[Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)、[{{site.data.keyword.cfee_full_notm}}](/docs/cloud-foundry?topic=cloud-foundry-about)、[Virtual Server (VSI)](/docs/vsi?topic=virtual-servers-getting-started-tutorial) 或 [{{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-getting_started)。

  VSI 部署可用于某些入门模板工具包。要使用此功能，请转至 [{{site.data.keyword.cloud_notm}} 仪表板](https://{DomainName})，然后单击**应用程序**磁贴中的**创建应用程序**。
  {: note}

配置管理器的基本创建可以在 `Sources/Application/Application.swift` 中找到。使用服务创建基于 Swift 的入门模板工具包应用程序时，会创建 `config` 文件夹和 `mappings.json` 文件。如果下载应用程序，那么 `config` 文件夹会包含 `localdev-config.json` 文件（其中含有服务的所有凭证），并存在于 `.gitignore` 文件中。

## 后续步骤
{: #next-configß notoc}

请查看下面三个库，以帮助应用程序从自己的环境中抽象自身：

* [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")
* [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")
* [Configuration](https://github.com/IBM-Swift/Configuration){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")
