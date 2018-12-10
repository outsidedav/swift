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

# 앱에 머신 러닝 추가
{: #overview}

[Core ML](https://developer.apple.com/documentation/coreml){:new_window}을 사용하여 광범위한 머신 러닝 모델 유형을 앱과 통합할 수 있습니다. 30개 이상의 계층 유형으로 광범위한 심화 학습을 지원하는 것 외에도 트리 앙상블, SVM 및 일반화 선형 모형과 같은 표준 모델도 지원합니다. 분석할 데이터를 원격으로 전송하는 대신, Core ML은 최대의 성능 및 효율성을 제공하기 위해 Metal 및 Accelerate와 같은 하위 레벨 기술을 통해 CPU 및 GPU를 완벽하게 활용합니다.

## 시작하기 전에
{: #before-you-begin}

Core ML 및 Watson Visualization을 사용하려면 다음 컴포넌트가 필요합니다.

  * iOS 11.0+
  * Xcode 9.0+
  * Swift 4.0+
  * Carthage

Carthage를 설치하려면 다음 `brew` 명령을 사용하십시오.
```
$ brew update
$ brew install carthage
```
{: codeblock}

## 1단계. {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}}을 사용하여 모델 훈련
{: #training-your-model}

현재 모델이 존재하지 않으면 원격으로 발견된 첫 번째 모델 또는 로컬로 존재하는 모델이 사용됩니다. 다음 gif 및 함께 제공되는 지시사항은 서비스를에 연결하고 모델을 훈련하는 방법을 보여줍니다.

![Core ML 모델 둘러보기](images/CoreMLWalkthrough.gif)

### Core ML 앱 대시보드에서 서비스 설정

1. **도구 실행**을 선택하여 스타터 킷의 대시보드에서 Visual Recognition 도구를 실행하십시오.
2. **모델 작성**을 선택하여 모델 작성을 시작하십시오. .
2. 프로젝트가 사용자가 작성한 Visual Recognition 인스턴스와 아직 연관되지 않은 경우 프로젝트가 작성됩니다. 그렇자 않으면 **모델 작성** 섹션으로 건너뛰십시오.
3. 프로젝트의 이름을 지정하고 **작성**을 클릭하십시오.
  **팁**: 스토리지가 정의되지 않으면 새로 고치기를 클릭하십시오.

### 프로젝트에 서비스 바인딩

1. 프로젝트를 작성한 후 프로젝트 대시보드가 표시됩니다.
2. 설정 탭으로 이동하여 **연관된 서비스**로 아래로 스크롤하고 **서비스 추가** -> **Watson**을 선택하십시오.
3. Watson 서비스 페이지에서 **Visual Recognition**을 선택하십시오.
4. 서비스 정보 페이지의 **기존** 탭을 선택한 후 대시보드에서 서비스 인스턴스를 선택하십시오.
5. 서비스가 바인드되었으므로 프로젝트 대시보드에서 **자산**을 선택한 후 **Visual Recognition Model 추가**를 클릭하여 모델 작성을 시작할 수 있습니다.

### 모델 작성

1. 원하는 경우 모델 작성 도구에서 분류자 이름을 수정하십시오. 기본적으로 애플리케이션은 지정된 경우를 제외하고 첫 번째 사용 가능한 항목을 사용합니다. 특정 모델을 사용할 경우 기본 보기 제어기에서 `defaultClassifierID` 필드를 추가해야 합니다.

2. 사이드바에서 .zip 파일로 모델 훈련 과정을 업로드하십시오. 그런 다음 각 데이터 세트를 선택하고 드롭 다운 메뉴에서 해당 데이터 세트를 모델에 추가하십시오. 분류자를 향상시키기 위해 고유한 이미지 세트를 사용하는 더 많은 클래스를 추가할 수 있습니다!

![클래스 추가](images/add_classes.png)

3. **모델 훈련**을 선택한 다음 모델이 완전히 훈련될 때까지 기다리십시오.

모두 완료되었습니다! 이제 [{{site.data.keyword.watson}} Developer Cloud Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window}를 사용하여 Core ML 모델을 다운로드하고 이를 앱으로 통합할 준비가 되었습니다.

## 2단계. 종속성 다운로드 및 빌드
{: #installing-dependencies}

1. 선호하는 텍스트 편집기를 사용하여 프로젝트의 루트 디렉토리(여기에 `.xcodeproj` 파일이 있음)에 **Cartfile**이라는 새 파일을 작성하십시오.

2. 종속성으로 {{site.data.keyword.watson}} Developer Cloud Swift SDK를 지정하는 행을 추가하십시오. 예를 들면, 다음과 같습니다.

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: pre}

  프로덕션 앱의 경우 SDK의 새 릴리스에서 발생하는 예상치 못한 변경사항을 방지하려면 특정 버전 요구사항을 지정할 수 있습니다.
  {: tip}

3. 터미널을 사용하여 프로젝트의 루트 디렉토리로 이동하고 Carthage를 실행하십시오.

  ```
  $ carthage update --platform iOS
  ```
  {: pre}

  그러면 {{site.data.keyword.watson}} Developer Cloud Swift SDK가 다운로드되고 프레임워크가 프로젝트의 `Carthage/Build/iOS` 폴더에서 빌드됩니다.

## 3단계. 앱에 이미지 분류 추가
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

## 4단계. 스타터 킷 사용
{: #coreml_starterkits}

스터터 킷을 사용하면 {{site.data.keyword.cloud_notm}} 및 Core ML의 기능을 빠르고 쉽게 활용할 수 있습니다. 스타터 킷을 사용하여 {{site.data.keyword.visualrecognitionshort}}을 클라이언트 iOS 앱 또는 서버 측 백엔드에 추가할 수 있습니다. Custom Vision Model for Core ML with {{site.data.keyword.watson}} 스타터 킷은 사용자 정의 {{site.data.keyword.visualrecognitionshort}} 모델을 작성하고 디바이스에서 로컬로 실행할 수 있는 Core ML 모델로 이를 인스턴스화하는 방법을 보여줍니다.

{{site.data.keyword.visualrecognitionshort}}을 스타터 킷에 추가하려면 다음 단계를 완료하십시오.

1. 작업할 [스타터 킷](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window}을 선택하십시오.
2. 기본 서비스를 사용하여 프로젝트를 작성하십시오.
3. **리소스 추가 > Watson > {{site.data.keyword.visualrecognitionshort}}**을 클릭하십시오.
4. **코드 다운로드**를 클릭하여 프로젝트를 다운로드하십시오. iOS 프로젝트의 경우 인증 정보가 해당 키 필드의 `BMSCredentials.plist` 파일에 삽입됩니다. 서버 측 Swift 프로젝트의 경우 `config/local-dev.json` 파일에서 이 인증 정보를 찾을 수 있습니다.

## 다음 단계
{: #coreml_next}

이제 사용자 정의 생성 Core ML 모델을 사용하여 이미지를 분석할 수 있습니다. 다음 옵션 중 하나를 사용하여 계속 진행하십시오.

* 클라우드 로직을 추가하십시오. [서버리스 앱 개발](/docs/swift/backend/functions.html)을 시작하십시오.
* {{site.data.keyword.visualrecognitionshort}}에서 제공하는 모든 기능을 활용하십시오. 자세한 내용은 [문서](/docs/services/visual-recognition/index.html)를 참조하십시오.
