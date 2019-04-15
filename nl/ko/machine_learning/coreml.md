---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: coreml swift, core ml swift, watson swift core, create model swift, image classification swift, version parameter swift, swift coreml watson

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Watson과 함께 Core ML 사용
{: #swift-coreml}

[Core ML](https://developer.apple.com/documentation/coreml){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 사용하여 광범위한 기계 학습 모델 유형을 앱과 통합할 수 있습니다. 30개 이상의 계층 유형으로 광범위한 심화 학습을 지원하는 것 외에도 트리 앙상블, SVM 및 일반화 선형 모형과 같은 표준 모델도 지원합니다. 분석할 데이터를 원격으로 전송하는 대신, Core ML은 최대의 성능 및 효율성을 제공하기 위해 Metal 및 Accelerate와 같은 하위 레벨 기술을 통해 CPU 및 GPU를 완벽하게 사용합니다.

다음 단계를 사용하여 Core ML 및 Watson Visual Recognition을 추가하면 모델을 훈련 및 작성하고, 종속성을 다운로드 및 빌드하고, 이미지 분류를 추가할 수 있습니다.
{: shortdesc}

## 시작하기 전에
{: #prereqs-coreml}

Swift로 Core ML 및 Watson Visual Recognition을 사용하려면 다음 컴포넌트가 필요합니다.

* iOS 11.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage 또는 Swift Package Manager

[CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘") 또는 [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 사용하여 [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 설치할 수 있습니다. 종속성을 관리하는 데 CocoaPods를 사용하면, 전체 Watson Swift SDK와는 반대로 필요한 프레임워크만 가져옵니다. CocoaPods를 처음 사용하는 경우 다음으로 이를 쉽게 설치할 수 있습니다.

```console
  sudo gem install cocoapods
```
{: codeblock}

## 1단계. {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}}을 사용하여 모델 훈련
{: #train-module-coreml}

현재 모델이 존재하지 않으면 원격으로 발견된 첫 번째 모델 또는 로컬로 존재하는 모델이 사용됩니다. 다음 gif 및 함께 제공되는 지시사항은 서비스를 Watson Studio에 연결하고 모델을 훈련하는 방법을 보여줍니다.

![Core ML 모델 둘러보기](images/CoreMLWalkthrough.gif)

### Core ML 앱 대시보드에서 서비스 설정
{: #service-coreml}

1. **도구 실행**을 선택하여 스타터 킷의 대시보드에서 Visual Recognition 도구를 시작하십시오.
2. **모델 작성**을 선택하여 모델 작성을 시작하십시오. .
2. 프로젝트가 사용자가 작성한 Visual Recognition 인스턴스와 아직 연관되지 않은 경우 프로젝트가 작성됩니다. 그렇자 않으면 **모델 작성** 섹션으로 건너뛰십시오.
3. 프로젝트의 이름을 지정하고 **작성**을 클릭하십시오.
  
  스토리지가 정의되지 않으면 새로 고치기를 클릭하십시오.
  {: tip}

### 프로젝트에 서비스 바인딩
{: #bind-service-coreml}

1. 프로젝트를 작성한 후 프로젝트 대시보드가 표시됩니다.
2. 설정 탭으로 이동하여 **연관된 서비스**로 아래로 스크롤하고 **서비스 추가** -> **Watson**을 선택하십시오.
3. Watson 서비스 페이지에서 **Visual Recognition**을 선택하십시오.
4. 서비스 정보 페이지의 **기존** 탭을 선택한 후 대시보드에서 서비스 인스턴스를 선택하십시오.
5. 서비스가 바인드되었으므로 프로젝트 대시보드에서 **자산**을 선택한 후 **Visual Recognition Model 추가**를 클릭하여 모델 작성을 시작할 수 있습니다.

### 모델 작성
{: #create-model-coreml}

1. 모델 작성 도구에서 분류자 이름을 업데이트할 수 있습니다. 특정 모델을 사용할 경우 기본 보기 제어기에서 `defaultClassifierID` 필드를 추가해야 합니다.

2. 사이드바에서 `.zip` 파일로 모델 훈련 과정을 업로드하십시오. 그런 다음 각 데이터 세트를 선택하고 드롭 다운 메뉴에서 해당 데이터 세트를 모델에 추가하십시오. 분류자를 향상시키기 위해 고유한 이미지 세트를 사용하는 더 많은 클래스를 추가할 수 있습니다!

![클래스 추가](images/add_classes.png)

3. **모델 훈련**을 선택한 다음 모델이 완전히 훈련될 때까지 기다리십시오.

모두 완료되었습니다! 이제 [Watson Developer Cloud Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 사용하여 Core ML 모델을 다운로드하고 이를 앱으로 통합할 준비가 되었습니다.

## 2단계. 종속성 다운로드 및 빌드
{: #install-depend-coreml}

선호하는 텍스트 편집기를 사용하여 `pod init`의 실행을 통해 프로젝트의 루트 디렉토리(여기에 `.xcodeproj` 파일이 있음)에 새 `Podfile`을 작성하십시오. 그런 다음 종속성으로 Watson Swift SDK의 {{site.data.keyword.visualrecognitionshort}} 프레임워크를 지정하는 행을 추가하십시오.

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

프로덕션 앱의 경우 새 Watson Swift SDK의 새 릴리스에서 발생하는 예상치 못한 변경사항을 방지하기 위해 [버전 요구사항](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 지정할 수도 있습니다.

`Podfile`이 올바르게 배치되면 이제 종속성을 다운로드할 수 있습니다. 터미널을 사용하여 프로젝트의 루트 디렉토리로 이동한 후 CocoaPods를 실행하십시오.

```console
  pod install
```
{: codeblock}

Cocoapods는 {{site.data.keyword.visualrecognitionshort}} 프레임워크를 다운로드하고 프로젝트의 `Pods/` 폴더에 이를 빌드합니다.

Pod 빌드 실패를 방지하려면 Xcode에서 프로젝트를 열 때 `.xcodeproj` 대신 `.xcworkspace`로 끝나는 파일을 여십시오.
{: tip}

## 3단계. 앱에 이미지 분류 추가
{: #add-image-coreml}

다음 샘플은 일반적으로 `ViewController.swift`에서 {{site.data.keyword.visualrecognitionshort}} Core ML 기능을 사용자의 애플리케이션에 추가하는 것을 도와줍니다. 다음 샘플을 사용하여 사용자의 유스 케이스에 적합한 로컬 모델 호출을 확장할 수 있습니다.

1. Visual Recognition을 위한 import 문을 추가하십시오.
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. API 키 및 버전을 전달하여 SDK를 초기화하십시오.
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  [버전 매개변수 문서](https://cloud.ibm.com/apidocs/visual-recognition#versioning){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 확인하거나 {site.data.keyword.visualrecognitionshort}} 서비스가 작성된 날짜를 사용하십시오.
  {: tip}

3. 다음 코드를 추가하여 Watson 분류자가 포함된 로컬 Core ML 모델을 다운로드하거나 업데이트하십시오.
  ```swift
  // Name of the classifier to use
  let classifierID = "your-classifier-ID-here"

  // Minimum confidence threshold for image recognition
        let threshold = 0.5

  // Update or download your model
  visualRecognition.updateLocalModel(classifierID: classifierID) { _, error in
      if let error = error {
          print(error)
      } else {
          print("model successfully updated")
      }                
  }
  ```
  {: codeblock}

4. 로컬 Core ML 모델의 사용을 통해 다음 코드를 추가하여 이미지를 분류하십시오.
  ```swift
  let image = UIImage(named: "your-image-filename")
  let classifierID = "your-classifier-ID-here"
  let threshold = 0.5

  // Classify your image using your model                                         
  visualRecognition.classifyWithLocalModel(image: image, classifierIDs: [classifierID], threshold: threshold) { classifiedImages, error in
      if let error = error {
          print(error)
    }
      guard let classifiedImages = classifiedImages? else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

5. Watson SDK에서 지원되는 기타 [Core ML 분류 기능](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 탐색하십시오. 

## 4단계. 스타터 킷 사용
{: #coreml_starterkits}

스터터 킷을 사용하면 {{site.data.keyword.cloud_notm}} 및 Core ML의 기능을 빠르고 쉽게 사용할 수 있습니다. 스타터 킷을 사용하여 {{site.data.keyword.visualrecognitionshort}}을 클라이언트 iOS 앱 또는 서버 측 백엔드에 추가할 수 있습니다. Custom Vision Model for Core ML with {{site.data.keyword.watson}} 스타터 킷은 사용자 정의 {{site.data.keyword.visualrecognitionshort}} 모델을 작성하고 디바이스에서 로컬로 실행할 수 있는 Core ML 모델로 이를 인스턴스화하는 방법을 보여줍니다.

{{site.data.keyword.visualrecognitionshort}}을 스타터 킷에 추가하려면 다음 단계를 완료하십시오.

1. 작업할 [스타터 킷](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 선택하십시오.
2. 기본 서비스를 사용하여 앱을 작성하십시오.
3. **서비스 추가 > Watson > {{site.data.keyword.visualrecognitionshort}}**을 클릭하십시오.
4. **코드 다운로드**를 클릭하여 프로젝트를 다운로드하십시오. iOS 프로젝트의 경우 인증 정보가 해당 키 필드의 `BMSCredentials.plist` 파일에 삽입됩니다. 서버 측 Swift 프로젝트의 경우 `config/local-dev.json` 파일에서 이 인증 정보를 찾을 수 있습니다.

## 다음 단계
{: #coreml_next}

이제 사용자 정의 생성 Core ML 모델을 사용하여 이미지를 분석할 수 있습니다. 다음 옵션 중 하나를 사용하여 계속 진행하십시오.

* [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 확인하고 기타 지원되는 Watson 서비스를 탐색하십시오. 
* 클라우드 로직을 추가하십시오. [서버리스(serverless) 앱 개발](/docs/swift/backend?topic=swift-serverless-dev-swift#serverless-dev-swift)을 시작하십시오.
* {{site.data.keyword.visualrecognitionshort}}에서 제공하는 모든 기능을 활용하십시오. 자세한 내용은 [문서](/docs/services/visual-recognition?topic=visual-recognition-index#index)를 참조하십시오.
