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

# 建立可用性高且安全的資料庫

若要充分利用可用性高且安全的資料庫，請將額外的邏輯內含至您的應用程式。使用提供的程式碼 Snippet，即可建立及存取 MongoDB 資料庫。 

目前，支援使用 {{site.data.keyword.ihsdbaas_full}} 的程式設計語言為 Swift 4.0（含 MongoKitten SDK 4.0.0）。

## 步驟 1. 建立資料庫叢集
{: #create_dbcluster}

1. 存取 {{site.data.keyword.ihsdbaas_full}} 服務配置畫面，其位於
https://console.bluemix.net/catalog/services/hyper-protect-dbaas.

2. 提供下列資訊：

	<dl>
		<dt>服務名稱：</dt>
		<dd>資料庫服務的名稱。</dd>

    <dt>選取要在其中部署的地區/位置：</dt>
    <dd>選取資料庫所在的資料中心。</dd>

    <dt>選取資源群組：</dt>
    <dd>如果沒有可供選取的資源群組，您可以在 IBM Cloud 儀表板上建立一個。</dd>

		<dt>叢集/抄本集名稱：</dt>
		<dd>資料庫叢集的名稱。</dd>

		<dt>資料庫管理者名稱：</dt>
		<dd>資料庫管理者 (DBA) 的使用者 ID。</dd>

		<dt>資料庫管理者密碼：</dt>
		<dd>DBA 使用者 ID 的密碼。</dd>

    <dt>確認資料庫管理者密碼：</dt>
    <dd>確認 DBA 使用者 ID 的密碼。</dd>

		<dt>資料庫類型：</dt>
		<dd>選取資料庫類型。目前，僅支援 MongoDB。</dd>

    <dt>授權合約：</dt>
    <dd>閱讀授權合約，然後勾選方框以確認合約。</dd>

    <dt>定價：</dt>
		<dd>現行的解決方案僅提供一個定價種類，也就是免費。在後續版本中，您可以選取定價種類。</dd>
	</dl>

3. 按一下**建立**。

  即會顯示 {{site.data.keyword.cloud_notm}} 儀表板。您可能需要重新整理瀏覽器，才能看到新叢集，其列示在「服務」區段中。</p></li>

4. 選取服務後，即會顯示叢集資訊。

5. 在叢集資訊的「管理」標籤中，按一下**開啟**。

	即會顯示 {{site.data.keyword.ihsdbaas_full}} 儀表板。

6. 針對已建立且屬於您資料庫叢集的三個資料庫實例，收集這三個資料庫實例的主機名稱及埠號。在[連接至資料庫](#connect_db)小節的步驟中，您需要主機名稱、埠號及使用者認證。

## 步驟 2. 使用入門範本套件來建立專案
{: #create_with_starter}

您需要一個以伺服器端 Swift Web 架構 Kitura 為基礎的入門範本套件。

![特性詳細資料](videos/StarterKit.gif){: gif}

使用從這個入門範本套件建立而成的現有專案，或建立一個新專案。

1. 開啟 {{site.data.keyword.cloud_notm}} App Service 儀表板，網址為 https://console.bluemix.net/developer/appservice/dashboard。

2. 選取**入門範本套件**標籤。

3. 選取 **Swift Kitura** Backend for Frontend 入門範本套件。請確定不要與「Swift Kitura Basic web-App 入門範本套件」混淆。
  即會顯示「建立新的專案」頁面。

4. 輸入專案詳細資料，然後按一下**建立專案**。
  即會顯示專案頁面。

5. 在專案頁面上，按一下**下載程式碼**。

6. 將壓縮檔展開到您的專案目錄。

## 步驟 3. 連接至資料庫
{: #connect_db}

若要確定資料傳送安全，請從 https://api.hypersecuredbaas.ibm.com/cert.pem 下載憑證管理中心 (CA) 檔案，並將它複製到您的專案目錄。

1. 切換至您的專案目錄，其中包含已展開的下載程式檔。

2. 建立一個名為 `cred.json` 的 JSON 檔，以儲存您對資料庫叢集的存取認證。

3. 輸入從[建立資料庫叢集](#create_dbcluster)的步驟收集到的值。該值必須指定在單一行中。
  ```hljs
  {
  "uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
  <Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
   /admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
  }
  ```
  {: codeblock}

  其中：
  <table>
  <tr>
    <th> 參數 </th>
    <th> 說明 </th>
  </tr>
  <tr>
    <td> &lt;<em>admin_ID</em>&gt; </td>
    <td> 資料庫管理者的使用者 ID，如[建立資料庫叢集](#create_dbcluster)中所指定。
	  </td>
  </tr>
  <tr>
    <td> &lt;<em>admin_pwd</em>&gt; </td>
    <td> 管理者密碼的使用者 ID，如[建立資料庫叢集](#create_dbcluster)中所指定。</td>
  </tr>
  <tr>
    <td> &lt;<em>Hostname_i</em>&gt; </td>
    <td> 資料庫抄本 <em>i</em> (<em>i</em>=1,2,3)，如[建立資料庫叢集](create_dbcluster)中所傳回。</td>
  </tr>
  <tr>
    <td> &lt;<em>PortNumber_i</em>&gt; </td>
    <td> 埠號 <em>i</em> (<em>i</em>=1,2,3)，如[建立資料庫叢集](#create_dbcluster)中所傳回。</td>
  </tr>
  <tr>
    <td> &lt;<em>CA_file</em>&gt; </td>
    <td> 已下載 CA 檔案的檔名。在部署期間，其會複製到 `/swift-project` 目錄。</td>
  </tr>
  </table>

4. 編輯 `Package.swift` 檔案，為了使用 MongoKitten SDK，新增套件相依關係。

  * 在相依關係區段中，新增下列這一行：
			
   ```hljs
   .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
   ```
   {: codeblock}

  * 在目標區段中，將相依關係 "MongoKitten" 新增至下列這一行。**附註：** 該值必須指定在單一行中。
			
   ```hljs
   .target(name: "Application", dependencies: [ "Kitura",
   "CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
   ```
   {: codeblock}

5. 編輯 `Sources/Application/Application.swift` 檔案，以使用 MongoKitten 來起始設定與 MongoDB 的連線功能。

  * 匯入 MongoKitten SDK：
		```
	import MongoKitten
		```
	{: codeblock}

  * 新增類別 `ApplicationServices`：
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

  * 在公用類別 `App` 中，新增下列這幾行以起始設定資料庫連線：
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

## 步驟 4. 驗證資料庫連線
{: #verify_database}

1. 驗證您的資料庫連線，方法為編輯 `Sources/Application/Application.swift` 檔案，以新增指令來測試資料庫連線。例如，在 `class ApplicationServices` 中，新增下列指令：

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

在[步驟 6](#use-step6) 中部署應用程式之後，如果您的資料庫連線成功，則會顯示下列訊息：

```hljs
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## 步驟 5. 內含應用程式碼
{: #embed_appcode}

您現在可以將自己的應用程式碼新增至專案。如需使用 MongoKitten API 的相關資訊，請參閱 http://beta.openkitten.org/tutorials/

## 步驟 6. 部署應用程式
{: #deploy_app}

您可以使用必要的建置工具，在本端執行應用程式，或透過 {{site.data.keyword.dev_cli_notm}}，在 {{site.data.keyword.cloud_notm}}（Cloud Foundry 或「Kubernetes 叢集」）中執行。

您可以在主機系統上，於本端執行應用程式，或在 Cloud Foundry 中或「Kubernetes 叢集」中執行。

1. [安裝](/docs/cli/reference/bluemix_cli/get_started.html) {{site.data.keyword.cloud_notm}} CLI

2. 使用 `ibmcloud plugin install dev` 指令來安裝 Developer Tools 外掛程式。

3. 將應用程式部署至[本端系統](#deploy_local)、[Cloud Foundry](#deploy_cf) 或 [Kubernetes 叢集](#deploy_cluster)。

### 本端部署
{: #deploy_local}

1. 確定 Docker 已安裝，且在您的本端主機系統上執行。您可以從 https://www.docker.com/community-edition#/download 中下載 Docker。

2. 切換至包含您專案檔的目錄。

3. 若要在本端電腦上部署應用程式，請輸入以下指令：
	```
	$ ibmcloud dev build
	...
	$ ibmcloud dev run
	```
	{: codeblock}

	這個步驟會建置您的應用程式，並在 Docker 容器內於本端執行。

### 部署至 Cloud Foundry
{: #deploy_cf}

1. 切換至包含您專案檔的目錄。

2. 登入您的 IBM Cloud 帳戶，然後將地區設為 `us-south`，如這裡所示：
	
  ```hljs
  $ ibmcloud login -a https://api.ng.bluemix.net
  $ ibmcloud target -o &lt;<em>your-organization</em>&gt; -s &lt;<em>your-space</em>&gt;
  ```
  {: codeblock}

  **附註：**發出 `ibmcloud login -a https://api.ng.bluemix.net` 指令，會自動將地區設為 **us-south**。

3. 若要將應用程式部署至 Cloud Foundry，請輸入這個指令：
	
  ```
$ ibmcloud dev deploy
	```
  {: codeblock}

  您會接收到一個可按一下的鏈結，連接至管理應用程式所在的位置。

### 部署至 Kubernetes 叢集
{: #deploy_cluster}

1. 在 https://console.bluemix.net/containers-kubernetes/clusters 中，建立一個 Kubernetes 叢集。

2. 按一下**建立叢集**。「存取」標籤會顯示資訊，指出如何存取已建立的 Kubernetes 叢集。

3. 若要顯示 Kubernetes 叢集的相關資訊，請開啟 {{site.data.keyword.cloud_notm}} 應用程式儀表板。儀表板會顯示您的服務清單，例如，已建立的叢集、資料庫叢集、Cloud Foundry 應用程式以及 Cloud Foundry 服務。

4. 切換至包含您專案檔的目錄。

5. 登入您的 {{site.data.keyword.cloud_notm}} 帳戶，然後將地區設為 us-south，如這裡所示：
	
  ```hljs
  $ ibmcloud login -a https://api.ng.bluemix.net
  $ ibmcloud target -o <your-organization> -s <your-space>
  ```
  {: codeblock}

  **附註：**發出 `ibmcloud login -a https://api.ng.bluemix.net` 指令，會自動將地區設為 **us-south**。

6. 若要在 Kubernetes 中部署應用程式，請輸入這個指令：
	
  ```
    $ ibmcloud dev deploy -t container
    ```
  {: codeblock}

  系統會提示您輸入 Kubernetes 叢集的名稱及 Docker 登錄。在提供資訊之後，您的應用程式即會部署至 Kubernetes 叢集。
