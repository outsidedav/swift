---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Kitura로 앱 작성
{: #kitura}

[Kitura](http://www.kitura.io)는 iOS 백엔드 및 웹 애플리케이션을 빌드하기 위한 서버 측 Swift 프레임워크입니다. 이 프레임워크는 Alamofire, RestKit 또는 Kitura에서 자체적으로 제공하는 [KituraKit](https://github.com/ibm-swift/kiturakit) SDK와 같은 URLSession SDK를 사용하여 iOS 애플리케이션에서 호출할 수 있는 REST API를 작성합니다.

Kitura는 데이터베이스, 머신 러닝 및 기타 서비스를 비롯하여 {{site.data.keyword. appid_short}}, {{site.data.keyword.mobilepushshort}} 및 {{site.data.keyword.mobileanalytics_short}}를 포함한 {{site.data.keyword.cloud}}에서 제공하는 모든 서비스 및 기능과 통합할 수 있습니다. 그런 다음 Kitura는 배치되고 {{site.data.keyword.cloud}}에서 Cloud Foundry 또는 Docker(Kubernetes 기반) 플랫폼 중 하나를 사용하여 자동으로 스케일링될 수 있습니다.

Kitura는 Kitura 애플리케이션의 작성, 빌드, 테스트 및 배치를 간소화하는 `kitura` [명령행 인터페이스(CLI)](http://www.kitura.io/en/starter/gettingstarted.html)를 제공합니다. Kitura CLI를 사용하여 빌드되는 애플리케이션에는 CLI Cloud Foundry, Docker 및 Kubernetes 기술을 지원하는 모든 클라우드에 배치하기 위한 전체 지원이 포함됩니다. 그러나 특히 {{site.data.keyword.cloud_notm}}를 위해 빌드하는 경우 브라우저에서 IBM Apple 개발 콘솔을 사용하거나 {{site.data.keyword.dev_cli_notm}}를 사용하는 것이 좋습니다. 또한 두 가지 방법에서는 기본 기술을 공유하지만, Apple 개발 콘솔 및 IBM Developer Tools는 사용자를 위한 호스팅된 프로젝트 및 배치 파이프라인을 작성하고 애플리케이션에 필요한 서비스를 프로비저닝합니다.

## 시작하기 전에

먼저, 다음 필수 소프트웨어를 갖추었는지 확인하십시오.  

* iOS 11.0+  
* Xcode 9.0  
* Swift 4.0+  
* CocoaPods  

## 1단계. 브라우저를 사용하여 Kitura 프로젝트 작성
{: #create_kitura}

1. Apple 개발 콘솔의 스타터 킷 섹션으로 이동하십시오. 사전 정의된 스타터(예: "프론트엔드 API를 위한 백엔드용 Swift")를 선택하거나 **프로젝트 작성** 타일을 사용하여 사용자 정의 프로젝트를 작성하십시오. **프로젝트 작성**을 클릭하십시오.

2. 프로젝트 이름을 지정하고 프로젝트를 배치할 위치를 선택하십시오. 애플리케이션을 배치할 위치가 확실하지 않으면 나중에 변경할 수 있으므로 기본값을 사용하십시오. 

3. Swift 언어를 선택하십시오. 서버 측 Swift 프로젝트가 작성됩니다. 또한 표시되는 항목은 호스트 및 도메인 옵션이며 애플리케이션을 위한 URL을 구성합니다. 확실하지 않으면 기본값을 사용하고 애플리케이션이 상주하는 도메인 제공자에서 고유한 사용자 정의 도메인을 제공할 수도 있습니다. 

4. 빌드한 REST API를 위한 OpenAPI(Swagger라고도 함) 정의를 제공할 수 있습니다. OpenAPI 정의가 있는 경우, REST API는 Kitura에 해당 핸들러 기능을 포함하도록 작성됩니다. OpenAPI 정의가 없는 경우, Kitura를 통해 Router API를 사용하여 REST API를 수동으로 쉽게 작성할 수 있으므로 문제가 되지 않습니다.

5. **프로젝트 작성**을 클릭하십시오.

프로젝트가 작성되며, 작성된 프로젝트는 아직 추가 서비스를 사용하지 않습니다. **리소스 추가** 단추를 클릭하거나 **코드 다운로드** 단추를 클릭함으로써 서비스를 추가하여 프로젝트의 코드를 가져올 수 있습니다. 또한 서비스를 기존 프로젝트에 쉽게 추가할 수도 있습니다.

## 2단계. 서비스 추가
{: #add_services}

1. **리소스 추가** 단추를 클릭하여 서비스를 추가하십시오. 서비스 카테고리가 표시됩니다. 예를 들어, **데이터**를 선택하여 사용 가능한 데이터베이스를 확인하고 **Cloudant NoSQL DB**를 선택하십시오.
2. 서비스의 가격 책정 플랜(예: 라이트)을 선택한 후 **작성**을 클릭하십시오.

사용자에게 애플리케이션의 인증 정보를 제공하고, 일부 경우 서비스에 대한 올바른 연결을 포함하도록 필수 코드를 프로젝트에 추가하는 서비스의 인스턴스가 작성됩니다. 이제 **리소스 추가** 단추를 사용하거나 **코드 다운로드**를 클릭함으로써 추가 서비스를 추가하여 프로젝트의 코드를 가져올 수 있습니다. 

프로젝트를 다운로드한 후, 앱에 대한 작업을 시작할 수 있습니다.

## 3단계. Xcode로 애플리케이션 개발
{: #develop_xcode}

프로젝트를 다운로드한 후, Xcode를 사용하여 프로젝트를 수정하고 개발한 다음 클라우드에 대한 배치를 위해 수정된 애플리케이션을 업로드할 수 있습니다.

1. Xcode 프로젝트를 작성하십시오.  
  Xcode에서 프로젝트를 사용하기 전에 Swift 패키지 관리자 명령을 사용하여 Xcode 프로젝트 파일을 작성해야 합니다. `Package.swift` 파일이 상주하는 다운로드한 프로젝트의 루트 디렉토리에서 다음 명령을 실행하십시오. 
  ```
  swift package generate-xcodeproj
  ```
  {: codeblock}

  Xcode 프로젝트는 열 수 있는 동일한 디렉토리에 작성됩니다.

2. 프로젝트의 Xcode 대상을 설정하십시오.  
  Kitura 서버를 실행하려면 도구 모음에 있는 **project_name-Package** 섹션을 클릭하고 메뉴에서 **project_name** 대상을 선택하여 스키마를 편집해야 합니다. 대상 디바이스가 **나의 Mac**으로 설정되어 있는지 확인하십시오.

3. Kitura 서버를 로컬로 실행하십시오.
  **실행**을 클릭하거나 `⌘+R` 단축키를 사용하여 Kitura 서버를 시작하십시오. 서버가 시작되면 다음 표준 Kitura URL이 실행 중인지 확인할 수 있습니다.
  * Kitura 모니터링: [http://localhost:8080/swiftmetrics-dash/]()
  * Kitura 상태 확인: [http://localhost:8080/health]()

## 5단계. REST API 추가
{: #add_restapi}

스켈레톤 Kitura 서버가 작성되지만 iOS 애플리케이션에서 사용할 수 있는 REST API를 제공하지 않습니다. 최소 코딩으로 Kitura에서 REST API를 추가하십시오. Kitura 서버로 저장되는 `Meal` 오브젝트를 리턴하도록 설계된 `/meals`의 `GET` 요청을 위한 REST API를 추가하려면 다음 단계를 사용하십시오. 

1. `Meal` 구조체를 `Sources/Application/Application.swift` 파일에 추가하십시오.  
  `App` 클래스를 선언하기 전에 다음을 추가하여 Codable 프로토콜을 준수하는 Meal 구조체를 작성하십시오.   
  ```swift
	struct Meal: Codable {    
	    var name: String
	    var photo: Data
	    var rating: Int
	}
  ```
  {: codeblock}

2. `let cloudEnv = CloudEnv()`를 다음 코드 섹션에 추가하여 `Meal` 오브젝트를 저장하려면 사전을 `Sources/Application/Application.swift` 파일에 추가하십시오.
  ```swift
  private var mealStore: [String: Meal] = [:]
  ```
  {: codeblock}

3. 다음 코드를 `postInit()` 함수에 추가하여 `/meals`의 `GET` 요청을 위한 핸들러를 `Sources/Application/Application.swift` 파일에 추가하십시오.  

  ```swift
  router.get("/meals", handler: loadHandler)
  ```
  {: codeblock}

4. `App` 클래스에서 다른 함수로 다음 코드를 추가하여 `Sources/Application/Application.swift` 파일에 대한 loadHandler 함수를 구현하십시오.  
  ```swift
  func loadHandler(completion: ([Meal]?, RequestError?) -> Void ) {
      let meals: [Meal] = self.mealStore.map({ $0.value })
    completion(meals, nil)
  }
  ```
  {: codeblock"}

이제 `Meals`의 배열 또는 오류로 응답하는 `/meals`의 `GET` 요청을 위한 REST API가 사용자에게 제공됩니다.

5. 프로젝트를 실행하십시오.  
  **실행**을 클릭하거나 `⌘+R` 단축키를 사용하십시오. 프롬프트되면 **수신 네트워크 연결 허용**을 선택하십시오.

6. 웹 브라우저가 서버에서 데이터를 요청하는 방식과 일치하는 `GET` 요청을 사용하여 REST API를 테스트하십시오. 

  다음 URL을 사용하여 REST API를 테스트할 수 있습니다.  
  ```
  * `GET /meals`:	[http://localhost:8080/meals]()
  ```
  {: codeblock}

  `Meal` 오브젝트가 현재 `mealStore`에 저장되어 있지 않으므로 비어 있는 배열이 `[]`로 리턴됩니다. 

데이터베이스에 데이터 저장을 포함하여 `Meal` 오브젝트를 저장하고, 페치하고, iOS 애플리케이션에서 삭제하기 위해 REST API 세트를 빌드하는 데 도움이 되는 [FoodTracker 백엔드](https://github.com/IBM/FoodTrackerBackend) 튜토리얼을 사용할 수 있습니다.
{: tip}

## 6단계. KituraKit을 iOS 애플리케이션에 설치
{: #kiturakit}

Kitura 서버를 사용하여 빌드된 REST API는 사용되는 클라이언트 라이브러리 또는 클라이언트가 작성되는 언어에 상관없이 애플리케이션에서 사용할 수 있는 표준 웹 API입니다. 이는 서버에 연결하기 위해 Alamofire, RestKit 또는 URLSession을 사용할 수 있음을 의미합니다. 또한 Kitura는 iOS에서의 REST API 호출을 간소화하기 위해 KituraKit 양식으로 최적화된 맞춤형 클라이언트 커넥터를 제공합니다. 

KituraKit은 Kitura에 사용되는 라우터 핸들러 API의 미러 이미지를 제공하여 별다른 노력 없이 클라이언트와 서버 간에 Swift 유형을 공유할 수 있습니다. 이 기능을 사용하면 서버에 전송되거나 서버로부터 수신된 데이터의 JSON 인코딩 및 디코딩을 명시적으로 수행하지 않아도 됩니다.

다음 단계는 KituraKit을 iOS 애플리케이션에 설치하고 Kitura를 사용하여 작성된 `GET /meals` REST API를 호출하기 위해 KituraKit을 사용하는 방법을 표시합니다.

1. 아직 Podfile이 없는 경우 iOS 애플리케이션 디렉토리의 루트에 Podfile을 작성하십시오. 
  ```
  pod init
  ```
  {: codeblock}

2. 사용자의 프로젝트에 적합한 iOS 11의 글로벌 플랫폼을 설정하도록 다음 행을 대체하여 Podfile을 편집하십시오.
  ```
  # platform :ios, '9.0'
  ```
  {: codeblock}

  다음 코드로 대체하십시오.
	```
  platform :ios, '11.0'
  ```
  {: codeblock}

3. `# Pods for <application name>` 아래에서 다음을 추가하여 KituraKit을 Podfile에 추가하십시오.
  ```
  pod 'KituraKit', :git => 'https://github.com/IBM-Swift/KituraKit.git', :branch => 'pod'
  ```
  {: codeblock}

4. Podfile을 저장하고 다음 명령을 사용하여 KituraKit을 설치하십시오.
  ```
  pod install
  ```
  {: codeblock}

5. 작업공간(프로젝트 아님)을 열어 Xcode에서 애플리케이션을 다시 여십시오. 이제 동일한 `Meal` 정의를 Kitura 서버에서 사용된 대로 iOS 애플리케이션에 추가할 수 있습니다.
  ```swift
  struct Meal: Codable {    
      var name: String
      var photo: Data
      var rating: Int
  }
  ```
  {: codeblock}

  다음 코드를 사용하여 Kitura 서버에 연결하십시오.
  
  ```swift
  import KituraKit
  
  // Connect to the Kitura server on http://localhost:8080
  guard let client = KituraKit(baseURL: "http://localhost:8080") else {
      print("Error creating KituraKit client")
      return
  }
  
  // Make a request to `GET /meals`, expecting a [Meal] or a RequestError
  client.get("/meals") { (meals: [Meal]?, error: RequestError?) in
      // Check for an error
      guard error == nil else {
          print("Error saving meal to Kitura: \(error!)")
          return
      }
      // Check for meals
      guard let meals = meals else {
          print("Received Meals!")
          return
      }
  }
  ```
  {: codeblock}

Kitura 서버는 Kitura의 완료 핸들러에서 매개변수와 일치하는 콜백을 포함한 `client.get("/meals")`을 사용하여 호출됩니다.

데이터베이스에 데이터 저장을 포함하여 `Meal` 오브젝트를 저장하고, 페치하고, iOS 애플리케이션에서 삭제하기 위해 REST API 세트를 빌드하는 데 도움이 되는 [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend) 튜토리얼을 사용할 수 있습니다.
{: tip}

## 다음 단계
{: #next notoc}

iOS 애플리케이션에서 호출할 수 있는 REST API를 제공하는 Kitura 서버가 설치되었으므로 서버를 {{site.data.keyword.cloud_notm}}에 배치할 준비가 되었습니다. Kubernetes가 있는 컨테이너, 보안 컨테이너 또는 Cloud Foundry를 사용하여 배치를 수행할 수 있습니다.
