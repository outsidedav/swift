---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-31"

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
{: #how-it-works-tone}

1. 您的應用程式選擇一批文字來進行分析（例如，文字訊息或 Twitter 資訊來源）。
2. 您的應用程式使用 Watson Swift SDK 將文字傳送至 {{site.data.keyword.toneanalyzershort}} 服務。
3. 此服務使用語言分析來分析文字，以識別情緒及語氣。
4. 服務的分析透過 [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk) 傳回您的應用程式。

## 開始之前
{: #prereqs-tone}

請確定您具備下列必要條件：

* OS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods、Carthage 或 Swift Package Manager

您可以使用 [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods)、[Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage) 或 [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager) 來安裝 [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk)。藉由使用 [CocoaPods](https://cocoapods.org/) 來管理相依關係，您只會得到您需要的架構，而不是整個 Watson Swift SDK。如果您是 CocoaPods 新手，可以輕鬆地安裝它：

```bash
$ sudo gem install cocoapods
```
{: codeblock}

## 步驟 1. 建立 Tone Analyzer 的實例
{: #create-instance-tone}

佈建 {{site.data.keyword.toneanalyzershort}} 服務的實例：

1. 在 {{site.data.keyword.cloud_notm}} 型錄中，選取 {{site.data.keyword.toneanalyzershort}}。即會開啟服務配置畫面。
2. 提供服務實例的名稱，或使用預設名稱。
3. 如果您要將實例連結至應用程式，請從「連接」功能表中選取應用程式。
4. 選取定價方案，然後按一下**建立**。
5. 選取**認證**標籤，以檢視您的服務認證。這些值用來從您的應用程式連接至服務。

## 步驟 2. 下載及建置相依關係
{: #download-depend-tone}

使用您最愛的文字編輯器，在您專案的根目錄（`.xcodeproj` 檔案所在之處）中，執行 `pod init` 建立一個新的 `Podfile`。然後，新增一行，指定 Watson Swift SDK 的 {{site.data.keyword.conversationshort}} 架構作為相依關係：

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonToneAnalyzerV3'
```
{: codeblock}

若為正式作業應用程式，您也可能想要指定特定的[版本需求](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions)，以避免新版本 Watson Swift SDK 有非預期的變更。

`Podfile` 就緒後，現在您可以下載相依關係。使用終端機來導覽至您專案的根目錄，然後執行 CocoaPods：

```console
pod install
```
{: codeblock}

Cocoapods 會下載 {{site.data.keyword.toneanalyzershort}} 架構，並且會在您專案的 `Pods/` 資料夾中建置它。

為了避免 Pod 建置失敗，當您在 Xcode 開啟專案時，請開啟以 `.xcworkspace` 為結尾的檔案，而不是 `.xcodeproj`。
{: tip}

## 步驟 3. 分析應用程式中的文字
{: #analyze-text-tone}

下列範例會協助您為應用程式新增 {{site.data.keyword.toneanalyzershort}} 功能，一般是在 `ViewController.swift`。使用下列範例，您可以為您的使用案例延伸 Tone Analyzer 呼叫。

1. 為 Tone Analyzer 新增一個 import 陳述式：
    
  ```swift
  import ToneAnalyzer
  ```
  {: codeblock}

2. 實例化 Tone Analyzer 服務：
  ```swift
  let toneAnalyzer = ToneAnalyzer(version: "yyyy-mm-dd", apiKey: "your-api-key-here")
  ```
  {: codeblock}

  參閱[版本參數文件](https://cloud.ibm.com/apidocs/tone-analyzer#versioning)，或使用建立 {{site.data.keyword.conversationshort}} 服務的日期。
  {: tip}

3. 提供分析用的文字，然後處理結果：
  ```swift
  // Text to analyze
  let review = """
      I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
          """

  // Analyze text
  let input = ToneInput(text: review)
  toneAnalyzer.tone(toneInput: .toneInput(input)) { response, error in
      if let error = error {
          print(error)
          return
      }

      guard let tones = response?.result?.documentTone.tones else {
          print("Failed to analyze the tone input")
          return
      }

      for tone in tones {
          print("\(tone.toneName): \(tone.score)")
      }
  }
  ```
  {: codeblock}

  當您執行程式碼時，會在主控台中看到下列分析：
  ```
  Sadness: 0.575803
  Tentative: 0.867377
  ```
  {: screen}

4. 探索 Watson SDK [Tone Analyzer 文件](https://watson-developer-cloud.github.io/swift-sdk/services/ToneAnalyzerV3/index.html)，以建置完備應用程式的功能。

## 使用入門範本套件
{: #tone_starterkits}

[入門範本套件](https://cloud.ibm.com/developer/appledevelopment/starter-kits)是使用 {{site.data.keyword.cloud_notm}} 功能最快的方式之一。選取 **Tone Analyzer for iOS with Watson** 入門範本套件，即可使用 {{site.data.keyword.toneanalyzershort}} 服務。此服務利用深度學習功能來評估文字通道。Tone Analyzer 應用程式會識別說話者的語氣（快樂、悲傷、自信等等），因為它與許多種類有關。

若要開始使用此入門範本套件，請執行下列動作：

1. 選取在[這裡](https://cloud.ibm.com/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson)找到的入門範本套件。
2. 建立含有預設服務的專案。
3. 按一下**下載程式碼**，以下載專案。服務認證會注入對應金鑰欄位的 `BMSCredentials.plist` 檔案中。

## 後續步驟
{: #tone_next}

做得好！現在，{{site.data.keyword.toneanalyzershort}} 已新增至您的應用程式。嘗試下列其中一個選項，以保持動力：

* 檢視 [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk) 並探索其他受支援的 Watson 服務。
* 如需相關資訊，請參閱 [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/)。
