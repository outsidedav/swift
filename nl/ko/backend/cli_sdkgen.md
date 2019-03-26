---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 생성된 SDK를 사용하여 앱에 백엔드 서비스 통합
{: #sdkgen-cli}

{{site.data.keyword.IBM}} SDK Generator 플러그인을 [{{site.data.keyword.cloud_notm}} CLI ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli/index.html){: new_window}에 설치할 수 있습니다.

이 {{site.data.keyword.IBM_notm}} SDK Generator 플러그인은 생성된 서비스를 사용하여 앱에 백엔드 서비스를 통합합니다. REST API가 변경되면 SDK를 다시 생성하고 SDK를 쉽게 업그레이드하기 위해 이전 SDK를 대체할 수 있습니다. 또한 CLI를 DevOps 파이프라인과 통합하고, 앱이 빌드될 때마다 SDK가 항상 API 스펙과 일치하는지 확인할 수 있습니다.

REST API 정의는 유효해야 하며 라이브 서버 엔드포인트에서 호스팅되거나 사용자 시스템의 로컬 파일이어야 합니다.

## 시작하기 전에
{: #prereqs-sdkgen}

다음 항목이 필요합니다.

* [{{site.data.keyword.Bluemix_notm}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://cloud.ibm.com){: new_window} 계정
* [Open API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.openapis.org/){: new_window} 스펙을 준수하는 유효한 API 정의

## SDK 플러그인 설치
{: #install-sdkgen}

1. [{{site.data.keyword.Bluemix}} CLI](/docs/cli/reference/bluemix_cli/get_started.html)를 설치하십시오.

2. [플러그인을 설치![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli/index.html#install_plug-in){: new_window}하십시오.

  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## SDK 생성
{: #commands-sdkgen}

다음을 입력하여 SDK를 생성하십시오. `ibmcloud sdk generate [arguments...] [command options]`

### 인수
{: #gen-args-sdkgen}

* `APP_NAME` - 현재 공간에 있는 Cloud Foundry 앱의 이름
* `OPENAPI_DOC_LOCATION` - 원시 REST API 정의 JSON 또는 yaml에 대한 URL 또는 상대 파일 경로
* `GENERATED_SDK_NAME`(선택사항) - 생성된 SDK의 이름

### 옵션
{: #gen-options-sdkgen}

* `PLATFORM`(필수)
   * `--ios` - iOS Swift SDK 생성
   * `--swift` - Swift 서버 SDK 생성
   * `--js` - JavaScript SDK 생성
* `LOCATION`(필수) - `OPENAPI_DOC_LOCATION`의 유형 지정
   * `-r` - 원격 URL
   * `-f` - 파일
   * `-a` - {{site.data.keyword.Bluemix_notm}}에서 실행되는 앱
   * `-l` - localhost URL
* `--output "YOUR_RELATIVE_PATH"`(선택사항) - `YOUR_RELATIVE_PATH`로 지정되는 디렉토리에 생성된 SDK를 배치합니다(기존 SDK를 겹쳐씀).
* `--unzip`(선택사항) - 생성된 SDK를 추출합니다(기존 SDK를 겹쳐씀).

### 사용법
{: #gen-usage-sdkgen}

{{site.data.keyword.Bluemix_notm}}에서 실행되는 Cloud Foundry에서 SDK를 생성하기 위해 CLI에 대한 매개변수로 앱의 이름을 사용할 수 있습니다. 다음 명령은 `SDK_Name`으로 앱의 이름을 사용합니다.

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
{: #validating-sdkgen-sdkgen}

다음 명령을 실행하십시오.
```
ibmcloud sdk validate [argument]
```
{: codeblock}

### 인수
{: #val-args-sdkgen}

* `APP_NAME` - 현재 공간에 있는 Cloud Foundry 앱의 이름
* `OPENAPI_DOC_LOCATION` - 원시 REST API 정의 JSON 또는 yaml에 대한 URL 또는 상대 파일 경로

### 사용법
{: #val-usage-sdkgen}

{{site.data.keyword.Bluemix_notm}}에서 실행되는 Cloud Foundry 앱의 API 스펙을 유효성 검증하기 위해 CLI에 대한 매개변수로 앱의 이름을 사용할 수 있습니다.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

API 스펙 문서 또는 로컬 JSON 또는 yaml 파일에 대한 URL에서 SDK를 유효성 검증하려면 다음 명령을 사용하십시오.
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}
