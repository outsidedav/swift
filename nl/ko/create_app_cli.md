---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: server-side swift, swift cli, swift dependency, swift commands app, create app swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# CLI로 서버 측 Swift 앱 작성
{: #swift_cli}

{{site.data.keyword.cloud}}는 Swift 개발자와 애플리케이션에 고객이 요구하는 보안, AI 및 가치가 포함되는 솔루션 및 서비스를 제공합니다. 광범위한 포트폴리오의 오퍼링 및 SDK를 사용하여 이 서비스를 이용하고 최첨단 애플리케이션을 시장에 신속하게 출시할 수 있습니다.
{: shortdesc}

다음 안내서를 참조하여 서버 측 Swift 앱을 빌드하고 로컬로 실행하고 배치할 수 있습니다. 표준 명령으로 이러한 조치를 실행하려면 {{site.data.keyword.dev_cli_long}} 사용 방법을 알아보십시오.

{{site.data.keyword.dev_cli_short}}를 사용하여 12개가 넘는 명령을 통해 서버 측 애플리케이션을 관리할 수 있습니다. [{{site.data.keyword.dev_cli_notm}} CLI](/docs/cli/idt?topic=cloud-cli-idt-cli#idt-cli)에서 `ibmcloud dev` 명령에 대해 알아보십시오.

## 1단계. 개발자를 위한 요구사항
{: #prereqs-swift-cli}

{{site.data.keyword.cloud_notm}}를 시작하려면 다음 요구사항을 충족하는지 확인하십시오.

### 운영 체제
{: #swift-cli-os-reqs}

최신 MacOS 지원 하드웨어를 사용하고 최신 iOS 릴리스로 테스트하여 Swift 앱을 개발하는 것이 가장 좋은 방법입니다. [Apple Developer](https://developer.apple.com/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 계정에 가입하여 실제 디바이스에서 테스트할 수 있습니다.

### iOS 및 Xcode
{: #swift-cli-ios_xcode}

- [Xcode 8+(이상) 설치](https://developer.apple.com/xcode/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")
- [iOS 디바이스 8(이상)에 배치](https://support.apple.com/downloads/ios){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")
- Apple에 앱을 제출하기 전에 [앱 스토어 제출 가이드라인](https://developer.apple.com/app-store/guidelines/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")을 검토하십시오.

### SDK 및 종속성 관리
{: #swift-cli-sdk-dependency}

다음 도구를 통해 다양한 {{site.data.keyword.cloud_notm}} 서비스에서 작동하도록 네이티브 SDK를 설치할 수 있습니다.

- [IBM Cloud SDK를 위한 CocoaPods 설치](https://cocoapods.org/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Carthage 설치 지원을 위한 Homebrew 설치](https://brew.sh/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Watson SDK를 위한 Carthage 설치](https://github.com/Carthage/Carthage){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")
  ```
  brew install carthage
  ```
  {: codeblock}

## 2단계. 로컬 개발을 위한 도구 설치
{: #swift-cli-install-tools}

{{site.data.keyword.cloud}}는 {{site.data.keyword.cloud_notm}}의 다양한 측면에서 작업하는 데 도움이 되는 로컬 CLI 도구를 제공합니다. 자세한 정보는 [{{site.data.keyword.dev_cli_long}} 정보](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)를 참조하십시오. 클라우드 배치 전에 로컬 Docker 이미지에서 Swift 백엔드를 테스트하기 위해 도구를 사용할 수 있습니다.

* MacOS 및 Linux의 경우 다음 명령을 실행하십시오.
  ```
  curl -sL http://ibm.biz/idt-installer | bash
  ```
  {: codeblock}

* Windows 10의 경우 관리자로 다음 명령을 실행하십시오.
  ```
  Set-ExecutionPolicy Unrestricted; iex(New-Object Net.WebClient).DownloadString('http://ibm.biz/idt-win-installer')
  ```
  {: codeblock}

  Windows PowerShell 아이콘을 마우스 오른쪽 단추로 클릭하고 **관리자로 실행**을 선택하십시오.
  {: tip}

## 3단계. Swift 앱 작성
{: #create-swift-app-cli}

1. {{site.data.keyword.dev_cli_short}} CLI에서 `ibmcloud dev create` 명령을 실행하여 사전 구성된 스타터를 생성하십시오. 
  ```
  ibmcloud dev create
  ```
  {: codeblock}

  프로젝트를 작성하려면 {{site.data.keyword.cloud_notm}} 계정으로 로그인해야 합니다. 처음 사용하는 사용자는 무료 계정으로 [등록 ](https://cloud.ibm.com/registration/?cm_sp=dw-bluemix-_-swift-_-devcenter){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")할 수 있습니다. `ibmcloud login` 명령을 사용하여 명령행에 로그인하십시오.
  {: tip}

2. 프롬프트되면 다음 예에서 표시된 대로 옵션 1, 옵션 6, 옵션 2를 차례대로 선택하십시오.
  ```
  ? Select a service type:                  
  1. Backend Service / Web App
  2. Mobile Client
  Enter a number> 1

  ? Select a language:
  1. Java - MicroProfile / Java EE
  2. Java - Spring
  3. Node
  4. Python - Django
  5. Python - Flask
  6. Swift
  Enter a number> 6

  ? Select a Starter Kit:
  1. Backend for Frontend - Swift Kitura - A starter for building 
  backend-for-frontend APIs in Swift, using the Kitura framework.
  2. Web App - Create Project
  3. Web App - Swift Kitura Basic - A starter that provides a basic web serving 
  application in Swift, using the Kitura framework.
  Enter a number> 2
  ```
  {: screen}

3. 애플리케이션의 이름을 제공하십시오.
  ```
  ? Enter a name for your application> swift_project
  ```
  {: screen}

4. OpenAPI 2.0 지원을 사용하도록 선택하십시오.
  ```
  ? Enable your application based on an OpenAPI 2.0 Specification document? [y/n]> y
  ```
  {: screen}

  OpenAPI 2.0 지원이 사용 가능한 경우 URL로 OpenAPI 2.0 문서에 대한 경로를 제공해야 합니다.
  ```
  ? Path to OpenAPI 2.0 document as a url (both yaml and json formats supported)> http://hostname.domain.com/path/to/file.json

  Generating application ...
  ```
  {: screen}

## 4단계. 애플리케이션 빌드, 실행 및 배치
{: #swift-cli-deploy}

이제 {{site.data.keyword.dev_cli_short}}를 사용하여 애플리케이션을 빌드, 실행 및 배치할 수 있습니다

1. **빌드**

  이제 애플리케이션 실행을 위한 전제조건인 애플리케이션을 빌드할 수 있습니다. 애플리케이션 디렉토리의 루트에서 다음 명령을 사용하여 앱을 빌드하십시오.
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **실행**

  빌드를 완료한 후 다음 명령을 사용하여 로컬 컨테이너에서 애플리케이션을 실행할 수 있습니다.
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  명령이 실행되면 애플리케이션의 랜딩 페이지를 볼 수 있는 로컬 호스트 및 포트가 표시됩니다.

3. **배치**

  `deploy` 명령을 실행하여 애플리케이션을 {{site.data.keyword.cloud_notm}}에 배치하십시오.
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}

## 다음 단계
{: #swift-cli-next notoc}

개발자가 여러 스타터 킷에서 앱을 작성하고 핵심 {{site.data.keyword.cloud_notm}} 최적화 서비스를 프로비저닝하고 연결한 후 작업 코드를 빠르게 다운로드(또는 지속적 딜리버리를 위해 설정)할 수 있도록 {{site.data.keyword.cloud_notm}} Developer Console for Apple을 사용하는 방법에 대해 알아보십시오. 사용자는 앱을 작성, 보기, 구성 및 관리하고 앱의 코드를 다운로드할 수 있습니다. Developer Console for Apple을 사용하여 빠르게 평가하고 새 앱으로 {{site.data.keyword.cloud_notm}} 서비스를 테스트할 수 있습니다.

시작하시겠습니까? [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")에 방문하여 시작하십시오.
{: tip}

자세한 정보는 [스타터 킷으로 Swift 앱 개발](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro)을 참조하십시오.
