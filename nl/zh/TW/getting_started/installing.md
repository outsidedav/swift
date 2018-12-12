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

# 將 SDK 安裝至用戶端應用程式
{: #installing}

{{site.data.keyword.cloud}} iOS SDK 支援各種熱門的相依關係管理程式，可讓您輕鬆易地在自己的應用程式內安裝及使用 {{site.data.keyword.cloud_notm}} 服務。

## 使用 CocoaPods 進行安裝
{: #installing_with_cocoapods}

若要使用 CocoaPods 來安裝 SDK，請將其新增至您的 `Podfile`。如果您的專案還沒有 `Podfile`，請使用 `pod init` 指令。
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

執行 `pod install`，然後開啟產生的 `.xcworkspace` 檔案。

如需相關資訊，請參閱 [CocoaPods 手冊](https://guides.cocoapods.org/using/index.html)。

## 使用 Carthage 進行安裝
{: #installing_with_carthage}

若要使用 Carthage 來安裝 SDK，請將此行新增至 `Cartfile`：
```
github "<github org name>/<github project name>"
```
{: codeblock}

執行 `carthage update`，以啟動建置程序。建置完成之後，將產生的架構新增至您的專案。 

如需相關資訊，請參閱 [Carthage getting started](https://github.com/Carthage/Carthage#getting-started) 文件。

## 使用 Swift Package Manager 進行安裝
{: #installing_with_swift_package_manager}

若要使用 Swift Package Manager 來安裝 SDK，請在 `Package.swift` 中，將下列這一行新增至相依關係中：
```
.Package(url: "<SDK git url>")
```
{: codeblock}

執行 `swift build`，以啟動建置程序。

如需相關資訊，請參閱 [Swift Package Manager 概觀](https://swift.org/package-manager/)。

## 手動安裝
{: #installing_manually}

若要手動安裝 SDK，請下載 SDK，然後以手動方式將原始檔新增至您的專案。
