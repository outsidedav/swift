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

# 시작하기 튜토리얼
{: #getting_started_swift}

{{site.data.keyword.cloud}}는 Swift 개발자가 고객이 요구하는 보안, AI 및 가치와 통합되는 애플리케이션을 빌드할 수 있는 솔루션 및 서비스를 제공합니다. 광범위한 포트폴리오의 오퍼링 및 SDK를 사용하여 이 서비스를 이용하고 최첨단 애플리케이션을 시장에 신속하게 출시할 수 있습니다. Swift 프로그래밍 안내서는 서비스를 신규 또는 기존 Swift(iOS 클라이언트 또는 서버 측 Swift) 애플리케이션에 추가하는 방법에 대해 설명합니다.
{: shortdesc}

다음 튜토리얼은 [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")에서 비어 있는 스타터 킷을 사용하여 {{site.data.keyword.mobileanalytics_full}}로 Swift 모바일 앱을 쉽게 작성할 수 있는 방법을 보여즙니다. 콘솔에서 {{site.data.keyword.mobileanalytics_short}} 서비스를 추가하고, 코드를 다운로드하고, Xcode에서 로컬로 iOS 앱을 실행하고, 앱을 구성 및 모니터합니다.

## 1단계. 개발자를 위한 요구사항
{: #dev-requirements-swift}

{{site.data.keyword.cloud_notm}}에서 iOS 개발을 시작하려면 다음 요구사항을 충족하는지 확인하십시오.

### 운영 체제
{: #swift-os-requirements}

최신 MacOS 지원 하드웨어를 사용하고 최신 iOS 릴리스로 테스트하여 Swift 앱을 개발하는 것이 가장 좋은 방법입니다. [Apple Developer](https://developer.apple.com/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 계정에 가입하여 실제 디바이스에서 테스트할 수 있습니다.

### iOS 및 Xcode
{: #ios_and_xcode}

- [Xcode 8+](https://developer.apple.com/xcode/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")(이상)을 설치하십시오. 
- [iOS 8 디바이스](https://support.apple.com/downloads/ios){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")(이상)에 배치하십시오. 
- Apple에 앱을 제출하기 전에 [앱 스토어 제출 가이드라인](https://developer.apple.com/app-store/guidelines/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")을 검토하십시오.

### SDK 및 종속성 관리
{: #swift-sdk-management}

다음 도구를 통해 다양한 {{site.data.keyword.cloud_notm}} 서비스에서 작동하도록 네이티브 SDK를 설치할 수 있습니다. 종속성 관리를 위해 CocoaPods 또는 Carthage를 사용할 수 있습니다.

* **CocoaPods 사용** - {{site.data.keyword.cloud_notm}} SDK를 지원하려면 [CocoaPods](https://cocoapods.org/)를 설치하십시오.
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}

* **카트리지 사용** - [Carthage ](https://github.com/Carthage/Carthage){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 또는 [Swift Package Manager](https://swift.org/package-manager/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 종속성 관리자를 통해 일부 SDK도 사용할 수 있습니다. 종속성 관리를 위해 Carthage를 사용하려면 다음 단계를 완료하십시오.

  Carthage 설치를 지원하려면 [Homebrew](https://brew.sh/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 설치하십시오. 
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

  Carthage를 설치하십시오.
  ```
  brew install carthage
  ```
  {: codeblock}

## 2단계. 사용자 정의 iOS Swift 앱 작성
{: #create-ios-app-swift}

1. [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")에 로그인하십시오.
2. **앱 작성**을 클릭하십시오.
3. [비어 있는 스타터](https://cloud.ibm.com/developer/appledevelopment/create-app){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 페이지에서 기본 구성을 사용하거나 필요에 따라 필드를 업데이트할 수 있습니다. **iOS Swift**가 선택된 언어인지 확인하십시오. **작성**을 클릭하십시오.

## 3단계. {{site.data.keyword.cloudant_short_notm}} 서비스 추가
{: #resources-swift}

이제 서비스를 Swift 애플리케이션에 추가할 수 있습니다. 이 튜토리얼의 완전히 관리되는 분산 `JSON` 문서 데이터베이스를 작성하는 Swift 앱에 {{site.data.keyword.cloudant_short_notm}} 서비스를 추가하십시오. Cloudant를 통해 데이터를 안전하게 보호하고 동기화를 유지하는 경량 프레임워크에서 앱의 확장성, 고가용성 및 내구성이 강화됩니다.

1. **앱 세부사항** 페이지에서 **서비스 추가**를 클릭하십시오.
2. **데이터**를 선택하고 **다음**을 클릭하십시오.
3. **Cloudant**를 선택하고 **다음**을 클릭하십시오.
4. **작성**을 클릭하십시오.
5. 서비스가 작성되면 클릭하여 시작하십시오. 이 새 페이지에서 **Cloudant 대시보드 실행**을 선택하여 데이터베이스 및 JSON 문서 작성을 시작하십시오.  또는 프로그래밍 방식으로 이를 수행할 수도 있습니다.

## 4단계. 코드 다운로드 및 클라이언트 SDK 설정
{: #run-locally-swift}

코드를 다운로드하려면 `Apps` > `Your App`에서 **코드 다운로드**를 클릭하십시오. 다운로드된 코드는 일부 기본 초기화 코드를 비롯하여 [SwiftCloudant SDK](https://github.com/cloudant/swift-cloudant){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")가 포함된 상태로 제공됩니다. 클라이언트 SDK는 CocoaPods 및 Swift Package Manager에서 사용할 수 있습니다. 이 솔루션에서는 CocoaPods를 사용합니다.

1. 다운로드된 코드를 추출하십시오. 그런 다음 터미널을 사용하여 압축을 푼 폴더로 이동하십시오.
  ```
  cd <Name of Project>
  ```

2. 폴더에는 필요한 종속성이 포함된 podfile가 있습니다. 다음 명령을 실행하여 종속성(클라이언트 SDK)을 설치하십시오.
  ```
  pod install
  ```
  {: codeblock}

## 5단계. 새 데이터베이스를 사용하도록 앱 구성
{: #config-db-swift}

1. Xcode를 사용하여 `.xcworkspace`로 끝나는 파일 이름을 열고 `ViewController.swift`로 이동하십시오. `SwiftCloudant`는 코어 Cloudant SDK입니다. `CouchDBClient`는 `SwiftCloudant`의 클래스이며 `ViewController.swift`에서 초기화됩니다.

  원래의 Xcode 프로젝트 파일인 `MyApp.xcworkspace` 대신 항상 새 Xcode 작업공간을 열어 두십시오.
  {: tip}

2. 다음 스니펫에 표시된 대로 데이터베이스 초기화 코드가 이미 포함되어 있습니다.
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

  서비스 인증 정보는 `BMSCredentials.plist` 파일의 일부입니다.
  {: note}

## 6단계. 데이터베이스 오퍼레이션 빌드
{: #build_ops-swift}

이제 데이터베이스 연결이 작동되고 SDK가 설정되었습니다. 특정 유스 케이스에 맞게 기본 [오퍼레이션 작성, 읽기, 업데이트 및 영구 삭제](/docs/swift/data?topic=swift-cloudant#cloudant)의 빌드를 시작할 수 있습니다.

## 다음 단계
{: #next-swift}

### 더 많은 서비스 추가
{: #moreresources-swift}

웹 콘솔에서 직접 더 많은 서비스(예: 다음과 같이 일반적으로 사용되는 서비스)를 iOS에 추가할 수 있습니다.

* [푸시 알림 서비스 추가](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate#gettingstartedtemplate)
* [앱 ID로 사용자 인증 추가](/docs/services/appid?topic=appid-getting-started#getting-started)

### {{site.data.keyword.cloud_notm}} 개발자 도구 사용
{: #devtools-swift}

완벽한 웹, 모바일 및 마이크로서비스 애플리케이션을 작성, 개발 및 배치하기 위한 명령행 접근 방식을 제공하는 [{{site.data.keyword.cloud_notm}} 개발자 도구](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)를 사용하여 Swift 앱을 개발하는 방법을 알아볼 수도 있습니다.
