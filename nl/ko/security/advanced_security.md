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
{:note: .deprecated}
{:tip: .tip} 

# 고급 보안이 적용된 빌드
{: #security}

Swift 개발자는 {{site.data.keyword.cloud}}가 업계에서 가장 강력한 보안 레벨로 저장 상태이거나 사용 중이거나 전송 중인 키 및 데이터를 보호하기 위해 제공하는 고급 보안 서비스를 쉽게 활용할 수 있습니다.
{:shortdesc}

쉬운 접근 방법으로 {{site.data.keyword.cloud}}의 모든 고급 보안 서비스에 대해 {{site.data.keyword.cloud_notm}} HyperSecure Platform Services를 직접 사용할 수 있습니다. 자세한 정보는 [{{site.data.keyword.cloud_notm}} HyperSecure Platform Services 시작하기](/docs/services/hypersecure-platform/index.html){:new_window}를 참조하십시오.

## {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} 사용

{{site.data.keyword.hscrypto}}는 업계에서 가장 강력한 보안 레벨로 키와 데이터에 암호화를 제공하는 시범 서비스입니다. System z의 보안 및 무결성을 클라우드에 제공합니다. 은행과 금융 서비스에서 따르는 동일한 최신 암호화 기술이 이제 {{site.data.keyword.cloud_notm}}를 통해 사용자에게 제공됩니다. 

{{site.data.keyword.hscrypto}}는 네트워크 주소 지정 가능 HSM(Hardware Security Module)을 제공하여 PKCS#11 API(Application Programming Interface)를 통해 안전한 암호화를 제공합니다. 암호화 하드웨어를 위한 가장 높은 보안 레벨 즉, FIPS 140-2 Level 4에 액세스할 수 있습니다. 또한 {{site.data.keyword.hscrypto}}는 IBM의 암호화 코프로세서에 대한 원격 액세스를 사용으로 설정하는 IBM Advanced Crypto Service Provider(ACSP) 솔루션을 활용할 수도 있습니다. 

{{site.data.keyword.hscrypto}}는 {{site.data.keyword.keymanagementserviceshort}} 서비스에 대한 키 저장소입니다. 

{{site.data.keyword.hscrypto}}에 대한 자세한 정보는 [{{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} 시작하기](/docs/services/hs-crypto/index.html){:new_window}를 참조하십시오.

## {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}} 사용

{{site.data.keyword.keymanagementserviceshort}}는 사용자가 {{site.data.keyword.cloud_notm}} 서비스 전체에서 앱을 위한 암호화된 키를 프로비저닝할 수 있도록 지원합니다. 키의 라이프사이클을 관리하면서 정보 도용을 방지하는 클라우드 기반 HSM(Hardware Security Module)으로 키가 보호되는 이점을 누릴 수 있습니다. {{site.data.keyword.hscrypto}}와 함께 사용하면 사용자의 키가 가장 강력한 보안 레벨의 FIPS 140-2 Level 4 인증서로 보호됩니다. 

{{site.data.keyword.keymanagementserviceshort}}에 대한 자세한 정보는 [{{site.data.keyword.keymanagementserviceshort}} 시작하기](/docs/services/keymgmt/index.html){:new_window}를 참조하십시오. 

## {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 사용
{: #hypersecure-dbaas}

{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS는 요청 시 안전한 데이터베이스를 제공하는 시범 {{site.data.keyword.cloud_notm}} 서비스입니다. 이는 유연하고 스케일링 가능한 플랫폼을 제공하여 빠르고 쉽게 MongoDB 데이터베이스를 프로비저닝하고 관리합니다. 

이 서비스를 사용하면 {{site.data.keyword.cloud_notm}}에서 데이터베이스 클러스터를 작성할 수 있습니다. 작성하는 각 데이터베이스 클러스터는 하나의 기본 인스턴스와 두 개의 보조 데이터베이스 인스턴스(기본 인스턴스에 대한 복제복의 역할을 함)로 구성됩니다. {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 로직은 데이터베이스 클러스터에서 MongoDB 데이터베이스를 작성하고 여기에 액세스합니다.

### 데이터 및 개인정보 보호

{{site.data.keyword.IBM_notm}}은 고가용성의 안전한 환경에서 데이터베이스를 호스팅합니다.
 * 데이터는 저장 시 및 전송 중에 암호화됩니다.
 * 시스템 하드웨어, 시스템 구성 및 데이터베이스 설정은 고가용성을 보장합니다.
 * 기본 기술은 {{site.data.keyword.IBM_notm}} 또는 써드파티가 사용자의 데이터에 액세스할 수 없도록 합니다. 

### 애플리케이션에 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS 로직 추가

애플리케이션에서 MongoDB 데이터베이스를 사용하려면
[데이터베이스 사용 시작하기](../hypersecure_dbaas/database-cluster.html)를 참조하십시오.  

### {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS에 대해 자세히 알아보기

{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS에 대해 자세히 알아보려면 [IBM Cloud Hyper Protect DBaaS 시작하기](/docs/services/hyper-protect-dbaas/index.html)를 참조하십시오.

## {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscontainers}} 사용

{{site.data.keyword.hscontainers}}는 Docker 및 Kubernetes 기술, 직관적인 사용자 경험, 기본 내장된 보안 및 격리를 결합하여 컴퓨팅 호스트의 클러스터에서 컨테이너화된 앱의 배치, 오퍼레이션, 스케일링 및 모니터링을 자동화하는 강력한 도구를 제공합니다. 

{{site.data.keyword.hscontainers}}는 스폰서 유저(sponsor user)만 사용할 수 있습니다. 전용 보안 지원을 원하는 경우 [IBM Z Client Feedback Program](https://www-01.ibm.com/marketing/iwm/iwmdocs/web/cc/earlyprograms/zcustomer.shtml)에서 스폰서 유저로 등록하여 애플리케이션을 {{site.data.keyword.hscontainers}} 클러스터에 배치하십시오.
{: tip}

스폰서 유저가 되기 전에 {{site.data.keyword.containershort_notm}}를 사용하여 애플리케이션을 보호할 수 있습니다. 자세한 정보는 [{{site.data.keyword.containershort_notm}} 시작하기](/docs/containers/container_index.html#container_index)를 참조하십시오.
