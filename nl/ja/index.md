---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-31"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# 入門チュートリアル
{: #getting_started_swift}

{{site.data.keyword.cloud}} には、顧客が求めるセキュリティー、AI、価値を組み込んだアプリケーションを Swift 開発者が作成できるようにする、ソリューションとサービスが用意されています。 幅広いオファリングのポートフォリオや SDK が用意されているので、これらのサービスを使用して最先端のアプリケーションを迅速にリリースすることが可能になります。 この Swift プログラミングでは、新規または既存の Swift アプリケーションにサービスを追加する方法について説明します (iOS クライアントまたはサーバー・サイド Swift のいずれでも構いません)。
{: shortdesc}

以下のチュートリアルでは、[{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits) の空のスターター・キットを使用して、{{site.data.keyword.mobileanalytics_full}} で簡単に Swift モバイル・アプリを作成する方法を示します。 {{site.data.keyword.mobileanalytics_short}} サービスを追加したり、コードをダウンロードしたり、iOS アプリを Xcode を使用してローカルで実行したり、アプリを構成したり、アプリをモニターしたりといった作業を、コンソールから行います。

## ステップ 1. 開発者向けの要件
{: #dev-requirements-swift}

{{site.data.keyword.cloud_notm}} で iOS の開発を始めるには、以下の要件を満たしていることを確認してください。

### オペレーティング・システム
{: #swift-os-requirements}

Swift アプリを開発する際のベスト・プラクティスは、MacOS がサポートする最新のハードウェアを使用し、最新の iOS リリースでテストすることです。 [Apple Developer](https://developer.apple.com/) アカウントに登録して、物理デバイス上でテストできるようにしてください。

### iOS および Xcode
{: #ios_and_xcode}

- [Xcode 8+](https://developer.apple.com/xcode/) (またはそれ以降) をインストールします。
- [iOS 8 デバイス](https://support.apple.com/downloads/ios) (またはそれ以降) にデプロイします。
- Apple にアプリを提出する前に、[App Store の提出に関するガイドライン](https://developer.apple.com/app-store/guidelines/)を確認してください。

### SDK と依存関係の管理
{: #swift-sdk-management}

以下のツールを使用すると、さまざまな {{site.data.keyword.cloud_notm}} サービスを扱うネイティブ SDK をインストールできます。依存関係の管理には CocoaPods または Carthage を使用できます。

* **CocoaPods を使用する場合** - {{site.data.keyword.cloud_notm}} SDK をサポートするための [CocoaPods](https://cocoapods.org/) をインストールします。
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}

* **Carthage を使用する場合** - 一部の SDK は、[Carthage](https://github.com/Carthage/Carthage) または [Swift Package Manager](https://swift.org/package-manager/) の依存関係マネージャーからも入手できます。依存関係の管理に Carthage を使用する場合は、次の手順を実行します。

  Carthage のインストールを支援するための [Homebrew](https://brew.sh/) をインストールします。
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

  Carthage をインストールします。
  ```
  brew install carthage
  ```
  {: codeblock}

## ステップ 2. カスタム iOS Swift アプリの作成
{: #create-ios-app-swift}

1. [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits) にログインします。
2. **「アプリの作成」**をクリックします。
3. [空のスターター](https://cloud.ibm.com/developer/appledevelopment/create-app)・ページでは、デフォルトの構成を使用することも、必要に応じて各フィールドを更新することもできます。 選択されている言語が **iOS Swift** であることを確認してください。 **「作成」**をクリックします。

## ステップ 3. {{site.data.keyword.cloudant_short_notm}} サービスの追加
{: #resources-swift}

これで、Swift アプリケーションにサービスを追加できます。このチュートリアルでは、{{site.data.keyword.cloudant_short_notm}} サービスを Swift アプリに追加します。これにより、完全に管理された分散型 `JSON` ドキュメント・データベースが作成されます。Cloudant は、データを安全に保護し同期した状態で維持する軽量のフレームワークで、スケーラビリティー、高可用性、耐久性をアプリに提供します。

1. アプリの詳細ページで、**「リソースの追加」**ボタンをクリックします。
2. **「データ」**を選択し、**「次へ」**をクリックします。
3. **「Cloudant」**を選択し、**「次へ」**をクリックします。
4. **「作成」**をクリックします。
5. サービスが作成されたら、そのサービスをクリックして開始できます。この新しいページで**「Cloudant ダッシュボードの起動 (Launch Cloudant Dashboard)」**を選択し、データベースと JSON 文書の作成を開始します。あるいは、この操作をプログラムで実行することもできます。

## ステップ 4. コードのダウンロードとクライアント SDK のセットアップ
{: #run-locally-swift}

コードをダウンロードするには、`「アプリ」`>`「アプリ (Your App)」`の下にある**「コードのダウンロード (Download Code)」**をクリックします。 ダウンロードされるコードには、[SwiftCloudant SDK](https://github.com/cloudant/swift-cloudant) といくつかの基本的な初期設定コードが含まれています。クライアント SDK は CocoaPods および Swift Package Manager で入手できます。このソリューションでは CocoaPods を使用します。

1. ダウンロードしたコードを解凍します。次に、端末を使用して、解凍したフォルダーにナビゲートします。
  ```
  cd <プロジェクト名>
  ```

2. このフォルダーには、必要な依存関係を持つ podfile が含まれています。 以下のコマンドを実行して、依存関係 (クライアント SDK) をインストールします。
  ```
  pod install
  ```
  {: codeblock}

## ステップ 5. 新しいデータベースを使用するためのアプリの構成
{: #config-db-swift}

1. Xcode で `.xcworkspace` で終わる名前のファイルを開き、`ViewController.swift` にナビゲートします。`SwiftCloudant` はコア Cloudant SDK です。`CouchDBClient` は `SwiftCloudant` のクラスであり、`ViewController.swift` で初期化されます。

  元の Xcode プロジェクト・ファイルではなく、必ず新しい Xcode ワークスペースである `MyApp.xcworkspace` を開いてください。{: tip}

2. データベース初期化コードは、以下のスニペットに示されているように既に組み込まれています。
  ```swift
  /* Read the Cloudant credentials and intialize database connection */
  if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"), let dictionary = NSDictionary(contentsOfFile: contents) {
            let url = URL(string: dictionary["cloudantUrl"] as! String)
            let client = CouchDBClient(url:url!,
                                       username:dictionary["cloudantUsername"] as? String,
                                       password:dictionary["cloudantPassword"] as? String)
        }
  ```
  {: codeblock}

  サービス資格情報は、`BMSCredentials.plist` ファイルに含まれています。
  {: note}

## ステップ 6. データベース操作の作成
{: #build_ops-swift}

作業データベースの接続と SDK の設定が完了したため、特定のユース・ケースについて基本的な[作成操作、読み取り操作、更新操作、および破棄操作](./data/cloudant.html#basic-operations)の作成を開始できます。

## 次のステップ
{: #next-swift}

### サービスの追加
{: #moreresources-swift}

以下のような一般的に使用されるサービスを、Web コンソールから iOS アプリに直接追加できます。

* [プッシュ通知サービスの追加](/docs/services/mobilepush/index.html)
* [アプリ ID を使用したユーザー認証の追加](/docs/services/appid/index.html)

### {{site.data.keyword.cloud_notm}} 開発者ツールの使用
{: #devtools-swift}

また、[{{site.data.keyword.cloud_notm}} 開発者ツール](/docs/cli/index.html)を使用して、Swift アプリの開発方法を学習することもできます。このツールは、完全な Web アプリケーション、モバイル・アプリケーション、マイクロサービス・アプリケーションの作成と開発とデプロイを行うためのコマンド・ライン・アプローチを提供します。
