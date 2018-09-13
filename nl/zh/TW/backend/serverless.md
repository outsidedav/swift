---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}

# 無伺服器開發
{: #serverless}

何謂無伺服器？無伺服器開發型樣是指其伺服器端邏輯在無狀態容器中執行的應用程式開發，該無狀態容器由事件觸發且很短暫（持續單一次執行），並由協力廠商完全管理。在此參照範例（也稱為「函數即服務 (FaaS)」）中，開發人員所提供的無狀態函數，可以在不明確佈建或管理伺服器的情況下，進行觸發然後執行。

藉由摘要伺服器端開發所需的基礎架構及架構，無伺服器架構容許開發人員專注於建置其應用程式，以及撰寫程式碼，並被動執行以變更資料。

IBM 的 FaaS 供應項目 [{{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/)，力求提供無縫的伺服器端開發經驗，而不需要任何特殊的伺服器端知識。使用無伺服器技術，您可以快速開發可擴充的後端解決方案，以符合幾乎所有工作量需求，而不需要提前佈建資源。對於具有無法預期之負載型樣或伺服器關閉時間過長的應用程式，{{site.data.keyword.openwhisk_short}} 可以是出色的雲端解決方案，其效能已獲得改進，且其「使用者付費」系統可協助降低成本。

## 架構變更
{: #comparison}

為了協助您瞭解切換成 FaaS 在架構上的好處，我們使用鏈結至資料庫的簡單 iOS 應用程式來比較傳統與 FaaS 架構。

在比較傳統的架構中，iOS 應用程式會卸載網路密集的作業，或在遠端集中處理資料，而且本身是由自己的服務或儲存空間選項所連接。在傳統系統中，沉重的負擔都壓在單一伺服器上，它既要處理鑑別，也要處理密集作業以將用戶端的壓力減到最低，還要根據其使用者提供同步化。

無伺服器架構可以變更這個結構，讓它看起來更像下列影像。

![](./images/Architecture.png) 圖 1. 無伺服器架構

無伺服器架構運用擴充性高且封裝大量伺服器端邏輯的函數，將部分邏輯卸載給用戶端（及外部服務），而不是在單一伺服器內處理所有處理作業及鑑別邏輯。

看一下圖解，您可以瞭解到下列重點：

1. 用戶端會針對「應用程式 ID」這類「身分提供者」進行鑑別。
2. 對 FaaS 後端 API 的呼叫包括存取記號。
3. 後端由 {{site.data.keyword.openwhisk_short}} 實作。公開為 Web 動作的無伺服器動作，預期會在要求標頭中傳送記號，並驗證其有效性（簽章及到期日），以提供實際 API 的存取權。
4. 當用戶端提交資料時，意見會儲存在 {{site.data.keyword.cloudant_short_notm}}。
5. 意見文字由 {{site.data.keyword.toneanalyzershort}} 進行處理。
6. 根據分析結果，由 {{site.data.keyword.mobilepushshort}} 將通知傳回給用戶端。
7. 用戶端接收到通知。

在純正的無伺服器模型中，因為無法儲存使用者的狀態，所以用戶端通常需要承擔額外的責任。例如，在此情況下，授權由用戶端及 {{site.data.keyword.appid_short_notm}} 身分提供者服務進行處理。

雖然無伺服器架構並非十全十美，但是在正確的團隊與使用條件下，仍有許多好處。請參閱一些特定範例，以瞭解一些常用的[使用案例](#use_cases)。

## 無伺服器好處
{: #benefits}

### 減少成本

外包與系統管理相關聯的時間與貨幣成本，可以減少與傳統後端伺服器相關聯的整體成本。此外，{{site.data.keyword.openwhisk_short}} 不同於傳統的計算技術，因為您僅支付程式碼在滿足要求時所花費的時間（四捨五入到最近的 100 毫秒）。相對於 VM 及容器等其他技術（其利用率不可能 100% 且會佔用雲端提供者系統的記憶體），可節省大量成本。

### 高可用性及可調整性

無伺服器架構基本上可立即調整且持續可用。

### 速度和簡化的開發

藉由刪除系統管理的需求，並提供簡單的部署介面，無伺服器參照範例加速了應用程式的開發速度。開發人員可以使用為了回應事件驅動世界而執行的動作序列，來快速建置應用程式。

## 範例使用案例
{: #use_cases}

### 行動式後端系統
![](./images/cloud-functions-rest-api-trigger.png)

行動開發人員可以輕鬆地存取伺服器端邏輯，並將運算密集作業外包給可擴充的雲端平台。使用 iOS SDK，您可以用 Swift 等語言來實作函數，並輕鬆地取用伺服器端函數，而不需要任何伺服器端經驗。

### 資料處理

![](./images/cloud-functions-cloudant-trigger.png)

您只要透過內建觸發程式更新資料儲存庫中的資料，就可以執行程式碼。您也可以透過實用的伺服器端程式設計模型，輕鬆地自動執行處理程序，例如，音訊正規化、旋轉影像、銳利化、降噪、產生縮圖或視訊轉碼。

### 認知資料處理

只要資料可供使用，您就可立即進行分析。讓您的函數充分運用功能強大的認知服務（例如，IBM Watson），來偵測影像或視訊中的物件或人員。

### 排定的作業

定期執行您的函數，並定義遵循類似 cron 語法的排程，以指定動作的預期執行時間。

## API 參考資料
{: #openwhisk_start_api notoc}

<!-- * [REST API Documentation](./openwhisk_reference.html#openwhisk_ref_restapi)-->
* [REST API](https://console.{DomainName}/apidocs/98)

## 相關鏈結
{: #general notoc}

* [探索：{{site.data.keyword.openwhisk_short}}](http://www.ibm.com/cloud-computing/bluemix/openwhisk/)
<!-- redirects to link above * [{{site.data.keyword.openwhisk_short}} on IBM developerWorks](https://developer.ibm.com/openwhisk/)-->
* [Apache OpenWhisk 專案網站](http://openwhisk.org)
* [無伺服器相關資訊](https://martinfowler.com/articles/serverless.html)
