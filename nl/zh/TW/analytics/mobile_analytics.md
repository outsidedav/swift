---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 收集行動分析
{: #mobile_analytics}

{{site.data.keyword.mobileanalytics_short}} on {{site.data.keyword.cloud_notm}} 可提供開發人員、管理者及商業利害關係人，對其行動應用程式的效能及其使用情形的見解。使用 {{site.data.keyword.mobileanalytics_short}} 服務，您可以：

 - **立即瞭解** - 查看即時效能及用量度量值。

 - **快速實作** - 在 {{site.data.keyword.cloud_notm}} 中建立服務實例、將 SDK 新增至專案、將兩行程式碼貼入應用程式，而且您已備妥可收集許多預先定義的度量值。

 - **收集您想要的資料** - 使用自訂事件來檢測應用程式、查看使用者如何與應用程式互動、追蹤花費，以及監視應用程式活動。

 - **檢視度量值摘要** - {{site.data.keyword.mobileanalytics_short}} 主控台提供現成圖表，而不需撰寫查詢。

 - **聚焦於對您重要的事物** - 依時間、配接器、應用程式、應用程式版本、OS、OS 版本或裝置模型來過濾度量值。

 - **快速找到問題** - 監視損毀狀態。設定重要度量值的警示觸發程式，並將警示遞送至任何 REST 端點。

 - **疑難排解主要原因** - 在應用程式中使用自訂記載，並自動上傳日誌，然後從主控台搜尋這些日誌。往下探查到損毀事件，以查看堆疊追蹤。

## 開始之前
{: #prereqs-analytics}

首先，請確定您具備下列必要條件：

 - iOS 8.0+ / watchOS 2.0+
 - Xcode 7.3、8.0
 - Swift 2.2 - 3.0
 - CocoaPods 或 Carthage

## 步驟 1. 建立 {{site.data.keyword.mobileanalytics_short}} 實例
{: #create-analytics}

1. 在 {{site.data.keyword.cloud_notm}} 型錄中，按一下**行動** > **{{site.data.keyword.mobileanalytics_short}}**。即會開啟服務配置畫面。
2. 提供服務實例的名稱，或使用預設名稱。
3. 按一下**建立**。
4. 在導覽窗格中，按一下**連線**，選取應用程式，並將其連結至您的服務。如果建立期間未連結服務實例與應用程式，您可以稍後再進行此操作。

## 步驟 2. 安裝 iOS Swift SDK
{: #install-analytics-swift}

此服務提供平台專用的 SDK，用來簡化應用程式開發作業。{{site.data.keyword.cloud_notm}} Mobile Services Swift SDK 可與 CocoaPods 或 Carthage 一同安裝。如需相關資訊，請參閱 [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics)。

您可以使用 {{site.data.keyword.mobileanalytics_full}} SDK 來檢測您的行動應用程式。Swift SDK 適用於 iOS 及 watchOS。

1. 請確定已正確設定 Xcode。若要瞭解如何設定 iOS 開發環境，請參閱 [Apple Developer 網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.apple.com/support/xcode/){: new_window}。閱讀 Client SDK Swift Analytics 的 [Xcode 需求 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#requirements){: new_window}。

2. 啟用位置 API，方法為在應用程式專案資料夾的 `Info.plist` 檔案中新增內容。例如，`Privacy - Location Usage Description`，並做適當的調整以新增位置 API，例如，「應用程式需要啟用位置服務」。

{{site.data.keyword.mobileanalytics_short}} SDK 隨 [CocoaPods ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cocoapods.org/){: new_window} 及 [Carthage ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/Carthage/Carthage#getting-started){: new_window}（Cocoa 專案的相依關係管理程式）一起配送。CocoaPods 及 Carthage 會自動從儲存庫中下載構件，並讓它們可供應用程式使用。選取 CocoaPods 或 Carthage：

### CocoaPods
{: #cocoapods-analytics}

1. 遵循 GitHub 上的 [{{site.data.keyword.Bluemix_notm}} Mobile Services Swift SDK 指示 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#cocoapods){: new_window}，使用 CocoaPods 來安裝 `BMSAnalytics`，並將其新增至您的 Podfile。

2. 在安裝 iOS Client SDK 之後，請[匯入並起始設定](sdk.html#initalize-ma-sdk) Analytics Client SDK。   

### Carthage
{: #carthage-analytics}

如果您不是使用 CocoaPods，則可以使用 [Carthage ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos){: new_window}，將架構新增至專案中。

1. 遵循 GitHub 上的 [Carthage 安裝指示 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#carthage){: new_window} 來安裝 `BMSAnalytics`。

2. 在安裝 iOS Client SDK 之後，請匯入然後起始設定 Analytics Client SDK。

## 步驟 3. 起始設定 SDK
{: #initialize-analytics}

使用 {{site.data.keyword.mobileanalytics_short}}，您可以收集下列種類的資料，每一種都需要不同程度的檢測：

**預先定義的資料**：
此種類包括適用於所有應用程式的一般用法及裝置資訊。其指出應用程式使用量、頻率或持續時間。在應用程式中起始設定 {{site.data.keyword.mobileanalytics_short}} SDK 之後，會自動收集預先定義的資料。

**應用程式日誌訊息**：
此種類可讓開發人員在整個應用程式中新增程式碼行，這些程式碼可記載自訂訊息，以協助開發和除錯。開發人員會將嚴重性/詳細層次指派給每一個日誌訊息。

**自訂事件**：
此種類包括您自行定義及應用程式特有的資料。這個資料顯示您應用程式內發生的事件，例如頁面檢視、按鈕點選或應用程式內採購。除了在應用程式中起始設定 {{site.data.keyword.mobileanalytics_short}} SDK 之外，您還必須為您要追蹤的每一個自訂事件新增一行程式碼。

## 步驟 4. 識別服務認證 API 金鑰值
{: #analytics-clientkey}

識別設定 Client SDK 之前的 **API 金鑰**值。需要有「API 金鑰」，才能起始設定 Client SDK。

1. 開啟 {{site.data.keyword.mobileanalytics_short}} 服務儀表板。
2. 展開**檢視認證**，以顯示「API 金鑰」值。當您起始設定 {{site.data.keyword.mobileanalytics_short}} Client SDK 時，需要有 API 金鑰值。

## 步驟 5. 起始設定應用程式來收集分析
{: #initalize-ma-sdk}

起始設定應用程式，以啟用將日誌傳送至 {{site.data.keyword.mobileanalytics_short}} 服務。

1. 匯入 Client SDK。

    Swift SDK 適用於 iOS 及 watchOS。將下列 `import` 陳述式新增至 `AppDelegate.swift` 專案檔開頭處，以匯入 `BMSCore` 和 `BMSAnalytics` 架構：
	```swift
	import BMSCore
	import BMSAnalytics
	```
	{: codeblock}  

2. 在應用程式中起始設定 {{site.data.keyword.mobileanalytics_short}} Client SDK。

	首先，起始設定 `BMSClient` 類別，方法為將起始設定碼放在應用程式委派的 `application(_:didFinishLaunchingWithOptions:)` 方法中或最適合您專案的位置中。
	```swift
	BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Make sure that you point to your region
	```
	{: codeblock}

	您必須起始設定具有 **bluemixRegion** 參數的 `BMSClient`。在起始設定程式中，**bluemixRegion** 值會指定您所使用的 {{site.data.keyword.Bluemix_notm}} 部署。

3. 您可以使用應用程式物件，並提供您的應用程式名稱，來起始設定 Analytics。

	您為應用程式所選取的名稱 (`your_app_name_here`) 會在 {{site.data.keyword.mobileanalytics_short}} 主控台中顯示為應用程式名稱。應用程式名稱是用來作為過濾器，以在儀表板中搜尋應用程式日誌。當您跨平台（例如，iOS）使用相同的應用程式名稱時，不論是從哪個平台傳送日誌，您都可以看到來自該同名應用程式的所有日誌。

	您還需要 [API 金鑰](#analytics-clientkey)值。
	```swift
	Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, collectLocation: true, deviceEvents: .lifecycle, .network)
	```
    {: codeblock}

4. 現在，應用程式已起始設定，且已備妥可收集分析。接下來，您可以將分析資料傳送至 {{site.data.keyword.mobileanalytics_short}} 服務。		

## 步驟 6. 收集用量分析
{: #usage-analytics}

您可以配置 {{site.data.keyword.mobileanalytics_short}} Client SDK 來記錄用量分析，並且將記錄的資料傳送至 {{site.data.keyword.mobileanalytics_short}} 服務。

使用下列 API 開始記錄及傳送用量分析：
```swift
// Disable recording of usage analytics (eg: to save disk space)
// Recording is enabled by default

Analytics.isEnabled = false

// Enable recording of usage analytics

Analytics.isEnabled = true

// Send recorded usage analytics to the Mobile Analytics Service

Analytics.send(completionHandler: { (response: Response?, error: Error?) in
    if let response = response {

	  // Handle Analytics send success here.
    }
    if let error = error {
        // Handle Analytics send failure here.
    }
})
```
{: codeblock}

用於記載事件的用法分析範例：
```swift
// Log a custom analytics event
let eventObject = ["customProperty": "propertyValue"]
Analytics.log(metadata: eventObject)
```
{: codeblock}

## 步驟 7. 使用日誌程式
{: #analytics-logger}

{{site.data.keyword.mobileanalytics_full}} Client SDK 提供的記載架構類似於您可能熟悉的其他記載架構，例如 `java.util.logging` 或 `log4j`。記載架構支援多個每一套件日誌程式實例、不同的記載層次、應用程式損毀的堆疊追蹤擷取等。

您也可以配置將記載的資料儲存至應用程式執行所在的裝置，並且於稍後將裝置日誌傳送至 {{site.data.keyword.mobileanalytics_short}} 服務。

{{site.data.keyword.mobileanalytics_short}} Client SDK 記載架構支援下列具有建議使用準則的記載層次（依最不詳細到最詳細程度列出）：

  * `FATAL` - 用於無法復原的損毀或停滯。`FATAL` 層次是保留用於記載無法復原的錯誤，這對使用者而言就像應用程式損毀
  * `ERROR` - 用於非預期的異常狀況或非預期的網路通訊協定錯誤
  * `WARN` - 用來記載非視為嚴重錯誤的用法警告（例如使用已淘汰的 API 或慢速網路回應）
  * `INFO` - 用於報告起始設定事件以及可能重要但不緊急的其他資料
  * `DEBUG` - 用於報告除錯陳述式，以協助開發人員解決應用程式問題報告

### 記載層次情境
{: #log-level-scenario}

日誌程式層次配置成 `FATAL` 時，日誌程式會擷取未捕捉的異常狀況，但不會擷取任何導致損毀事件的日誌。您可以設定更詳細的日誌程式層次，確保也一併擷取可能會導致 `FATAL` 日誌程式項目（例如 `WARN` 及 `ERROR`）的日誌。

日誌程式層次設定成 `DEBUG` 時，您還會取得 Mobile Analytics Client SDK 日誌，包括在傳送日誌時。

### 日誌程式用法範例
{: #sample-logger-usage}

**附註：**請先確定您的應用程式已配置成使用 {{site.data.keyword.mobileanalytics_short}} Client SDK，然後使用記載架構。

下列程式碼 Snippet 顯示「日誌程式」用法範例：
```swift
// Configure Logger. Disabled by default;

Logger.isLogStorageEnabled = true

// Change the minimum log level (optional). Default is - LogLevel.debug

Logger.logLevelFilter = LogLevel.info

// Multiple log instances can be created to organize your logs

let logger1 = Logger.logger(name: "feature1Logger")
let logger2 = Logger.logger(name: "feature2Logger")

// Log messages with different levels

logger1.debug(message: "debug message for feature 1")

// logger1.debug message isn't logged as logLevelFilter is set to info

logger2.info(message: "info message for feature 2")

// Send logs to the Mobile Analytics Service

Logger.send(completionHandler: { (response: Response?, error: Error?) in
        if let response = response {
        logger.debug(message: "Status code: \(response.statusCode)")
        logger.debug(message: "Response: \(response.responseText)")
    }
    if let error = error {
        logger.error(message: "Error: \(error)")
    }
})
```
{: codeblock}

基於隱私權考量，您可以針對使用發行模式建置的應用程式停用「日誌程式」輸出。「日誌程式」類別預設會將日誌列印至 Xcode 主控台。在您目標的建置設定中，將 `-D RELEASE_BUILD` 旗標新增至發行建置配置的**其他 Swift 旗標**區段。
{: tip}

## 步驟 8. 記載位置資料
{: #location-logging}

可能會透過這個提供的 API 從應用程式記載行動裝置的位置：
```swift
Analytics.logLocation();
```

此 API 可讓應用程式將其位置（以緯度、經度形式表示）傳送至應用程式階段作業之間的伺服器。除了明確的 `location-logging` API 呼叫之外，SDK 會傳送每個應用程式階段作業的裝置位置。位置會針對起始應用程式階段作業環境定義，以及使用者交換器應用程式階段作業（亦即，應用程式階段作業之間的使用者交換器）環境定義來傳送。起始設定 SDK 時，必須啟用位置 API。

若要呼叫 `location-logging` API，請在 SDK 起始設定中，將 `collectLocation` 參數設為 `true`：
```swift
Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
```

## 步驟 9. 進行網路要求
{: #network-requests}

您可以配置 {{site.data.keyword.mobileanalytics_short}} Client SDK 來[進行網路要求](/docs/mobile/sdk_network_request.html)。您務必要已經起始設定 `BMSClient` 及 `BMSAnalytics`，並且匯入 Client SDK。

## 步驟 10. 報告損毀分析
{: #report-crash-analytics}

將分析及日誌資訊傳送至 {{site.data.keyword.mobileanalytics_short}}，即可查看[應用程式損毀資料](app-monitoring.html#monitor-app-crash)。

`Analytics.send()` 方法會在**損毀**頁面上移入**損毀概觀**及**損毀**表格。您可以使用起始設定及傳送處理程序進行分析的方式來啟用圖表。

`Logger.send()` 方法會在**疑難排解**頁面上移入**損毀摘要**及**損毀詳細資料**表格。您必須藉由在應用程式碼中新增陳述式，讓您的應用程式儲存及傳送日誌，以移入圖表：
```swift
Logger.isLogStorageEnabled = true
Logger.logLevelFilter = LogLevel.Fatal // or greater
```
{: codeblock}

請參閱 iOS [日誌程式用法範例](sdk.html##sample-logger-usage)。

## 步驟 11. 追蹤作用中使用者
{: #trackingusers}

如果您的應用程式可以辨識裝置上的唯一使用者，則您可以追蹤有多少位使用者正在積極使用您的應用程式，方法是將作用中使用者的使用者名稱傳遞至 {{site.data.keyword.mobileanalytics_short}}。

使用 `hasUserContext=true` 來起始設定 {{site.data.keyword.mobileanalytics_short}}，以啟用使用者追蹤。否則，{{site.data.keyword.mobileanalytics_short}} 對於每個裝置只會擷取一位使用者。

請新增下列程式碼，以在使用者登入時進行追蹤：
```swift
Analytics.userIdentity = "username"
```
{: codeblock}

## 步驟 12. 測試應用程式
{: #appid_testing}

所有項目都正確配置嗎？測試時間到！

1. 開啟應用程式。如果您有 Web 應用程式，請使用瀏覽器。如果您有 iOS 用戶端應用程式，請用 Xcode 模擬器開啟。
2. 在模擬器或裝置上編譯並執行應用程式。
3. 使用 GUI，逐步進行登入應用程式的處理程序。
4. 移至 {{site.data.keyword.mobileanalytics_short}} 主控台，以查看應用程式的用量分析。您也可以藉由[設定警示](/docs/services/mobileanalytics/app-monitoring.html#alerts)及[監視應用程式損毀](/docs/services/mobileanalytics/app-monitoring.html#monitor-app-crash)來監視應用程式。

## 下一步
{: #next-analytics notoc}

 - 若要進一步瞭解服務，請閱讀[文件](/docs/services/mobileanalytics/index.html#getting-started-tutorial)。

 - 如需使用行動服務及 {{site.data.keyword.Bluemix_notm}} 的簡介，請參閱[開始使用 IBM Cloud 上的行動應用程式](/docs/services/mobile/index.html)。

 - 「入門範本套件」是運用 {{site.data.keyword.cloud_notm}} 特性最快的方式之一。請檢視[行動開發人員儀表板](https://cloud.ibm.com/developer/mobile/dashboard)中的所有可用入門範本套件。下載程式碼。執行應用程式！

 - 您可以使用 [Swagger 使用者介面](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/)，來快速檢閱 REST API 文件。
