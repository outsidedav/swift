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

# 创建高可用性的安全数据库
{: #create-database-cluster}

为了充分利用高可用性的安全数据库，可以在应用程序中嵌入额外的逻辑。通过使用提供的代码片段，可以创建和访问 MongoDB 数据库。 

目前，使用 {{site.data.keyword.ihsdbaas_full}} 时受支持的编程语言是带有 MongoKitten SDK 4.0.0 的 Swift 4.0。

## 步骤 1. 创建数据库集群
{: #create_dbcluster}

1. 访问[{{site.data.keyword.ihsdbaas_full}}服务配置](https://{DomainName}/catalog/services/hyper-protect-dbaas){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 屏幕。

2. 提供以下信息：

	<dl>
		<dt>服务名称：</dt>
		<dd>数据库服务的名称。</dd>

    <dt>选择要在其中部署的区域/位置：</dt>
    <dd>选择数据库所在的数据中心。</dd>

    <dt>选择资源组：</dt>
    <dd>如果没有资源组可选择，那么可以在 IBM Cloud“仪表板”上创建资源组。</dd>

		<dt>集群/副本集名称：</dt>
		<dd>数据库集群的名称。</dd>

		<dt>数据库管理员名称：</dt>
		<dd>数据库管理员 (DBA) 的用户标识。</dd>

		<dt>数据库管理员密码：</dt>
		<dd>DBA 用户标识的密码。</dd>

    <dt>确认数据库管理员密码：</dt>
    <dd>确认 DBA 用户标识的密码。</dd>

		<dt>数据库类型：</dt>
		<dd>选择数据库类型。目前，仅支持 MongoDB。</dd>

    <dt>许可协议：</dt>
    <dd>阅读许可协议，并选中该框以确认您同意。</dd>

    <dt>定价：</dt>
		<dd>当前解决方案仅提供一个定价类别，该类别是免费的。在更高版本中，可以选择定价类别。</dd>
	</dl>

3. 单击**创建**。

  这将显示 {{site.data.keyword.cloud_notm}}“仪表板”。您可能必须刷新浏览器才能看到新集群在“服务”部分中列出。</p></li>

4. 选择服务时，将显示集群信息。

5. 在集群信息的“管理”选项卡中，单击**打开**。

	这将显示 {{site.data.keyword.ihsdbaas_full}}“仪表板”。

6. 收集三个所创建数据库实例（属于您的数据库集群）的主机名和端口号。要执行[连接到数据库](#connect_db)部分中的步骤，您需要有主机名、端口号和用户凭证。

## 步骤 2. 使用入门模板工具包创建项目
{: #create_starter}

您需要基于服务器端 Swift Web 框架 Kitura 的入门模板工具包。

![功能部件详细信息](videos/StarterKit.gif){: gif}

使用通过此入门模板工具包创建的现有项目，或者创建新项目。

1. 打开 [{{site.data.keyword.cloud_notm}} App Service 仪表板](https://{DomainName}/developer/appservice/dashboard){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。

2. 选择**入门模板工具包**选项卡。

3. 选择 **Swift Kitura 服务于前端的后端**入门模板工具包。一定不要将它与“Swift Kitura Basic Web 应用程序”入门模板工具包相混淆。这将显示“新建项目”页面。

4. 输入项目详细信息，然后单击**创建项目**。这将显示项目页面。

5. 在项目页面上，单击**下载代码**。

6. 将压缩文件展开到项目目录中。

## 步骤 3. 连接到数据库
{: #connect_db}

要确保数据传输的安全，请下载认证中心 (CA) 文件，并将其复制到项目目录。

1. 切换到包含已下载并且已展开的代码文件的项目目录。

2. 创建名为 `cred.json` 的 JSON 文件，以将访问凭证存储到数据库集群。

3. 输入从[创建数据库集群](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)的步骤中收集的值。这些值必须在一行中指定。
  ```hljs
  {
  "uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
  <Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
   /admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
  }
  ```
  {: codeblock}

### 数据库参数描述
{: #db-parameter-descriptions}

请参阅以下数据库参数描述：

* `admin_ID` - [创建数据库集群](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)中指定的数据库管理员的用户标识。
* `admin_pwd` - [创建数据库集群](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)中指定的管理员密码的用户标识。
* `Hostname_i` - [创建数据库集群](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)中返回的数据库副本 *i* (*i*=1,2,3)。
* `PortNumber_i` - [创建数据库集群](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)中返回的端口号 *i* (*i*=1,2,3)。
* `CA_file` - 下载的 CA 文件的文件名。部署期间，会将其复制到 `/swift-project` 目录。

4. 编辑 `Package.swift` 文件并添加包依赖项，以使用 MongoKitten SDK。

  * 在 dependencies 部分中，添加以下行：
			
   ```swift
   .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
   ```
   {: codeblock}

  * 在 targets 部分中，将依赖项“MongoKitten”添加到以下行。**注：**这些值必须在一行中指定。
			
   ```swift
   .target(name: "Application", dependencies: [ "Kitura",
   "CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
   ```
   {: codeblock}

5. 编辑 `Sources/Application/Application.swift` 文件，以使用 MongoKitten 初始化与 MongoDB 的连接。

  * 导入 MongoKitten SDK：
		```swift
	  import MongoKitten
	  ```
	  {: codeblock}

  * 添加 `ApplicationServices` 类：
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

  * 在公共类 `App` 中，添加以下行以初始化数据库连接：
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

## 步骤 4. 验证数据库连接
{: #verify_database}

1. 通过编辑 `Sources/Application/Application.swift` 文件并添加用于测试数据库连接的命令，以验证数据库连接。例如，在 `class ApplicationServices` 中添加以下命令：

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

在[步骤 6](#use-step6) 中部署应用程序后，将显示以下消息（如果数据库连接成功）：

```
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## 步骤 5. 嵌入应用程序代码
{: #embed_appcode}

现在，您可以将自己的应用程序代码添加到项目。有关更多信息，请参阅 [MongoKitten API](http://beta.openkitten.org/tutorials/){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 文档。

## 步骤 6. 部署应用程序
{: #deploy-dbcluster}

您可以使用必要的构建工具[在本地](/docs/swift?topic=swift-swift_cli#swift-install-tools)运行应用程序，也可以将其部署到 {{site.data.keyword.cloud_notm}}。

要在仪表板中创建部署工具链，请单击**部署**。根据您所选方法的指示信息来设置部署目标：
  * **部署到 IBM Kubernetes Service**。此选项将创建一个主机集群（称为工作程序节点）来部署和管理高可用性应用程序容器。可以创建集群或部署到现有集群。有关更多信息，请参阅[将应用程序部署到 Kubernetes 集群](/docs/containers?topic=containers-app)。
  * **部署到 Cloud Foundry**。此选项可部署云本机应用程序，而无需管理底层基础架构。如果您的帐户有权访问 {{site.data.keyword.cfee_full_notm}}，那么可以选择部署程序类型**公共云**或 **Enterprise Environment**，可使用这些类型来创建和管理隔离的环境，以用于专门为您的企业托管 Cloud Foundry 应用程序。有关更多信息，请参阅[将应用程序部署到 Cloud Foundry Public](/docs/cloud-foundry-public?topic=cloud-foundry-public-deployingapps) 和[将应用程序部署到 {{site.data.keyword.cfee_full_notm}}](/docs/cloud-foundry?topic=cloud-foundry-deploy_apps)。
  * **部署到虚拟服务器**。此选项会供应虚拟服务器实例，装入包含您的应用程序的映像，创建 DevOps 工具链，并为您启动第一个部署周期。有关更多信息，请参阅[将应用程序部署到虚拟服务器](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server)。
  
