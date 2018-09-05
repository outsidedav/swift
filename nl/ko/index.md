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
{:tip: .tip}

# 시작하기 튜토리얼
{: #set_up}

{{site.data.keyword.cloud}}는 Swift 개발자와 애플리케이션에 고객이 요구하는 보안, AI 및 가치를 부여할 수 있는 솔루션 및 서비스를 제공합니다. 광범위한 포트폴리오의 오퍼링 및 SDK를 사용하여 이 서비스를 활용하고 최첨단 애플리케이션을 시장에 신속하게 출시할 수 있습니다. Swift 프로그래밍 안내서는 서비스를 신규 또는 기존 Swift(iOS 클라이언트 또는 서버 측 Swift) 애플리케이션에 추가하는 방법에 대해 설명합니다.
{: shortdesc}

다음 튜토리얼은 [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits)에서 비어 있는 스타터 킷을 사용하여 {{site.data.keyword.mobileanalytics_full}}로 Swift 모바일 앱을 쉽게 작성할 수 있는 방법을 표시하는 시작점입니다. 콘솔에서 {{site.data.keyword.mobileanalytics_short}} 서비스를 추가하고, 코드를 다운로드하고, Xcode에서 로컬로 iOS 앱을 실행하고, 앱을 구성 및 모니터합니다. 

## 1단계. 개발자를 위한 요구사항
{: #dev-requirements}

{{site.data.keyword.cloud_notm}}에서 iOS 개발을 시작하려면 다음 요구사항을 충족하는지 확인하십시오. 

### 운영 체제

최신 MacOS 지원 하드웨어를 사용하고 최신 iOS 릴리스로 테스트하여 Swift 앱을 개발하는 것이 가장 좋은 방법입니다. [Apple Developer](https://developer.apple.com/) 계정에 가입하여 실제 디바이스에서 테스트할 수 있습니다. 

### iOS 및 Xcode
{: #ios_and_xcode}

- [Xcode 8+](https://developer.apple.com/xcode/)(이상)
- [iOS 8 디바이스](https://support.apple.com/downloads/ios)(이상)에 배치
- Apple에 앱을 제출하기 전에 [앱 스토어 제출 가이드라인](https://developer.apple.com/app-store/guidelines/)을 검토하십시오.

### SDK 및 종속성 관리

다음 도구를 통해 다양한 {{site.data.keyword.cloud_notm}} 서비스에서 작동하도록 네이티브 SDK를 설치할 수 있습니다.

1. {{site.data.keyword.cloud_notm}} SDK를 지원하려면 [CocoaPods](https://cocoapods.org/)를 설치하십시오.
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
2. Carthage 설치를 지원하려면 [Homebrew](https://brew.sh/)를 설치하십시오.
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

3. Watson SDK를 지원하려면 [Carthage](https://github.com/Carthage/Carthage)를 설치하십시오.
  ```
  brew install carthage
  ```
  {: codeblock}

## 2단계. 사용자 정의 iOS Swift 앱 작성
{: #create-ios-app}

1. [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits)에 로그인하십시오.
2. **앱 작성**을 클릭하십시오.
3. [비어 있는 스타터](https://console.bluemix.net/developer/appledevelopment/create-app) 페이지에서 기본 구성을 사용하거나 필요에 따라 필드를 업데이트할 수 있습니다. **iOS Swift**가 선택된 언어인지 확인하십시오. **작성**을 클릭하십시오.

## 3단계. {{site.data.keyword.mobileanalytics_short}} 서비스 추가
{: #adding-services}

1. 앱 세부사항 페이지에서 **리소스 추가** 단추를 클릭하십시오.
2. **모바일**을 선택하고 **다음**을 클릭하십시오.
3. **{{site.data.keyword.mobileanalytics_short}}**를 선택하고 **다음**을 클릭하십시오.
4. **작성**을 클릭하십시오.

## 4단계. 코드 다운로드 및 클라이언트 SDK 설정
{: #run-locally}

코드를 다운로드하려면 `Apps` > `Your App`에서 **코드 다운로드**를 클릭하십시오. 다운로드된 코드에는 **{{site.data.keyword.mobileanalytics_short}}** 클라이언트 SDK가 포함되어 있습니다. 클라이언트 SDK는 CocoaPods 및 Carthage에서 사용할 수 있습니다. 이 솔루션의 경우 CocoaPods를 사용하십시오.

1. 다운로드된 코드를 압축 해제하십시오. 그런 다음 터미널을 사용하여 압축 해제된 폴더로 이동하십시오.
  ```
  cd <Name of Project>
  ```
2. 폴더에는 필요한 종속성이 포함된 podfile가 있습니다. 다음 명령을 실행하여 종속성(클라이언트 SDK)을 설치하십시오.
  ```
  pod install
  ```
  {: codeblock}

## 5단계. {{site.data.keyword.mobileanalytics_short}}를 사용하도록 앱 구성
{: #configure-analytics}

1. Xcode에서 `.xcworkspace`를 열고 `AppDelegate.swift`로 이동하십시오.
  **참고:** 원래의 Xcode 프로젝트 파일 `MyApp.xcworkspace` 대신 새 Xcode 작업공간을 항상 여십시오.
   ![Xcode 열기](images/Xcode.png)

  `BMSCore`는 코어 SDK이며 모바일 클라이언트 SDK의 기반입니다. `BMSClient`는 BMSCore의 클래스이며 AppDelegate.swift에서 초기화됩니다. BMSCore와 함께 {{site.data.keyword.mobileanalytics_short}} SDK도 이미 프로젝트에 가져왔습니다.
  
2. 다음 스니펫에 표시된 대로 분석 초기화 코드가 이미 포함되어 있습니다.
  ```
  // Analytics client SDK is configured to record lifecycle events.
         	Analytics.initialize(appName:dictionary["appName"] as? String,
        			     apiKey: dictionary["analyticsApiKey"] as? String,
        	        	     deviceEvents: .lifecycle)

        	// Enable Logger (disabled by default) and set level to ERROR (DEBUG by default).
        	Logger.isLogStorageEnabled = true
        	Logger.logLevelFilter = .error
  ```
  {: codeblock}

  **참고:** 서비스 신임 정보는 `BMSCredentials.plist` 파일의 일부입니다.

3. `logger`를 사용하여 사용 분석을 수집하십시오. `ViewController.swift` 파일을 찾아서 다음 코드를 확인하십시오. 
  ```
   func didBecomeActive(_ notification: Notification) {
       Analytics.send()
       Logger.send()
    }
  ```
  {: codeblock}

   고급 분석 및 로깅 기능은 [사용 분석 수집](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics) 및 [로깅](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger)을 참조하십시오.
   {:tip}

## 6단계. {{site.data.keyword.mobileanalytics_short}}로 앱 모니터링
{{site.data.keyword.mobileanalytics_short}} 서비스는 주요 애플리케이션 사용량 정보와 모바일 애플리케이션 개발자 및 애플리케이션 소유자를 위한 성능 인사이트를 제공합니다. {{site.data.keyword.mobileanalytics_short}}를 사용하여 애플리케이션 소유자 및 개발자는 사용자 측에서 발생하는 사항을 이해할 수 있습니다. 이러한 인사이트를 통해 사용자와 관련된 더 나은 애플리케이션을 빌드하고 모바일 애플리케이션의 대열에 진입할 수 있습니다. 

이 서비스에는 개발자 및 애플리케이션 소유자가 모바일 애플리케이션 성능을 모니터하고, 사용 통계를 보고, 디바이스 로그를 검색할 수 있는 {{site.data.keyword.mobileanalytics_short}} 콘솔이 있습니다. 

1. 사용자가 작성한 모바일 앱에서 `{{site.data.keyword.mobileanalytics_short}}` 서비스를 열거나 서비스 옆에 있는 수직으로 된 세 개의 점을 클릭하고 `Open Dashboard`를 선택하십시오.
2. `Demo Mode`를 사용 안함으로 설정하여 LIVE 사용자, 세션 및 기타 앱 데이터를 볼 수 있습니다. 다음 기준별로 분석 정보를 필터링할 수 있습니다.
    * 날짜
    * 애플리케이션
    * 운영 체제
    * 앱의 버전
         ![{{site.data.keyword.mobileanalytics_short}}](images/mobile_analytics.png)
3. [여기를 클릭](https://console.bluemix.net/docs/services/mobileanalytics/app-monitoring.html#monitoringapps)하여 경보, 앱 충돌 모니터 및 네트워크 요청 모니터를 설정하십시오.

## 다음 단계
{: #next-steps}

### 더 많은 서비스 추가
웹 콘솔에서 직접 더 많은 서비스(예: 다음과 같이 일반적으로 사용되는 서비스)를 iOS에 추가할 수 있습니다.

* [푸시 알림 서비스 추가](/push/push_notifications.html)
* [앱 ID로 사용자 인증 추가](/authenticate/app_id.html)

### {{site.data.keyword.cloud_notm}} 개발자 도구 사용
엔드-투-엔드 웹, 모바일 및 마이크로서비스 애플리케이션을 작성, 개발 및 배치하기 위한 명령행 접근 방식을 제공하는 [{{site.data.keyword.cloud_notm}} 개발자 도구](../cli/index.html)를 사용하여 Swift 앱을 개발하는 방법을 알아볼 수도 있습니다.

