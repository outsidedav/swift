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
{:tip: .tip}

# 使用產生的 SDK 將後端服務新增至您的應用程式
{: #sdk-cli}

「{{site.data.keyword.IBM}} SDK 產生器」外掛程式可以安裝在 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/bluemix_cli/get_started.html)。

使用「{{site.data.keyword.IBM_notm}} SDK 產生器」外掛程式，您可以使用產生的 SDK，將後端服務整合至您的應用程式。當 REST API 發生變更時，您可以重新產生 SDK 並取代舊的 SDK，以輕鬆升級 SDK。接著，您便可以將 CLI 新增至 DevOps 管線，並確定每次建置應用程式時，SDK 一律與 API 規格一致。

REST API 定義必須有效，且在即時伺服器端點或您系統的本端檔案上進行管理。

## 開始之前
{: #prereqs}

請確定您具備下列必要條件：

* [{{site.data.keyword.cloud_notm}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://bluemix.net){: new_window} 帳戶。
* 符合[開放式 API ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.openapis.org/){: new_window} 規格的有效 API 定義。

## 安裝 SDK 外掛程式
{: #installation}

1. [安裝 {{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/bluemix_cli/get_started.html)。

2. [安裝 SDK 外掛程式](/docs/cli/sdk/index.html)。
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## 產生 SDK
{: #commands}

輸入以下指令來產生 SDK：
```
ibmcloud sdk generate [arguments...] [command options]
```
{: codeblock}

### 引數
{: #gen-args}

* **APP_NAME** - 您現行空間中的 Cloud Foundry 應用程式名稱。
* **OPENAPI_DOC_LOCATION** - 原始 REST API 定義 JSON 或 yaml 的 URL 或相對檔案路徑。
* **GENERATED_SDK_NAME**（選用）- 所產生 SDK 的名稱。

### 選項
{: #gen-options}

**平台**（必要）：
  * `--ios` - 產生 iOS Swift SDK。
  * `--swift` - 產生 Swift 伺服器 SDK。
  * `--js` - 產生 JavaScript SDK。

**位置**（必要）：

指定 `OPENAPI_DOC_LOCATION` 引數的類型。

  * `-r` - 遠端 URL。
  * `-f` - 檔名。
  * `-a` - 在 {{site.data.keyword.coud_notm}} 上執行的應用程式
  * `-l` - 本端主機 URL。

**選用**：
  * `--output "YOUR_RELATIVE_PATH"` - 將產生的 SDK 放在由 `YOUR_RELATIVE_PATH` 指定的目錄中（如果存在現有 SDK，則會被改寫）。
  * `--unzip` - 解壓縮產生的 SDK（如果有 SDK 構件存在，則會改寫現有資料）。

### 用法
{: #gen-usage}

若要從 {{site.data.keyword.cloud_notm}} 中所執行的 Cloud Foundry 應用程式產生 SDK，您可以使用應用程式的名稱作為 CLI 的參數。下列指令使用應用程式的名稱作為 `SDK_Name`。

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

若要從開放式 API 定義檔或是本端 JSON 或 yaml 檔案的 URL 產生 SDK，請使用下列指令。

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}


## 驗證開放式 API 定義
{: #validating}

執行下列指令：`ibmcloud sdk validate [argument]`

### 引數
{: #val-args}

* `APP_NAME` - 您現行空間中的 Cloud Foundry 應用程式名稱。
* `OPENAPI_DOC_LOCATION` - 原始 REST API 定義 JSON 或 yaml 的 URL 或相對檔案路徑。

### 用法
{: #val-usage}

若要驗證在 {{site.data.keyword.cloud_notm}} 中執行之 Cloud Foundry 應用程式的 API 規格，您可以使用應用程式的名稱作為 CLI 的參數。
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

若要透過 API 規格文件的 URL、或本端 JSON 或 yaml 檔案的 URL 來驗證 SDK，請使用下列指令：
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

