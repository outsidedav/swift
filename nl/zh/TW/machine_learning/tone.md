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

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

IBM Watson Tone Analyzer 服務可讓您的應用程式瞭解文字形式的情緒及語氣。您可以使用此服務更充分的瞭解您使用者的對話，或者協助使用者瞭解他人是如何理解其書面通訊的。

## 如何運作
{: ##how-it-works}

1. 您的應用程式選擇一批文字來進行分析（例如，文字訊息或 Twitter 資訊來源）。
2. 您的應用程式使用 Watson Swift SDK 將文字傳送至 {{site.data.keyword.toneanalyzershort}} 服務。
3. 此服務使用語言分析來分析文字，以識別情緒及語氣。
4. 服務的分析透過 Watson Swift SDK 傳回您的應用程式。

## 開始之前
{: ###before-you-begin}

首先，請確定您具備下列必要條件：
<ul>
  <li>iOS 8.0+</li>
  <li>Xcode 9.0+</li>
  <li>Swift 3.2+ 或 Swift 4.0+</li>
  <li>Carthage</li>
</ul>

建議使用 [Carthage](https://github.com/Carthage/Carthage) 來管理相依關係，並為您的應用程式建置 Watson Swift SDK。如果您是 Carthage 新手，可以使用 [Homebrew](http://brew.sh/) 來安裝 Carthage：

```bash
$ brew update
$ brew install carthage
```
{: codeblock}

## 步驟 1. 建立 Tone Analyzer 的實例
{: #create-and-configure-an-instance-of-tone-analyzer}

佈建 {{site.data.keyword.toneanalyzershort}} 服務的實例：

1. 在 {{site.data.keyword.cloud_notm}} 型錄中，選取 {{site.data.keyword.toneanalyzershort}}。即會開啟服務配置畫面。
2. 提供服務實例的名稱，或使用預設名稱。
3. 如果您要將實例連結至應用程式，請從「連接」功能表中選取應用程式。
4. 選取定價方案，然後按一下**建立**。
5. 選取**認證**標籤，以檢視您的服務認證。這些值用來從您的應用程式連接至服務。

## 步驟 2. 下載及建置相依關係
{: ###download-and-build-dependencies}

使用您最愛的文字編輯器，在您專案的根目錄（`.xcodeproj` 檔案所在之處）中，建立一個稱為 `Cartfile` 的新檔案。然後，新增一行，指定 Watson Swift SDK 作為相依關係：

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

若為正式作業應用程式，您也可能想要指定特定的[版本需求](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement)，以避免新版本 Watson Swift SDK 有非預期的變更。

`Cartfile` 就緒後，現在您可以下載並建置相依關係。使用終端機來導覽至您專案的根目錄，然後執行 Carthage：
  
  ```bash
  $ carthage update --platform iOS
  ```
  {: codeblock}

Carthage 下載 Watson Swift SDK，並會在您專案的 `Carthage/Build/iOS` 資料夾中建置其架構。

## 步驟 3. 將架構新增至應用程式
{: ###add-frameworks-to-your-app}

### 鏈結 Tone Analyzer 步驟

現在，Carthage 已建置 Watson Swift SDK 架構，您必須鏈結 Tone Analyzer 架構與您的應用程式。

1. 以 Xcode 開啟您的應用程式，然後選取您的專案以開啟其設定。
2. 選取您的應用程式目標，然後開啟**一般標籤**。
3. 往下捲動到「已鏈結的架構及程式庫」區段，然後按一下 `+` 圖示。
4. 在出現的視窗中，選擇**新增其他...**，然後導覽至 `Carthage/Build/iOS` 目錄。選取 **ToneAnalyzerV3.framework**，以與您的應用程式鏈結。

### 複製 Tone Analyzer 步驟

除了_鏈結_ Tone Analyzer 架構，您也必須將其_複製_ 到應用程式中，才能在運行環境進行存取。Xcode 有數個不同的方式可複製或內含架構，但是您可以使用 Carthage Script 來避免特定的 [App Store 提交錯誤](http://www.openradar.me/radar?id=6409498411401216)。

1. 以 Xcode 開啟您應用程式目標的設定時，導覽至**建置階段**標籤。
2. 按一下 `+` 圖示，然後選取**新建執行 Script 階段**。
3. 將下列指令新增至執行 Script 階段：`/usr/local/bin/carthage copy-frameworks`。
4. 將 Tone Analyzer 架構新增至「輸入檔」清單：`$(SRCROOT)/Carthage/Build/iOS/ToneAnalyzerV3.framework`。

現在，您已準備好開始在您的應用程式使用 Watson Swift SDK！

## 步驟 4. 分析應用程式中的文字
{: #analyze-text-in-your-app}

1. 以 Xcode 開啟 `ViewController.swift` 檔案。
2. 為 Tone Analyzer 新增一個 import 陳述式：
    ```swift
    import ToneAnalyzerV3
    ```
    {: codeblock}

3. 建立一個空函數，稱為 `toneAnalyzerExample`，然後從 `viewDidLoad` 呼叫該函數。
4. 將下列程式碼新增至 `toneAnalyzerExample` 函數。請務必更新服務的使用者名稱和密碼。
  ```swift
  import UIKit
  import ToneAnalyzerV3

  class ViewController: UIViewController {

      override func viewDidLoad() {
          super.viewDidLoad()
          toneAnalyzerExample()
      }

      func toneAnalyzerExample() {

          // instantiate service
          let toneAnalyzer = ToneAnalyzer(
              username: "your-username-here",
              password: "your-password-here",
              version: "yyyy-mm-dd"
          )

          // text to analyze
          let review = """
              I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
          """

          // analyze text
          let toneInput = ToneInput(text: review)
          let failure = { (error: Error) in print(error) }
          toneAnalyzer.tone(toneInput: toneInput, contentType: "application/json", failure: failure) { analysis in
              for tone in analysis.documentTone.tones! {
                  print("\(tone.toneName): \(tone.score)")
              }
          }
      }
  }
  ```
  {: codeblock}

當您執行應用程式時，會在主控台中看到下列分析：
```
Sadness: 0.575803
Tentative: 0.867377
```
{: screen}

## 使用入門範本套件
{: #tone_starterkits}

[入門範本套件](https://console.bluemix.net/developer/appledevelopment/starter-kits)是運用 {{site.data.keyword.cloud_notm}} 功能最快的方式之一。選取 **Tone Analyzer for iOS with Watson** 入門範本套件，即可使用 {{site.data.keyword.toneanalyzershort}} 服務。此服務利用深度學習功能來評估文字通道。Tone Analyzer 應用程式會識別說話者的語氣（快樂、悲傷、自信等等），因為它與許多種類有關。

若要開始使用此入門範本套件，請執行下列動作：

1. 選取在[這裡](https://console.bluemix.net/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson)找到的入門範本套件。
2. 建立含有預設服務的專案。
3. 按一下**下載程式碼**，以下載專案。服務認證會注入對應金鑰欄位的 `BMSCredentials.plist` 檔案中。

## 後續步驟
{: #tone_next}

做得好！現在，{{site.data.keyword.toneanalyzershort}} 已新增至您的應用程式。嘗試下列其中一個選項，以保持動力：

* 檢視 [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk)。
* 如需相關資訊，請參閱 [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/)。

