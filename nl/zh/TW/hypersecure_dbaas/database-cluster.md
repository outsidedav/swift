---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: swift database, secure database swift, cluster database swift, mongokitten swift, verify database swift, credentials swift, storage api swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:gif: data-image-type='gif'}
{:tip: .tip}

# 建立可用性高且安全的資料庫
{: #create-database-cluster}

若要充分利用可用性高且安全的資料庫，請將額外的邏輯內含至您的應用程式。使用提供的程式碼 Snippet，即可建立及存取 MongoDB 資料庫。 

目前，支援使用 {{site.data.keyword.ihsdbaas_full}} 的程式設計語言為 Swift 4.0（含 MongoKitten SDK 4.0.0）。

## 步驟 1. 建立資料庫叢集
{: #create_dbcluster}

1. 存取 [{{site.data.keyword.ihsdbaas_full}} 服務配置](https://{DomainName}/catalog/services/hyper-protect-dbaas){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 畫面。

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
{: #create_starter}

您需要一個以伺服器端 Swift Web 架構 Kitura 為基礎的入門範本套件。

![特性詳細資料](videos/StarterKit.gif){: gif}

使用從這個入門範本套件建立而成的現有專案，或建立一個新專案。

1. 開啟 [{{site.data.keyword.cloud_notm}} 應用程式服務儀表板](https://{DomainName}/developer/appservice/dashboard){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")。

2. 選取**入門範本套件**標籤。

3. 選取 **Swift Kitura** Backend for Frontend 入門範本套件。請確定不要與「Swift Kitura Basic web-App 入門範本套件」混淆。即會顯示「建立新的專案」頁面。

4. 輸入專案詳細資料，然後按一下**建立專案**。即會顯示專案頁面。

5. 在專案頁面上，按一下**下載程式碼**。

6. 將壓縮檔展開到您的專案目錄。

## 步驟 3. 連接至資料庫
{: #connect_db}

若要確保資料傳送的安全，請下載憑證管理中心 (CA) 檔案，並將其複製到專案目錄。

1. 切換至您的專案目錄，其中包含已展開的下載程式檔。

2. 建立一個名稱為 `cred.json` 的 JSON 檔，以儲存您對資料庫叢集的存取認證。

3. 輸入從[建立資料庫叢集](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)的步驟收集到的值。該值必須指定在單一行中。
  ```hljs
  {
  "uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
  <Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
   /admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
  }
  ```
  {: codeblock}

### 資料庫參數說明
{: #db-parameter-descriptions}

請參閱下列資料庫參數說明：

* `admin_ID` - [建立資料庫叢集](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)中指定的資料庫管理者的使用者 ID。
* `admin_pwd` - [建立資料庫叢集](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)中指定的管理者密碼的使用者 ID。
* `Hostname_i` - [建立資料庫叢集](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)中傳回的資料庫抄本 *i* (*i*=1,2,3)。
* `PortNumber_i` - [建立資料庫叢集](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)中傳回的埠號 *i* (*i*=1,2,3)。
* `CA_file` - 下載的 CA 檔案的檔名。在部署期間，其會複製到 `/swift-project` 目錄。

4. 編輯 `Package.swift` 檔案，為了使用 MongoKitten SDK，新增套件相依關係。

  * 在相依關係區段中，新增下列這一行：
   ```swift
   .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
   ```
   {: codeblock}

  * 在目標區段中，將相依關係 "MongoKitten" 新增至下列這一行。**附註：** 該值必須指定在單一行中。
   ```swift
   .target(name: "Application", dependencies: [ "Kitura",
   "CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
   ```
   {: codeblock}

5. 編輯 `Sources/Application/Application.swift` 檔案，以使用 MongoKitten 來起始設定與 MongoDB 的連線功能。

  * 匯入 MongoKitten SDK：
		```swift
	  import MongoKitten
	  ```
	  {: codeblock}

  * 新增類別 `ApplicationServices`：
    ```swift
	  cclass ApplicationServices {
	  /* Service references */
	  public let mongoDBService: MongoKitten.Database
	  public let myCredFile = "/swift-project/cred.json"

    public init() throws {
        /* Read credentials from json file cred.json */
        struct ResponseData: Decodable {
            var uri: String
        }
        let data = try? Data(contentsOf: URL(fileURLWithPath: myCredFile))
        let decoder = JSONDecoder()
        let jsonData = try decoder.decode(ResponseData.self, from: data!)

        /* Run service initializers */
        let server = try Server(jsonData.uri)
        mongoDBService = MongoKitten.Database(named: "admin", atServer: 		server)
    }
	}
	```
	{: codeblock}

  * 在公用類別 `App` 中，新增下列這幾行以起始設定資料庫連線：
    ```swift
	  public class App {
	  ...
	  let services: ApplicationServices

	  public init() throws {
	  /* Services */
	  services = try ApplicationServices()
	  }
	  ...
    ```
    {: codeblock}

## 步驟 4. 驗證資料庫連線
{: #verify_database}

1. 驗證您的資料庫連線，方法為編輯 `Sources/Application/Application.swift` 檔案，以新增指令來測試資料庫連線。例如，在 `class ApplicationServices` 中，新增下列指令：

	```swift
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

```
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## 步驟 5. 內含應用程式碼
{: #embed_appcode}

您現在可以將自己的應用程式碼新增至專案。如需相關資訊，請參閱 [MongoKitten API](http://beta.openkitten.org/tutorials/){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 文件。

## 步驟 6. 部署應用程式
{: #deploy-dbcluster}

您可以使用必要的建置工具，在[本端](/docs/swift?topic=swift-swift_cli#swift-install-tools)執行應用程式，或部署至 {{site.data.keyword.cloud_notm}}。

若要在儀表板中建立部署工具鏈，請按一下**部署**。根據所選擇方法的指示，設定部署目標：
  * **部署到 IBM Kubernetes Service**。此選項可建立主機（稱為工作者節點）的叢集，以部署及管理高可用性應用程式容器。您可以建立叢集或部署至現有的叢集。如需相關資訊，請參閱[將應用程式部署到 Kubernetes 叢集](/docs/containers?topic=containers-app)。
  * **部署至 Cloud Foundry**。此選項可部署雲端原生應用程式，您不需要管理基礎架構。如果您的帳戶有權存取 {{site.data.keyword.cfee_full_notm}}，則可以選取**公用雲端**或**企業環境**的部署者類型，您能夠使用此類型來建立及管理隔離環境，專供您的企業用來管理 Cloud Foundry 應用程式。如需相關資訊，請參閱[將應用程式部署到 Cloud Foundry Public](/docs/cloud-foundry-public?topic=cloud-foundry-public-deployingapps) 和[將應用程式部署到 {{site.data.keyword.cfee_full_notm}}](/docs/cloud-foundry?topic=cloud-foundry-deploy_apps)。
  * **部署到虛擬伺服器**。此選項可佈建虛擬伺服器實例、載入包含應用程式的映像檔、建立 DevOps 工具鏈，以及為您起始第一個部署週期。如需相關資訊，請參閱[將應用程式部署到虛擬伺服器](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server)。
  
