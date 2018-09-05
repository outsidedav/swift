---

copyright:
  years: 2018
lastupdated: "2018-08-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 스타터 킷을 사용하여 Swift 앱 작성
{: #intro}

{{site.data.keyword.cloud_notm}} Developer Console for Apple을 통해 개발자는 여러 스타터 킷에서 앱을 작성하고, 키 {{site.data.keyword.cloud_notm}} 최적화 서비스를 프로비저닝하고 연결할 수 있습니다. 사용자는 앱을 작성, 보기, 구성 및 관리하고 앱의 코드를 다운로드할 수 있습니다. 스타터 킷을 사용하면 신속하게 평가하고 새 앱으로 {{site.data.keyword.cloud_notm}} 서비스를 평가하고 테스트할 수 있습니다. 

시작하시겠습니까? [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits)에 방문하여 시작하십시오.
{: tip}

## 스타터 킷의 개념
{: #starter_kits}

{{site.data.keyword.cloud_notm}} Developer Experience를 사용하여 여러 스타터 킷에서 선택할 수 있습니다. 스타터 킷은 {{site.data.keyword.cloud_notm}}에게 원하는 언어로 클라우드 배치 준비가 완료된 스켈레톤 프로덕션 앱을 동적으로 어셈블할 것을 지시합니다. 각 스타터 킷은 특정한 실제 유스 케이스에 언어, 프레임워크 및 패턴을 구현하여 코드를 다시 생성하지 않고 재사용하도록 합니다. 

스타터 킷은 프로덕션에 바로 사용할 수 있으며 런타임(예: Swift)을 사용하여 핵심 패턴 구현을 시연하는 데 중점을 둡니다. 일부 경우 스타터 킷은 서비스의 통합을 강조표시하는 단순 사용자 경험을 제공합니다. 다른 경우 스타터 킷은 정교한 유스 케이스의 사용자 정의할 수 있는 구현을 나타냅니다. 

스타터 킷에는 {{site.data.keyword.cloud_notm}}가 포터블 코드를 사용하여 자동으로 스캐폴드된 앱을 생성할 수 있고 스타터 킷에서 앱을 생성할 때 자동 프로비저닝될 리소스를 지정할 수 있는 지시사항이 있습니다. 

## {{site.data.keyword.cloud_notm}} Developer Console for Apple 사용
{: #journey}

{{site.data.keyword.cloud_notm}} Developer Console for Apple은 특정 유스 케이스에 대한 Swift 스타터 앱을 빌드할 완벽한 경로를 제공합니다. 과정에서 수행할 단계를 살펴보십시오. 

### 개요 화면
{: #overview_screen}

개요 화면은 Watson, 날씨와 같은 유스 케이스 세트에 맞게 사용자 조정된 컨텐츠를 제공합니다. 개요 화면에서 문서를 참조하거나, 교육 리소스에 액세스하거나, 서비스를 찾아보거나, 기능을 갖춘 스타터 킷을 참조하거나, 대형 콜렉션에 연결할 수 있습니다. 왼쪽 탐색 영역에서 `Starter Kits`를 클릭하여 스타터 킷 보기로 이동하십시오.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple 개요 화면](images/overview_screen.png "개요 화면") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple 개요 화면*

### 스타터 킷 보기
{: #starter_kits_view}

스타터 킷 보기는 유스 케이스 영역에 특정한 스타터 킷의 콜렉션을 표시합니다. 스타터 킷 카드에서 다양한 링크를 클릭하여 데모 및 자세한 정보를 볼 수 있습니다. 앱 작성 보기로 이동할 스타터 킷을 선택하십시오.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple 스타터 킷 보기](images/starter_kits_screen.png "스타터 킷 보기") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple 스타터 킷 보기*

### 앱 작성 보기
{: #create_new_app_view}

**앱 작성** 보기에서 앱의 이름을 지정하고 배치 및 라우팅 정보를 제공할 수 있습니다. 오른쪽에서 앱을 작성할 때 자동으로 프로비저닝되는 서비스(가격 책정 플랜 및 각 조항 포함)도 볼 수 있습니다. `Create`를 클릭하여 앱 세부사항 보기로 이동하십시오. {{site.data.keyword.cloud_notm}}에 로그인하지 않은 경우 지금 로그인해야 합니다.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple 새 앱 작성 보기](images/create_new_project_screen.png "새 작성 보기") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple 새 앱 작성 보기*

## 앱 세부사항 보기
{: #app_details_view}

앱 세부사항 보기는 앱에 구성된 서비스의 목록을 표시합니다. 목록의 각 항목마다 서비스 이름, 기타 정보의 링크 및 수직으로 정렬된 세 개의 점이 있는 **조치** 단추를 볼 수 있습니다. **조치** 단추 옵션을 사용하여 앱에서 서비스를 제거하고, 서비스에 대한 대시보드를 열고, 서비스를 삭제할 수 있습니다. 서비스 인스턴스를 제거하면 이 앱과의 연관이 제거되고 서비스 인스턴스가 삭제되지 않습니다. 또한 이 보기에서 서비스 신임 정보가 통합되므로 서비스 신임 정보를 가져오기 위해 각 개별 서비스 인스턴스 보기를 방문할 필요가 없습니다. 

![{{site.data.keyword.cloud_notm}} Developer Console for Apple 앱 세부사항 보기](images/project_details_screen.png "앱 세부사항 보기") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple 앱 세부사항 보기*

앱 세부사항 보기를 사용하여 이전 스타터 킷에 포함되지 않은 앱에 신규 또는 기존 서비스를 추가할 수 있습니다. 서비스 목록 상자 추가 서비스에서 **리소스 추가** 링크를 클릭하십시오. 사용 가능한 서비스는 앱의 유형과 지역에서 사용할 수 있는 서비스에 따라 달라지므로 모든 앱과 연관되도록 모든 서비스를 사용할 수 있는 것은 아닙니다. 

![{{site.data.keyword.cloud_notm}} Developer Console for Apple 리소스 추가 대화 상자](images/add_resource_screen.png "리소스 추가 대화 상자") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple 리소스 추가 대화 상자*

앱 세부사항 보기에서 두 가지 방법으로 코드에 액세스할 수 있습니다.
*  생성할 **코드 다운로드**를 선택하고 앱에 적합한 코드를 다운로드하십시오.

### 앱 목록 보기
{: #app_list_view}

앱 목록 보기에서 작성된 모든 앱을 나열할 수 있습니다. 여기서 앱의 이름을 바꾸거나 앱을 삭제할 수 있습니다. 앱 세부사항 보기로 돌아가려면 앱 이름 행을 클릭하십시오. 

![{{site.data.keyword.cloud_notm}} Developer Console for Apple 앱 목록 보기](images/project_list_screen.png "앱 목록 보기") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple 앱 목록 보기*

자세한 정보는 [{{site.data.keyword.cloud_notm}} Developer Console for Apple 학습 리소스](https://console.bluemix.net/developer/appledevelopment/learning-resources)를 참조하십시오.
{: tip}
