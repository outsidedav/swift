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

# Swift 앱에서 상태 검사 사용
{: #healthcheck}

상태 검사(Health check)는 서버 측 애플리케이션이 올바르게 작동하는지 여부를 판별할 수 있는 단순 메커니즘을 제공합니다. [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) 및 [Kubernetes](https://www.ibm.com/cloud/container-service)와 같은 여러 배치 환경에서 서비스의 인스턴스가 트래픽을 허용할 준비가 되었는지 여부를 판별하기 위해 상태 엔드포인트를 폴링하도록 구성될 수 있습니다. 

상태 검사는 일반적으로 HTTP를 통해 이용되며 `UP` 또는 `DOWN` 상태를 표시하기 위해 표준 리턴 코드를 사용합니다. `UP`의 경우 `200`, `DOWN`의 경우 `5xx`의 리턴을 예로 들 수 있습니다. 구체적으로 애플리케이션이 요청을 처리할 수 없거나 아직 시작되지 않은 경우(서비스 사용 불가능) `503`이 사용되고 서버에서 오류 조건이 발생할 때 `500`이 사용됩니다. 상태 검사의 리턴값은 변수이지만 최소 JSON 응답(예: `{“status”: “UP”}`)은 일관성을 제공합니다. 

Cloud Foundry는 하나의 상태 엔드포인트를 사용하여 서비스 인스턴스가 요청을 처리할 수 있는지 여부를 표시합니다. Kubernetes에서 상태 검사 엔드포인트는 `readiness` 엔드포인트와 동등합니다. 애플리케이션은 자동 라우팅 결정을 판별하는 데 도움이 되도록 이 엔드포인트를 정의해야 합니다. 이 엔드포인트 성공 또는 실패에 허용할 수 있는 폴백이 없는 경우 필요한 다운스트림 서비스의 고려사항이 포함될 수 있습니다. 다운스트림 서비스를 확인하는 경우 상태 검사로 인해 시스템 전체에서 로드를 최소화하기 위해 결과를 캐싱하는 것은 때때로 유용합니다. 

Kubernetes는 추가 [활동](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) 엔드포인트를 정의하여 애플리케이션이 프로세스의 다시 시작 여부를 표시할 수 있습니다. 고려사항의 서브세트는 특정 메모리 사용량 임계값에 도달하면 `liveness`가 실패할 수 있는 경우에 적용됩니다. 앱이 Kubernetes에서 실행되면 필요한 경우 프로세스가 다시 시작하도록 `liveness` 엔드포인트 추가를 고려하십시오. 

## 기존 Swift 앱에 상태 검사 추가
{: #add-healthcheck-existing}

[상태](https://github.com/IBM-Swift/Health) 라이브러리를 통해 상태 검사를 Swift 애플리케이션에 쉽게 추가할 수 있습니다. 상태 검사는 확장 가능합니다. DoS 공격을 방지하기 위한 [캐싱](https://github.com/IBM-Swift/Health#caching) 또는 [사용자 정의 검사](https://github.com/IBM-Swift/Health#implementing-a-health-check) 추가에 대한 정보는 [상태](https://github.com/IBM-Swift/Health) 정보를 참조하십시오. 

상태 라이브러리를 기존 Swift 앱에 추가하려면 사용되는 대상에 이를 추가하도록 `Package.swift` 파일의 *dependencies:* 섹션에 상태 라이브러리를 지정하십시오. 
```swift
  .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
```
{: codeblock}

그 후 다음 초기화 코드를 애플리케이션에 추가하십시오. 
```swift
import Health

let health = Health()
```
{: codeblock}

그런 다음 상태 검사 엔드포인트를 정의하도록 라우트 정의를 추가하십시오.
```
router.get("/health") { request, response, next in
  // let status = health.status.toDictionary()
  let status = health.status.toSimpleDictionary()
  if health.status.state == .UP {
    try response.send(json: status).end()
  } else {
    try response.status(.serviceUnavailable).send(json: status).end()
  }
}
```
{: codeblock}

`/health` 엔드포인트에 액세스하여 브라우저에서 앱의 상태를 확인하십시오. 코드는 단순 사전에서 정의된 대로 페이로드 `{"status": "UP"}`을 리턴합니다. 

**Codable** 또는 표준 사전 사용과 같은 대체 구현의 경우 [상태 라이브러리 예](https://github.com/IBM-Swift/Health#usage)를 참조하십시오.

## 서버 측 Swift 스타터 킷 앱에서 상태 검사에 액세스
{: #healthcheck-starterkit}

스타터 킷을 사용하여 Kitura 기반 Swift 앱을 생성하는 경우 기본적으로 기본 상태 검사 엔드포인트인 `/health`가 표시됩니다. 엔드포인트는 [상태](https://github.com/IBM-Swift/Health) 라이브러리에서 지원되는 Swift 4에서 사용 가능한 Codable 프로토콜을 활용합니다. 

상태 오브젝트의 초기화와 같은 기본 초기화 코드는 `Sources/Application.swift`에서 발생하지만, 상태 확인 엔드포인트는 다음 코드가 포함된 `/Sources/Application/Routes/HealthRoutes.swift` 파일에서 제공됩니다. 
```swift
import LoggerAPI
import Health
import KituraContracts

func initializeHealthRoutes(app: App) {

    app.router.get("/health") { (respondWith: (Status?, RequestError?) -> Void) -> Void in
        if health.status.state == .UP {
            respondWith(health.status, nil)
        } else {
            respondWith(nil, RequestError(.serviceUnavailable, body: health.status))
        }
    }

}
```
{: codeblock}

예에서는 `/health` 엔드포인트에 액세스할 때 `{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}`과 같은 페이로드를 생성하는 표준 사전을 사용합니다. 
