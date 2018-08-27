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
{:tip: .tip}

# 入門指導教學
{: #set_up}

{{site.data.keyword.cloud}} 提供解決方案及服務，讓 Swift 開發人員及應用程式可以提供客戶所需的安全性、AI 及價值。有了廣泛的產品組合及 SDK，您可以利用這些服務，並將最頂尖的應用程式快速推至市場。Swift 程式設計手冊會教您如何將服務新增至新的或現有的 Swift 應用程式，無論該應用程式是 iOS 用戶端或伺服器端 Swift。
{: shortdesc}

下列指導教學是個進入點，顯示如何從 [{{site.data.keyword.cloud_notm}}Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits)，使用空白的「入門範本套件」來輕鬆建立含有 {{site.data.keyword.mobileanalytics_full}} 的 Swift 行動應用程式。從主控台中，新增 {{site.data.keyword.mobileanalytics_short}} 服務、下載程式碼、以 Xcode 在本端執行 iOS 應用程式、配置，然後監視應用程式。

## 步驟 1. 開發人員需求
{: #dev-requirements}

若要在 {{site.data.keyword.cloud_notm}} 上開始使用 iOS 開發，請確定您符合下列需求。

### 作業系統

開發 Swift 應用程式的最佳作法是使用最新 MacOS 支援的硬體，並使用最新的 iOS 版本進行測試。註冊一個 [Apple Developer](https://developer.apple.com/) 帳戶，以便在實體裝置上進行測試。

### iOS 及 Xcode
{: #ios_and_xcode}

- 安裝 [Xcode 8+](https://developer.apple.com/xcode/)（或更高版本）
- 部署至 [iOS 第 8 版裝置](https://support.apple.com/downloads/ios)（或更高版本）。
- 應用程式提交至 Apple 之前，檢閱 [App Store 提交準則](https://developer.apple.com/app-store/guidelines/)。

### SDK 及相依關係管理

下列工具確保您可以安裝原生 SDK，以使用各種「{{site.data.keyword.cloud_notm}} 服務」。

1. 安裝 [CocoaPods](https://cocoapods.org/) 以支援 {{site.data.keyword.cloud_notm}} SDK：
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
2. 安裝 [Homebrew](https://brew.sh/) 以協助安裝 Carthage：
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

3. 安裝 [Carthage](https://github.com/Carthage/Carthage) 以支援 Watson SDK：
  ```
  brew install carthage
  ```
  {: codeblock}

## 步驟 2. 建立自訂 iOS Swift 應用程式
{: #create-ios-app}

1. 登入 [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits)。
2. 按一下**建立應用程式**。
3. 在[空白入門範本](https://console.bluemix.net/developer/appledevelopment/create-app)頁面上，您可以使用預設配置，或視需要更新欄位。確定選定的語言是 **iOS Swift**。按一下**建立**。

## 步驟 3. 新增 {{site.data.keyword.mobileanalytics_short}} 服務
{: #adding-services}

1. 從「應用程式詳細資料」頁面，按一下**新增資源**按鈕。
2. 選取**行動**，然後按**下一步**。
3. 選取 **{{site.data.keyword.mobileanalytics_short}}**，然後按**下一步**。
4. 按一下**建立**。

## 步驟 4. 下載程式碼並設定 Client SDK
{: #run-locally}

若要下載程式碼，請在 `Apps` > `Your App` 下，按一下**下載程式碼**。即會出現已下載的程式碼，並包含 **{{site.data.keyword.mobileanalytics_short}}** Client SDK。Client SDK 可用於 CocoaPods 及 Carthage。此解決方案使用 CocoaPods。

1. 解壓縮已下載的程式碼。然後使用終端機，導覽至已解壓縮的資料夾。
  ```
  cd <Name of Project>
  ```
2. 此資料夾包含一個 POD 檔，內含必要的相依關係。執行下列指令，以安裝相依關係 (Client SDK)：
  ```
  pod install
  ```
  {: codeblock}

## 步驟 5. 配置應用程式以使用 {{site.data.keyword.mobileanalytics_short}}
{: #configure-analytics}

1. 以 Xcode 開啟 `.xcworkspace`，然後導覽至 `AppDelegate.swift`。
  **附註：**確保您一律開啟新的 Xcode 工作區，而非原始 Xcode 專案檔：`MyApp.xcworkspace`。
   ![開啟 Xcode](images/Xcode.png)

  `BMSCore` 是 Core SDK，而且是 Mobile Client SDK 的基礎。`BMSClient` 是 BMSCore 的類別，且已在 AppDelegate.swift 中起始設定。隨著 BMSCore，{{site.data.keyword.mobileanalytics_short}} SDK 已匯入至專案。
  
2. 已包括分析起始設定碼，如下列 Snippet 所示：
  ```
  // Analytics client SDK is configured to record lifecycle events.
         	Analytics.initialize(appName:dictionary["appName"] as? String,
        			     apiKey: dictionary["analyticsApiKey"] as? String,
        	        	     deviceEvents: .lifecycle)

        	// Enable Logger (disabled by default) and set level to ERROR (DEBUG by default).
        	Logger.isLogStorageEnabled = true
        	Logger.logLevelFilter = .error
  ```
  {: codeblock}

  **附註：**服務認證是 `BMSCredentials.plist` 檔的一部分。

3. 使用 `logger` 來收集用量分析。導覽至 `ViewController.swift` 檔，以查看下列程式碼。
  ```
   func didBecomeActive(_ notification: Notification) {
       Analytics.send()
       Logger.send()
    }
  ```
  {: codeblock}

   如需進階「分析」及記載功能，請參閱[收集用量分析](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics)及[記載](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger)。
   {:tip}

## 步驟 6. 以 {{site.data.keyword.mobileanalytics_short}} 監視應用程式
{{site.data.keyword.mobileanalytics_short}} 服務為行動應用程式開發人員及應用程式擁有者，提供重要的應用程式用量及效能洞察。使用 {{site.data.keyword.mobileanalytics_short}}，應用程式擁有者及開發人員可以瞭解使用者端正在發生什麼事。他們可以使用這個洞察來建置更優良且與使用者相關的應用程式，並在行動應用程式中脫穎而出。

此服務包括的「{{site.data.keyword.mobileanalytics_short}} 主控台」，可讓開發人員及應用程式擁有者監視行動應用程式效能、查看用法統計資料，以及可能搜尋裝置日誌。

1. 從您建立的行動應用程式開啟 `{{site.data.keyword.mobileanalytics_short}}` 服務，或按一下服務旁邊的三個垂直小圓點，然後選取 `Open Dashboard`。
2. 藉由停用 `Demo Mode`，您可以看到「作用中使用者」、「階段作業」及其他「應用程式資料」。您可以依據下列準則來過濾分析資訊：
    * 日期。
    * 應用程式。
    * 作業系統。
    * 應用程式版本。
         ![{{site.data.keyword.mobileanalytics_short}}](images/mobile_analytics.png)
3. [請按一下這裡](https://console.bluemix.net/docs/services/mobileanalytics/app-monitoring.html#monitoringapps)，以設定警示、「監視應用程式」損毀及「監視」網路要求。

## 後續步驟
{: #next-steps}

### 新增其他服務
您可以直接從 Web 主控台新增其他服務至您的 iOS 應用程式，例如下列常用的服務：

* [新增 Push Notifications 服務](/push/push_notifications.html)
* [新增含有 App ID 的使用者鑑別](/authenticate/app_id.html)

### 使用 {{site.data.keyword.cloud_notm}} 開發人員工具
您也可以學習如何使用 [{{site.data.keyword.cloud_notm}} 開發人員工具](../cli/index.html)來開發 Swift 應用程式，該工具提供一個指令行方法來建立、開發及部署端對端 Web、行動及微服務應用程式。

