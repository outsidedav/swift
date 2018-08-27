---

copyright:
  years: 2018
lastupdated: "2018-08-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 使用 Kitura 建立應用程式
{: #kitura}

[Kitura](http://www.kitura.io) 是一個伺服器端 Swift 架構，用來建置 iOS 後端及 Web 應用程式。此架構可建立 REST API，只要使用 URLSession SDK（例如，Alamofire、RestKit），或 Kitura 自己提供的 [KituraKit](https://github.com/ibm-swift/kiturakit) SDK，即可從 iOS 應用程式呼叫該 REST API。

Kitura 可與 {{site.data.keyword.cloud}} 提供的所有服務與功能整合，包括 {{site.data.keyword. appid_short}}、{{site.data.keyword.mobilepushshort}} 及 {{site.data.keyword.mobileanalytics_short}}，以及資料庫、機器學習及其他服務。然後，可以在 {{site.data.keyword.cloud}} 中，使用 Cloud Foundry 或 Docker（Kubernetes 型）平台，來部署及自動擴充 Kitura。

Kitura 提供 `kitura` [指令行介面 (CLI)](http://www.kitura.io/en/starter/gettingstarted.html)，可簡化建立、建置、測試及部署 Kitura 應用程式。使用 Kitura CLI 所建置的應用程式完全支援部署至支援 Cloud Foundry、Docker 及 Kubernetes 技術的任何雲端。然而，如果您是特別針對 {{site.data.keyword.cloud}} 進行建置，則建議您在瀏覽器中使用 IBM Apple Development Console，或者使用 {{site.data.keyword.dev_cli_notm}}。此外，當這兩種方法使用相同的基礎技術時，Apple Development Console 和 IBM Developer Tools 會為您建立一個受管理專案及部署管線，並佈建您應用程式所需的服務。

## 開始之前

首先，請確定您具備下列必要條件：  

* iOS 11.0+  
* Xcode 9.0  
* Swift 4.0+  
* Cocoapods  

## 步驟 1. 使用瀏覽器建立 Kitura 專案

1. 移至 Apple Development Console 的「入門範本套件」區段。選取預先定義的入門範本，例如，"Swift for Backend for Frontend API"，或使用**建立專案**磚來建立自訂專案。按一下**建立專案**。
2. 命名您的專案，並選取專案的部署地點。如果您不確定要在哪裡部署應用程式，請使用預設值，因為稍後還可以變更預設值。
3. 選取 Swift 語言。即會建立伺服器端 Swift 專案。同時還會顯示「主機」與「網域」選項，該選項可形成應用程式的 URL。如果您不確定，請使用預設值，而且您也可以提供來自網域提供者，且應用程式所在的專屬自訂網域。
4. 您可以為您要建置的 REST API 提供 OpenAPI（又稱為 Swagger）定義。如果您有這類定義，則會在 Kitura 中建立包含對應處理程式函數的 REST API。如果您沒有 OpenAPI 定義，也請不用擔心，因為 Kitura 可讓您使用其 Router API，輕易地手動建立 REST API。
5. 按一下**建立專案**。

即會建立一個專案，但是該專案尚未使用任何其他服務或功能。您可以使用**新增資源**按鈕來新增服務，或者按一下**下載程式碼**按鈕來取得專案的程式碼。您也可以輕易地將服務新增至現有的專案。

## 步驟 2. 新增服務

1. 按一下**新增資源**按鈕，以新增服務。即會顯示服務種類的畫面。例如，選取**資料**可查看可用的資料庫，然後選取 **Cloudant NoSQL DB**。
2. 選取服務的定價方案，例如，「精簡」，然後按一下**建立**。

即會建立一個服務實例，其會提供應用程式的服務認證，在某些情況下，還會新增必要的程式碼至您的專案，以包含連至服務的正確連線。您現在可以使用**新增資源**按鈕來新增其他服務，或者按一下**下載程式碼**按鈕來取得專案的程式碼。

下載您的專案之後，您可以開始使用您的應用程式。

## 步驟 3. 使用 Xcode 開發應用程式
下載您的專案之後，您可以使用 Xcode 來修改及開發該專案，然後上傳已修改的應用程式，以便部署至雲端。

1. 建立 Xcode 專案。  
  您必須先使用 Swift Package Manager 指令來建立 Xcode 專案檔，才能以 Xcode 使用您的專案。請從已下載專案的根目錄（包含 `Package.swift` 檔），執行下列指令：
  ```
  swift package generate-xcodeproj
  ```
  {: codeblock}

  即會在相同的目錄中建立 Xcode 專案，您稍後可以開啟。

2. 設定專案的 Xcode 目標。  
  若要執行 Kitura 伺服器，您必須按一下工具列上的 **project_name-Package** 區段，然後選取功能表中的 **project_name** 目標，以編輯架構。請檢查目標裝置是否設為 **My Mac**。

3. 本端執行 Kitura 伺服器  
  按一下**執行**按鈕，或使用`⌘+R` 索引鍵快速鍵來啟動 Kitura 伺服器。一旦啟動伺服器之後，您可以檢查下列標準 Kitura URL 是否正在執行：
  * Kitura 監視：[http://localhost:8080/swiftmetrics-dash/]()
  * Kitura 性能檢查：[http://localhost:8080/health]()

## 步驟 5. 新增 REST API
即會建立架構 Kitura 伺服器，但其不提供 iOS 應用程式可使用的任何 REST API。使用最少的程式碼，在 Kitura 中新增 REST API。下列步驟顯示如何為了 `/meals` 上的 `GET` 要求，新增一個 REST API，其設計來傳回 Kitura 伺服器所儲存的 `Meal` 物件。

1. 將 `Meal` 結構新增至 `Sources/Application/Application.swift` 檔：  
  宣告應用程式類別之前，新增下列以建立符合 Codable 通訊協定的 Meal 結構：  
  ```swift
	struct Meal: Codable {
	    var name: String
	    var photo: Data
	    var rating: Int
	}
  ```
  {: codeblock}

2. 將 `let cloudEnv = CloudEnv()` 新增至下列程式碼區段，以新增「字典」至 `Sources/Application/Application.swift` 檔，以儲存 `Meal` 物件：
 
  ```swift
  private var mealStore: [String: Meal] = [:]
  ```
  {: codeblock}

3. 將下列程式碼新增至`postInit()` 函數，以將 `/meals` 上 `GET` 要求的處理程式新增至 `Sources/Application/Application.swift` 檔：  

  ```swift
  router.get("/meals", handler: loadHandler)
  ```
  {: codeblock}

4. 將下列程式碼新增成 `App` 類別中的另一個函數，以將 loadHandler 函數實作至 `Sources/Application/Application.swift` 檔：  

  ```swift
  func loadHandler(completion: ([Meal]?, RequestError?) -> Void ) {
      let meals: [Meal] = self.mealStore.map({ $0.value })
    completion(meals, nil)
  }
  ```
  {: codeblock"}

現在，您有一個 REST API 適用於 `/meals` 上的 `GET` 要求，其會回應 `Meals` 的任何陣列或錯誤。

5. 執行專案。  
  按下**執行**按鈕，或使用`⌘+R` 索引鍵快速鍵。如果系統提示您，請選取**容許送入的網路連線**。

6. 測試 REST API。
  如果使用 `GET` 要求（其符合 Web 瀏覽器要求伺服器中資料的方式），您可以使用下列 URL 來測試 REST API：  
  ```
  * `GET /meals`:	[http://localhost:8080/meals]()
  ```

  傳回空陣列 `[]`，因為目前沒有任何 `Meal` 物件儲存在 `mealStore` 中。 

您可以使用 [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend) 指導教學，其可協助您建置一組 REST API，用於儲存 `Meal` 物件，以及從 iOS 應用程式中提取及刪除該物件，包括儲存資料在資料庫中。
{: tip}

## 步驟 6. 將 KituraKit 安裝至 iOS 應用程式
使用 Kitura 伺服器所建置的 REST API 為標準 Web API，可從任何應用程式中使用，不論使用的用戶端程式庫為何，或者用戶端以哪個語言撰寫。這表示您可以使用 Alamofire、RestKit 或 URLSession 來連線至伺服器。此外，Kitura 也提供最佳的訂製用戶端連接器，以便簡化以 KituraKit 形式從 iOS 呼叫其 REST API 的作業。 

KituraKit 提供 Kitura 中所用路由器處理程式 API 的鏡映影像，使其可能輕易地在用戶端及伺服器之間共用 Swift 類型。針對傳送至伺服器，或接收自伺服器的資料，此特性都不需要對該資料明確執行 JSON 編碼及解碼。

下列步驟顯示如何將 KituraKit 安裝至您的 iOS 應用程式中，並用來呼叫使用 Kitura 所建立的 `GET /meals` REST API：

1. 在 iOS 應用程式目錄的根目錄中，建立一個 POD 檔（如果沒有的話）：
  ```
  pod init
  ```
  {: codeblock}

2. 編輯 POD 檔，為您的專案設定一個 iOS 11 廣域平台，做法為將下列行：
  ```
  # platform :ios, '9.0'
  ```
  {: codeblock}

  取代為下列程式碼：
  ```
  platform :ios, '11.0'
  ```
  {: codeblock}

3. 透過在 `# Pods for <application name>` 下新增下列內容，將 KituraKit 新增至 POD 檔：
  ```
  pod 'KituraKit', :git => 'https://github.com/IBM-Swift/KituraKit.git', :branch => 'pod'
  ```
  {: codeblock}

4. 儲存 POD 檔並使用下列指令來安裝 KituraKit：
  ```
  pod install
  ```
  {: codeblock}

5. 透過開啟工作區（而非專案）的方式，重新開啟形式為 Xcode 的應用程式。您現在可以將 Kitura 伺服器中所使用的相同 `Meal` 定義，新增至您的 iOS 應用程式：
  ```swift
  struct Meal: Codable {
      var name: String
      var photo: Data
      var rating: Int
  }
  ```
  {: codeblock}

  然後使用下列程式碼來製作連至 Kitura 伺服器的連線：
  
  ```swift
  import KituraKit
  
  // Connect to the Kitura server on http://localhost:8080
  guard let client = KituraKit(baseURL: "http://localhost:8080") else {
      print("Error creating KituraKit client")
      return
  }
  
  // Make a request to `GET /meals`, expecting a [Meal] or a RequestError
  client.get("/meals") { (meals: [Meal]?, error: RequestError?) in
      // Check for an error
      guard error == nil else {
          print("Error saving meal to Kitura: \(error!)")
          return
      }
      // Check for meals
      guard let meals = meals else {
          print("Received Meals!")
          return
      }
  }
  ```
  {: codeblock}

使用 `client.get("/meals")` 呼叫 Kitura 伺服器，而傳回的回呼符合 Kitura 完成處理程式中的參數。

您可以使用 [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend) 指導教學，其可協助您建置一組 REST API，用於儲存 `Meal` 物件，以及從 iOS 應用程式中提取及刪除該物件，包括儲存資料在資料庫中。
{: tip}

## 後續步驟
現在，您有 Kitura 伺服器，且您的 iOS 應用程式可以呼叫其所提供的 REST API，您已準備好將您的伺服器部署至 {{site.data.keyword.cloud_notm}}。使用 Containers with Kubernetes、Secure Containers 或 Cloud Foundry，即可完成部署作業。
