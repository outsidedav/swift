---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# 서비스 기능으로 모바일 백엔드
{: #mbaas}

애플리케이션은 두 가지 접근 방식 중 하나를 통해 외부 기능을 사용할 수 있습니다.
* 애플리케이션과 해당 서비스 간의 직접적인 사용. MBaaS(Mobile Backend as a Service) 플랫폼의 일부로 호스팅됩니다.
* BFF(Backend For Frontend) 컴포넌트를 통한 사용. 컴포지션을 제공하고 서비스를 제어할 수 있으며 애플리케이션에 대한 백엔드 로직을 추가할 수 있습니다.

일반적으로 MBaaS 스타일 접근 방식을 통해 모바일 애플리케이션에서 외부 기능을 직접 추가하는 것이 좀 더 간단합니다. 첫 번째 서비스 세트를 추가하기 위해 필요한 항목 및 인프라 수가 줄어듭니다. 그러나 이 방법에는 최고의 유연성 및 BFF 배치를 통해 가능한 범위가 부족합니다.

{{site.data.keyword.cloud_notm}} 플랫폼의 이점 중 하나는 미리 결정할 필요가 없다는 것입니다. 제공된 Swift 기반 SDK를 사용하여 모바일 디바이스에서 제공된 모든 서비스를 직접 사용할 수 있습니다. {{site.data.keyword.cloud_notm}} 플랫폼은 {{site.data.keyword.openwhisk_short}}가 사용된 서버리스 모델을 통하거나 Kitura와 같은 프레임워크가 사용된 서버 측 Swift를 통해 Swift를 사용한 백엔드 로직 작성도 지원합니다. 이 기능으로 인해, BFF 모델을 통해 백엔드 로직을 추가하려는 경우 Swift를 사용하여 수행할 수 있고 애플리케이션과 BFF 간의 Swift SDK 사용을 마이그레이션하도록 선택할 수 있습니다. 결과적으로 최소한의 추가 노력으로 MBaaS 스타일 접근 방식에서 BFF 접근 방식으로 점진적으로 이동할 수 있습니다.
