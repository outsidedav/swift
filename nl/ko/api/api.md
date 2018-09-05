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

# iOS 앱에 API 추가
{: #api_connect}

API가 {{site.data.keyword.cloud_notm}}의 외부에서 유지보수되는지 여부에 관계없이 사용자는 API Connect를 사용하여 {{site.data.keyword.cloud}}에서 API를 관리할 수 있습니다. 사용을 제어하고, 채택률을 높이며, 통계를 추적할 수 있도록 API를 관리하는 방법에 대해 알아보십시오.

## API Connect의 인스턴스 작성

카탈로그로 이동하고 API Connect의 인스턴스를 작성하여 API를 관리하십시오.

`Menu->APIs`를 사용하여 API Connect Management 콘솔에 액세스하십시오.

![API Connect](images/apiconnect.png)

백엔드 및 프론트 엔드 개발을 시작하기 전에 고유한 API 계약을 정의하는 경우 API Connect 도구를 사용하여 이 프로세스를 가속화하십시오. 디지털 개발 팀과 협업하여 iOS 앱 및 백엔드 로직 간의 API 계약을 빌드하고 정의할 수 있습니다. [{{site.data.keyword.openwhisk}}](/docs/openwhisk/index.html)를 사용하거나 Kubernetes 또는 [Cloud Foundry](/docs/cloud-foundry/index.html)에서 [Swift 런타임](/docs/runtimes/swift/index.html)을 통해 이 로직을 전달할 수 있습니다.

API를 정의하면 여러 가지 도구로 Open API 스펙(Swagger)을 정의할 수 있습니다.

- [Swagger Editor](http://editor.swagger.io/)
- [API Designer](https://www.ibm.com/support/knowledgecenter/en/SSFS6T/com.ibm.apic.toolkit.doc/task_apionprem_composing_apis.html)
- [Loopback](https://loopback.io/)

## 관리 API 정의

클라이언트 애플리케이션 및 백엔드 로직 간의 API 게이트웨이를 관리하는 API 프록시를 정의할 수 있습니다. Open API 스펙(Swagger 문서) YAML 또는 JSON을 사용하여 프록시를 작성하려면 다음 단계를 수행하십시오. 

1. `Menu -> APIs` 콘솔을 열고 API 프록시를 클릭하십시오.
2. **API 정의 가져오기 YAML 또는 JSON**을 클릭하십시오.
3. 이전에 작성한 YAML 또는 JSON 파일을 선택하십시오.
4. 저장 후 노출하십시오.

백엔드 로직 애플리케이션에 연결되는 URL을 가리키도록 외부 엔드포인트를 구성해야 합니다. 

## Swift 백엔드 작성

이 API를 기반으로 백엔드 Swift 앱을 작성할 수 있습니다. 

Apple 개발 콘솔에서 다음 단계를 수행하십시오.

1. **스타터 킷**을 선택하십시오.
2. **앱 작성**을 클릭하십시오.
3. 언어로 **Swift**를 선택하십시오.

YAML 및 JSON 파일을 선택한 후 **작성**을 클릭하십시오. 백엔드 Swift 앱이 작성됩니다.

그런 다음 코드를 **다운로드**하거나 **클라우드에 배치**하고 GIT 저장소를 로컬 머신에 복제할 수 있습니다. 지식 안내서의 지시사항에 따라 XCode에서 서버 측 앱을 열 수 있습니다.

**소스** 폴더에서 API에 맵핑하는 REST 엔드포인트를 작성한 Swift 파일을 정의하는 라우트를 볼 수 있습니다. 

PetStore Open API를 사용하는 다음 예제를 참조하십시오.
```swift
import Kitura
import KituraContracts

func initializePet_Routes(app: App) {
    app.router.post("\(basePath)/pet") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.put("\(basePath)/pet") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.get("\(basePath)/pet/findByStatus") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.get("\(basePath)/pet/findByTags") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.get("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.post("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.delete("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.post("\(basePath)/pet/:petId/uploadImage") { request, response, next in
        response.send(json: [:])
        next()
    }
}
```
{: codeblock}

{{site.data.keyword.openwhisk_short}} 또는 전체 스택 Swift 런타임을 사용하여 API를 작성하고 API Connect 정의가 작성되면 iOS 앱에서 API를 이용할 수 있습니다.

## iOS 앱 모바일 앱에서 API 이용

iOS 앱에서 백엔드 API를 이용하려면 Apple 콘솔을 사용하여 모바일 스타터 킷을 작성하십시오. 스타터 킷 보기를 사용하여 원하는 유형의 iOS Swift 스타터 킷을 작성하십시오.

**리소스 추가**를 클릭하고 API를 선택하십시오. 

![API 대화 상자](../images/apidialog.png)

API는 iOS 앱에 추가됩니다. 앱을 위한 코드를 *다운로드*하는 경우 API의 이름을 따른 iOS 소스 폴더에 포함된 폴더를 볼 수 있습니다.

지식 안내서의 단계에 따라 종속된 모든 SDK를 iOS 앱으로 `pod update`하십시오.  

iOS 앱에는 API를 위해 생성된 SDK 바인딩을 포함한 폴더가 있습니다. 이 폴더에는 세 개의 하위 폴더인 `Assets`, `Source` 및 `Docs`가 있습니다. 

![iOS 폴더](../images/sdkfolder.png)

`Assets` 폴더에는 API에 대한 URL을 관리하는 파일이 포함되어 있으며, 기본값은 `localhost:3000`입니다. API 라우트를 참조하려면 값을 변경해야 합니다. API 정의에는 API 이름 및 라우트 섹션이 포함되어 있습니다. 라우트의 끝에 있는 **복사 아이콘**을 클릭하여 URL을 복사하십시오. API를 호출하기 위해 외부 클라이언트를 사용으로 설정하려면 *관리 API 노출* 옵션이 켜져 있는지 확인십시오.

![API 라우트](../images/apiroute.png)  

`PLIST` 파일을 열고 SDK가 {{site.data.keyword.cloud_notm}}로 API를 호출할 수 있도록 API 라우트에서 복사한 값으로 호스트 값을 대체하십시오.

## 문서

SDK가 iOS 앱 프로젝트에 포함된 경우 **Docs 폴더**에서 *README.html* 파일을 사용할 수 있습니다. 외부 브라우저에서 Docs 폴더를 열고 프로젝트 사용 방법에 대한 지시사항을 읽어 보십시오. 

## API 변경 후 SDK 재작성

API가 변경되거나 새 기능을 사용할 수 있게 되고 {{site.data.keyword.openwhisk}}가 추가된 경우 `ibmcloud sdk` 명령을 사용하여 클라이언트 SDK를 재작성할 수 있습니다. 자세한 정보, 예제 및 구문 도움말은 [SDK 생성기](/docs/cli/sdk/index.html) 문서를 확인하십시오.

SDK를 작성할 수 있도록 하려면 Open API 스펙(Swagger) YAML 또는 JSON 파일을 사용하십시오. {{site.data.keyword.cloud_notm}}에서 API 관리 기능을 사용하여 이 파일을 검색할 수 있습니다. 

1. `Menu -> APIs -> Managed APIs`로 이동하십시오. 
2. 최신 Open API 스펙을 검색할 API를 선택하십시오.  
3. 그런 다음 **탐색기** 메뉴를 선택하십시오.

![API 탐색기](../images/downloadyaml.png)

4. 다운로드 아이콘을 선택하여 API용 yaml을 다운로드하고 이 파일을 iOS 앱 프로젝트 디렉토리에 저장하십시오.

5. 다음 단계는 `ibmcloud sdk` CLI 명령을 실행하는 것입니다.
    ```
    ibmcloud sdk generate --ios --unzip --output ./MyAppFunctions -f ./mobile-bff-functions-1.0.0.yaml SDKMyFunctions
    ```
    {: codeblock}

    iOS 앱 프로젝트 디렉토리에 SDK가 재작성되어 API에 대한 작업을 계속 수행할 수 있습니다. 

## 참조

다음 SDK 예제는 스타터 킷에서 {{site.data.keyword.openwhisk_short}}에 대해 작성됩니다. iOS 앱에 포함할 수 있는 각각의 조치 및 코드의 Swift 스니펫을 볼 수 있습니다.

### 기본 API 메소드
 * [`getCreate`](#getCreate)
 * [`getDelete`](#getDelete)
 * [`getDeleteall`](#getDeleteall)
 * [`getRead`](#getRead)
 * [`getReadall`](#getReadall)
 * [`getUpdate`](#getUpdate)

### `getCreate` 사용
{: #getCreate}

```swift
public static func getCreate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### `getCreate`의 매개변수

- **completionHandler**(필수)
    - 클로저(closure)는 `Response?` 및 `Error?`를 인수로 취합니다. 

### `getCreate`로 인증

인증이 필요하지 않음

### `getCreate`를 사용하는 예제
```swift
DefaultAPI.getCreate() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### `getDelete` 사용
{: #getDelete}

```swift
public static func getDelete(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### `getDelete`의 매개변수

- **completionHandler**(필수)
    - 클로저(closure)는 `Response?` 및 `Error?`를 인수로 취합니다. 

### `getDelete`로 인증

인증이 필요하지 않음

### `getDelete`를 사용하는 예제
```swift
DefaultAPI.getDelete() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### `getDeleteall` 사용
{: #getDeleteall}

```swift
public static func getDeleteall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### `getDeleteall`의 매개변수

- **completionHandler**(필수)
    - 클로저(closure)는 `Response?` 및 `Error?`를 인수로 취합니다. 

### `getDeleteall`로 인증

인증이 필요하지 않음

### `getDeleteall`을 사용하는 예제

```swift
DefaultAPI.getDeleteall() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### `getRead` 사용
{: #getRead}

```swift
public static func getRead(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### `getRead`의 매개변수

- **completionHandler**(필수)
    - 클로저(closure)는 `Response?` 및 `Error?`를 인수로 취합니다. 

### `getRead`로 인증

인증이 필요하지 않음

### `getRead`를 사용하는 예제
```swift
DefaultAPI.getRead() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### `getReadall` 사용
{: #getReadall}

```swift
public static func getReadall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### `getReadall`의 매개변수

- **completionHandler**(필수)
    - 클로저(closure)는 `Response?` 및 `Error?`를 인수로 취합니다. 

### `getReadall`로 인증

인증이 필요하지 않음

### `getReadall`을 사용하는 예제
```swift
DefaultAPI.getReadall() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### `getUpdate` 사용
{: #getUpdate}

```swift
public static func getUpdate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### `getUpdate`의 매개변수

- **completionHandler**(필수)
    - 클로저(closure)는 `Response?` 및 `Error?`를 인수로 취합니다. 

### `getUpdate`로 인증

인증이 필요하지 않음

### `getUpdate`를 사용하는 예제
```swift
DefaultAPI.getUpdate() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

