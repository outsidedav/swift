---

copyright:
  years: 2018
lastupdated: "2018-08-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Swift로 로깅
{: #logging_swift}

로그는 서비스 실패 이유와 과정을 진단하는 데 필요합니다. 로그는 메트릭의 측정 대상인 애플리케이션 성능을 모니터하는 데 사용하기 위한 것은 아니지만 경보의 소스로 사용될 수 있으며 여기에는 집계된 메트릭에서 얻을 수 있는 것보다 자세한 내용이 포함될 수 있습니다. 

클라우드 인프라에서의 작업으로 얻을 수 있는 이점 중 하나는 애플리케이션에서 로그 파일 관리와 같은 수 많은 항목에 대해 문제가 발생하지 않는 것입니다. 클라우드 환경에서 프로세스의 일시적인 특성을 고려하여 로그가 수집되고, 분석을 위해 일반적으로 중앙화된 위치로 전송되어야 합니다. 클라우드 환경에 로그하는 가장 일관된 방법은 표준 출력 및 오류 스트림으로 로그 항목을 전송하고 인프라가 나머지 항목을 처리할 수 있도록 하는 것입니다. 

시간 경과에 따라 애플리케이션이 발전하므로 사용자가 로그할 수 있는 특성이 변경될 수 있습니다. JSON 로그 형식을 사용하면 다음과 같은 이점이 있습니다.
* 로그의 본문을 좀 더 쉽게 검색하고 집계할 수 있도록 로그의 색인화가 가능합니다. 
* 구문 분석이 문자열에서 요소의 위치에 의존하지 않으므로 로그가 변경하기 쉽습니다. 

명령행 도구를 사용하여 로그를 페치하는 경우 JSON 형식의 로깅을 사용하면 로그를 읽은 것이 좀 더 어려워질 수 있습니다. 로컬 개발 및 디버깅을 위해 일반 텍스트 형식의 로그가 제공되도록 환경 변수를 사용하여 사용되는 로그 형식을 전환할 수 있습니다. 

## Swift 앱에 로깅 추가

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger)는 많이 사용되는 Swift에 대한 경량의 로깅 프레이워크이며 표준 출력으로의 로깅 및 다양한 로그 레벨과 같은 다양한 기본적인 이점을 제공합니다. 

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI)는 Swift로 다양한 유형의 로거에 대한 공통 로깅 인터페이스를 제공하는 로거 프로토콜입니다. Kitura는 그 구현 과정에서 `LoggerAPI`를 사용합니다. 

`HeliumLogger`를 활용하려면 사용되는 대상에 이를 추가하도록 `Package.swift`에서 **dependencies:**에 다음을 추가하십시오. 
```swift
.package(url: "https://github.com/IBM-Swift/HeliumLogger.git", from: "1.7.1")
```
{: codeblock}

`HeliumLogger`를 초기화하고 `LoggerAPI`에서 사용되는 로거로 이를 설정하도록 다음 인스트루먼테이션 코드를 추가하십시오. 
```swift
import HeliumLogger
import LoggerAPI

let logger = HeliumLogger(.verbose)
Log.logger = logger

Log.info("This is an informational log message.")
```
{: codeblock}

제공된 예에서 [로그 레벨](http://ibm-swift.github.io/HeliumLogger/)은 명시적으로 `.verbose`(기본값)로 설정됩니다. 

로그 메시지 사용자 정의에 대한 자세한 정보는 [HeliumLogger API 참조 문서](http://ibm-swift.github.io/HeliumLogger/)를 참조하십시오.

## 스타터 킷으로 로깅
{: #monitoring}

기본적으로 {{site.data.keyword.cloud_notm}} 앱 서비스를 사용하여 작성된 Swift 앱은 `HeliumLogger`와 함께 제공됩니다. 기본적으로 또는 클라우드 환경에서 앱을 실행하면 다음 출력이 생성됩니다. 
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

이러한 메시지는 로컬 실행의 결과인 `stdout`에 있거나 [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) 및 [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/) 개발의 로그(`ibmcloud app logs --recent <APP_NAME>` 및 `kubectl logs <deployment name>`로 각각 액세스됨)에 있습니다. 

`/Sources/AppName/main.swift` 파일에서 다음 코드를 볼 수 있습니다. 
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

로그 레벨은 애플리케이션 시작 로그와 같은 정보 레벨 메시지를 로그하도록 명시적으로 `.info`로 설정됩니다.
{: tip}

## 다음 단계
{: #next_steps}

각 배치 환경별 로그 보기에 대해 자세히 알아보십시오. 
* [Kubernetes 로그](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [Cloud Foundry 로그Logs](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [{{site.data.keyword.openwhisk}} 로그 및 모니터링](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

로그 집계자 사용:
* [{{site.data.keyword.cloud_notm}} 로그 분석](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Private ELK 스택](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
