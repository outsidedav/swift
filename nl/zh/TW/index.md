---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: getting started swift, custom app, create app swift, stater kit swift, apple app swift, swift dependency, ios development

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# 入門指導教學
{: #getting_started_swift}

{{site.data.keyword.cloud}} 提供解決方案及服務，讓 Swift 開發人員能夠建置整合了客戶所需之的安全性、AI 及價值的應用程式。有了廣泛的產品組合及 SDK，您可以使用這些服務，並將最頂尖的應用程式快速推向市場。本 Swift 程式設計說明如何將服務新增至新的或現有 Swift 應用程式，無論該應用程式是 iOS 用戶端或伺服器端 Swift。
{: shortdesc}

下列指導教學顯示如何從 [{{site.data.keyword.cloud_notm}}Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")，使用空白的「入門範本套件」來輕鬆建立含有 {{site.data.keyword.mobileanalytics_full}} 的 Swift 行動應用程式。從主控台中，新增 {{site.data.keyword.mobileanalytics_short}} 服務、下載程式碼、以 Xcode 在本端執行 iOS 應用程式、配置，然後監視應用程式。

## 步驟 1. 開發人員需求
{: #dev-requirements-swift}

若要在 {{site.data.keyword.cloud_notm}} 上開始使用 iOS 開發，請確定您符合下列需求。

### 作業系統
{: #swift-os-requirements}

開發 Swift 應用程式的最佳作法是使用最新的 MacOS 支援硬體，並使用最新的 iOS 版本進行測試。註冊一個 [Apple Developer](https://developer.apple.com/){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 帳戶，以在實體裝置上進行測試。

### iOS 及 Xcode
{: #ios_and_xcode}

- 安裝 [Xcode 8+](https://developer.apple.com/xcode/){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")（或更新版本）。
- 部署至 [iOS 8 裝置](https://support.apple.com/downloads/ios){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")（或更新版本）。
- 將應用程式提交至 Apple 之前，請檢閱 [App Store 提交準則](https://developer.apple.com/app-store/guidelines/){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")。

### SDK 及相依關係管理
{: #swift-sdk-management}

下列工具確保您可以安裝原生 SDK，以使用各種 {{site.data.keyword.cloud_notm}} 服務。您可以使用 CocoaPods 或 Carthage 來管理相依關係。

* **使用 CocoaPods** - 安裝 [CocoaPods](https://cocoapods.org/) 以支援 {{site.data.keyword.cloud_notm}} SDK：
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}

* **使用 Carthage** - 部分 SDK 也可以透過 [Carthage](https://github.com/Carthage/Carthage){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 或 [Swift Package Manager](https://swift.org/package-manager/){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 相依關係管理程式取得。若要使用 Carthage 來管理相依關係，請執行下列步驟：

  安裝 [Homebrew](https://brew.sh/){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 以協助 Carthage 安裝：
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

  安裝 Carthage：
  ```
  brew install carthage
  ```
  {: codeblock}

## 步驟 2. 建立自訂 iOS Swift 應用程式
{: #create-ios-app-swift}

1. 登入 [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")。
2. 按一下**建立應用程式**。
3. 在[空白入門範本](https://cloud.ibm.com/developer/appledevelopment/create-app){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 頁面上，您可以使用預設配置，或視需要更新欄位。確定選定的語言是 **iOS Swift**。按一下**建立**。

## 步驟 3. 新增 {{site.data.keyword.cloudant_short_notm}} 服務
{: #resources-swift}

您現在可以新增服務至您的 Swift 應用程式。針對此指導教學，請新增 {{site.data.keyword.cloudant_short_notm}} 服務至您的 Swift 應用程式，這樣會建立完全受管理的分散式 `JSON` 文件資料庫。Cloudant 為您的應用程式帶來使用輕量型架構的可調整性、高可用性以及延續性，讓您的資料保持安全且同步。

1. 從**應用程式詳細資料**頁面，按一下**新增服務**。
2. 選取**資料**，然後按**下一步**。
3. 選取 **Cloudant**，然後按**下一步**。
4. 按一下**建立**。
5. 建立服務之後，請按一下它以便加以啟動。在這個新的頁面上，選取**啟動 Cloudant 儀表板**，開始建立資料庫和 JSON 文件。或者，可以用程式設計的方式完成此作業。

## 步驟 4. 下載程式碼並設定 Client SDK
{: #run-locally-swift}

若要下載程式碼，請在 `Apps` > `Your App` 下，按一下**下載程式碼**。下載的程式碼會包含 [SwiftCloudant SDK](https://github.com/cloudant/swift-cloudant){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")，以及部分基本起始設定碼。用戶端 SDK 提供於 CocoaPods 和 Swift Package Manager。此解決方案使用 CocoaPods。

1. 解壓縮下載的程式碼。然後，使用終端機，導覽至已解壓縮的資料夾。
  ```
  cd <Name of Project>
  ```

2. 此資料夾包括一個 podfile，內含必要的相依關係。執行下列指令，以安裝相依關係 (Client SDK)：
  ```
  pod install
  ```
  {: codeblock}

## 步驟 5. 配置應用程式以使用新資料庫
{: #config-db-swift}

1. 用 Xcode 開啟以 `.xcworkspace` 為結尾的檔名，並導覽至 `ViewController.swift`。`SwiftCloudant` 是核心 Cloudant SDK。`CouchDBClient` 是 `SwiftCloudant` 的類別，且已在 `ViewController.swift` 中起始設定。

  請務必開啟新的 Xcode 工作區，而非原始 Xcode 專案檔：`MyApp.xcworkspace`。
  {: tip}

2. 已包括資料庫起始設定碼，如下列 Snippet 所示：
  ```swift
  /* Read the Cloudant credentials and intialize database connection */
  if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"), let dictionary = NSDictionary(contentsOfFile: contents) {
            let url = URL(string: dictionary["cloudantUrl"] as! String)
            let client = CouchDBClient(url:url!,
                                       username:dictionary["cloudantUsername"] as? String,
                                       password:dictionary["cloudantPassword"] as? String)
        }
  ```
  {: codeblock}

  服務認證是 `BMSCredentials.plist` 檔案的一部分。
  {: note}

## 步驟 6. 建置資料庫作業
{: #build_ops-swift}

既然您已設定可運作的資料庫連線及 SDK，便可以開始為您的特定使用案例建置完備基本的[建立、讀取、更新和破壞作業](/docs/swift/data?topic=swift-cloudant#cloudant)。

## 後續步驟
{: #next-swift}

### 新增其他服務
{: #moreresources-swift}

您可以直接從 Web 主控台新增其他服務至您的 iOS 應用程式，例如下列常用服務：

* [新增 Push Notifications 服務](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate#gettingstartedtemplate)
* [新增含有 App ID 的使用者鑑別](/docs/services/appid?topic=appid-getting-started#getting-started)

### 使用 {{site.data.keyword.cloud_notm}} Developer Tools
{: #devtools-swift}

您也可以學習如何使用 [{{site.data.keyword.cloud_notm}} Developer Tools](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) 來開發 Swift 應用程式，該工具提供一個指令行方法來建立、開發及部署完整的 Web、行動及微服務應用程式。
