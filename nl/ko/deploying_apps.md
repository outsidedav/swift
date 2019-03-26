---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Swift 앱 배치 및 통합
{: #deploy_apps-swift}

도구 체인으로 또는 명령행 인터페이스를 사용하여 Swift 앱을 배치할 수 있습니다. 도구 체인은 도구 통합 세트입니다. 명령행 인터페이스는 앱 및 서비스 인스턴스를 배치할 수 있는 간단한 방법입니다.

자세한 정보는 [앱 배치](/docs/apps/dep-app-tool.html#deploying-apps)를 참조하십시오.

## 서버리스 Swift 앱 개발
{: #serverless-swift}

서버리스 개발을 위해 IBM의 FaaS(Functions as a Service) 오퍼링인 {{site.data.keyword.openwhisk}}를 사용할 수 있습니다. {{site.data.keyword.openwhisk_short}}를 통해 서버를 작성하거나 관리하지 않고 HTTP를 통해 웹 또는 모바일 앱에서 이벤트 또는 직접 호출에 응답하여 애플리케이션 로직을 실행할 수 있습니다.

{{site.data.keyword.openwhisk_short}}는 개발자가 애플리케이션 로직 작성에 계속 집중할 수 있도록 Auto-Scaling, 가용성 관리 및 유지보수와 같은 시스템 관리를 수행합니다.

자세한 정보는 [서버리스 앱 개발](/docs/apps/deploying/functions.html#serverless)을 참조하십시오.

## 생성된 SDK를 사용하여 백엔드 서비스 통합
{: #backend_gensdk-swift}

이 {{site.data.keyword.IBM_notm}} SDK Generator 플러그인은 생성된 서비스를 사용하여 앱에 백엔드 서비스를 쉽게 통합합니다. REST API가 변경되면 SDK를 다시 생성하고 SDK를 업그레이드하기 위해 이전 SDK를 대체할 수 있습니다. 또한 CLI를 DevOps 파이프라인과 통합하고, 앱이 빌드될 때마다 SDK가 항상 API 스펙과 일치하는지 확인할 수 있습니다.

자세한 정보는 [생성된 SDK를 사용하여 백엔드 서비스 통합](/docs/swift/backend/cli_sdkgen.html#sdk-cli)을 참조하십시오.

## Kubernetes 클러스터에 배치
{: #deploy_kube-swift}

Watson Tone Analyzer를 활용하는 컨테이너화된 Node.js 앱을 배치하기 위해 {{site.data.keyword.cloud_notm}} Kubernetes Service를 사용하는 방법을 알아볼 수 있습니다. 제공된 시나리오에서 가상의 PR 회사는 {{site.data.keyword.cloud_notm}} 서비스를 사용하여 보도 자료를 분석하고 메시지의 톤에 대한 피드백을 받습니다. 자세한 정보는 [Kubernetes 클러스터에 앱 배치](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial) 튜토리얼을 참조하십시오.

## Cloud Foundry에 배치
{: #swift-deploy-cf}

이 옵션으로 기본 인프라를 관리하지 않아도 클라우드 기반 앱이 배치됩니다. 

앱을 [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry/index.html#about)에 배치할 계획인 경우 [{{site.data.keyword.cloud_notm}} 계정을 준비](/docs/cloud-foundry/prepare-account.html#prepare)해야 합니다.

계정에 {{site.data.keyword.cfee_full_notm}}에 대한 액세스 권한이 있는 경우 **[퍼블릭 클라우드](/docs/cloud-foundry-public/about-cf.html#about-cf)** 또는 **[엔터프라이즈 환경](/docs/cloud-foundry-public/cfee.html#cfee)** 중에서 배치자 유형을 선택하여 사용자 엔터프라이즈 전용의 Cloud Foundry 애플리케이션을 호스팅하기 위한 격리된 환경을 작성하고 관리할 수 있습니다. 

## Virtual Server에 배치
{: #virtual_deploy-swift}

가상 서비스는 모든 워크로드 유형에 대해 보다 높은 수준의 투명성, 예측 가능성 및 자동화를 제공합니다. Terraform은 인프라를 안전하고 효율적으로 빌드, 변경 및 버전화하는 데 사용됩니다.

자세한 정보는 [Virtual Server에 배치](/docs/apps/vsi-deploy.html#vsi-deploy)를 참조하십시오.
