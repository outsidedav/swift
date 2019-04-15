---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: swift sdk plug-in, sdk generator swift, generated sdk swift, devops pipeline swift, open api swift, sdkgen swift, ibmcloud sdk swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 생성된 SDK를 사용하여 앱에 백엔드 서비스 추가
{: #sdk-cli}

{{site.data.keyword.IBM}} SDK Generator 플러그인을 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)에 설치할 수 있습니다.

{{site.data.keyword.IBM_notm}} SDK Generator 플러그인을 통해 생성된 SDK를 사용하여 백엔드 서비스를 앱에 통합할 수 있습니다. REST API가 변경되면 SDK를 다시 생성하고 SDK를 쉽게 업그레이드하기 위해 이전 SDK를 대체할 수 있습니다. 그런 다음 CLI를 DevOps 파이프라인에 추가하고, 앱이 빌드될 때마다 SDK가 항상 API 스펙과 일치하는지 확인할 수 있습니다.

REST API 정의는 유효해야 하며 라이브 서버 엔드포인트에서 호스팅되거나 사용자 시스템의 로컬 파일이어야 합니다.

## 시작하기 전에
{: #prereqs-sdk-cli}

다음 전제조건을 갖추었는지 확인하십시오.

* [{{site.data.keyword.cloud_notm}}](http://cloud.ibm.com){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘") 계정
* [Open API ](https://www.openapis.org/){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘") 스펙을 준수하는 유효한 API 정의

## SDK 플러그인 설치
{: #install-sdk-cli}

1. [{{site.data.keyword.cloud_notm}} CLI를 설치](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)하십시오.

2. SDK 플러그인을 설치하십시오.
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

[{{site.data.keyword.dev_cli_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#install_plug-in)을 설치할 수 있습니다. 여기에는 기본 {{site.data.keyword.cloud_notm}} CLI가 포함되어 있으며, 기타 유용한 로컬 도구와 함께 `sdk-gen` 플러그인도 포함되어 있습니다.
{: tip}

## SDK 생성
{: #commands-sdk-cli}

다음을 입력하여 SDK를 생성하십시오.
```
ibmcloud sdk generate [arguments...] [command options]
```
{: codeblock}

### 인수
{: #gen-args-sdk-cli}

* **APP_NAME** - 현재 공간에 있는 Cloud Foundry 앱의 이름
* **OPENAPI_DOC_LOCATION** - 원시 REST API 정의 JSON 또는 yaml에 대한 URL 또는 상대 파일 경로
* **GENERATED_SDK_NAME**(선택사항) - 생성된 SDK의 이름

### 옵션
{: #gen-options-sdk-cli}

**플랫폼**(필수):
  * `--ios` - iOS Swift SDK 생성
  * `--swift` - Swift 서버 SDK 생성
  * `--js` - JavaScript SDK 생성

**위치**(필수):

`OPENAPI_DOC_LOCATION` 인수의 유형을 지정합니다.

  * `-r` - 원격 URL
  * `-f` - 파일 이름
  * `-a` - {{site.data.keyword.cloud_notm}}에서 실행되는 앱
  * `-l` - localhost URL

**선택사항**:
  * `--output "YOUR_RELATIVE_PATH"` - `YOUR_RELATIVE_PATH`로 지정되는 디렉토리에 생성된 SDK를 배치합니다(존재하는 경우 기존 SDK를 겹쳐씀).
  * `--unzip` - 생성된 SDK를 추출합니다(SDK 아티팩트가 존재하는 경우 기존 데이터를 겹쳐씀).

### 사용법
{: #gen-usage-sdk-cli}

{{site.data.keyword.cloud_notm}}에서 실행되는 Cloud Foundry에서 SDK를 생성하기 위해 CLI에 대한 매개변수로 앱의 이름을 사용할 수 있습니다. 다음 명령은 `SDK_Name`으로 앱의 이름을 사용합니다.

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

Open API 정의 파일 또는 로컬 JSON 또는 yaml 파일에 대한 URL에서 SDK를 생성하려면 다음 명령을 사용하십시오.

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}


## Open API 정의 유효성 검증
{: #validating-sdk-cli}

`ibmcloud sdk validate [argument]` 명령을 실행하십시오.

### 인수
{: #val-args-sdk-cli}

* `APP_NAME` - 현재 공간에 있는 Cloud Foundry 앱의 이름
* `OPENAPI_DOC_LOCATION` - 원시 REST API 정의 JSON 또는 yaml에 대한 URL 또는 상대 파일 경로

### 사용법
{: #val-usage-sdk-cli}

{{site.data.keyword.cloud_notm}}에서 실행되는 Cloud Foundry 앱의 API 스펙을 유효성 검증하기 위해 CLI에 대한 매개변수로 앱의 이름을 사용할 수 있습니다.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

API 스펙 문서 또는 로컬 JSON 또는 yaml 파일에 대한 URL에서 SDK를 유효성 검증하려면 다음 명령을 사용하십시오.
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

