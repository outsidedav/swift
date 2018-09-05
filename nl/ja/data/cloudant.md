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

# {{site.data.keyword.cloud_notm}} へのドキュメントの保管
{: #cloudant}

{{site.data.keyword.cloudantfull}} は、ドキュメント指向の DataBase as a Service (DBaaS) です。JSON フォーマットのドキュメントとしてデータを保管します。スケーラビリティー、高可用性、耐久性を考慮に入れて構築されており、Swift アプリケーション用に構成しやすくなっています。MapReduce、{{site.data.keyword.cloudant_short_notm}} Query、フルテキスト索引付け、地理情報索引付けなどのさまざまな索引付けオプションが用意されています。複製機能により、データベース・クラスター、デスクトップ PC、モバイル・デバイス間で簡単にデータを同期させておくことができます。
{:shortdesc}

{{site.data.keyword.cloudant_short_notm}} を使用するためのすべての方法については、[{{site.data.keyword.cloudant_short_notm}} Basics](/docs/services/Cloudant/basics/index.html#cloudant-nosql-db-basics) を参照してください。

## 始めに

まず、以下の前提条件が整っていることを確認してください。
 * CocoaPods (バージョン 1.1.0 以上)
 * iOS (バージョン 9 以上)
 * MacOS (バージョン 10.11.5 以上)
 * Xcode (バージョン 9.0.1 以上)

[{{site.data.keyword.cloudant_short_notm}} SDK for Swift ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/cloudant/swift-cloudant) は、Swift 3.2 を使用して作成されています。{{site.data.keyword.cloudant_short_notm}} を Kitura で使用する予定の場合は、Swift 4.0 で作成された [Kitura-CouchDB ライブラリー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/IBM-Swift/Kitura-CouchDB) を確認してください。
{: tip}

## ステップ 1. {{site.data.keyword.cloudant_short_notm}} インスタンスの作成

[IBM Cloud 上の Cloudant NoSQL DB インスタンスの作成に関するチュートリアル ![外部リンク・アイコン](../images/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/services/Cloudant/tutorials/create_service.html#creating-a-cloudant-nosql-db-instance-on-ibm-cloud){:new_window} を参照して、サービスのインスタンスをプロビジョンしてください。


## ステップ 2. SDK のインストール

### iOS Swift SDK のインストール

Swift Cloudant SDK を使用すると、アプリをコーディングしやすくなります。この SDK は、アプリ・コードにインストールする必要があります。

1. 既存の Xcode プロジェクト・ディレクトリーを開き、`Podfile` に移動します。
2. プロジェクト・ターゲットの下に、`SwiftCloudant` ポッドの依存関係を追加します。次の例に示すように、`use_frameworks!` コマンドもターゲットの下に必要です。
```
    target '<yourTarget>' do
      use_frameworks!
        pod 'SwiftCloudant', :git => 'https://github.com/cloudant/swift-cloudant.git'
    end
    ```
    {: screen}
3. `SwiftCloudant` の依存関係をダウンロードします。
    ```
    pod install
    ```
    {: pre}

### サーバー・サイド Swift SDK のインストール

サーバー・サイド開発用 Swift Package Manager とともに使用するには、`Package.swift` 内の依存関係に次の行を追加します。
```swift
.Package(url: "https://github.com/cloudant/swift-cloudant.git")
```
{: pre}

## ステップ 3. SDK の初期化

アプリ内の SDK を初期化した後、{{site.data.keyword.cloudant_short_notm}} を活用してデータを保管し始めることができます。

1.  ご使用の `AppDelegate.swift` またはサーバー・サイドの swift ファイルに、次の import を追加します。
    ```
    import SwiftCloudant
    ```
    {:pre}
2. データベースへの接続を初期化します。
    ```swift
    let cloudantURL = NSURL(string: "https://username.cloudant.com")!
    let client = CouchDBClient(url: cloudantURL, username: "username", password: "password")
    let dbName = "database"
    ```
    {: codeblock}

### 基本操作
これらの基本操作は、初期化済みクライアントを使用してドキュメントの作成、読み取り、破棄を行うための基本的アクションを示しています。

#### ドキュメントの作成
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

#### ドキュメントの読み取り
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

#### ドキュメントの削除
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


## ステップ 4. アプリのテスト
{: #cloudant_testing}

すべて適切にセットアップされていますか?テストしてみましょう!

1. アプリケーションを実行します。このとき、初期化操作とドキュメント作成などのそれぞれの操作を必ず呼び出すようにしてください。
2. Web ブラウザーで既に作成した {{site.data.keyword.cloudant_short_notm}} サービス・インスタンスに戻り、サービス・ダッシュボードを開きます。
3. 使用されているデータベースを選択すると、ダッシュボードにドキュメントが表示されます。

問題が発生する場合は、 [{{site.data.keyword.cloudant_short_notm}} API リファレンス](/docs/services/Cloudant/api/index.html#api-reference-overview)を確認してください。


## 次のステップ
{: #cloudant_next notoc}

お疲れさまでした。ある程度のセキュア・パーシスタンスをアプリに追加しました。この調子で、以下のいずれかのオプションを試してみてください。

* [{{site.data.keyword.cloudant_short_notm}} SDK for Swift ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/cloudant/swift-cloudant) のソース・コードを調べる。
* スターター・キットは、IBM Cloud の機能を活用する最速の方法の一つです。**Infinite Scrolling with Cloudant NoSQL for iOS** スターター・キットは、ViewController を拡張してページ編集を使用してデータを表示する方法を示しています。このパターンのアプリは iOS 開発者にとって一般的なもので、{{site.data.keyword.cloudant_short_notm}} の機能を示すための良い例です。[モバイル開発者ダッシュボード](https://console.bluemix.net/developer/mobile/dashboard)で、使用可能なスターター・キットを確認できます。コードをダウンロードし、アプリを実行してください。
* {{site.data.keyword.cloudant_short_notm}} が提供する必要のある機能のすべてについてさらに学習し、それらの機能を利用するため、[資料を確認してください](/docs/services/Cloudant/index.html)。
