---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: swift-cfenv, service bindings swift, environment swift, swift configuration, cloudenvironment swift, VCAP_SERVICES swift, swift credentials

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Swift 환경 구성
{: #configuration}

클라우드 네이티브 개발에는 구성 데이터를 처리하는 방법에서 교차하는 밀접하게 관련된 두 가지 관행이 있습니다. 첫 번째는 개발부터 프로덕션에 이르는 애플리케이션의 과정에서 오류가 발생할 가능성을 최소화하기 위해 변하지 않는 아티팩트를 빌드해야 한다는 것입니다. 두 번째는 환경별 코드로 작성되는 문제를 방지하기 위해 개발 및 프로덕션 환경 간의 동일성을 위해 노력해야 한다는 것입니다. 

서비스 구성 및 인증 정보(서비스 바인딩) 관리는 플랫폼별로 다릅니다. Cloud Foundry는 환경 변수 `VCAP_SERVICES`로 애플리케이션에 문자열로 표시되는 JSON 오브젝트로 서비스 바인딩 세부사항을 저장합니다. Kubernetes는 환경 변수로 컨테이너화된 애플리케이션에 전달되거나 임시 볼륨으로 마운트될 수 있는 `ConfigMaps` 또는 `Secrets`의 문자열로 표시되는 JSON 또는 플랫 속성으로 서비스를 저장합니다. 로컬 테스팅이 종종 클라우드에 실행되는 모든 항목에서 간소화된 형태로 파생되므로 로컬 개발에는 고유한 구성이 있습니다. 환경별 코드 경로 없이 포터블(portable) 방식으로 이러한 변형에 대해 작업하는 것은 어려운 문제일 수 있습니다.

포터블 애플리케이션과 환경별 위치에서 서비스 바인딩(또는 기타 구성) 찾기를 캡슐화하기 위해 사용할 수 있는 유틸리티를 작성하는 데 도움이 되는 간단한 가이드라인을 따를 수 있습니다.클라우드 지원을 기존 애플리케이션에 추가해야 하거나 스타터 킷으로 앱을 작성해야 하는 경우 모두 목표는 많은 개발 플랫폼에 사용될 Swift 앱의 이식성을 제공하는 것입니다.

## 기존 Swift 애플리케이션에 {{site.data.keyword.cloud_notm}} 추가
{: #addcloud-env}

특정 환경 값을 추상화하기 위한 경로는 하나의 클라우드 환경과 다른 클라우드 환경 간에 다를 수 있습니다. [CloudEnvironment](https://github.com/IBM-Swift/CloudEnvironment.git){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘") 라이브러리는 여러 클라우드 제공자에서 환경 구성 및 인증 정보를 추상화하므로 Swift 앱은 로컬로 또는 Cloud Foundry, Cloud Foundry Enterprise Environment, Kubernetes, {{site.data.keyword.openwhisk}} 또는 가상 인스턴스에서 지속적으로 정보에 액세스할 수 있습니다. 인증 정보 추상화는 `CloudEnvironment` 라이브러리에서 제공하며, 이는 내부적으로 Cloud Foundry 구성을 위해 [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 사용하고 구성 관리자로 [Configuration](https://github.com/IBM-Swift/Configuration){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 사용합니다. 

`CloudEnvironment`를 통해 Swift 애플리케이션이 해당 값을 검색하기 위해 사용할 수 있는 검색 키를 정의하여 애플리케이션의 소스 코드에서 하위 레벨 세부사항을 추상화할 수 있습니다.

`CloudEnvironment` 라이브러리는 소스 코드에서 사용할 수 있는 일관된 검색 키를 제공합니다. 그런 다음 라이브러리는 구성 속성 또는 서비스 인증 정보가 포함된 JSON 오브젝트를 찾기 위해 검색 패턴의 배열에서 검색합니다. 

### Swift 애플리케이션에 CloudEnvironment 패키지 추가
{: #add-cloudenv}

Swift 애플리케이션에서 `CloudEnvironment` 패키지를 사용하려면 `Package.swift` 파일의 **dependencies:** 섹션에서 이를 지정하십시오.
```swift
.package(url: "https://github.com/IBM-Swift/CloudEnvironment.git", from: "8.0.0"),
```
{: codeblock}

그 후 다음 인스트루먼테이션 코드를 애플리케이션에 추가하십시오.
```swift
import CloudEnvironment

let cloudEnv = CloudEnv()
```
{: codeblock}

### 인증 정보에 액세스
{: #access-credentials}

`CloudEnvironment` 라이브러리가 초기화되었으므로 다음 예에 표시된 대로 인증 정보에 액세스할 수 있습니다.
```swift
let cloudantCredentials = cloudEnv.getCloudantCredentials(name: "cloudant-credentials")
// cloudantCredentials.username, cloudantCredentials.password, cloudantCredentials.url, etc.
let objStorageCredentials = cloudEnv.getObjectStorageCredentials(name: "object-storage-credentials")
// objStorageCredentials.username, objStorageCredentials.password, objStorageCredentials.projectID, etc.

let service1Credentials = cloudEnv.getDictionary("service1-credentials")
let service1CredentialsStr = cloudEnv.getString("service1-credentials")

// Get a default PORT and URL
let port = cloudEnv.port
let url = cloudEnv.url
```
{: codeblock}

이 예는 서비스에 맞는 인증 정보 세트에 대한 액세스를 제공하며, 이제 이러한 [지원되는 서비스 또는 일반 사전](https://github.com/IBM-Swift/CloudEnvironment#supported-services){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")에 대한 연결을 초기화하는 데 사용할 수 있습니다. Cloud Foundry 특정 구성을 위한 [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 확인하고 구성 데이터 로드 시 [구성 세부사항](https://github.com/IBM-Swift/Configuration){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 확인하십시오.

## 서비스 인증 정보 이해
{: #service_creds}

`CloudEnvironment` 라이브러리는 `config` 디렉토리에 있는 `mappings.json`이라는 파일을 사용하여 인증 정보가 각 서비스마다 저장되는 위치를 전달합니다. `mappings.json` 파일은 다음 세 가지 검색 패턴 유형을 사용하는 값의 검색을 지원합니다.
- **`cloudfoundry`** - Cloud Foundry의 서비스 환경 변수에서 값을 검색하는 데 사용되는 패턴 유형(`VCAP_SERVICES`) Cloud Foundry Enrprise Edition의 경우 자세한 정보는 이 [시작하기 튜토리얼](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started)을 참조하십시오.
- **`env`** - Kubernetes 또는 Functions로 환경 변수에 맵핑되는 값을 검색하는 데 사용되는 패턴 유형
- **`file`** - JSON 파일에서 값을 검색하는 데 사용되는 패턴 유형. 경로는 Swift 애플리케이션의 루트 폴더에 상대적이어야 합니다.

검색 키와 연관된 값의 배열은 표시되는 순서로 검색됩니다.

다음은 `mappings.json` 파일의 예를 보여줍니다.
```javascript
{
    "cloudant-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-cloudant-db",
                "env:my_awesome_cloudant_db_credentials",
                "file:localdev/my-awesome-cloudant-db-credentials.json"
            ]
        }
    },
    "object-storage-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-object-storage",
                "env:my_awesome_object_storage_credentials",
                "file:localdev/my-awesome-object-storage-credentials.json"
            ]
        }
    }
}
```
{: codeblock}

이 예에서 `cloudant-credentials` 및 `object-storage-credentials`는 Swift 애플리케이션에서 해당 인증 정보를 검색하는 데 사용하는 검색 키입니다. 검색 패턴의 배열에 따라 구성 관리자는 먼저 `VCAP_SERVICES`에서 `my-awesome-cloudant-db`를 검색하고, 그런 다음 환경 변수로 `my_awesome_cloudant_db_credentials`를 검색합니다. 이 항목이 정의되지 않으면 `localdev` 폴더에서 `my-awesome-object-storage-credentials.json`의 컨텐츠를 검색합니다. 

애플리케이션이 로컬로 실행되면 이전 예에서 표시된 대로 파일(예: `localdev/my-awesome-object-storage-credentials.json`)에 저장된 인증 정보를 사용할 수 있습니다. 로컬로 액세스할 서비스에 대한 연결 정보(예: 사용자 이름, 비밀번호 및 호스트 이름)는 모두 이 파일에 저장됩니다. 

보안상의 이유로 인증 정보 파일은 저장소에 속하지 않습니다. 이전 예에서 `localdev` 폴더는 로컬 인증 정보를 저장하는 데 사용되므로 실수로 커미트하는 것을 방지하려면 이 폴더를 `.gitignore` 파일에 추가해야 합니다. 스타터 킷 앱을 사용하는 경우 이 폴더가 사용자를 위해 작성되며 `.gitignore` 파일에 있습니다.

`mappings.json` 파일에 대한 자세한 정보는 [서비스 인증 정보 이해](#service_creds) 절을 확인하십시오.

## 스타터 킷 앱에서 Swift 구성 관리자 사용
{: #configmanager-swift}

[스타터 킷](https://cloud.ibm.com/developer/appledevelopment/starter-kits/){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")으로 작성된 Swift 앱은 로컬로, 그리고 여러 Cloud 배치 대상(CF, K8, VSI 및 Functions)에서도 실행하는 데 필요한 인증 정보 및 구성과 함께 자동으로 제공됩니다. 구성 관리자의 기본 작성은 `Sources/Application/Application.swift`에서 찾을 수 있습니다. 서비스가 포함된 Swift 기반 스타터 킷을 작성하는 경우 `config` 폴더 및 `mappings.json` 파일이 사용자를 위해 작성됩니다. 앱을 다운로드하는 경우 `config` 폴더는 사용자의 서비스를 위한 모든 인증 정보가 있는 `localdev-config.json` 파일을 포함하며 `.gitignore` 파일에 있습니다.

## 다음 단계
{: #next-configß notoc}

애플리케이션이 환경에서 자체적으로 추상화하는 데 도움이 되는 다음 세 가지 라이브러리를 확인하십시오.

* [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")
* [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")
* [Configuration](https://github.com/IBM-Swift/Configuration){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")
