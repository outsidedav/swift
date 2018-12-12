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
{:tip: .tip}

# 配置 Swift 环境
{: #configuration}

云本机开发有两个密切相关的实践，这两个实践的配置数据处理方式类似。第一个实践是，必须构建不可变工件，以最大限度地减少应用程序从开发到生产的过程中引入错误的可能性。第二个实践是，必须尽量在开发环境和生产环境之间消除差异，以避免因特定于环境的代码而引发的问题。 

服务配置和凭证（服务绑定）的管理因平台而变化。Cloud Foundry 会将服务绑定详细信息存储在一个字符串化的 JSON 对象中，该对象将以环境变量 `VCAP_SERVICES` 的形式传递到应用程序。Kubernetes 将服务绑定存储为 `ConfigMaps` 或 `Secrets` 中的字符串化 JSON 或平面属性，这些属性可以作为环境变量传递到容器化应用程序或作为临时卷进行安装。本地开发具有自己的配置，因为本地测试通常是云中运行的任何内容的一种简化的衍生方法。在没有特定于环境的代码路径的情况下，以可移植方式使用这些变体可能难度很大。

您可以按照简单的准则来编写可移植应用程序，还可以编写一些实用程序，用于封装特定于环境的位置中的查找服务绑定（或其他配置）。无论是为现有应用程序添加云支持，还是使用入门模板工具包来创建应用程序，目标都是为 Swift 应用程序提供可移植性，使之可用于众多部署平台上。

## 向现有 Swift 应用程序添加 {{site.data.keyword.cloud_notm}}
{: #addcloud-env}

抽象出环境值的路径可能因云环境而异。[CloudEnvironment](https://github.com/IBM-Swift/CloudEnvironment.git) 库从各种云提供者中抽象环境配置和凭证，以便 Swift 应用程序可以通过在本地或者在 Cloud Foundry、Kubernetes 或 {{site.data.keyword.openwhisk}} 中运行，从而以一致的方式访问信息。凭证抽象由 `CloudEnvironment` 库提供，此库在内部将 [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv) 用于 Cloud Foundry 配置，将 [Configuration](https://github.com/IBM-Swift/Configuration) 用作配置管理器。

使用 `CloudEnvironment` 时，可通过定义查询键，让 Swift 应用程序使用该键来搜索其对应的值，从而从应用程序的源代码中抽象出低级别详细信息。

`CloudEnvironment` 库提供了可在源代码中使用的一致查询键。然后，库会在整个搜索模式数组中进行搜索，以查找具有配置属性或服务凭证的 JSON 对象。 

### 向 Swift 应用程序添加 CloudEnvironment 包
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

此示例提供对服务的凭证集的访问权，这些凭证集现在可用于初始化与这些[支持的服务或通用字典](https://github.com/IBM-Swift/CloudEnvironment#supported-services)的连接。请查看用于特定于 Cloud Foundry 的配置的 [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api)，以及有关装入配置数据的 [Configuration 详细信息](https://github.com/IBM-Swift/Configuration)。

## 了解服务凭证
{: #service_creds}

`CloudEnvironment` 库将名为 `mappings.json` 的文件（位于 `config` 目录中）用于表达每个服务的凭证存储位置。`mappings.json` 文件支持搜索使用以下三种搜索模式类型的值：
- **`cloudfoundry`** - 此模式类型用于在 Cloud Foundry 的服务环境变量 (`VCAP_SERVICES`) 中搜索值。
- **`env`** - 此模式类型用于搜索映射到环境变量的值，如在 Kubernetes 或 Functions 中。
- **`file`** - 此模式类型用于在 JSON 文件中搜索值。路径必须相对于 Swift 应用程序的根文件夹。

与查询键关联的值的数组将按其显示的顺序进行搜索。

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

出于安全原因，凭证文件不属于存储库。在上一个示例中，`localdev` 文件夹用于存储本地凭证，因此您必须将此文件夹添加到 `.gitignore` 文件以避免意外落实。如果使用的是入门模板工具包应用程序，那么会创建此文件夹，并将其包含在 `.gitignore` 文件中。

有关 `mappings.json` 文件的更多信息，请查看[了解服务凭证](configuration.html#service_creds)部分。

## 通过入门模板工具包应用程序使用 Swift 配置管理器

使用[入门模板工具包](https://console.bluemix.net/developer/appledevelopment/starter-kits/)创建的 Swift 应用程序会自动随附在本地运行所需的凭证和配置，以及在许多 Cloud 部署环境（CF、K8s、VSI 和 Functions）中运行所需的凭证和配置。配置管理器的基本创建可以在 `Sources/Application/Application.swift` 中找到。使用服务创建基于 Swift 的入门模板工具包应用程序时，会创建 `config` 文件夹和 `mappings.json` 文件。如果下载应用程序，那么 `config` 文件夹会包含 `localdev-config.json` 文件（其中含有服务的所有凭证），并存在于 `.gitignore` 文件中。

## 后续步骤
{: #next notoc}

请查看下面三个库，以帮助应用程序从自己的环境中抽象自身：

* [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env)
* [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv)
* [Configuration](https://github.com/IBM-Swift/Configuration)
