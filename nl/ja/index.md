---

copyright:
  years: 2018
lastupdated: "2018-11-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 入門チュートリアル
{: #set_up}

{{site.data.keyword.cloud}} には、顧客が求めるセキュリティー、AI、価値を組み込んだアプリケーションを Swift 開発者が作成できるようにする、ソリューションとサービスが用意されています。幅広いオファリングのポートフォリオや SDK が用意されているので、これらのサービスを使用して最先端のアプリケーションを迅速にリリースすることが可能になります。 この Swift プログラミングでは、新規または既存の Swift アプリケーションにサービスを追加する方法について説明します (iOS クライアントまたはサーバー・サイド Swift のいずれでも構いません)。
{: shortdesc}

以下のチュートリアルでは、[{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits) の空のスターター・キットを使用して、{{site.data.keyword.mobileanalytics_full}} で簡単に Swift モバイル・アプリを作成する方法を示します。{{site.data.keyword.mobileanalytics_short}} サービスを追加したり、コードをダウンロードしたり、iOS アプリを Xcode を使用してローカルで実行したり、アプリを構成したり、アプリをモニターしたりといった作業を、コンソールから行います。

## ステップ 1. 開発者向けの要件
{: #dev-requirements}

{{site.data.keyword.cloud_notm}} で iOS の開発を始めるには、以下の要件を満たしていることを確認してください。

### オペレーティング・システム

Swift アプリを開発する際のベスト・プラクティスは、MacOS がサポートする最新のハードウェアを使用し、最新の iOS リリースでテストすることです。[Apple Developer](https://developer.apple.com/) アカウントに登録して、物理デバイス上でテストできるようにしてください。

### iOS および Xcode
{: #ios_and_xcode}

- [Xcode 8+](https://developer.apple.com/xcode/) (またはそれ以降) をインストールします。
- [iOS 8 デバイス](https://support.apple.com/downloads/ios) (またはそれ以降) にデプロイします。
- Apple にアプリを提出する前に、[App Store の提出に関するガイドライン](https://developer.apple.com/app-store/guidelines/)を確認してください。

### SDK と依存関係の管理

以下のツールを使用すると、さまざまな {{site.data.keyword.cloud_notm}} サービスを扱うネイティブ SDK をインストールできます。

1. {{site.data.keyword.cloud_notm}} SDK をサポートするための [CocoaPods](https://cocoapods.org/) をインストールします。
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
2. Carthage のインストールを支援するための [Homebrew](https://brew.sh/) をインストールします。
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

3. Watson SDK をサポートするための [Carthage](https://github.com/Carthage/Carthage) をインストールします。
  ```
  brew install carthage
  ```
  {: codeblock}

## ステップ 2. カスタム iOS Swift アプリの作成
{: #create-ios-app}

1. [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits) にログインします。
2. **「アプリの作成」**をクリックします。
3. [空のスターター](https://console.bluemix.net/developer/appledevelopment/create-app)・ページでは、デフォルトの構成を使用することも、必要に応じて各フィールドを更新することもできます。 選択されている言語が **iOS Swift** であることを確認してください。 **「作成」**をクリックします。

## ステップ 3. {{site.data.keyword.mobileanalytics_short}} サービスの追加
{: #adding-services}

1. アプリの詳細ページで、**「リソースの追加」**ボタンをクリックします。
2. **「モバイル」**を選択して、**「次へ」**をクリックします。
3. **{{site.data.keyword.mobileanalytics_short}}** を選択して、**「次へ」**をクリックします。
4. **「作成」**をクリックします。

## ステップ 4. コードのダウンロードとクライアント SDK のセットアップ
{: #run-locally}

コードをダウンロードするには、`「アプリ」`>`「アプリ (Your App)」`の下にある**「コードのダウンロード (Download Code)」**をクリックします。 ダウンロードしたコードには、**{{site.data.keyword.mobileanalytics_short}}** クライアント SDK が含まれます。 クライアント SDK は、CocoaPods および Carthage で入手できます。 本ソリューションでは CocoaPods を使用します。

1. ダウンロードしたコードを unzip します。 次に、端末を使用して、解凍したフォルダーにナビゲートします。
  ```
  cd <プロジェクト名>
  ```
2. このフォルダーには、必要な依存関係を持つ podfile が含まれています。 以下のコマンドを実行して、依存関係 (クライアント SDK) をインストールします。
  ```
  pod install
  ```
  {: codeblock}

## ステップ 5. {{site.data.keyword.mobileanalytics_short}} を使用するためのアプリの構成
{: #configure-analytics}

1. Xcode で `.xcworkspace` を開き、`AppDelegate.swift` にナビゲートします。
  **注:** 元の Xcode プロジェクト・ファイルではなく、次のように必ず新しい Xcode ワークスペースである `MyApp.xcworkspace` を開いてください。
   ![Open Xcode](images/Xcode.png)

  `BMSCore` はコア SDK で、Mobile Client SDK のベースとなるものです。 `BMSClient` は `BMSCore` のクラスで、`AppDelegate.swift` で初期化されます。 {{site.data.keyword.mobileanalytics_short}} SDK は、`BMSCore` とともに既にプロジェクトにインポートされています。
  
2. 分析初期化コードは、以下のスニペットに示されているように既に組み込まれています。
  ```
  // Analytics client SDK is configured to record lifecycle events.
         	Analytics.initialize(appName:dictionary["appName"] as? String,
        			     apiKey: dictionary["analyticsApiKey"] as? String,
        	        	     deviceEvents: .lifecycle)

        	// Enable Logger (disabled by default) and set level to ERROR (DEBUG by default).
        	Logger.isLogStorageEnabled = true
        	Logger.logLevelFilter = .error
  ```
  {: codeblock}

  **注 :** サービス資格情報は、`BMSCredentials.plist` ファイルに含まれています。

3. `logger` を使用して使用分析を収集します。 `ViewController.swift` ファイルにナビゲートして、以下のコードを表示します。
  ```
   func didBecomeActive(_ notification: Notification) {
       Analytics.send()
       Logger.send()
    }
  ```
  {: codeblock}

   高度な分析およびロギング機能については、[使用分析の収集](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics)および[ロギング](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger)を参照してください。
   {:tip}

## ステップ 6. {{site.data.keyword.mobileanalytics_short}} を使用したアプリのモニター
{{site.data.keyword.mobileanalytics_short}} サービスは、モバイル・アプリケーション開発者とアプリケーション所有者に、主なアプリケーションの使用量とパフォーマンスに関する洞察を提供します。 {{site.data.keyword.mobileanalytics_short}} を使用することにより、アプリケーション所有者および開発者は、ユーザー側で何が起こっているかを把握することができます。 この洞察を活用することで、ユーザーにとって意義のあるより良いアプリケーションを構築し、それを数多のモバイル・アプリケーションの中で際立たせることができます。

このサービスには {{site.data.keyword.mobileanalytics_short}} コンソールが含まれています。これを使用して、開発者とアプリケーション所有者は、モバイル・アプリケーションのパフォーマンスの監視、使用量の統計の確認、デバイス・ログの検索を実行することができます。

1. 作成したモバイル・アプリから `{{site.data.keyword.mobileanalytics_short}}` サービスを開くか、サービスの横にある垂直に並んだ 3 つのドットをクリックして`「ダッシュボードを開く (Open Dashboard)」`を選択します。
2. 「ライブ・ユーザー (LIVE Users)」、「セッション (Sessions)」、およびその他の「アプリ・データ (App Data)」は、`デモ・モード`を無効にすると見ることができます。 分析情報のフィルタリングには、以下の基準を使用できます。
    * 日付。
    * アプリケーション。
    * オペレーティング・システム。
    * アプリのバージョン。
         ![{{site.data.keyword.mobileanalytics_short}}](images/mobile_analytics.png)
3. アラートの設定、アプリの異常終了のモニター、およびネットワーク要求のモニターを行うには、[ここをクリック](https://console.bluemix.net/docs/services/mobileanalytics/app-monitoring.html#monitoringapps)します。

## 次のステップ
{: #next-steps}

### サービスの追加
以下のような一般的に使用されるサービスを、Web コンソールから iOS アプリに直接追加できます。

* [プッシュ通知サービスの追加](/docs/services/mobilepush/index.html)
* [アプリ ID を使用したユーザー認証の追加](/docs/services/appid/index.html)

### {{site.data.keyword.cloud_notm}} 開発者ツールの使用
また、[{{site.data.keyword.cloud_notm}} 開発者ツール](../cli/index.html)を使用して、Swift アプリの開発方法を学習することもできます。このツールは、完全な Web アプリケーション、モバイル・アプリケーション、マイクロサービス・アプリケーションの作成と開発とデプロイを行うためのコマンド・ライン・アプローチを提供します。

