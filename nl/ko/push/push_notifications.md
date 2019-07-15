---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-21"

keywords: push swift, swift notifications, push notifications swift, sending push swift, configure service instance swift, provider credentials swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.mobilepushshort}} 전송
{: #push_notifications}

모바일 디바이스 및 웹 애플리케이션에 실시간 알림을 전송하도록 {{site.data.keyword.cloud}}에서 {{site.data.keyword.mobilepushshort}} 서비스를 사용하여 Swift 앱을 향상시키십시오.

 - 알림은 모든 애플리케이션 사용자 또는 선택된 사용자 또는 디바이스의 세트에 전달될 수 있습니다.
 - 대화식 및 자동 알림을 모두 지원합니다.
 - 고객은 알림에 대한 특정 태그 또는 주제를 등록하도록 선택할 수 있습니다.
 - 앱 소유자는 알림을 수신하도록 등록된 디바이스 수와 전송된 알림 수를 분석할 수 있습니다.

MobileFirst Services Starter 표준 유형의 일부 또는 {{site.data.keyword.cloud_notm}} [전용 서비스](/docs/dedicated?topic=dedicated-dedicated#dedicated) 중 하나로 {{site.data.keyword.mobilepushshort}} 서비스를 사용하도록 선택할 수 있습니다. SDK(Software Development Kit) 및 [REST API ](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/notifications/rest-apis/){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 사용하여 클라이언트 애플리케이션을 추가로 개발할 수 있습니다.

![푸시 개요](images/push_notification_lifecycle.jpg "푸시 개요")

## 시작하기 전에
{: #prereqs-push}

먼저, 다음 필수 소프트웨어를 갖추었는지 확인하십시오.

 - iOS 8.0+
 - Xcode 7.3, 8.0
 - Swift 2.3 - 4.0
 - CocoaPods 또는 Carthage

## 1단계. {{site.data.keyword.mobilepushshort}}의 인스턴스 작성
{: #push_create}

1. {{site.data.keyword.cloud_notm}} 카탈로그에서 **모바일** > **{{site.data.keyword.mobilepushshort}}**를 클릭하십시오. 서비스 구성 화면이 열립니다.
2. 서비스 인스턴스에 이름을 지정하거나 사전 설정된 이름을 사용하십시오.
3. **작성**을 클릭하십시오.
4. 탐색 분할창에서 **연결**을 클릭하여 앱을 선택하고 서비스에 바인드하십시오. 작성 중에 서비스 인스턴스를 바인드되지 않은 상태로 둔 경우 나중에 서비스 인스턴스를 앱에 바인드할 수 있습니다.


## 2단계. 알림 제공자 인증 정보 얻기
{: #get_creds-push}

푸시 알림 서비스를 설정하려면 APN(Apple Push Notification Service)에서 필수 인증 정보를 가져와야 합니다. 해당 단계에 따라 [APN 인증 정보를 얻고 구성 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1){: new_window}하십시오.


## 3단계. 서비스 인스턴스 구성
{: #config-resource-push}

{{site.data.keyword.mobilepushshort}} 서비스를 사용하여 알림을 전송하려면 작성한 `.p12` 키 저장소를 업로드하십시오. 이 키 저장소에는 애플리케이션을 빌드하고 공개하는 데 필요한 개인 키 및 SSL 인증서가 포함되어 있습니다. REST API를 사용하여 APN 인증서를 업로드할 수도 있습니다.

`.cer` 파일이 키 체인 액세스에 있으면 이 파일을 컴퓨터로 내보내서 `.p12` 인증서를 작성하십시오.

APN 사용에 대한 자세한 정보는 [iOS Developer Library: 로컬 및 푸시 알림 프로그래밍 안내서 ](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 참조하십시오.

Push Notifications 서비스 콘솔에서 APN을 설정하려면 다음 단계를 완료하십시오.

1. {{site.data.keyword.mobilepushshort}} 서비스 콘솔에서 **구성**을 선택하십시오.
2. **모바일** 옵션을 선택하여 **APN 알림 인증 정보** 양식의 정보를 업데이트하십시오.
3. 다음 옵션 중 하나를 선택하십시오.
	- **모바일** 옵션의 경우
		1. **샌드박스**(개발) 또는 **프로덕션**(분배)를 선택한 후 작성한 `p.12` 인증서를 업로드하십시오.
		  ![{{site.data.keyword.mobilepushshort}} 콘솔](images/wizard.jpg "푸시 알림 모바일 구성")

		2. **비밀번호** 필드에서 `.p12` 인증서 파일과 연관된 비밀번호를 입력한 후 **저장**을 클릭하십시오.
	- **웹** 옵션의 경우
		- Safari 푸시 섹션에서 필수 정보로 양식을 업데이트하십시오.
		- **웹 사이트 이름**: 알림 센터에서 제공된 웹 사이트 이름입니다.
		- **웹 사이트 푸시 ID**: 웹 사이트 푸시 ID에 대한 역 도메인 문자열로 업데이트하십시오. 예: `web.com.example.www`.
		- **웹 사이트 URL**: 푸시 알림에 등록되는 웹 사이트의 URL을 입력하십시오. 예: `https://www.example.com`.
		- **허용된 도메인**: (선택적 매개변수) 사용자로부터 권한을 요청하는 웹 사이트의 목록입니다. URL은 쉼표로 구분된 값이어야 합니다. 정보가 제공되지 않은 경우에는 웹 사이트 URL의 값이 사용됩니다.
		- **URL 형식 문자열**: 알림을 클릭할 때 분석할 URL입니다. 예: `https://www.example.com`. URL에 http 또는 https 스키마가 사용되었는지 확인하십시오.
		- **Safari 웹 푸시 인증서**: `.p12` 인증서를 업로드하고 비밀번호를 제공하십시오.
4. **저장**을 클릭하십시오.

  ![{{site.data.keyword.mobilepushshort}} 콘솔](images/push_configure_safari.jpg "푸시 알림 웹 구성")

## 4단계. 서비스 클라이언트 SDK 설정
{: #service-client-push}

iOS 애플리케이션이 디바이스에 대한 푸시 알림을 받을 수 있으려면 {{site.data.keyword.mobilepushshort}} 서비스에 대한 iOS SDK를 구성해야 합니다.

{{site.data.keyword.cloud_notm}} Mobile Services Swift SDK는 Cocoapods 또는 Carthage로 설치할 수 있습니다. 자세한 정보는 [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 참조하십시오.

## 5단계. 알림 전송
{: #send-notify-push}

애플리케이션을 개발한 후 기본 푸시 알림을 전송할 수 있습니다.

기본 푸시 알림을 전송하려면 다음 단계를 완료하십시오.

1. **알림 전송**을 선택하고 **받는 사람** 옵션을 선택하여 메시지를 작성하십시오. 지원되는 옵션은 **태그별 디바이스**, **디바이스 ID**, **사용자 ID**, **iOS 디바이스**, **웹 알림** 및 **모든 디바이스**입니다.
**참고**: **모든 디바이스** 옵션을 선택하는 경우 {{site.data.keyword.mobilepushshort}}에 등록된 모든 디바이스가 알림을 수신합니다.

  ![알림 전송 화면](images/tag_notification.jpg "알림 전송 화면")

2. **메시지** 필드에서 메시지를 작성하십시오. 필요에 따라 선택적 옵션을 구성하도록 선택하십시오.
3. **전송**을 클릭하십시오.
3. 디바이스 또는 브라우저가 알림을 수신했는지 확인하십시오.

  다음 화면 캡처는 디바이스에서 포그라운드로 푸시 알림을 처리하는 경보 상자를 보여줍니다.
  
  ![Android에서 포그라운드 푸시 알림](images/Android_Screenshot.jpg "포그라운드 알림 경보")

  다음 화면 캡처는 백그라운드로 푸시 알림을 보여줍니다.
  
  ![iOSd에서 백그라운드 푸시 알림](images/background.png "백그라운드 알림 경보")

### 선택적 설정
{: #optional-push}

iOS 디바이스에 알림을 전송하기 위해 {{site.data.keyword.mobilepushshort}} 설정을 사용자 정의할 수 있습니다. 다음 선택적 사용자 정의 옵션이 지원됩니다.

- **배지**: 애플리케이션 배지에 표시되는 숫자를 나타냅니다. 기본값은 0(영)이며 배지를 표시하지 않습니다.
- **사운드**: 알림을 수신할 때 재생되는 사운드 클립을 표시합니다. 기본값 또는 앱에서 번들된 사운드 리소스의 이름을 지원합니다.
- **추가 페이로드**: 알림에 대한 사용자 정의 페이로드 값을 지정합니다.

[대화식 알림](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#interactive-notifications){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘") 및 [리치 미디어 알림](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enabling-rich-media-notifications){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 사용으로 설정하도록 선택할 수도 있습니다.

## 6단계. 전달된 알림 모니터링
{: #monitor-push}

{{site.data.keyword.mobilepushshort}} 서비스는 전송된 메시지의 상태를 확인하는 데 도움이 되는 모니터링 유틸리티를 제공합니다. 모니터링 유틸리티를 구성하려면 [iOS 애플리케이션용 모니터링 사용](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enable-monitoring){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 참조하십시오.

## 다음 단계
{: #next-push notoc}

 - 서비스에 대해 자세히 알아보고 모든 기능을 활용하려면 [문서](/docs/services/mobilepush?topic=mobile-pushnotification-overview-push)를 검토하십시오.

 - 모바일 서비스 및 {{site.data.keyword.cloud_notm}}에 대한 작업의 소개는 [{{site.data.keyword.cloud_notm}}에서 모바일 앱 시작하기](/docs/services/mobile?topic=mobile-about)를 참조하십시오.

 - 스타터 킷은 {{site.data.keyword.cloud_notm}}의 기능을 사용할 수 있는 가장 빠른 방법 중 하나입니다. [모바일 개발자 대시보드](https://{DomainName}/developer/mobile/dashboard){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")에서 사용 가능한 스타터 킷을 보십시오. 코드를 다운로드하십시오. 앱을 실행하십시오!

 - [Swagger UI](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/notifications/rest-apis/){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 사용하여 REST API 문서를 빠르게 검토할 수 있습니다.
