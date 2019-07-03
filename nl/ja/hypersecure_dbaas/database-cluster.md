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

# 可用性の高いセキュアなデータベースの作成
{: #create-database-cluster}

可用性の高いセキュアなデータベースを最大限に活用するには、追加のロジックをアプリケーションに組み込みます。 用意されているコード・スニペットを使用することで、MongoDB データベースを作成し、それにアクセスすることができます。 

現時点で、{{site.data.keyword.ihsdbaas_full}} を使用する際にサポートされるプログラミング言語は、MongoKitten SDK 4.0.0 での Swift 4.0 です。

## ステップ 1. データベース・クラスターの作成
{: #create_dbcluster}

1. [{{site.data.keyword.ihsdbaas_full}} サービス構成](https://{DomainName}/catalog/services/hyper-protect-dbaas){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") 画面にアクセスします。

2. 以下の情報を指定します。

	<dl>
		<dt>サービス名:</dt>
		<dd>データベース・サービスの名前。</dd>

    <dt>デプロイする地域/ロケーションの選択:</dt>
    <dd>データベースが配置されているデータ・センターを選択します。</dd>

    <dt>リソース・グループの選択:</dt>
    <dd>選択できるリソース・グループがない場合は、IBM Cloud ダッシュボードで作成することができます。</dd>

		<dt>クラスター / Replicaset 名:</dt>
		<dd>データベース・クラスターの名前。</dd>

		<dt>データベース管理者名:</dt>
		<dd>データベース管理者 (DBA) のユーザー ID。</dd>

		<dt>データベース管理者パスワード:</dt>
		<dd>DBA ユーザー ID のパスワード。</dd>

    <dt>データベース管理者パスワードの確認 (Confirm Database Admin Password):</dt>
    <dd>DBA ユーザー ID のパスワードを確認します。</dd>

		<dt>データベース・タイプ:</dt>
		<dd>データベース・タイプを選択します。 現時点では、MongoDB のみサポートされています。</dd>

    <dt>ご使用条件:</dt>
    <dd>ご使用条件を読み、ボックスにチェック・マークを付けて同意します。</dd>

    <dt>料金:</dt>
		<dd>現行のソリューションでは、無料の料金カテゴリーが 1 つ設定されているだけです。 後のバージョンで、料金カテゴリーを選択できるようになります。</dd>
	</dl>

3. **「作成」**をクリックします。

  {{site.data.keyword.cloud_notm}} ダッシュボードが表示されます。 新規クラスターを表示するには、ブラウザーを最新表示しなければならない場合があります。新規クラスターは「サービス」セクションにリストされます。</p></li>

4. サービスを選択すると、クラスター情報が表示されます。

5. クラスター情報の「管理」タブで、**「開く」**をクリックします。

	{{site.data.keyword.ihsdbaas_full}} ダッシュボードが表示されます。

6. データベース・クラスターに属する、3 つの作成済みデータベース・インスタンスのホスト名とポート番号を収集します。 [データベースへの接続](#connect_db)セクションのステップで、ホスト名、ポート番号、ユーザー資格情報が必要になります。

## ステップ 2. スターター・キットを使用したプロジェクトの作成
{: #create_starter}

サーバー・サイド Swift Web フレームワークの Kitura をベースにしたスターター・キットが必要です。

![機能の詳細](videos/StarterKit.gif){: gif}

このスターター・キットから作成された既存のプロジェクトを使用するか、または新規プロジェクトを作成します。

1. [{{site.data.keyword.cloud_notm}}「アプリ・サービス」ダッシュボード](https://{DomainName}/developer/appservice/dashboard){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を開きます。

2. **「スターター・キット」**タブを選択します。

3. **Swift Kitura** Backend for Frontend スターター・キットを選択します。 Swift Kitura Basic Web アプリ・スターター・キットと混同しないようにしてください。
  「新規プロジェクトの作成」ページが表示されます。

4. プロジェクトの詳細を入力し、**「プロジェクトの作成」**をクリックします。
  プロジェクト・ページが表示されます。

5. プロジェクト・ページで、**「コードのダウンロード」**をクリックします。

6. 圧縮ファイルをプロジェクト・ディレクトリーに解凍します。

## ステップ 3. データベースへの接続
{: #connect_db}

セキュアなデータ転送を行えるように、認証局 (CA) ファイルをダウンロードし、プロジェクト・ディレクトリーにコピーします。

1. ダウンロードした解凍済みのコード・ファイルがある、プロジェクト・ディレクトリーに変更します。

2. データベース・クラスターに対するアクセス資格情報を格納する、`cred.json` という名前の JSON ファイルを作成します。

3. [データベース・クラスターの作成](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)の手順で収集した値を入力します。 それらの値は、単一行で指定する必要があります。
  ```hljs
  {
  "uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
  <Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
   /admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
  }
  ```
  {: codeblock}

### データベース・パラメーターの説明
{: #db-parameter-descriptions}

以下のデータベース・パラメーターの説明を参照してください。

* `admin_ID` - [データベース・クラスターの作成](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)で指定したデータベース管理者のユーザー ID。
* `admin_pwd` - [データベース・クラスターの作成](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)で指定した管理者パスワードのユーザー ID。
* `Hostname_i` - [データベース・クラスターの作成](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)で返されたデータベース・レプリカ *i* (*i*=1,2,3)。
* `PortNumber_i` - [データベース・クラスターの作成](/docs/swift?topic=swift-create-database-cluster#create_dbcluster)で返されたポート番号 *i* (*i*=1,2,3)。
* `CA_file` - ダウンロードした CA ファイルのファイル名。 デプロイメント中に、このファイル名がディレクトリー `/swift-project` にコピーされます。

4. MongoKitten SDK の使用に必要なパッケージ依存関係を追加するために、
`Package.swift` ファイルを編集します。

  * 依存関係セクションで、次の行を追加します。
   ```swift
   .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
   ```
   {: codeblock}

  * ターゲット・セクションで、次の行に依存関係 "MongoKitten" を追加します。 **注:** この値は、1 行で指定する必要があります。
   ```swift
   .target(name: "Application", dependencies: [ "Kitura",
   "CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
   ```
   {: codeblock}

5. MongoKitten を使用して MongoDB への接続を初期化するために、`Sources/Application/Application.swift` ファイルを編集します。

  * MongoKitten SDK をインポートします。
    ```swift
	  import MongoKitten
	  ```
	  {: codeblock}

  * クラス `ApplicationServices` を追加します。
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

  * パブリック・クラス `App` で、データベース接続を初期化するために次の行を追加します。
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

## ステップ 4. データベース接続の検証
{: #verify_database}

1. データベース接続をテストするコマンドを追加するためにファイル `Sources/Application/Application.swift` を編集して、データベース接続を検証します。
例えば、`class ApplicationServices` で次のコマンドを追加します。

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

[ステップ 6](#use-step6) でアプリケーションをデプロイした後、データベースへの接続に成功すると、次のメッセージが表示されます。

```
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## ステップ 5. アプリケーション・コードの組み込み
{: #embed_appcode}

ここで、独自のアプリケーション・コードをプロジェクトに追加できます。 詳しくは、[MongoKitten API](http://beta.openkitten.org/tutorials/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") 資料を参照してください。

## ステップ 6. アプリケーションのデプロイ
{: #deploy-dbcluster}

アプリケーションは、必要なビルド・ツールを使用して[ローカルで](/docs/swift?topic=swift-swift_cli#swift-install-tools)実行するか、または {{site.data.keyword.cloud_notm}} にデプロイすることができます。

ダッシュボードでデプロイメント・ツールチェーンを作成するには、**「デプロイ」**をクリックします。 次の方法のうちの選択する方法に応じた手順に従って、デプロイメント・ターゲットをセットアップします。
  * **IBM Kubernetes Service にデプロイします**。 このオプションでは、可用性の高いアプリ・コンテナーをデプロイして管理するためのワーカー・ノードと呼ばれるホストのクラスターを作成します。 クラスターを作成したり、既存のクラスターにデプロイしたりすることができます。 詳しくは、[Kubernetes クラスターへのアプリのデプロイ](/docs/containers?topic=containers-app)を参照してください。
  * **Cloud Foundry へのデプロイ**。 このオプションでは、基礎となるインフラストラクチャーの管理を必要とせずに、クラウド・ネイティブ・アプリをデプロイします。 ご使用のアカウントに {{site.data.keyword.cfee_full_notm}} へのアクセス権限がある場合、デプロイヤー・タイプとして、**パブリック・クラウド**または**エンタープライズ環境**のいずれかを選択できます。エンタープライズ環境を使用すると、自社専用に Cloud Foundry アプリをホスティングする隔離された環境を作成して管理できます。 詳しくは、[Cloud Foundry パブリックへのアプリのデプロイ](/docs/cloud-foundry-public?topic=cloud-foundry-public-deployingapps)と[{{site.data.keyword.cfee_full_notm}} へのアプリのデプロイ](/docs/cloud-foundry?topic=cloud-foundry-deploy_apps)を参照してください。
  * **仮想サーバーにデプロイします**。 このオプションによって、仮想サーバー・インスタンスがプロビジョンされ、アプリを含むイメージがロードされ、DevOps ツールチェーンが作成され、最初のデプロイメント・サイクルが開始されます。 詳しくは、[仮想サーバーへのアプリのデプロイ](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server)を参照してください。
  
