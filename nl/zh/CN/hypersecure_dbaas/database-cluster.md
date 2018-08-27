---

copyright:
  years: 2018
lastupdated: "2018-08-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:gif: .gif}
{:tip: .tip}

# 创建高可用性的安全数据库

为了充分利用高可用性的安全数据库，可以在应用程序中嵌入额外的逻辑。通过使用提供的代码片段，可以创建和访问 MongoDB 数据库。 

目前，通过 MongoKitten SDK 4.0.0 支持使用 {{site.data.keyword.ihsdbaas_full}} 的编程语言是 Swift 4.0。

## 步骤 1. 创建数据库集群
{: #create_dbcluster}

1. 访问 {{site.data.keyword.ihsdbaas_full}} 服务配置屏幕：
https://console.bluemix.net/catalog/services/hyper-protect-dbaas.

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
		<dd>当前解决方案仅提供一个定价类别：免费。在更高版本中，可以选择定价类别。</dd>
	</dl>

3. 单击**创建**。

  这将显示 {{site.data.keyword.cloud_notm}}“仪表板”。您可能必须刷新浏览器才能看到新集群在“服务”部分中列出。</p></li>

4. 选择服务时，将显示集群信息。

5. 在集群信息的“管理”选项卡中，单击**打开**。

	这将显示 {{site.data.keyword.ihsdbaas_full}}“仪表板”。

6. 获取属于数据库集群的三个已创建数据库实例的主机名和端口号。您需要主机名、端口号和用户凭证以用于[连接到数据库](#connect_db)部分中的各步骤。

## 步骤 2. 使用入门模板工具包创建项目
{: #create_with_starter}

您需要基于服务器端 Swift Web 框架 Kitura 的入门模板工具包。

![功能部件详细信息](videos/StarterKit.gif){: gif}

使用通过此入门模板工具包创建的现有项目，或者创建新项目。

1. 打开 {{site.data.keyword.cloud_notm}} App Service 仪表板：https://console.bluemix.net/developer/appservice/dashboard.

2. 选择**入门模板工具包**选项卡。

3. 选择 **Swift Kitura 服务于前端的后端**入门模板工具包。一定不要将它与“Swift Kitura Basic Web 应用程序”入门模板工具包相混淆。这将显示“新建项目”页面。

4. 输入项目详细信息，然后单击**创建项目**。这将显示项目页面。

5. 在项目页面上，单击**下载代码**。

6. 将下载的 zip 文件展开到项目目录。

## 步骤 3. 连接到数据库
{: #connect_db}

要确保安全数据传输，请从以下目录下载认证中心 (CA) 文件：
https://api.hypersecuredbaas.ibm.com/cert.pem, and copy it to your project directory.

1. 切换到项目目录，其中包含展开的下载代码文件。

2. 创建名为 `cred.json` 的 JSON 文件，以将访问凭证存储到数据库集群。

3. 输入因[创建数据库集群](#create_dbcluster)而获取的值。这些值必须在一行中指定。

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
	    <th> 参数    </th>
	    <th> 描述</th>
	  </tr>
	  <tr>
	    <td> &lt;<em>admin_ID</em>&gt;</td>
	    <td> [创建数据库集群](#create_dbcluster)中指定的数据库管理员的用户标识。
	  </td>
	  </tr>
	  <tr>
	    <td> &lt;<em>admin_pwd</em>&gt;</td>
	    <td> [创建数据库集群](#create_dbcluster)中指定的数据库管理员密码。
	  </td>
	  </tr>
	  <tr>
	    <td> &lt;<em>Hostname_i</em>&gt;</td>
	    <td> [创建数据库集群](create_dbcluster)中返回的数据库副本 <em>i</em>（<em>i</em>=1、2、3）。</td>
	  </tr>
	  <tr>
	    <td> &lt;<em>PortNumber_i</em>&gt;</td>
	    <td> [创建数据库集群](#create_dbcluster)中返回的端口号 <em>i</em>（<em>i</em>=1、2、3）。</td>
	  </tr>
	  <tr>
	    <td> &lt;<em>CA_file</em>&gt;</td>
	    <td> 下载的 CA 文件的文件名。部署期间，会将其复制到 `/swift-project` 目录。</td>
	  </tr>
	</table>

4. 编辑 `Package.swift` 文件并添加包依赖项，以使用 MongoKitten SDK。

	a. 在 dependencies 部分中，添加以下行：
			```hljs
			 .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
			```
			{: codeblock}

	b. 在 targets 部分中，将依赖项“MongoKitten”添加到以下行。**注：**这些值必须在一行中指定。
			```hljs
			 .target(name: "Application", dependencies: [ "Kitura",
                        				"CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
			```
			{: codeblock}

5. 编辑 `Sources/Application/Application.swift` 文件，以使用 MongoKitten 初始化与 MongoDB 的连接。

	a. 导入 MongoKitten SDK：
		```
		import MongoKitten
		```
		{: codeblock}

	b. 添加 `ApplicationServices` 类：

		```hljs
		cclass ApplicationServices {
	    // 服务引用
	    public let mongoDBService: MongoKitten.Database
	    public let myCredFile = "/swift-project/cred.json"

	    public init() throws {
	        // 从 JSON 文件 cred.json 读取凭证
	        struct ResponseData: Decodable {
	            var uri: String
	        }
	        let data = try? Data(contentsOf: URL(fileURLWithPath: myCredFile))
	        let decoder = JSONDecoder()
	        let jsonData = try decoder.decode(ResponseData.self, from: data!)

	        // 运行服务初始化程序
	        let server = try Server(jsonData.uri)
	        mongoDBService = MongoKitten.Database(named: "admin", atServer: 		server)
	    }
		}
		```
		{: codeblock}

	c. 在公共类 `App` 中，添加以下行以初始化数据库连接：

		```hljs
		public class App {
	    ...
	    let services: ApplicationServices

	    public init() throws {
	        // 服务
	        services = try ApplicationServices()

	    }
	    ...
    	```
    	{: codeblock}

## 步骤 4. 验证数据库连接
{: #verify_database}

1. 通过编辑 `Sources/Application/Application.swift` 文件并添加用于测试数据库连接的命令，以验证数据库连接。例如，在 `class ApplicationServices` 中添加以下命令：

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

在[步骤 6](#use-step6) 中部署应用程序后，将显示以下消息（如果数据库连接成功）：

```hljs
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## 步骤 5. 嵌入应用程序代码
{: #embed_appcode}

现在，您可以将自己的应用程序代码添加到项目。有关使用 MongoKitten API 的更多信息，请参阅 http://beta.openkitten.org/tutorials/。

## 步骤 6. 部署应用程序
{: #deploy_app}

可以使用必要的构建工具在本地运行应用程序，也可以通过 {{site.data.keyword.dev_cli_notm}} 在 {{site.data.keyword.cloud_notm}}（Cloud Foundry 或 Kubernetes 集群）中运行应用程序。

可以在主机系统上本地运行应用程序，也可以在 Cloud Foundry 或 Kubernetes 集群中运行应用程序。

1. [安装](/docs/cli/reference/bluemix_cli/get_started.html) {{site.data.keyword.cloud_notm}} CLI

2. 使用 `ibmcloud plugin install dev` 命令安装 Developer Tools 插件。

3. 将应用程序部署到[本地系统](#deploy_local)、[Cloud Foundry](#deploy_cf) 或 [Kubernetes 集群](#deploy_cluster)。

### 本地部署
{: #deploy_local}

1. 确保 Docker 已在本地主机系统上安装并运行。可以从 https://www.docker.com/community-edition#/download 下载 Docker。

2. 切换到包含项目文件的目录。

3. 要在本地计算机上部署应用程序，请输入以下命令：
	```
	$ ibmcloud dev build
	...
	$ ibmcloud dev run
	```
	{: codeblock}

	此步骤构建应用程序，并在 Docker 容器内本地运行该应用程序。

### 部署到 Cloud Foundry
{: #deploy_cf}

1. 切换到包含项目文件的目录。

2. 登录到 IBM Cloud 帐户，并将区域设置为 ``，如下所示：
	```hljs
	$ ibmcloud login -a https://api.ng.bluemix.net
	...
	$ ibmcloud target -o &lt;<em>your-organization</em>&gt; -s &lt;<em>your-space</em>&gt;
	```
	{: codeblock}

    **注：**发出 `ibmcloud login -a https://api.ng.bluemix.net` 命令会自动将区域设置为 **us-south**。

3. 要将应用程序部署到 Cloud Foundry，请输入以下命令：
	```
	$ ibmcloud dev deploy
	```
	{: codeblock}

	您会收到指向应用程序托管位置的可单击链接。

### 部署到 Kubernetes 集群
{: #deploy_cluster}

1. 创建 Kubernetes 集群：https://console.bluemix.net/containers-kubernetes/clusters。

2. 单击**创建集群**。“访问”选项卡显示有关如何访问已创建 Kubernetes 集群的信息。

3. 要显示有关 Kubernetes 集群的信息，请打开 {{site.data.keyword.cloud_notm}} 应用程序仪表板。该仪表板显示服务列表，例如创建的集群、数据库集群、Cloud Foundry 应用程序和 Cloud Foundry 服务。

4. 切换到包含项目文件的目录。

5. 登录到 {{site.data.keyword.cloud_notm}} 帐户，并将区域设置为 us-south，如下所示：
	```hljs
	$ ibmcloud login -a https://api.ng.bluemix.net
	...
	$ ibmcloud target -o <your-organization> -s <your-space>
	```
	{: codeblock}

	**注：**发出 `ibmcloud login -a https://api.ng.bluemix.net` 命令会自动将区域设置为 **us-south**。

6. 要在 Kubernetes 中部署应用程序，请输入以下命令：
	```
    $ ibmcloud dev deploy -t container
    ```
    {: codeblock}

	系统将提示您输入 Kubernetes 集群和 Docker 注册表的名称。提供这些信息后，即会将应用程序部署到 Kubernetes 集群。
