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

# 配置 Swift 環境
{: #configuration}

雲端原生開發有 2 個密切相關的作法，它們在您的配置資料處理方式有所交集。第一種是您必須建置不可變的構件，如此一來，在您的應用程式從開發到正式作業的過程中，產生錯誤的可能性便可降到最低。第二種是您必須在開發與正式作業環境之間進行同位檢查，以避免環境特定程式碼所建立的問題。 

服務配置和認證（服務連結）的管理因平台而異。Cloud Foundry 將服務連結詳細資料儲存至字串化 JSON 物件，然後作為環境變數 `VCAP_SERVICES` 傳遞給應用程式。Kubernetes 則是將服務連結儲存成 `ConfigMaps` 或 `Secrets` 中的字串化 JSON 或平面屬性，然後作為環境變數傳遞給容器化應用程式，或裝載成暫存磁區。本端開發有自己的配置，因為本端測試通常衍生自雲端中所執行的作業，並進行簡化。在沒有環境特定程式碼路徑的情況下，以可攜方式在這些變動因素下作業，相當具有挑戰性。

您可以遵循簡單準則來協助您撰寫可攜式應用程式以及公用程式，您可以使用這些公用程式在環境特定位置中封裝尋找服務連結（或其他配置）。不論您需要新增雲端支援至現有應用程式，或使用「入門範本套件」來建立應用程式，目標是提供可攜性，以便 Swift 應用程式能用於許多部署平台。

## 新增 {{site.data.keyword.cloud_notm}} 至現有 Swift 應用程式
{: #addcloud-env}

抽取環境值的路徑，可能會因雲端環境不同而有異。[CloudEnvironment](https://github.com/IBM-Swift/CloudEnvironment.git) 程式庫摘要了各種雲端提供者的環境配置與認證，讓您的 Swift 應用程式在本端執行，或是在 Cloud Foundry、Kubernetes 或 {{site.data.keyword.openwhisk}} 中執行時，都可以一致存取資訊。認證摘要由 `CloudEnvironment` 程式庫提供，其在內部使用 [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv) 來進行 Cloud Foundry 配置，並且使用 [Configuration](https://github.com/IBM-Swift/Configuration) 作為配置管理程式。

使用 `CloudEnvironment`，您可以藉由定義 Swift 應用程式可用來搜尋其對應值的查閱索引鏈，從您應用程式的原始碼中摘要低層次的詳細資料。

`CloudEnvironment` 程式庫提供一致的查閱索引鏈，可用在您的原始碼中。然後，程式庫會在搜尋型樣陣列之間搜尋，以尋找具有配置屬性或服務認證的 JSON 物件。 

### 新增 CloudEnvironment 套件至 Swift 應用程式
若要在您的 Swift 應用程式中使用 `CloudEnvironment` 套件，請在 `Package.swift` 檔案的 **dependencies:** 區段中，指定該套件：
```swift
.package(url: "https://github.com/IBM-Swift/CloudEnvironment.git", from: "8.0.0"),
```
{: codeblock}

然後，將下列檢測代碼新增至您的應用程式：
```swift
import CloudEnvironment

let cloudEnv = CloudEnv()
```
{: codeblock}

### 存取認證
現在，已起始設定 `CloudEnvironment` 程式庫，您可以存取認證，如下列範例所示：
```swift
let cloudantCredentials = cloudEnv.getCloudantCredentials(name: "cloudant-credentials")
// cloudantCredentials.username, cloudantCredentials.password, cloudantCredentials.url, etc.
let objStorageCredentials = cloudEnv.getObjectStorageCredentials(name: "object-storage-credentials")
// objStorageCredentials.username, objStorageCredentials.password, objStorageCredentials.projectID, etc.

let service1Credentials = cloudEnv.getDictionary("service1-credentials")
let service1CredentialsStr = cloudEnv.getString("service1-credentials")

// Get a default PORT and URL
let port = cloudEnv.port
let url = cloudEnv.url
```
{: codeblock}

此範例提供服務的認證集存取權，現在可用來起始設定與這些[受支援服務或一般字典](https://github.com/IBM-Swift/CloudEnvironment#supported-services)的連線。請參閱 [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api) 的 Cloud Foundry 特定配置，以及有關載入配置資料的[配置詳細資料](https://github.com/IBM-Swift/Configuration)。

## 瞭解服務認證
{: #service_creds}

`CloudEnvironment` 程式庫使用名為 `mappings.json` 的檔案（位於 `config` 目錄中），來傳達每個服務的認證的儲存位置。`mappings.json` 檔案支援搜尋使用下列 3 個搜尋型樣類型的值：
- **`cloudfoundry`** - 此為型樣類型，用來在 Cloud Foundry 的服務環境變數 (`VCAP_SERVICES`) 中搜尋值。
- **`env`** - 此為型樣類型，用來搜尋對映至環境變數的值，如在 Kubernetes 或 Functions 中。
- **`file`** - 此為型樣類型，用來在 JSON 檔案中搜尋值。路徑必須相對於 Swift 應用程式的根資料夾。

與查閱索引鍵相關聯的值陣列，會以其出現的順序進行搜尋。

下列顯示 `mappings.json` 檔案的範例：
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

在此範例中，`cloudant-credentials` 及 `object-storage-credentials` 為查閱索引鍵，您的 Swift 應用程式會使用該索引鍵來查閱對應的認證。根據搜尋型樣的陣列，配置管理程式會先在 `VCAP_SERVICES` 中尋找 `my-awesome-cloudant-db`，接著尋找 `my_awesome_cloudant_db_credentials` 作為環境變數。如果其尚未定義，則會在 `localdev` 資料夾中，尋找 `my-awesome-object-storage-credentials.json` 的內容。 

當應用程式在本端執行時，它可以使用儲存在檔案（例如 `localdev/my-awesome-object-storage-credentials.json`）中的認證，如前一個範例所示。您要本端存取之服務的連線資訊（例如，使用者名稱、密碼及主機名稱），全部都儲存在這個檔案中。 

基於安全考量，認證檔並不適合儲存庫。在前一個範例中，`localdev` 資料夾用來儲存本端認證，因此您必須將此資料夾新增至 `.gitignore` 檔案，以避免意外確定。如果您使用「入門範本套件」應用程式，則會為您建立此資料夾，並出現在 `.gitignore` 檔案中。

如需 `mappings.json` 檔案的相關資訊，請參閱[瞭解服務認證](configuration.html#service_creds)小節。

## 透過入門範本套件應用程式使用 Swift 配置管理程式

使用[入門範本套件](https://console.bluemix.net/developer/appledevelopment/starter-kits/)建立的 Swift 應用程式會自動隨附認證及配置，該配置需要在本端執行，也需要在許多 Cloud 部署環境（CF、K8s、VSI 及 Functions）中執行。配置管理程式的基本建立作業位於 `Sources/Application/Application.swift`。當您建立含有服務的 Swift 型入門範本套件應用程式時，會為您建立 `config` 資料夾和 `mappings.json` 檔案。如果您已下載應用程式，`config` 資料夾會包含 `localdev-config.json` 檔案，其中具有您服務的所有認證，且會出現在 `.gitignore` 檔案中。

## 後續步驟
{: #next notoc}

請參閱以下 3 個程式庫，以協助您的應用程式從其環境中自行摘要：

* [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env)
* [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv)
* [Configuration](https://github.com/IBM-Swift/Configuration)
