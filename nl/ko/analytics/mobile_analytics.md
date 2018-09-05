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

# 모바일 분석 수집
{: #mobile_analytics}

{{site.data.keyword.cloud_notm}}의 {{site.data.keyword.mobileanalytics_short}}는 개발자, IT 관리자 및 비즈니스 이해 당사자에게 모바일 앱의 수행 방법 및 사용 방법에 대한 인사이트를 제공합니다. {{site.data.keyword.mobileanalytics_short}} 서비스를 사용하여 다음을 수행할 수 있습니다.

 - **즉각적인 인사이트 확보** - 실시간으로 성능 및 사용 메트릭을 확인합니다.

 - **몇 분 안에 구현** - {{site.data.keyword.cloud_notm}}에서 서비스 인스턴스를 작성하고, SDK를 프로젝트에 추가한 다음, 두 행의 코드를 애플리케이션에 붙여넣으면 많은 수의 사전 정의된 메트릭을 수집할 준비가 됩니다.

 - **원하는 데이터 수집** - 사용자 정의 이벤트로 앱을 인스트루먼트화하고, 사용자가 앱과 상호작용하는 방법을 확인하며, 구매 추적 및 앱 활동을 모니터합니다.

 - **한 눈에 메트릭 보기** - {{site.data.keyword.mobileanalytics_short}} 콘솔은 조회를 작성할 필요 없이 기본 차트를 제공합니다.

 - **중요 항목에 주력** - 시간, 어댑터, 애플리케이션, 애플리케이션 버전, OS, OS 버전 또는 디바이스 모델별로 메트릭을 필터링합니다.

 - **문제를 빠르게 발견** - 충돌 상태를 모니터합니다. 중요한 메트릭에 대한 경보 트리거를 설정하고 REST 엔드포인트에 경보를 라우트합니다. 

 - **근본 원인에 대한 문제점 해결** - 애플리케이션에서 사용자 정의 로깅을 사용하고 자동으로 로그를 업로드한 후 콘솔에서 로그를 검색합니다. 충돌 이벤트를 드릴 다운하여 스택 추적을 확인합니다.

## 시작하기 전에

먼저, 다음 필수 소프트웨어를 갖추었는지 확인하십시오. 

 - iOS 8.0+ / watchOS 2.0+
 - Xcode 7.3, 8.0
 - Swift 2.2 - 3.0
 - Cocoapods 또는 Carthage

## 1단계. {{site.data.keyword.mobileanalytics_short}}의 인스턴스 작성
{: #mobile_analytics_create}

1. {{site.data.keyword.cloud_notm}} 카탈로그에서 **모바일** > **{{site.data.keyword.mobileanalytics_short}}**를 클릭하십시오. 서비스 구성 화면이 열립니다.
2. 서비스 인스턴스에 이름을 지정하거나 사전 설정된 이름을 사용하십시오.
3. **작성**을 클릭하십시오.
4. 탐색 분할창에서 **연결**을 클릭하여 앱을 선택하고 서비스에 바인드하십시오. 작성 중에 서비스 인스턴스를 바인드되지 않은 상태로 둔 경우 나중에 서비스 인스턴스를 앱에 바인드할 수 있습니다.

## 2단계. iOS Swift SDK 설치

서비스는 플랫폼별 SDK를 제공하여 애플리케이션 개발을 간소화합니다. {{site.data.keyword.cloud_notm}} Mobile Services Swift SDK는 Cocoapods 또는 Carthage로 설치할 수 있습니다. 자세한 정보는 [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics)를 참조하십시오.

{{site.data.keyword.mobileanalytics_full}} SDK를 사용하여 모바일 애플리케이션을 인스트루먼트화할 수 있습니다. Swift SDK는 iOS 및 watchOS에서 사용할 수 있습니다.

1. Xcode를 올바르게 설정했는지 확인하십시오. iOS 개발 환경을 설정하는 방법을 알아보려면 [Apple Developer 웹 사이트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.apple.com/support/xcode/){: new_window}를 참조하십시오. 클라이언트 SDK Swift 분석을 위한 [Xcode 요구사항 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#requirements){: new_window}을 읽어 보십시오. 

2. 앱의 프로젝트 폴더에서 `Info.plist` 파일의 특성을 추가하여 위치 API를 사용으로 설정하십시오. 예를 들면, `Privacy - Location Usage Description`입니다. 위치 API를 추가하려면 "앱에서 위치 서비스를 사용으로 설정해야 함"과 같은 적절한 이유를 제공하십시오. 

{{site.data.keyword.mobileanalytics_short}} SDK는 Cocoa 프로젝트용 종속성 관리자인 [CocoaPods ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cocoapods.org/){: new_window} 및 [Carthage ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/Carthage/Carthage#getting-started){: new_window}로 분배됩니다. CocoaPods 및 Carthage는 애플리케이션에서 아티팩트를 사용할 수 있도록 저장소에서 아티팩트를 자동으로 다운로드합니다. CocoaPods 또는 Carthage를 선택하십시오.

### CocoaPods
{: #cocoapods}

1. Cocoapods를 사용하고 이를 프로파일에 추가하여 `BMSAnalytics`를 설치하려면 GitHub의 [{{site.data.keyword.Bluemix_notm}} Mobile Services Swift SDK 지시사항 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#cocoapods){: new_window}을 따르십시오.

2. iOS 클라이언트 SDK를 설치한 후 Analytics 클라이언트 SDK를 [가져오고 초기화](sdk.html#initalize-ma-sdk)하십시오.   

### Carthage
{: #carthage}

CocoaPods를 사용하지 않는 경우 [Carthage ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos){: new_window}를 사용하여 프레임워크를 프로젝트에 추가할 수 있습니다.

1. `BMSAnalytics`를 설치하려면 GitHub의 [Carthage 설치 지시사항 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#carthage){: new_window}을 따르십시오.

2. iOS 클라이언트 SDK를 설치한 후 가져온 다음 Analytics 클라이언트 SDK를 초기화하십시오.

## 3단계. SDK 초기화

{{site.data.keyword.mobileanalytics_short}}를 사용하여 다음 카테고리의 데이터를 수집할 수 있으며, 각각 다른 수준의 인스트루먼테이션이 필요합니다. 

**사전 정의된 데이터**:
이 카테고리에는 모든 애플리케이션에 적용되는 일반적인 사용 및 디바이스 정보가 포함됩니다. 이는 애플리케이션 사용의 볼륨, 빈도 또는 지속 기간을 표시합니다. 사전 정의된 데이터는 애플리케이션에서 {{site.data.keyword.mobileanalytics_short}} SDK를 초기화한 후 자동으로 수집됩니다.

**애플리케이션 로그 메시지**:
이 카테고리를 통해 개발자는 개발 및 디버깅을 지원하기 위해 사용자 정의 메시지를 로그할 수 있는 코드 행을 애플리케이션 전반에 추가할 수 있습니다. 개발자는 각 로그 메시지에 심각도/상세 레벨을 지정합니다. 

**사용자 정의 이벤트**:
이 카테고리에는 사용자가 자체적으로 정의하며 앱에 특정한 데이터가 포함됩니다. 이 데이터는 앱에서 발생하는 이벤트(예: 페이지 보기, 단추 탭 또는 인 앱 구매)를 표시합니다. 사용자 앱에서 {{site.data.keyword.mobileanalytics_short}} SDK를 초기화하는 것 외에도 추적할 각 사용자 정의 이벤트마다 코드 행을 추가해야 합니다.

## 4단계. 서비스 신임 정보 API 키 값 식별
{: #analytics-clientkey}

클라이언트 SDK를 설정하기 전에 **API 키** 값을 식별하십시오. API 키는 클라이언트 SDK를 초기화하는 데 필요합니다.

1. {{site.data.keyword.mobileanalytics_short}} 서비스 대시보드를 여십시오.
2. **신임 정보 보기**를 펼쳐 API 키 값을 표시하십시오. {{site.data.keyword.mobileanalytics_short}} 클라이언트 SDK를 초기화할 때 API 키 값이 필요합니다.

## 5단계. 분석을 수집하기 위해 애플리케이션 초기화
{: #initalize-ma-sdk}

{{site.data.keyword.mobileanalytics_short}} 서비스에 로그를 전송할 수 있도록 애플리케이션을 초기화하십시오.

1. 클라이언트 SDK를 가져오십시오.

    Swift SDK는 iOS 및 watchOS에서 사용할 수 있습니다. 다음 `import`문을 `AppDelegate.swift` 프로젝트 파일의 시작 부분에 추가하여 `BMSCore` 및 `BMSAnalytics` 프레임워크를 가져오십시오.
	```
	import BMSCore
	import BMSAnalytics
	```
	{: codeblock}  

2. 애플리케이션에서 {{site.data.keyword.mobileanalytics_short}} 클라이언트 SDK를 초기화하십시오.

	먼저 애플리케이션 위임의 `application(_:didFinishLaunchingWithOptions:)` 메소드 또는 프로젝트에 가장 적합한 위치에 초기화 코드를 배치하여 `BMSClient` 클래스를 초기화하십시오.
	```
	BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Make sure that you point to your region
	```
	{: codeblock}

	**bluemixRegion** 매개변수를 사용하여 `BMSClient`를 초기화해야 합니다. 초기자(initializer)에서 **bluemixRegion** 값은 사용 중인 {{site.data.keyword.Bluemix_notm}} 배치를 지정합니다.

3. 애플리케이션 오브젝트를 사용하고 애플리케이션의 이름을 제공하여 분석을 초기화하십시오. 

	애플리케이션(`your_app_name_here`)에 대해 선택한 이름이 애플리케이션 이름으로 {{site.data.keyword.mobileanalytics_short}} 콘솔에 표시됩니다. 애플리케이션 이름은 대시보드에서 애플리케이션 로그를 검색하기 위한 필터로 사용됩니다. 플랫폼(예: iOS) 전체에서 동일한 애플리케이션 이름을 사용하는 경우 로그가 전송된 플랫폼에 상관없이 동일한 이름으로 해당 애플리케이션에서 모든 로그를 볼 수 있습니다.

	[API 키](#analytics-clientkey) 값도 필요합니다.
	```
	Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, collectLocation: true, deviceEvents: .lifecycle, .network)
	```
    {: codeblock}

4. 이제 애플리케이션이 초기화되고 분석을 수집할 준비가 되었습니다. 다음으로, 분석 데이터를 {{site.data.keyword.mobileanalytics_short}} 서비스에 전송할 수 있습니다.		

## 6단계. 사용 분석 수집
{: #app-monitoring-gathering-analytics}

사용 분석을 기록하고 기록된 데이터를 {{site.data.keyword.mobileanalytics_short}} 서비스로 전송하도록 {{site.data.keyword.mobileanalytics_short}} 클라이언트 SDK를 구성할 수 있습니다.

사용 분석의 기록 및 전송을 시작하려면 다음 API를 사용하십시오. 
```
// Disable recording of usage analytics (eg: to save disk space)
// Recording is enabled by default

Analytics.isEnabled = false

// Enable recording of usage analytics

Analytics.isEnabled = true

// Send recorded usage analytics to the Mobile Analytics Service

Analytics.send(completionHandler: { (response: Response?, error: Error?) in
    if let response = response {

	  // Handle Analytics send success here.
    }
    if let error = error {
        // Handle Analytics send failure here.
    }
})
```
{: codeblock}

이벤트 로깅을 위한 샘플 사용 분석:
```
// Log a custom analytics event
let eventObject = ["customProperty": "propertyValue"]
Analytics.log(metadata: eventObject)
```
{: codeblock}

## 7단계. 로거 사용

{{site.data.keyword.mobileanalytics_full}} 클라이언트 SDK는 사용자에게 익숙한 다른 로깅 프레임워크(예: `java.util.logging` 또는 `log4j`)와 유사한 로깅 프레임워크를 제공합니다. 로깅 프레임워크는 여러 패키지별 로거 인스턴스, 다양한 로그 레벨, 애플리케이션 충돌에 대한 스택 추적 캡처 등을 지원합니다.

또한 애플리케이션이 실행 중인 디바이스에 저장할 로그된 데이터를 구성하고 나중에 디바이스 로그를 {{site.data.keyword.mobileanalytics_short}} 서비스에 전송합니다.

{{site.data.keyword.mobileanalytics_short}} 클라이언트 SDK 로깅 프레임워크는 가장 상세하지 않은 항목부터 가장 상세한 항목 순으로 나열되는 다음 로그 레벨을 지원하며, 권장되는 사용 가이드라인이 함께 제공됩니다.

  * `FATAL` - 복구할 수 없는 충돌 또는 정지에 사용됩니다. `FATAL` 레벨은 애플리케이션 충돌로 사용자에게 표시되는 복구 불가능한 오류를 로그하기 위해 예약됩니다.
  * `ERROR` - 예상치 못한 예외 또는 예상치 못한 네트워크 프로토콜 오류에 사용됩니다.
  * `WARN` - 더 이상 사용되지 않는 API 사용 또는 느린 네트워크 응답과 같이 치명적인 오류로 간주되지 않는 사용 경고를 로그하는 데 사용됩니다.
  * `INFO` - 중요하나 긴급하지는 않은 초기화 이벤트 및 기타 데이터를 보고하는 데 사용됩니다.
  * `DEBUG` - 개발자가 애플리케이션 결함을 해결하는 데 도움을 주기 위해 디버그 명령문을 보고하는 데 사용됩니다.

### 로그 레벨 시나리오
{: #log-level-scenario}

로거 레벨이 `FATAL`로 구성된 경우 로거가 미발견 예외를 캡처하지만 충돌 이벤트를 발생시키는 임의의 로그를 캡처하지 않습니다. `FATAL` 로거 항목(예: `WARN` 및 `ERROR`)이 발생할 수 있는 로그도 캡처되도록 좀 더 상세한 로거 레벨을 설정할 수 있습니다.

로거 레벨이 `DEBUG`로 설정된 경우 로그를 전송할 때 포함되는 Mobile Analytics 클라이언트 SDK 로그도 가져옵니다.

### 샘플 로거 사용법
{: #sample-logger-usage}

**참고:** 로깅 프레임워크를 사용하기 전에 애플리케이션이 {{site.data.keyword.mobileanalytics_short}} 클라이언트 SDK를 사용하도록 구성되었는지 확인하십시오.

다음 코드 스니펫은 샘플 로거 사용법을 보여줍니다.
```
// Configure Logger. Disabled by default;

Logger.isLogStorageEnabled = true

// Change the minimum log level (optional). Default is - LogLevel.debug

Logger.logLevelFilter = LogLevel.info

// Multiple log instances can be created to organize your logs

let logger1 = Logger.logger(name: "feature1Logger")
let logger2 = Logger.logger(name: "feature2Logger")

// Log messages with different levels

logger1.debug(message: "debug message for feature 1")

// logger1.debug message isn't logged as logLevelFilter is set to info

logger2.info(message: "info message for feature 2")

// Send logs to the Mobile Analytics Service

Logger.send(completionHandler: { (response: Response?, error: Error?) in
        if let response = response {
        logger.debug(message: "Status code: \(response.statusCode)")
        logger.debug(message: "Response: \(response.responseText)")
    }
    if let error = error {
        logger.error(message: "Error: \(error)")
    }
})
```
{: codeblock}

개인정보 보호를 위해 릴리스 모드에서 빌드된 애플리케이션을 위해 로거 출력을 사용 안함으로 설정할 수 있습니다. 기본적으로 로거 클래스는 Xcode 콘솔에 대한 로그를 인쇄합니다. 대상을 위한 빌드 설정에서 `-D RELEASE_BUILD` 플래그를 릴리스 빌드 구성의 **기타 Swift 플래그** 섹션에 추가하십시오.
{: tip}

## 8단계. 위치 데이터 로깅
{: #location-logging}

제공되는 다음 API를 통해 앱에서 모바일 디바이스의 위치를 로그할 수 있습니다.
```
Analytics.logLocation();
```

API를 통해 앱은 앱 세션 간에 위치(위도 및 경도)를 서버로 전송할 수 있습니다. `location-logging` API에 대한 명시적인 호출 외에도 SDK는 모든 앱 세션에 대해 디바이스 위치를 전송합니다. 위치는 초기 앱 세션 컨텍스트 및 사용자 전환 앱 세션(즉, 앱 세션 간의 사용자 전환) 컨텍스트에 대해 전송됩니다. SDK 초기화에서 위치 API를 사용으로 설정해야 합니다.

`location-logging` API를 호출하려면 SDK 초기화에서 `collectLocation` 매개변수를 `true`로 설정하십시오.
```
Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
```

## 9단계. 네트워크 요청 작성
{: #network-requests}

[네트워크 요청을 작성](/docs/mobile/sdk_network_request.html)하도록 {{site.data.keyword.mobileanalytics_short}} 클라이언트 SDK를 구성할 수 있습니다. 이미 `BMSClient` 및 `BMSAnalytics`를 초기화하고 클라이언트 SDK를 가져왔는지 확인하십시오.

## 10단계. 출동 분석 보고
{: #report-crash-analytics}

분석 및 로그 정보를 {{site.data.keyword.mobileanalytics_short}}에 전송하여 [애플리케이션 충돌 데이터](app-monitoring.html#monitor-app-crash)를 볼 수 있습니다.

`Analytics.send()` 메소드는 **충돌** 페이지에 있는 **충돌 개요** 및 **충돌** 표를 채웁니다. 차트는 초기화를 사용하고 분석을 위한 프로세스를 전송하여 사용으로 설정되며, 특별한 구성은 필요하지 않습니다.

`Logger.send()` 메소드는 **문제점 해결** 페이지에 있는 **충돌 요약** 및 **충돌 세부사항** 표를 채웁니다. 애플리케이션 코드에 다음 명령문을 추가하여 애플리케이션이 로그를 저장하고 전송하여 차트를 채울 수 있도록 설정해야 합니다.
```
Logger.isLogStorageEnabled = true
Logger.logLevelFilter = LogLevel.Fatal // or greater
```
{: codeblock}

iOS [샘플 로거 사용법](sdk.html##sample-logger-usage)을 참조하십시오.

## 11단계. 활성 사용자 추적
{: #trackingusers}

애플리케이션이 디바이스에서 고유한 사용자를 인식할 수 있는 경우 활성 사용자의 이름을 {{site.data.keyword.mobileanalytics_short}}로 전달하여 애플리케이션을 능동적으로 사용 중인 사용자의 수를 추적할 수 있습니다.

{{site.data.keyword.mobileanalytics_short}}를 `hasUserContext=true` 상태로 초기화하여 사용자 추적을 사용으로 설정하십시오. 그렇지 않으면 {{site.data.keyword.mobileanalytics_short}}에서는 디바이스당 한 명의 사용자만 캡처합니다.

다음 코드를 추가하여 사용자가 로그인하는 시점을 추적하십시오. 
```
Analytics.userIdentity = "username"
```
{: codeblock}

## 12단계. 앱 테스트
{: #appid_testing}

모든 항목이 올바르게 설정되어 있습니까? 이제 테스트를 수행하십시오.

1. 앱을 여십시오. 웹 애플리케이션이 있는 경우에는 브라우저를 사용하십시오. Xcode 에뮬레이터가 포함된 iOS 클라이언트 애플리케이션이 있는 경우에 앱을 여십시오.
2. 에뮬레이터 또는 디바이스에서 애플리케이션을 컴파일하고 실행하십시오.
3. GUI를 사용하여 애플리케이션에 로그인하는 프로세스를 살펴보십시오.
4. 애플리케이션에 대한 사용 분석을 확인하려면 {{site.data.keyword.mobileanalytics_short}} 콘솔로 이동하십시오. 또한 [경보 설정](/docs/services/mobileanalytics/app-monitoring.html#alerts) 및 [앱 충돌 모니터링](/docs/services/mobileanalytics/app-monitoring.html#monitor-app-crash)을 통해 애플리케이션을 모니터할 수도 있습니다.

## 다음에 수행할 작업
{: #what-to-do-next notoc}

 - 서비스에 대해 자세히 알아보려면 [문서](/docs/services/mobileanalytics/index.html#getting-started-tutorial)를 읽어 보십시오.

 - 모바일 서비스 및 {{site.data.keyword.Bluemix_notm}}에 대한 작업의 소개는 [IBM Cloud에서 모바일 앱 시작하기](/docs/services/mobile/index.html)를 참조하십시오.

 - 스타터 킷은 {{site.data.keyword.cloud_notm}}의 기능을 활용할 수 있는 가장 빠른 방법 중 하나입니다. [모바일 개발자 대시보드](https://console.bluemix.net/developer/mobile/dashboard)에서 사용 가능한 모든 스타터 킷을 보십시오. 코드를 다운로드하십시오. 앱을 실행하십시오!

 - [Swagger UI](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/)를 사용하여 REST API 문서를 빠르게 검토할 수 있습니다.
