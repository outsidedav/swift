---

copyright:
  years: 2017-2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.cloud_notm}}에 문서 저장
{: #cloudant}

{{site.data.keyword.cloudantfull}}는 문서 기반 DBaaS(DataBase as a Service)입니다. 데이터를 JSON 형식의 문서로 저장합니다. 확장성, 고가용성 및 내구성을 염두에 두고 빌드되었으며 Swift 애플리케이션에서 사용하도록 쉽게 구성할 수 있습니다. MapReduce, {{site.data.keyword.cloudant_short_notm}} 조회, 전체 텍스트 색인 작성 및 지리 공간 색인 작성이 포함된 광범위한 색인 작성 옵션이 함께 제공됩니다. 복제 기능을 사용하면 데이터베이스 클러스터, 데스크탑 PC 및 모바일 디바이스 간에 데이터 동기화를 쉽게 유지할 수 있습니다.
{:shortdesc}

{{site.data.keyword.cloudant_short_notm}}를 사용할 수 있는 모든 방법은 [{{site.data.keyword.cloudant_short_notm}} 기본](/docs/services/Cloudant/basics/index.html#cloudant-nosql-db-basics)을 참조하십시오.

## 시작하기 전에

먼저, 다음 필수 소프트웨어를 갖추었는지 확인하십시오. 
 * CocoaPods(버전 1.1.0 이상)
 * iOS(버전 9 이상)
 * MacOS(버전 10.11.5 이상)
 * Xcode(버전 9.0.1 이상)

[Swift용 {{site.data.keyword.cloudant_short_notm}} SDK![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudant/swift-cloudant)는 Swift 3.2로 빌드됩니다. Kitura를 사용하여 {{site.data.keyword.cloudant_short_notm}}를 사용할 계획인 경우 Swift 4.0으로 빌드된 [Kitura-CouchDB 라이브러리 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/IBM-Swift/Kitura-CouchDB)를 참조하십시오.
{: tip}

## 1단계. {{site.data.keyword.cloudant_short_notm}}의 인스턴스 작성

[IBM Cloud에서 Cloudant NoSQL DB 인스턴스 작성 튜토리얼![외부 링크 아이콘](../images/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/docs/services/Cloudant/tutorials/create_service.html#creating-a-cloudant-nosql-db-instance-on-ibm-cloud){:new_window}을 참조하여 서비스의 인스턴스를 프로비저닝하십시오. 


## 2단계. SDK 설치

### iOS Swift SDK 설치

Swift Cloudant SDK를 사용하면 앱을 보다 쉽게 코딩할 수 있습니다. SDK가 앱 코드에 설치되어야 합니다.

1. `Podfile`에 대한 기존 Xcode 프로젝트 디렉토리를 여십시오. 
2. 프로젝트 대상에서 `SwiftCloudant` 팟(Pod)의 종속성을 추가하십시오. 다음 예에서 표시된 대로 `use_frameworks!` 명령도 대상 아래에 있는지 확인하십시오.
    ```
    target '<yourTarget>' do
      use_frameworks!
        pod 'SwiftCloudant', :git => 'https://github.com/cloudant/swift-cloudant.git'
    end
    ```
    {: screen}
3. `SwiftCloudant` 종속성을 다운로드하십시오.
    ```
    pod install
    ```
    {: pre}

### 서버 측 Swift SDK 설치

서버 측 개발을 위해 Swift 패키지 관리자로 사용하려면 다음 행을 `Package.swift`의 종속성에 추가하십시오.
```swift
.Package(url: "https://github.com/cloudant/swift-cloudant.git")
```
{: pre}

## 3단계. SDK 초기화

앱에서 SDK를 초기화한 후 {{site.data.keyword.cloudant_short_notm}} 활용을 시작하여 데이터를 저장할 수 있습니다. 

1.  다음 가져오기를 `AppDelegate.swift` 또는 서버 측 Swift 파일에 추가하십시오.
    ```
    import SwiftCloudant
    ```
    {:pre}
2. 데이터베이스에 대한 연결을 초기화하십시오.
    ```swift
    let cloudantURL = NSURL(string: "https://username.cloudant.com")!
    let client = CouchDBClient(url: cloudantURL, username: "username", password: "password")
    let dbName = "database"
    ```
    {: codeblock}

### 기본 오퍼레이션
다음 기본 오퍼레이션은 초기화된 클라이언트를 사용하여 문서의 작성, 읽기 및 영구 삭제를 위한 기본 조치를 설명합니다. 

#### 문서 작성
```swift
let create = PutDocumentOperation(id: "doc1", body: ["hello": "world"], databaseName: dbName) {(response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while creating a document. Error: \(error)")
    } else {
        print("Created document \(response?["id"]) with revision id \(response?["rev"])")
    }
}
client.add(operation: create)
```
{: codeblock}

#### 문서 읽기
```swift
let read = GetDocumentOperation(id: "doc1", databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while reading a document. Error: \(error)")
    } else {
        print("Read document: \(response)")
    }   
}
client.add(operation: read)
```
{: codeblock}

#### 문서 삭제
```swift
let delete = DeleteDocumentOperation(id: "doc1",
    revision: "1-revisionidhere",
    databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while deleting a document. Error: \(error)")
    } else {
        print("Document deleted")
    }   
}
client.add(operation: delete)
```
    {: codeblock}


## 4단계. 앱 테스트
{: #cloudant_testing}

모든 항목이 올바르게 설정되어 있습니까? 테스트하십시오!

1. 초기화 및 각 오퍼레이션(예: 문서 작성)을 호출할 수 있도록 애플리케이션을 실행하십시오. 
2. 웹 브라우저에서 이전에 작성한 {{site.data.keyword.cloudant_short_notm}} 서비스 인스턴스로 돌아가서 서비스 대시보드를 여십시오. 
3. 사용되는 데이터베이스를 선택하고 대시보드에서 문서를 확인할 수 있습니다. 

문제가 있습니까? [{{site.data.keyword.cloudant_short_notm}} API 참조](/docs/services/Cloudant/api/index.html#api-reference-overview)를 확인하십시오.


## 다음 단계
{: #cloudant_next notoc}

잘 하셨습니다! 앱에 보안 지속성 레벨을 추가했습니다. 다음 옵션 중 하나를 사용하여 계속 진행하십시오. 

* [Swift용 {{site.data.keyword.cloudant_short_notm}} SDK ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudant/swift-cloudant) 소스 코드를 보십시오. 
* 스타터 킷은 IBM Cloud의 기능을 활용할 수 있는 가장 빠른 방법 중 하나입니다. **Infinite Scrolling with Cloudant NoSQL for iOS** 스타터 킷은 페이지 번호 매기기를 사용하여 데이터를 표시하기 위해 ViewController를 확장하는 방법을 보여줍니다. 이 앱 패턴은 iOS 개발자에게 일반적이며 {{site.data.keyword.cloudant_short_notm}}의 기능을 설명하기 위한 좋은 예입니다. [모바일 개발자 대시보드](https://console.bluemix.net/developer/mobile/dashboard)에서 사용 가능한 스타터 킷을 보십시오. 코드를 다운로드하십시오. 앱을 실행하십시오!
* {{site.data.keyword.cloudant_short_notm}}에서 제공하는 모든 기능을 자세히 알아보고 활용하십시오. [이 문서를 확인](/docs/services/Cloudant/index.html)하십시오!
