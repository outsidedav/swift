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

{{site.data.keyword.visualrecognitionfull}} 서비스를 통해 앱이 머신 러닝을 사용하여 빠르고 정확하게 시각적 컨텐츠의 태그를 지정하고, 시각적 컨텐츠를 분류하고, 훈련할 수 있습니다. 서비스는 사실상 모든 시각적 컨텐츠를 분류하고, 몇 분 안에 자신의 사용자 정의 모델을 훈련하며, 면을 감지하는 데 도움을 줄 수 있습니다.

## 작동 방법
{: ##how-it-works}

1. 앱이 분석할 이미지를 선택합니다.
2. 앱이 Watson Swift SDK를 사용하여 이미지를 {{site.data.keyword.visualrecognitionshort}} 서비스에 전송합니다.
3. 서비스가 분류 분석을 통해 이미지를 분석하여 화면, 오브젝트, 면 등을 식별합니다.
4. Watson Swift SDK를 통해 서비스의 분석 결과가 앱으로 리턴됩니다.

## 시작하기 전에
{: ###before-you-begin}

먼저, 다음 필수 소프트웨어를 갖추었는지 확인하십시오.
<ul>
  <li>iOS 8.0+</li>
  <li>Xcode 9.0+</li>
  <li>Swift 3.2+ 또는 Swift 4.0+</li>
  <li>Carthage</li>
</ul>

[Carthage](https://github.com/Carthage/Carthage)를 사용하여 종속성을 관리하고 사용자의 애플리케이션에 적합한 Watson Swift SDK를 빌드하는 것이 좋습니다. Carthage를 처음 사용하는 경우 [Homebrew](http://brew.sh/)를 사용하여 설치할 수 있습니다.

```bash
$ brew update
$ brew install carthage
```

## 1단계. Visual Recognition의 인스턴스 작성
{: ###create-and-configure-an-instance-of-visual-recognition}

{{site.data.keyword.visualrecognitionshort}} 서비스의 인스턴스를 프로비저닝하십시오.

1. {{site.data.keyword.cloud_notm}} 카탈로그에서 **{{site.data.keyword.visualrecognitionshort}}**를 클릭하십시오. 서비스 구성 화면이 열립니다.
2. 서비스 인스턴스에 이름을 지정하거나 사전 설정된 이름을 사용하십시오.
3. 인스턴스를 앱에 바인드하려면 **연결** 메뉴에서 앱을 선택하십시오.
4. 가격 책정 플랜을 선택하고 **작성**을 클릭하십시오.
5. **인증 정보** 탭을 선택하여 서비스 인증 정보를 확인하십시오. 이 값은 앱에서 서비스를 연결하는 데 사용됩니다.

## 2단계. 종속성 다운로드 및 빌드
{: ###download-and-build-dependencies}

선호하는 텍스트 편집기를 사용하여 프로젝트의 루트 디렉토리(여기에 `.xcodeproj` 파일이 있음)에 `Cartfile`이라는 파일을 작성하십시오. 그런 다음 종속성으로 Watson Swift SDK를 지정하는 행을 추가하십시오.
```
github "watson-developer-cloud/swift-sdk"
```
{: pre}

프로덕션 앱의 경우 새 Watson Swift SDK의 새 릴리스에서 발생하는 예상치 못한 변경사항을 방지하려면 특정 [버전 요구사항](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) 을 지정할 수 있습니다.

`Cartfile`가 올바르게 배치되면 이제 종속성을 다운로드하고 빌드할 수 있습니다. 터미널을 사용하여 프로젝트의 루트 디렉토리로 이동한 후 Carthage를 실행하십시오.

```bash
carthage update --platform iOS
```
{: pre}

Carthage는 Watson Swift SDK를 다운로드하고 프로젝트의 `Carthage/Build/iOS` 폴더에서 프레임워크를 빌드합니다.

## 3단계. 앱에 프레임워크 추가
{: ###add-frameworks-to-your-app}

### Visual Recognition 연결 단계:

Watson Swift SDK 프레임워크가 Carthage에서 빌드되었으므로 앱을 사용하여 Visual Recognition 프레임워크를 연결해야 합니다.

1. Xcode에서 앱을 열고 프로젝트를 선택하여 설정을 여십시오.
2. 앱 대상을 선택한 후 **일반** 탭을 여십시오.
3. "연결된 프레임워크 및 라이브러리" 섹션으로 아래로 스크롤하고 `+` 아이콘을 클릭하십시오.
4. 표시되는 창에서 **기타 항목 추가...**를 선택하고 `Carthage/Build/iOS` 디렉토리로 이동하십시오. **VisualRecognitionV3.framework**를 선택하여 앱을 통해 연결하십시오.

### Visual Recognition 복사 단계:

Visual Recognition 프레임워크에 _연결_하는 것 외에도 이 프레임워크를 앱에 _복사_하여 런타임 시 액세스할 수 있게 해야 합니다. Xcode에는 프레임워크를 복사하고 임베드할 수 있는 여러 가지 방법이 있으나, 특정 [앱 스토어 제출 버그](http://www.openradar.me/radar?id=6409498411401216)를 방지하기 위해 Carthage 스크립트를 사용할 수 있습니다.

1. Xcode에 앱 대상의 설정이 열려 있는 상태에서 **빌드 단계** 탭으로 이동하십시오.
2. `+` 아이콘을 클릭한 후 **새 실행 스크립트 단계**를 선택하십시오.
3. `/usr/local/bin/carthage copy-frameworks` 명령을 실행 스크립트 단계에 추가하십시오.
4. `$(SRCROOT)/Carthage/Build/iOS/VisualRecognitionV3.framework` Visual Recognition 프레임워크를 **입력 파일** 목록에 추가하십시오.

이제 앱에서 Watson Swift SDK에 대한 작업을 시작할 준비가 되었습니다!

## 4단계. 앱에서 이미지 분석
{: #analyze-images-in-your-app}

1. Xcode에서 `ViewController.swift` 파일을 여십시오.

1. Visual Recognition을 위한 import 문을 추가하십시오.
    ```swift
    import VisualRecognitionV3
    ```
    {: pre}

1. API 키 및 버전(오늘 날짜 사용할 수 있음)을 전달하여 SDK를 초기화하십시오.
    ```swift
    let visualRecognition = VisualRecognition(apiKey: "your-api-key", version: "yyyy-mm-dd")
    ```
    {: pre}

1. 다음 코드를 추가하여 이미지를 분류하십시오.
    ```swift
    let url = "your-image-url"
    let failure = { (error: Error) in print(error) }
    visualRecognition.classify(url: url, failure: failure) { classifiedImages in
        print(classifiedImages)
    }
    ```
    {: pre}

## 스타터 킷 사용
{: #recognition_starterkits}

[스타터 킷](https://console.bluemix.net/developer/appledevelopment/starter-kits)은 {{site.data.keyword.cloud_notm}}의 기능을 활용할 수 있는 가장 빠른 방법 중 하나입니다. **Visual Recognition for iOS with Watson** 스타터 킷을 선택하여 {{site.data.keyword.visualrecognitionshort}} 서비스를 사용할 수 있습니다. 이 서비스는 이미지를 평가하고 분류합니다. 모바일 디바이스에서 신규 또는 기존 이미지를 업로드하고, Visual Recognition 앱이 이미지 컨텐츠의 태그를 빠르게 지정하고 이미지 컨텐츠를 분류합니다.

시작하려면 다음을 수행하십시오.
1. [여기](https://console.bluemix.net/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson)에서 스타터 킷을 선택하십시오.
2. 기본 서비스를 사용하여 프로젝트를 작성하십시오.
3. **코드 다운로드**를 클릭하여 프로젝트를 다운로드하십시오. 서비스 인증 정보가 해당 키 필드의 `BMSCredentials.plist`에 삽입됩니다.

## 다음 단계
{: #recognition_next}

잘 하셨습니다! 이제 앱에서 Visual Recognition을 사용할 수 있습니다. 다음 옵션 중 하나를 사용하여 계속 진행하십시오.
* [GitHub에서 Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk)를 보십시오.
* 자세한 정보는 [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/)을 참조하십시오.

