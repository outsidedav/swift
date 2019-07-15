---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-07"

keywords: develop swift, swift local, service credentials swift, developer tools swift, swift cli, ibmcloud build swift, ibmcloud swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 로컬로 개발
{: #develop-locally}

로컬로 개발하여 Swift 앱을 쉽게 빌드, 실행 및 테스트할 수 있습니다. {{site.data.keyword.dev_cli_short}}를 사용하여 표준 명령을 통해 이 조치를 수행할 수 있습니다. 

{{site.data.keyword.dev_cli_short}}를 사용하여 12개가 넘는 명령을 통해 서버 측 애플리케이션을 관리할 수 있습니다. [{{site.data.keyword.dev_cli_notm}} CLI `ibmcloud dev` 명령](/docs/cli/idt?topic=cloud-cli-idt-cli)에서 다양한 명령에 대해 자세히 알아보십시오.

## 시작하기 전에
{: #prereqs-local}

로컬로 개발하려면 {{site.data.keyword.dev_cli_notm}}를 설치해야 합니다. 다음 명령을 실행하여 설치 스크립트를 실행하십시오.
```
curl -sL https://ibm.biz/idt-installer | bash
```
{: codeblock}

{{site.data.keyword.dev_cli_notm}}의 구성 및 사용에 대해 자세히 알아보려면 [{{site.data.keyword.dev_cli_notm}} CLI 설정](/docs/cli?topic=cloud-cli-getting-started)을 참조하십시오

## 서비스 인증 정보 검색
{: #credentials-local}

Git에서 애플리케이션을 복제한 후에는, 서비스의 인증 정보가 사용자 애플리케이션에 적합한 Git 저장소에 저장되지 않으므로 애플리케이션에 바인드된 서비스의 인증 정보를 검색해야 합니다. 인증 정보를 검색하면 바인드된 서비스를 사용할 수 있습니다. 애플리케이션 디렉토리의 루트에서 다음 명령을 실행하여 인증 정보를 쉽게 다운로드할 수 있습니다.
```
ibmcloud dev get-credentials
```
{: codeblock}

## 애플리케이션 빌드, 실행 및 배치
{: #build-deploy-local}

1. **빌드** - 이제 애플리케이션 실행을 위한 전제조건인 애플리케이션을 빌드할 수 있습니다.
  애플리케이션 디렉토리의 루트에서 다음 명령을 사용하여 앱을 빌드하십시오.
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **실행** - 빌드를 완료한 후 다음 명령을 사용하여 로컬 컨테이너에서 애플리케이션을 실행할 수 있습니다.
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  명령이 실행되면 애플리케이션의 랜딩 페이지를 볼 수 있는 로컬 호스트 및 포트가 표시됩니다.

3. **배치** - `deploy` 명령을 사용하여 애플리케이션을 {{site.data.keyword.cloud_notm}}에 배치하십시오.
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}
