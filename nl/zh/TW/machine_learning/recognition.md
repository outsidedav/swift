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

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

{{site.data.keyword.visualrecognitionfull}} 服務可讓您的應用程式利用機器學習，來快速且準確地標記、分類及訓練視覺化內容。此服務可協助虛擬分類任何視覺化內容、快速訓練您自己的自訂模型，以及偵測臉孔。

## 如何運作
{: ##how-it-works}

1. 您的應用程式選擇要進行分析的一批影像。
2. 您的應用程式使用 Watson Swift SDK 將影像傳送至 {{site.data.keyword.visualrecognitionshort}} 服務。
3. 此服務使用分類分析來分析影像，以識別場景、物件、臉孔及其他資訊。
4. Watson Swift SDK 將服務的分析傳回您的應用程式。

## 開始之前
{: ###before-you-begin}

首先，請確定您具備下列必要條件：
<ul>
  <li>iOS 8.0+</li>
  <li>Xcode 9.0+</li>
  <li>Swift 3.2+ 或 Swift 4.0+</li>
  <li>Carthage</li>
</ul>

建議您使用 [Carthage](https://github.com/Carthage/Carthage) 來管理相依關係，並為您的應用程式建置 Watson Swift SDK。如果您是 Carthage 新手，可以與 [Homebrew](http://brew.sh/) 一起安裝 Carthage：

```bash
$ brew update
$ brew install carthage
```

## 步驟 1. 建立視覺化識別的實例
{: ###create-and-configure-an-instance-of-visual-recognition}

佈建 {{site.data.keyword.visualrecognitionshort}} 服務的實例：

1. 在 {{site.data.keyword.cloud_notm}} 型錄中，選取 **{{site.data.keyword.visualrecognitionshort}}**。即會開啟服務配置畫面。
2. 提供服務實例的名稱，或使用預設名稱。
3. 如果您要將您的實例連結至應用程式，請從**連接**功能表中選取應用程式。
4. 選取定價方案，然後按一下**建立**。
5. 選取**認證**標籤，以檢視您的服務認證。這些值用來從您的應用程式連接至服務。

## 步驟 2. 下載及建置相依關係
{: ###download-and-build-dependencies}

使用您最愛的文字編輯器，在您專案的根目錄（`.xcodeproj` 檔所在之處）中，建立一個稱為 `Cartfile` 的檔案。然後，新增一行，指定 Watson Swift SDK 作為相依關係：
```
github "watson-developer-cloud/swift-sdk"
```
{: pre}

若為正式作業應用程式，您可以指定特定的[版本需求](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement)，以避免新版本 SDK 有非預期的變更。

`Cartfile` 就定位之後，現在您可以下載並建置相依關係。使用終端機來導覽至您專案的根目錄，然後執行 Carthage：

```bash
carthage update --platform iOS
```
{: pre}

Carthage 下載 Watson Swift SDK，並在您專案的 `Carthage/Build/iOS` 資料夾中建置其架構。

## 步驟 3. 新增架構至應用程式
{: ###add-frameworks-to-your-app}

### 鏈結視覺化識別步驟：

現在，Carthage 已建置 Watson Swift SDK 架構，您必須鏈結「視覺化識別」架構與您的應用程式。

1. 以 Xcode 開啟您的應用程式，然後選取您的專案以開啟其設定。
2. 選取您的應用程式目標，然後開啟**一般**標籤。
3. 往下捲動到「已鏈結的架構及程式庫」區段，然後按一下 `+` 圖示。
4. 在出現的視窗中，選擇**新增其他...**，然後導覽至 `Carthage/Build/iOS` 目錄。選取 **VisualRecognitionV3.framework**，以與您的應用程式鏈結。

### 複製視覺化識別步驟：

除了_鏈結_「視覺化識別」架構，您也必須將其_複製_ 到應用程式中，才能在執行時期進行存取。Xcode 有數個不同的方法可複製或內含架構，但是您可以使用 Carthage Script 來避免特定的 [App Store 提交錯誤](http://www.openradar.me/radar?id=6409498411401216)。

1. 以 Xcode 開啟您應用程式目標的設定時，導覽至**建置階段**標籤。
2. 按一下 `+` 圖示，然後選取**新建執行 Script 階段**。
3. 將下列指令新增至執行 Script 階段：`/usr/local/bin/carthage copy-frameworks`。
4. 將「視覺化識別」架構新增至**輸入檔**清單：`$(SRCROOT)/Carthage/Build/iOS/VisualRecognitionV3.framework`。

現在，您已準備好開始在您的應用程式使用 Watson Swift SDK！

## 步驟 4. 分析應用程式中的影像
{: #analyze-images-in-your-app}

1. 以 Xcode 開啟 `ViewController.swift` 檔。

1. 為「視覺化識別」新增一個 import 陳述式：
    ```swift
    import VisualRecognitionV3
    ```
    {: pre}

1. 傳遞 API 金鑰及版本（您可以使用今天的日期）來起始設定 SDK：
    ```swift
    let visualRecognition = VisualRecognition(apiKey: "your-api-key", version: "yyyy-mm-dd")
    ```
    {: pre}

1. 新增下列程式碼以分類影像：
    ```swift
    let url = "your-image-url"
    let failure = { (error: Error) in print(error) }
    visualRecognition.classify(url: url, failure: failure) { classifiedImages in
        print(classifiedImages)
    }
    ```
    {: pre}

## 使用入門範本套件
{: #recognition_starterkits}

[入門範本套件](https://console.bluemix.net/developer/appledevelopment/starter-kits)是利用 {{site.data.keyword.cloud_notm}} 功能最快的方式之一。選取 **Visual Recognition for iOS with Watson** 入門範本套件，便可使用 {{site.data.keyword.visualrecognitionshort}} 服務。此服務會評估並分類您的影像。從您的行動裝置上傳新的或現有的影像，然後「視覺化識別」應用程式便會快速標記並分類影像內容。

若要開始使用，請執行下列動作：
1. 選取在[這裡](https://console.bluemix.net/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson)找到的入門範本套件。
2. 建立含有預設服務的專案。
3. 按一下**下載程式碼**，以下載專案。服務認證注入對應金鑰欄位的 `BMSCredentials.plist` 檔中。

## 後續步驟
{: #recognition_next}

做得好！現在，您的應用程式可以使用「視覺化識別」了。嘗試下列其中一個選項，以保持動力：
* 檢視 [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk)。
* 如需相關資訊，請參閱 [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/)。

