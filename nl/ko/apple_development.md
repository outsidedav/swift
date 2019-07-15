---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: watson swift, swift developer, apple development, ios oveview, developer console, swift, apple console

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:tip .tip}

# {{site.data.keyword.cloud_notm}}에서의 Apple 개발
{: #ios_dev}

{{site.data.keyword.cloud}}는 Swift 개발자와 애플리케이션에 고객이 요구하는 보안, AI 및 가치를 부여할 수 있는 솔루션 및 서비스를 제공합니다. 광범위한 포트폴리오의 오퍼링 및 SDK를 사용하여 이 서비스를 활용하고 최첨단 애플리케이션을 시장에 신속하게 출시할 수 있습니다.

## 하나의 개발 플랫폼: IBM Cloud
{: #platform}

{{site.data.keyword.cloud_notm}}에 내장된 개발자 기능은 다양한 스킬 세트에 맞춰 조정되며, 앱을 생성, 전달, 실행 및 관리할 수 있는 하나의 플랫폼을 제공합니다. 예를 들어, 언급된 코그너티브 앱에서 {{site.data.keyword.cloud_notm}} 기능의 관심 항목은 다음과 같습니다.

* [**{{site.data.keyword.cloud_notm}} 개발자 콘솔**](/docs/apps?topic=creating-apps-getting-started)은 {{site.data.keyword.cloud_notm}}의 기능 세트로 디지털 및 Cloud Native 개발자가 프로덕션에 바로 사용할 수 있는 앱 빌드를 시작하는 데 도움을 줍니다. 여기에는 서비스의 자동 프로비저닝 및 DevOps 도구 체인에 대한 "원클릭" 배치가 포함됩니다.

* [**IBM Watson Data Platform**](https://dataplatform.cloud.ibm.com/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")을 통해 보다 쉽게 데이터 콜렉션을 구성하고, 데이터를 세분화한 후 시각화하고, 인사이트를 발견하고, 코그너티브 앱에 사용할 모델을 빌드할 수 있습니다.

* [**IBM Streaming Analytics**](/docs/services/StreamingAnalytics?topic=StreamingAnalytics-gettingstarted#gettingstarted)는 데이터 스트림을 조정하고 분석합니다. 이는 실시간으로 다양한 유형의 데이터 소스로부터 도착한 정보를 수집, 분석 및 상관시킬 수 있는 고급 분석 플랫폼입니다.

* [**{{site.data.keyword.cloud_notm}} Continuous Delivery 서비스**](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-getting-started)는 DevOps 도구 체인을 설정하여 앱의 지속적 딜리버리를 자동화합니다. 모니터링, 로깅, 추적 및 경보와 같은 관리 기능을 포함하도록 도구 체인을 쉽게 개선할 수 있습니다. 또한 [DevOps Insights 서비스](/docs/services/DevOpsInsights?topic=DevOpsInsights-getting-started#getting-started)를 사용하여 고급 기계 학습을 도구 체인이 적용할 수 있습니다.

{{site.data.keyword.cloud_notm}} 플랫폼은 더욱 많은 기능을 제공하며 종합 개발 플랫폼으로 사용할 수 있습니다.

## {{site.data.keyword.cloud_notm}} 기능의 개요
{: #capabilities}

{{site.data.keyword.cloud_notm}} 개발자 콘솔을 사용하면 앱 빌드를 "올바른" 방식으로 몇 분 안에 시작할 수 있습니다. 개발자 콘솔의 필수 요소는 다음과 같습니다.

* {{site.data.keyword.cloud_notm}} 플랫폼 전체에서 발견되는 주제 세트 또는 채널 중심 개발자 콘솔.
* 다양한 언어 및 아키텍처 패턴으로 프로덕션에 바로 사용할 수 있는 스타터 앱을 생성하는 특정 유스 케이스 스타터 킷.
* 서비스의 자동 프로비저닝.
* 포터블 앱 구조를 사용하여 앱 컴포넌트 관리.
* [DevOps 도구 체인](/docs/services/DevOpsInsights?topic=DevOpsInsights-getting-started#getting-started)의 원클릭 작성.

사용자가 고품질의 프로덕션에 바로 사용할 수 있는 앱을 빠르게 빌드할 수 있도록 {{site.data.keyword.cloud_notm}} 개발자 콘솔이 지원하는 방법을 이해하려면 다음 요소를 자세히 참조하십시오.

## 개발자 콘솔
{: #developer_consoles}

{{site.data.keyword.cloud_notm}} 개발자 콘솔은 관심 영역(예: Watson, 보안 또는 재무) 또는 디지털 채널(예: 모바일 또는 웹 앱)을 표시합니다. [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/dashboard){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")은 Apple 개발자를 위해 개발되었으며, 이를 통해 {{site.data.keyword.cloud_notm}} 플랫폼에서 지원하는 애플리케이션 및 서비스를 빠르게 시험할 수 있습니다. {{site.data.keyword.cloud_notm}} 콘솔의 메뉴 아이콘을 클릭하여 이 콘솔에 액세스할 수 있습니다. 예를 들어, 다음 개발자 콘솔을 참조하십시오.

* [Watson 개발자 콘솔](https://{DomainName}/developer/watson/dashboard){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")
* [모바일 개발자 콘솔](https://{DomainName}/developer/mobile/dashboard){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")
* [웹 앱 개발자 콘솔](https://{DomainName}/developer/appservice/dashboard){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")
* [보안 개발자 콘솔](https://{DomainName}/developer/security/dashboard){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")
* [재무 개발자 콘솔](https://{DomainName}/developer/finance/dashboard){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")

<!--Cloud native development is the process of developing apps that are optimized to leverage capabilities engendered from running on the cloud.  Flexibility, portability, scaling, rapid development, continuous delivery, and a close coupling development and operations ("devops) are characteristics of cloud applications. The {{site.data.keyword.cloud}} developer console quickly gets you started building cloud native applications that are ready for team development and bound for production use.-->


<!--![Overview of elements of the {{site.data.keyword.cloud_notm}} developer console](images/elements_of_devex.png "Overview of elements of the {{site.data.keyword.cloud_notm}} developer console") <br> *Overview of elements of the {{site.data.keyword.cloud_notm}} developer console*-->

각 개발자 콘솔은 해당 콘솔의 집중 영역과 관련된 스타터 킷을 제공하며 작동 가능한 프로덕션에 바로 사용할 수 있는 앱을 몇 분 안에 작성할 수 있도록 일관되고 직관적인 워크플로우를 제공합니다.

## 스타터 킷으로 앱 작성
{: #starterkit-apps}

[스타터 킷](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro)을 활용하여 앱을 구성하는 코드, 데이터, 서비스 및 도구 체인의 연관이 포함된 Swift 앱을 작성할 수 있습니다.
