---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:gif: .gif}
{:tip: .tip}

# 가용성이 높고 안전한 데이터베이스 작성

가용성이 높고 안전한 데이터베이스를 최대한 활용하기 위해 추가 로직을 애플리케이션에 임베드합니다. 제공된 코드 스니펫을 사용하여 MongoDB 데이터베이스를 작성하고 액세스할 수 있습니다. 

{{site.data.keyword.ihsdbaas_full}} 사용을 위해 현재 지원되는 프로그래밍 언어는
MongoKitten SDK 4.0.0이 포함된 Swift 4.0입니다. 

## 1단계. 데이터베이스 클러스터 작성
{: #create_dbcluster}

1. 다음 웹 사이트에서 {{site.data.keyword.ihsdbaas_full}} 서비스 구성 화면에 액세스하십시오.
https://console.bluemix.net/catalog/services/hyper-protect-dbaas.

2. 다음 정보를 제공하십시오.

	<dl>
		<dt>서비스 이름:</dt>
		<dd>데이터베이스 서비스의 이름입니다.</dd>

    <dt>배치할 지역/위치 선택:</dt>
    <dd>데이터베이스가 위치한 데이터 센터를 선택합니다.</dd>

    <dt>리소스 그룹 선택:</dt>
    <dd>리소스 그룹을 선택할 수 없는 경우 IBM Cloud 대시보드에서 리소스 그룹을 작성할 수 있습니다.</dd>

		<dt>클러스터/복제 세트 이름:</dt>
		<dd>데이터베이스 클러스터의 이름입니다.</dd>

		<dt>데이터베이스 관리 이름:</dt>
		<dd>데이터베이스 관리자(DBA)의 사용자 ID입니다.</dd>

		<dt>데이터베이스 관리 비밀번호:</dt>
		<dd>DBA 사용자 ID의 비밀번호입니다.</dd>

    <dt>데이터베이스 관리 비밀번호 확인:</dt>
    <dd>DBA 사용자 ID의 비밀번호를 확인합니다.</dd>

		<dt>데이터베이스 유형:</dt>
		<dd>데이터베이스 유형을 선택합니다. 현재 MongoDB만 지원됩니다.</dd>

    <dt>라이센스 계약</dt>
    <dd>라이센스 계약을 읽고 계약을 확인했으면 선택란을 선택하십시오.</dd>

    <dt>가격 책정:</dt>
		<dd>현재 솔루션은 하나의 가격 책정 카테고리만 무료로 제공합니다. 이후 버전에서는 가격 책정 카테고리를 선택할 수 있습니다.</dd>
	</dl>

3. **작성**을 클릭하십시오.

  {{site.data.keyword.cloud_notm}} 대시보드가 표시됩니다. 서비스 섹션에 나열된 새 클러스터를 보려면 브라우저를 새로 고쳐야 할 수도 있습니다.</p></li>

4. 서비스를 선택하면 클러스터 정보가 표시됩니다.

5. 클러스터 정보의 관리 탭에서 **열기**를 클릭하십시오.

	{{site.data.keyword.ihsdbaas_full}} 대시보드가 표시됩니다.

6. 호스트 이름과 데이터베이스 클러스터에 속하는 작성된 세 가지 데이터베이스 인스턴스의 포트 번호를 수집하십시오. [데이터베이스에 연결](#connect_db) 섹션의 단계에서 호스트 이름, 포트 번호 및 사용자 인증 정보가 필요합니다.

## 2단계. 스타터 킷을 사용하여 프로젝트 작성
{: #create_with_starter}

서버 측 Swift 웹 프레임워크 Kitura를 기반으로 한 스타터 킷이 필요합니다.

![기능 세부사항](videos/StarterKit.gif){: gif}

스타터 킷에서 작성된 기존 프로젝트를 사용하거나 새 프로젝트를 작성하십시오.

1. https://console.bluemix.net/developer/appservice/dashboard 에서 {{site.data.keyword.cloud_notm}} 앱 서비스 대시보드를 여십시오.

2. **스타터 킷** 탭을 선택하십시오.

3. 프론트 엔드 스타터 킷에 적합한 **Swift Kitura** 백엔드를 선택하십시오. Swift Kitura 기본 웹 앱 스타터 킷과 혼동하지 마십시오.
  새 프로젝트 작성 페이지가 표시됩니다.

4. 프로젝트 세부사항을 입력하고 **프로젝트 작성**을 클릭하십시오.
  프로젝트 페이지가 표시됩니다.

5. 프로젝트 페이지에서 **코드 다운로드**를 클릭하십시오.

6. 프로젝트 디렉토리에 압축한 파일의 압축을 푸십시오. 

## 3단계. 데이터베이스에 연결
{: #connect_db}

안전한 데이터 전송을 위해 다음 웹 사이트에서 인증 기관(CA) 파일을 다운로드하고 프로젝트 디렉토리에 이를 복사하십시오. 
https://api.hypersecuredbaas.ibm.com/cert.pem, and copy it to your project directory.

1. 압축을 푼 다운로드된 코드 파일이 있는 프로젝트 디렉토리로 변경하십시오.

2. `cred.json`이라고 하는 JSON 파일을 작성하여 액세스 인증 정보를 데이터베이스 클러스터에 저장하십시오.

3. [데이터베이스 클러스터 작성](#create_dbcluster)의 단계에서 수집된 값을 입력하십시오. 값은 한 행에 지정해야 합니다.
  ```hljs
  {
  "uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
  <Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
   /admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
  }
  ```
  {: codeblock}

  여기서,
  <table>
  <tr>
    <th> 매개변수 </th>
    <th> 설명 </th>
  </tr>
  <tr>
    <td> &lt;<em>admin_ID</em>&gt; </td>
    <td> [데이터베이스 클러스터 작성](#create_dbcluster)에서 지정된 데이터베이스 관리자의 사용자 ID입니다.
  </td>
  </tr>
  <tr>
    <td> &lt;<em>admin_pwd</em>&gt; </td>
    <td> [데이터베이스 클러스터 작성](#create_dbcluster)에서 지정된 관리자 비밀번호의 사용자 ID입니다. </td>
  </tr>
  <tr>
    <td> &lt;<em>Hostname_i</em>&gt; </td>
    <td> [데이터베이스 클러스터 작성](create_dbcluster)에서 리턴된 데이터베이스 복제본 <em>i</em>(<em>i</em>=1,2,3)입니다. </td>
  </tr>
  <tr>
    <td> &lt;<em>PortNumber_i</em>&gt; </td>
    <td> [데이터베이스 클러스터 작성](#create_dbcluster)에서 리턴된 포트 번호 <em>i</em>(<em>i</em>=1,2,3)입니다. </td>
  </tr>
  <tr>
    <td> &lt;<em>CA_file</em>&gt; </td>
    <td> 다운로드된 CA 파일의 파일 이름입니다. 배치 중에 `/swift-project` 디렉토리에 복사됩니다.</td>
  </tr>
  </table>

4. `Package.swift` 파일을 편집하여 MongoKitten SDK 사용을 위한 패키지 종속성을
복사하십시오.

  * 종속성 섹션에서 다음 행을 추가하십시오.
   ```hljs
   .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
   ```
   {: codeblock}

  * 대상 섹션에서 다음 행에 종속성 "MongoKitten"을 추가하십시오. **참고:** 값은 한 행에 지정해야 합니다.
   ```hljs
   .target(name: "Application", dependencies: [ "Kitura",
   "CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
   ```
   {: codeblock}

5. `Sources/Application/Application.swift` 파일을 편집하여 MongoKitten 사용을 통해 MongoDB에 대한 연결을 초기화십시오.

  * MongoKitten SDK를 가져오십시오.
    ```
	import MongoKitten
	```
	{: codeblock}

  * 클래스 `ApplicationServices`를 추가하십시오.
    ```hljs
	cclass ApplicationServices {
	// Service references
	    public let mongoDBService: MongoKitten.Database
	    public let myCredFile = "/swift-project/cred.json"

    public init() throws {
        // Read credentials from json file cred.json
	        struct ResponseData: Decodable {
            var uri: String
	        }
	        let data = try? Data(contentsOf: URL(fileURLWithPath: myCredFile))
	        let decoder = JSONDecoder()
	        let jsonData = try decoder.decode(ResponseData.self, from: data!)

        // Run service initializers
	        let server = try Server(jsonData.uri)
	        mongoDBService = MongoKitten.Database(named: "admin", atServer: 		server)
	    }
	}
	```
	{: codeblock}

  * 공용 클래스 `App`에서 다음 행을 추가하여 데이터베이스 연결을 초기화하십시오.
    ```hljs
	public class App {
	...
	let services: ApplicationServices

	public init() throws {
	   // Services
	    services = try ApplicationServices()
	 }
	...
    ```
    {: codeblock}

## 4단계. 데이터베이스 연결 확인
{: #verify_database}

1. `Sources/Application/Application.swift` 파일을 편집하고 데이터베이스 연결을 테스트하는 명령을 추가하여 데이터베이스 연결을 확인하십시오.
예를 들어, `class ApplicationServices`에서 다음 명령을 추가하십시오.

	```hljs
		class ApplicationServices {
		    ...
		    public init() throws {
		        ...
		        let server = try Server(jsonData.uri)
		        mongoDBService = MongoKitten.Database(named: "admin", atServer: server)
		        if server.isConnected {
		            print("Connected to mongodb: " + String(describing: mongoDBService))
		        }
		        ...
		    }
		}
	```
	{: codeblock}

[6단계](#use-step6)에서 설명된 대로 애플리케이션을 배치한 후 데이터베이스에 연결되면
다음 메시지가 표시됩니다.

```hljs
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## 5단계. 애플리케이션 코드 임베드
{: #embed_appcode}

이제 고유한 애플리케이션 코드를 프로젝트에 추가할 수 있습니다. MongoKitten API 작업에 대한 자세한 정보는 http://beta.openkitten.org/tutorials/ 를 참조하십시오.

## 6단계. 애플리케이션 배치
{: #deploy_app}

필요한 빌드 도구를 사용하여 로컬로 또는 {{site.data.keyword.dev_cli_notm}}를 통해 {{site.data.keyword.cloud_notm}}(Cloud Foundry 또는 Kubernetes 클러스터)에서 애플리케이션을 실행할 수 있습니다.

애플리케이션을 호스트 시스템에서 로컬로 또는 Cloud Foundry 또는 Kubernetes 클러스터에서 실행할 수 있습니다.

1. {{site.data.keyword.cloud_notm}} CLI를 [설치](/docs/cli/reference/bluemix_cli/get_started.html)하십시오.

2. `ibmcloud plugin install dev` 명령을 사용하여 개발자 도구 플러그인을 설치하십시오.

3. 애플리케이션을 [로컬 시스템](#deploy_local), [Cloud Foundry](#deploy_cf) 또는 [Kubernetes 클러스터](#deploy_cluster)에 배치하십시오.

### 로컬로 배치
{: #deploy_local}

1. Docker가 설치되어 있고 로컬 호스트 시스템에서 실행 중인지 확인하십시오. https://www.docker.com/community-edition#/download 에서 Docker를 다운로드할 수 있습니다.

2. 프로젝트 파일이 포함된 디렉토리로 전환하십시오.

3. 애플리케이션을 로컬 컴퓨터에 배치하려면 다음 명령을 입력하십시오.
	```
	$ ibmcloud dev build
	...
	$ ibmcloud dev run
	```
	{: codeblock}

	이 단계에서는 애플리케이션을 빌드하고 Docker 컨테이너 내부에서 애플리케이션을 로컬로 실행합니다.

### Cloud Foundry에 배치
{: #deploy_cf}

1. 프로젝트 파일이 포함된 디렉토리로 전환하십시오.

2. 다음에 표시된 대로 IBM Cloud 계정에 로그인하고 지역을 `us-south`로 설정하십시오.
  ```hljs
  $ ibmcloud login -a https://api.ng.bluemix.net
  $ ibmcloud target -o &lt;<em>your-organization</em>&gt; -s &lt;<em>your-space</em>&gt;
  ```
  {: codeblock}

  **참고:** `ibmcloud login -a https://api.ng.bluemix.net` 명령을 실행하면 지역이 **us-south**로 자동 설정됩니다.

3. 애플리케이션을 Cloud Foundry에 배치하려면 다음 명령을 입력하십시오.
  ```
$ ibmcloud dev deploy
  ```
  {: codeblock}

  애플리케이션이 호스팅되는 위치에 대해 클릭 가능한 링크를 받게 됩니다.

### Kubernetes 클러스터에 배치
{: #deploy_cluster}

1. https://console.bluemix.net/containers-kubernetes/clusters 에서 Kubernetes 클러스터를 작성하십시오.

2. **클러스터 작성**을 클릭하십시오. 액세스 탭에는 작성된 Kubernetes 클러스터에 액세스하는 방법에 대한 정보가 표시됩니다.

3. Kubernetes 클러스터에 대한 정보를 표시하려면 {{site.data.keyword.cloud_notm}} 앱 대시보드를 여십시오. 대시보드에는 작성된 클러스터, 데이터베이스 클러스터, Cloud Foundry 앱 및 Cloud Foundry 서비스와 같은 서비스의 목록이 표시됩니다.

4. 프로젝트 파일이 포함된 디렉토리로 전환하십시오.

5. 다음에 표시된 대로 {{site.data.keyword.cloud_notm}} 계정에 로그인하고 지역을 us-south로 설정하십시오.
  ```hljs
  $ ibmcloud login -a https://api.ng.bluemix.net
  $ ibmcloud target -o <your-organization> -s <your-space>
  ```
  {: codeblock}

  **참고:** `ibmcloud login -a https://api.ng.bluemix.net` 명령을 실행하면 지역이 **us-south**로 자동 설정됩니다.

6. Kubernetes에서 애플리케이션을 배치하려면 다음 명령을 입력하십시오.
  ```
    $ ibmcloud dev deploy -t container
  ```
  {: codeblock}

  Kubernetes 클러스터 및 Docker 레지스트리의 이름에 대해 프롬프트됩니다. 정보가 제공되면 애플리케이션이 Kubernetes 클러스터에 배치됩니다.
