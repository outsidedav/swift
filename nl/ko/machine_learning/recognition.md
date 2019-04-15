---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: swift visual recognition, train swift, cocoapods swift, swift sdk install, starter kit watson swift, image swift classify, machine learning swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

{{site.data.keyword.visualrecognitionfull}} 서비스를 통해 앱이 기계 학습을 사용하여 빠르고 정확하게 시각적 컨텐츠의 태그를 지정하고, 시각적 컨텐츠를 분류하고, 훈련할 수 있습니다. 서비스는 사실상 모든 시각적 컨텐츠를 분류하고, 몇 분 안에 자신의 사용자 정의 모델을 훈련하며, 면을 감지하는 데 도움을 줄 수 있습니다.

## 작동 방법
{: #how-it-works-recognition}

1. 앱이 분석할 이미지를 선택합니다.
2. 앱이 Watson Swift SDK를 사용하여 이미지를 {{site.data.keyword.visualrecognitionshort}} 서비스에 전송합니다.
3. 서비스가 분류 분석을 통해 이미지를 분석하여 화면, 오브젝트, 면 등을 식별합니다.
4. Watson Swift SDK를 통해 서비스의 분석 결과가 앱으로 리턴됩니다.

## 시작하기 전에
{: #prereqs-recognition}

다음 전제조건을 갖추었는지 확인하십시오.

* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage 또는 Swift Package Manager

[CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘") 또는 [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 사용하여 [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 설치할 수 있습니다. 종속성을 관리하는 데 CocoaPods(https://cocoapods.org/)를 사용하면, 전체 Watson Swift SDK와는 반대로 필요한 프레임워크만 가져옵니다. CocoaPods를 처음 사용하는 경우 다음으로 이를 쉽게 설치할 수 있습니다.
```console
  sudo gem install cocoapods
```
{: codeblock}

## 1단계. Visual Recognition의 인스턴스 작성
{: #create-instance-recognition}

{{site.data.keyword.visualrecognitionshort}} 서비스의 인스턴스를 프로비저닝하십시오.

1. {{site.data.keyword.cloud_notm}} 카탈로그에서 **{{site.data.keyword.visualrecognitionshort}}**를 클릭하십시오. 서비스 구성 화면이 열립니다.
2. 서비스 인스턴스에 이름을 지정하거나 사전 설정된 이름을 사용하십시오.
3. 인스턴스를 앱에 바인드하려면 **연결** 메뉴에서 앱을 선택하십시오.
4. 가격 플랜을 선택하고 **작성**을 클릭하십시오.
5. **인증 정보** 탭을 선택하여 서비스 인증 정보를 확인하십시오. 이 값은 앱에서 서비스를 연결하는 데 사용됩니다.

## 2단계. 종속성 다운로드 및 빌드
{: #download-depend-recognition}

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

## 3단계. 앱에서 이미지 분석
{: #analyze-images-recognition}

다음 샘플은 일반적으로 `ViewController.swift`에서 {{site.data.keyword.visualrecognitionshort}} 기능을 사용자의 애플리케이션에 추가하는 것을 도와줍니다. 다음 샘플을 사용하여 사용자의 유스 케이스에 적합한 Visual Recognition 호출을 확장할 수 있습니다.

1. Visual Recognition을 위한 import 문을 추가하십시오.
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. API 키 및 버전(오늘 날짜 사용할 수 있음)을 전달하여 SDK를 초기화하십시오.
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  [버전 매개변수 문서](https://cloud.ibm.com/apidocs/visual-recognition#versioning){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 확인하거나 {site.data.keyword.conversationshort}} 서비스가 작성된 날짜를 사용하십시오.
  {: tip}

3. 다음 코드를 추가하여 이미지를 분류하십시오.
  ```swift
  let url = "your-image-url"
  visualRecognition.classify(url: url) { response, error in
      if let error = error {
          print(error)
    }
      guard let classifiedImages = response?.result else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

Visual Recognition 프레임워크에서 지원되는 여러 가지 분류 방법이 있습니다. Watson SDK [Visual Recognition 문서](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 탐색하여 애플리케이션에 가장 적합한 분류 방법을 찾으십시오.
{: tip}

## 스타터 킷 사용
{: #recognition_starterkits}

[스타터 킷](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")은 {{site.data.keyword.cloud_notm}}의 기능을 사용할 수 있는 가장 빠른 방법 중 하나입니다. **Visual Recognition for iOS with Watson** 스타터 킷을 선택하여 {{site.data.keyword.visualrecognitionshort}} 서비스를 사용할 수 있습니다. 이 서비스는 이미지를 평가하고 분류합니다. 모바일 디바이스에서 신규 또는 기존 이미지를 업로드하고, Visual Recognition 앱이 이미지 컨텐츠의 태그를 빠르게 지정하고 이미지 컨텐츠를 분류합니다.

시작하려면 다음을 수행하십시오.
1. [여기](https://cloud.ibm.com/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")에서 스타터 킷을 선택하십시오.
2. 기본 서비스를 사용하여 프로젝트를 작성하십시오.
3. **코드 다운로드**를 클릭하여 프로젝트를 다운로드하십시오. 서비스 인증 정보가 해당 키 필드의 `BMSCredentials.plist`에 삽입됩니다.

## 다음 단계
{: #recognition_next notoc}

잘 하셨습니다! 이제 앱에서 Visual Recognition을 사용할 수 있습니다. 다음 옵션 중 하나를 사용하여 계속 진행하십시오.
* [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 확인하고 기타 지원되는 Watson 서비스를 탐색하십시오. 
* 자세한 정보는 [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 참조하십시오.
