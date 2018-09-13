---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 將機器學習新增至應用程式
{: #overview}

使用 [Core ML](https://developer.apple.com/documentation/coreml){:new_window}，您可以將各式各樣的機器學習模型類型整合至您的應用程式。除了支援超過 30 種層次類型的廣泛深度學習之外，還支援標準模型，例如，樹狀結構組合、SVM 及一般線性模型。Core ML 利用低階技術（例如 Metal 及 Accelerate），無縫充分運用 CPU 及 GPU 來提供最高效能與效率，而非遠端傳送資料以進行分析。

## 開始之前
{: #before-you-begin}

若要使用 Core ML 及 Watson Visualization，您需要下列元件：

  * iOS 11.0+
  * Xcode 9.0+
  * Swift 4.0+
  * Carthage

若要安裝 Carthage，請使用下列 `brew` 指令：
```
$ brew update
$ brew install carthage
```
{: codeblock}

## 步驟 1. 使用 {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}} 訓練模型
{: #training-your-model}

如果沒有現行模型，則會使用遠端找到的第一個模型，或存在於本端的任何一個模型。下列 gif 及隨附的指示顯示如何將您的服務鏈結至 Watson Studio，並訓練您的模型。

![Core ML 模型逐步演練](images/CoreMLWalkthrough.gif)

### 從 Core ML 應用程式儀表板設定服務

1. 從「入門範本套件」儀表板中，選取**啟動工具**，以啟動「視覺化識別工具」。
2. 選取**建立模型**，以開始建立您的模型。
2. 如果專案尚未與您建立的「視覺化識別」實例相關聯，則會建立一個專案。否則，會跳到**建立模型**區段。
3. 為專案命名，然後按一下**建立**。
  **提示**：如果未定義任何儲存空間，請按一下重新整理。

### 將服務連結至專案

1. 建立您的專案之後，即會顯示專案儀表板。
2. 移至設定標籤，往下捲動至**關聯的服務**，選取**新增服務** -> **Watson**。
3. 從 Watson 服務頁面中，選取**視覺化識別**。
4. 在服務資訊頁面上，選取**現有**標籤，然後從儀表板中選取您的服務實例。
5. 現在已連結您的服務，您可以開始建立模型，從「專案」儀表板選取**資產**，然後按一下**新增視覺化識別模型**。

### 建立模型

1. 需要時，從模型建立工具修改分類器名稱。依預設，除非指定，否則應用程式會使用第一個可用的模型。如果您要使用特定的模型，請務必修改主要「視圖控制器」中的 `defaultClassifierID` 欄位。

2. 從資訊看板，以 .zip 檔上傳模型訓練課程。然後，從下拉功能表選取每個資料集，並將其新增至您的模型。您可以隨意增加使用您專屬影像集的類別，以加強分類器！

![新增類別](images/add_classes.png)

3. 選取**訓練模型**，然後等待模型完成訓練。

都完成了！現在，您已準備好可以下載 Core ML 模型，然後使用 [{{site.data.keyword.watson}} Developer Cloud Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window} 將其整合至您的應用程式。

## 步驟 2. 下載及建置相依關係
{: #installing-dependencies}

1. 使用您最愛的文字編輯器，在您專案的根目錄（**.xcodeproj** 檔案所在之處）中，建立一個名為 `Cartfile` 的新檔案。

2. 新增一行，指定 {{site.data.keyword.watson}} Developer Cloud Swift SDK 作為相依關係，例如：

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: pre}

  若為正式作業應用程式，您也可能想要指定特定的版本需求，以避免新版本 SDK 有非預期的變更。
  {: tip}

3. 使用終端機來導覽至您專案的根目錄，然後執行 Carthage：

  ```
  $ carthage update --platform iOS
  ```
  {: pre}

  然後會下載 {{site.data.keyword.watson}} Developer Cloud Swift SDK，而其架構則建置在您專案的 `Carthage/Build/iOS` 資料夾中。

## 步驟 3. 將影像分類新增至應用程式
{: #adding-image-classification}

```Swift
//
//  ViewController.swift
//  CoreMLImageClassificationExample
//
//  Created by Aaron Liberatore on 3/1/18.
//  Copyright © 2018 Aaron Liberatore. All rights reserved.
//

import UIKit
import VisualRecognitionV3

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        initializeCoreMLExample()
    }

    func failureHandler(_ error: Error) {
        print(error)
    }

    func initializeCoreMLExample() {
        // Configuration

        // Your Watson Visual Recognition service api key
        let apiKey = "your-apiKey-here"

        // Name of the classifier to use
        let classifierID = "your-classifier-here"

        // Minimum confidence threshold for image recognition
        let threshold = 0.5

        // Initialize Service
        let visualRecognition = VisualRecognition(apiKey: apiKey, version: "03-01-2018")

        // Update or download your model
        visualRecognition.updateLocalModel(classifierID: classifierID,
                                           failure: failureHandler) {

            // Classify your image using your model                                         
            visualRecognition.classifyWithLocalModel(image: image,
                                                     classifierIDs: [classifierID],
                                                     threshold: threshold,
                                                     failure: self.failureHandler) { classifiedImages in

                 print(classifiedImages)
             }            
        }
    }
}
```
{: codeblock}

## 步驟 4. 使用入門範本套件
{: #coreml_starterkits}

使用入門範本套件，您可以快速且輕鬆地運用 {{site.data.keyword.cloud_notm}} 及 Core ML 的功能。您可以使用入門範本套件，將 {{site.data.keyword.visualrecognitionshort}} 新增至任何用戶端 iOS 應用程式或伺服器端後端。Custom Vision Model for Core ML with {{site.data.keyword.watson}} Starter Kit 說明如何建立自訂的 {{site.data.keyword.visualrecognitionshort}} 模型，並將其實例化成可在您裝置上本端執行的 Core ML 模型。

若要將 {{site.data.keyword.visualrecognitionshort}} 新增至入門範本套件，請完成下列步驟：

1. 選取您要使用的[入門範本套件](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window}。
2. 建立含有預設服務的專案。
3. 按一下**新增資源 > Watson > {{site.data.keyword.visualrecognitionshort}}**。
4. 按一下**下載程式碼**，以下載專案。若為 iOS 專案，認證會插入對應金鑰欄位的 `BMSCredentials.plist` 檔案中。若為伺服器端 Swift 專案，您可以在 `config/local-dev.json` 檔案中找到這些認證。

## 後續步驟
{: #coreml_next}

現在，您可以使用自訂產生的 Core ML 模型來分析影像。嘗試下列其中一個選項，以保持動力：

* 新增雲端邏輯。開始於[開發無伺服器應用程式](/docs/swift/backend/functions.html)。
* 充分運用 {{site.data.keyword.visualrecognitionshort}} 提供的所有特性。如需詳細資料，請參閱[文件](/docs/services/visual-recognition/index.html)。
