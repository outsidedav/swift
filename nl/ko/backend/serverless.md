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
{:note:.deprecated}

# 서버리스 개발
{: #serverless}

서버리스(serverless)란 무엇입니까? 서버리스 개발 패턴은 서버 측 로직이 이벤트로 트리거되고, 일시적이며(한 번의 실행으로 지속됨), 써드파티에 의해 완전히 관리되는 Stateless 컨테이너에서 실행되는 애플리케이션 개발을 말합니다. FaaS(Functions as a Service)로도 알려져 있는 이 패러다임에서는 개발자가 명시적으로 서버를 프로비저닝하거나 관리하지 않은 상태에서 트리거되고 실행될 수 있는 Stateless 기능을 제공합니다. 

서버 측 개발에 필요한 인프라 및 프레임워크를 추상화하여, 서버리스 아키텍처를 통해 개발자는 자신의 애플리케이션을 빌드하고 반응적으로 실행하여 데이터를 변경하는 코드를 작성하는 데 중점을 둘 수 있습니다. 

IBM의 FaaS 오퍼링인 [{{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/)는 서버 측 전문 지식을 필요로 하지 않으면서 완벽한 서버 측 개발 경험을 제공하는 데 주력합니다. 서버리스 기술을 통해 사용자는 스케일링 가능한 백엔드 솔루션을 빠르게 개발하여 미리 리소스를 프로비저닝할 필요 없이 실질적으로 워크로드 요구를 충족시킬 수 있습니다. 예측 불가능한 로드 패턴 또는 높은 서버 작동 중지 시간이 있는 애플리케이션의 경우 {{site.data.keyword.openwhisk_short}}는 향상된 성능을 갖춘 뛰어난 클라우드 솔루션이 될 수 있으며 "종량과금제" 시스템을 통해 비용을 줄일 수 있습니다. 

## 아키텍처 변경
{: #comparison}

사용자가 FaaS로 전환하여 얻을 수 있는 아키텍처 이점을 이해하는 데 도움이 되도록 데이터베이스에 연결된 단순 iOS 애플리케이션을 사용하여 일반적인 FaaS 아키텍처를 비교합니다.

보다 일반적인 아키텍처에서 iOS 애플리케이션은 네트워크 집중 태스크를 오프로드하거나 고유한 서비스 또는 스토리지 옵션으로 자체적으로 연결된 중앙 위치에서 원격으로 데이터를 처리합니다. 전통적인 시스템의 경우 어려운 작업은 인증, 처리 집중 태스크를 처리하는 단일 서버에 배치되어 클라이언트에 대한 스트레스를 최소화하고 사용자 기반 전반에 동기화를 제공합니다.

서버리스 아키텍처는 다음 이미지와 더욱 유사하게 보이도록 이 구조를 변경할 수 있습니다.

![](./images/Architecture.png) 그림 1. 서버리스 아키텍처

단일 서버 내에서 모든 처리 및 인증 로직을 처리하는 대신, 서버리스 아키텍처는 많은 서버 측 로직을 캡슐화하는 확장성이 높은 기능들을 활용하며 일부 로직을 클라이언트(및 외부 서비스)에 오프로드합니다.

Schematic을 보면 다음 사항을 확인할 수 있습니다.

1. 클라이언트는 ID 제공자(예: 앱 ID)를 비교하여 인증합니다.
2. 액세스 토큰을 포함하여 FaaS 백엔드 API를 호출합니다.
3. 백엔드는 {{site.data.keyword.openwhisk_short}}로 구현됩니다. 웹 조치로 노출되는 서버리스 조치는 요청 헤더에서 토큰을 전송할 것으로 예상하고 실제 API에 대한 액세스를 제공하도록 유효성(서명 및 만기 날짜)을 검증합니다.
4. 클라이언트가 데이터를 제출하면 피드백은 {{site.data.keyword.cloudant_short_notm}}에 저장됩니다.
5. 피드백 텍스트는 {{site.data.keyword.toneanalyzershort}}로 처리됩니다.
6. 분석 결과에 따라 알림은 {{site.data.keyword.mobilepushshort}}을 통해 다시 클라이언트에게 전송됩니다.
7. 클라이언트가 알림을 수신합니다.

순수 서버리스 모델에서는 사용자 상태를 저장할 수 없기 때문에 클라이언트에 추가 책임이 부여되기도 합니다. 예를 들어, 이 경우 권한 부여는 클라이언트 및 {{site.data.keyword.appid_short_notm}} ID 제공자 서비스로 처리됩니다.

서비리스 아키텍처가 항상 이상적이지는 않지만, 적절한 팀과 사용 조건 하에서는 실질적인 이점을 제공할 수 있습니다. 가장 일반적인 [유스 케이스](#use_cases) 몇 가지에 대해 자세히 알아보려면 몇 가지 구체적인 예제를 확인하십시오.

## 서버리스의 이점
{: #benefits}

### 비용 절감

시스템 관리와 연관된 시간 및 비용을 아웃소싱하면 일반적인 백엔드 서버와 연관된 전체 비용을 줄일 수 있습니다. 또한 코드가 요청을 충족시키는 데 걸린 시간에 대해서만 가장 근접한 100ms로 반올림하여 비용을 지불하면 되므로 {{site.data.keyword.openwhisk_short}}는 기존의 일반적인 컴퓨팅 기술과는 다릅니다. 100% 활용될 가능성이 없지만 클라우드 제공자의 시스템에서 메모리를 차지하는 다른 기술(예: VM 및 컨테이너)에 비해 비용을 크게 절감할 수 있습니다. 

### 고가용성 및 확장성

서버리스 아키텍처는 거의 지속적인 가용성과 함께 즉각적인 확장성을 제공합니다.

### 속도 및 개발 간소화

시스템을 관리할 필요 없이 개발을 위한 단순 인터페이스를 제공하여, 서버리스 패러다임은 애플리케이션 개발을 가속화합니다. 개발자는 이벤트 중심 환경에 응답하여 실행되는 조치 시퀀스로 앱을 빠르게 빌드할 수 있습니다.

## 유스 케이스 예제
{: #use_cases}

### 모바일 백엔드
![](./images/cloud-functions-rest-api-trigger.png)

모바일 개발자는 쉽게 서버 측 로직에 액세스하고 컴퓨팅 집중 태스크를 스케일링 가능한 클라우드 플랫폼으로 아웃소싱할 수 있습니다. Swift와 같은 언어로 기능을 구현하고 필요한 서버 측 경험 없이 iOS SDK를 사용하여 서버 측 기능을 쉽게 이용할 수 있습니다. 

### 데이터 처리

![](./images/cloud-functions-cloudant-trigger.png)

내장 트리거를 통해 데이터가 데이터 저장소에서 업데이트될 때마다 코드를 실행할 수 있습니다. 오디오 표준화, 이미지 회전, 선명 효과, 소음 감소, 썸네일 생성 또는 비디오 트랜스코딩과 같은 프로세스를 쉽게 자동화할 수 있습니다.

### 코그너티브 데이터 처리

데이터를 사용할 수 있게 되면 분석할 수 있습니다. 기능에서 IBM Watson과 같이 강력한 코그너티브 서비스를 활용하여 이미지 또는 비디오에서 오브젝트 또는 사용자를 감지하도록 하십시오.

### 스케줄된 태스크

주기적으로 기능을 실행하고, Cron 형식의 구문을 따르는 스케줄을 정의하여 조치가 실행되어야 할 때를 지정할 수 있습니다.

## API 참조
{: #openwhisk_start_api notoc}

<!-- * [REST API Documentation](./openwhisk_reference.html#openwhisk_ref_restapi)-->
* [REST API](https://console.{DomainName}/apidocs/98)

## 관련 링크
{: #general notoc}

* [검색: {{site.data.keyword.openwhisk_short}}](http://www.ibm.com/cloud-computing/bluemix/openwhisk/)
<!-- redirects to link above * [{{site.data.keyword.openwhisk_short}} on IBM developerWorks](https://developer.ibm.com/openwhisk/)-->
* [Apache OpenWhisk 프로젝트 웹 사이트](http://openwhisk.org)
* [서버리스에 대해 알아보기](https://martinfowler.com/articles/serverless.html)
