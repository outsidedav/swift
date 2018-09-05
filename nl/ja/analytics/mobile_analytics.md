---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# モバイル分析の収集
{: #mobile_analytics}

{{site.data.keyword.cloud_notm}} の {{site.data.keyword.mobileanalytics_short}} は、開発者、IT 管理者、ビジネス関係者に、モバイル・アプリのパフォーマンスや使用状況についての洞察を提供します。{{site.data.keyword.mobileanalytics_short}} サービスを使用すると、以下のことが可能です。

 - **即時に洞察を得る** - リアルタイムでパフォーマンスと使用に関するメトリックを確認できます。

 - **数分で実装する** - {{site.data.keyword.cloud_notm}} でサービス・インスタンスを作成し、SDK をプロジェクトに追加し、2 行のコードをアプリケーションに貼り付ければ、数多くの定義済みメトリックを収集する準備が整います。

 - **必要なデータを収集する** - アプリにカスタム・イベントを装備し、ユーザーがアプリとどのように対話しているかを発見し、購入を追跡し、アプリのアクティビティーをモニターします。

 - **一見で分かるメトリックを表示する** - {{site.data.keyword.mobileanalytics_short}} コンソールにはすぐに使用できるグラフが用意されているため、クエリーを作成する必要がありません。

 - **重要な事柄に注意を向けることができる** - メトリックを、時間、アダプター、アプリケーション、アプリケーション・バージョン、OS、OS バージョン、デバイス・モデルによってフィルター処理できます。

 - **問題を迅速に発見する** - 異常終了状況をモニターできます。重大なメトリックにアラート・トリガーを設定し、アラートを任意の REST エンドポイントに経路指定します。

 - **根本原因をトラブルシューティングする** - カスタム・ロギングをアプリケーションで使用して自動的にログをアップロードし、コンソールから検索します。異常終了イベントについて詳しく調べ、スタック・トレースを確認します。

## 始めに

最初に、以下の前提条件を満たしていることを確認してください。

 - iOS 8.0 以上/watchOS 2.0 以上
 - Xcode 7.3、8.0
 - Swift 2.2 - 3.0
 - Cocoapods または Carthage

## ステップ 1. {{site.data.keyword.mobileanalytics_short}} インスタンスの作成
{: #mobile_analytics_create}

1. {{site.data.keyword.cloud_notm}} カタログで、**「モバイル」** > **「{{site.data.keyword.mobileanalytics_short}}」**とクリックします。サービス構成画面が開きます。
2. サービス・インスタンスに名前を付けます。または、事前設定された名前を使用します。
3. **「作成」**をクリックします。
4. ナビゲーション・ペインで、**「接続」**をクリックしてアプリを選択し、サービスにバインドします。作成時にアンバインドの状態のままにする場合、後ほど、アプリにサービス・インスタンスをバインドできます。

## ステップ 2. iOS Swift SDK のインストール

サービスによって、アプリケーション開発を簡略化するためのプラットフォーム固有の SDK が提供されます。{{site.data.keyword.cloud_notm}} Mobile Services Swift SDK は、Cocoapods と Carthage のどちらにもインストールできます。詳細については、[https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics) を参照してください。

{{site.data.keyword.mobileanalytics_full}} SDK を使用してモバイル・アプリケーションを装備できます。Swift SDK は iOS および watchOS で使用できます。

1. Xcode が正しくセットアップされていることを確認します。iOS 開発環境のセットアップ方法については、[Apple Developer Web サイト![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.apple.com/support/xcode/){: new_window}を参照してください。Client SDK Swift Analytics については、[Xcode 要件![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#requirements){: new_window}をご覧ください。

2. アプリのプロジェクト・フォルダーにある `Info.plist` ファイルにプロパティーを追加して、ロケーション API を有効にします。例えば、`Privacy - Location Usage Description` と、「アプリではロケーション・サービスを有効にすることが必要です」などのロケーション API を追加する適切な理由を指定します。

{{site.data.keyword.mobileanalytics_short}} SDK は、Cocoa プロジェクトの従属関係マネージャーである [CocoaPods ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cocoapods.org/){: new_window}と [Carthage ![外部リンク・アイコン ](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/Carthage/Carthage#getting-started){: new_window} と共に配布されます。CocoaPods と Carthage では、アプリケーションで成果物を利用できるようにリポジトリーから自動的に成果物がダウンロードされます。CocoaPods または Carthage を選択します。

### CocoaPods
{: #cocoapods}

1. GitHub の[{{site.data.keyword.Bluemix_notm}} Mobile Services Swift SDK の説明![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#cocoapods){: new_window}に従い、Cocoapods を使用して `BMSAnalytics` をインストールし、それを Podfile に追加します。

2. iOS Client SDK のインストール後、Analytics Client SDK を[インポートして初期設定](sdk.html#initalize-ma-sdk)します。   

### Carthage
{: #carthage}

CocoaPods を使用していない場合、[Carthage ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos){: new_window}を使用してプロジェクトにフレームワークを追加できます。

1. GitHub の [Carthage インストールの指示![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#carthage){: new_window}に従い、`BMSAnalytics` をインストールします。

2. iOS Client SDK のインストール後、Analytics Client SDK をインポートして初期設定します。

## ステップ 3. SDK の初期化

{{site.data.keyword.mobileanalytics_short}} を使用すると、以下のカテゴリーのデータを収集できます。各データ・カテゴリーには、それぞれ異なる程度の計測が必要です。

**事前定義データ**: このカテゴリーには、すべてのアプリケーションに適用される一般的な使用情報およびデバイス情報が含まれます。
これは、アプリケーションが使用するボリューム、頻度、または期間を示します。事前定義データは、アプリケーションで {{site.data.keyword.mobileanalytics_short}} SDK を初期設定すると自動的に収集されます。

**アプリケーション・ログ・メッセージ**: このカテゴリーでは、開発者は、開発およびデバッグに役立つカスタム・メッセージをログに記録できるコード行をアプリケーション全体に追加できます。開発者は、各ログ・メッセージに重大度/冗長度のレベルを割り当てます。

**カスタム・イベント**: このカテゴリーには、ユーザー自身が定義するデータと、アプリ固有のデータが含まれます。
このデータは、ページ・ビュー、ボタン・タップ、アプリ内購入など、アプリ内で発生するイベントを表します。アプリでの {{site.data.keyword.mobileanalytics_short}} SDK の初期化に加えて、追跡するカスタム・イベントごとにコード行を追加する必要があります。

## ステップ 4. サービス資格情報 API 鍵の値の識別
{: #analytics-clientkey}

クライアント SDK をセットアップする前に、**API 鍵**の値を識別します。クライアント SDK を初期化するには、API 鍵が必要です。

1. {{site.data.keyword.mobileanalytics_short}} サービス・ダッシュボードを開きます。
2. **「資格情報の表示」**を展開して、API 鍵の値を表示します。{{site.data.keyword.mobileanalytics_short}} Client SDK を初期設定するときにこの API 鍵の値が必要です。

## ステップ 5. 分析を収集するためのアプリケーションの初期設定
{: #initalize-ma-sdk}

{{site.data.keyword.mobileanalytics_short}} サービスにログを送れるようにアプリケーションを初期設定します。

1. Client SDK をインポートします。

    Swift SDK は iOS および watchOS で使用できます。以下の `import` ステートメントを `AppDelegate.swift` プロジェクト・ファイルの先頭に追加して、`BMSCore` および `BMSAnalytics` フレームワークをインポートします。
```
	import BMSCore
	import BMSAnalytics
	```
	{: codeblock}  

2. {{site.data.keyword.mobileanalytics_short}} Client SDK をアプリケーション内で初期設定します。

	最初に、初期設定コードを、アプリケーション・デリゲートの `application(_:didFinishLaunchingWithOptions:)` メソッド内か、プロジェクトで最も適切な場所に置き、`BMSClient` クラスを初期化します。
	```
	BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Make sure that you point to your region
	```
	{: codeblock}

	**bluemixRegion** パラメーターを使用して `BMSClient` を初期化する必要があります。初期化指定子では、**bluemixRegion** 値が、使用している {{site.data.keyword.Bluemix_notm}} デプロイメントを指定します。

3. アプリケーション・オブジェクトを使用して分析を初期設定し、アプリケーション名を付けます。

	アプリケーション用に選択した名前 (`your_app_name_here`) は、アプリケーション名として {{site.data.keyword.mobileanalytics_short}} コンソールに表示されます。アプリケーション名は、ダッシュボードでアプリケーション・ログを検索する場合にフィルターとして使用されます。複数のプラットフォーム (例えば iOS) で同じアプリケーション名を使用すると、ログの送信元がどのプラットフォームであっても、同じ名前のアプリケーションからのすべてのログを表示できます。

	また、[API 鍵](#analytics-clientkey)の値も必要です。
	```
	Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, collectLocation: true, deviceEvents: .lifecycle, .network)
	```
    {: codeblock}

4. これで、アプリケーションが初期設定され、分析を収集する準備が整いました。次に、分析データを {{site.data.keyword.mobileanalytics_short}} サービスに送信できます。		

## ステップ 6. 使用分析の収集
{: #app-monitoring-gathering-analytics}

使用分析を記録し、記録されたデータを {{site.data.keyword.mobileanalytics_short}} サービスに送信するように、{{site.data.keyword.mobileanalytics_short}} Client SDK を構成できます。

使用統計の記録と送信を開始するには、以下の API を使用します。
```
// Disable recording of usage analytics (eg: to save disk space)
// Recording is enabled by default

Analytics.isEnabled = false

// Enable recording of usage analytics

Analytics.isEnabled = true

// Send recorded usage analytics to the Mobile Analytics Service

Analytics.send(completionHandler: { (response: Response?, error: Error?) in
    if let response = response {

	  // Handle Analytics send success here.
    }
    if let error = error {
        // Handle Analytics send failure here.
    }
})
```
{: codeblock}

イベントをログに記録するための使用分析の例:
```
// Log a custom analytics event
let eventObject = ["customProperty": "propertyValue"]
Analytics.log(metadata: eventObject)
```
{: codeblock}

## ステップ 7. ロガーの使用

{{site.data.keyword.mobileanalytics_full}} Client SDK には、`java.util.logging` や `log4j` など、ユーザーが精通している他のログ・フレームワークに似たロギング・フレームワークが用意されています。このロギング・フレームワークは、複数のパッケージ別ロガー・インスタンス、さまざまなログ・レベル、アプリケーション異常終了のスタック・トレースのキャプチャーなどをサポートしています。

また、ログに記録されたデータをアプリケーションが稼働するデバイスに保管し、あとでこれらのデバイス・ログを {{site.data.keyword.mobileanalytics_short}} サービスに送信するように構成することもできます。

{{site.data.keyword.mobileanalytics_short}} Client SDK のロギング・フレームワークでは、以下のログ・レベルがサポートされます。詳細の程度が最も低いものから高いものの順に、推奨される使用法ガイドラインと共にリストします。

  * `FATAL` - リカバリー不能な異常終了またはハングの場合に使用します。`FATAL` レベルは、ユーザーがアプリケーション異常終了として認識するリカバリー不能エラーのロギング用に予約されています。
  * `ERROR` - 予期しない例外または予期しないネットワーク・プロトコル・エラーに使用します。
  * `WARN` - 推奨されない API の使用やネットワーク応答の遅延など、致命的なエラーではないと考えられる使用上の警告をログに記録する場合に使用します。
  * `INFO` - 初期設定イベントや、重要と思われるが緊急ではないその他のデータを報告する場合に使用します。
  * `DEBUG` - 開発者がアプリケーションの問題を解決する際に役立つデバッグ・ステートメントを報告する場合に使用します。

### ログ・レベルのシナリオ
{: #log-level-scenario}

ロガー・レベルが `FATAL` に設定されている場合、ロガーはキャッチされていない例外をキャプチャーしますが、異常終了イベントにつながるログはキャプチャーしません。より詳細なロガー・レベルを設定して、`FATAL` ロガー項目につながる可能性のあるログ (`WARN` や `ERROR` など) もキャプチャーするようにできます。

ロガー・レベルが `DEBUG` に設定されている場合は、ログの送信時に含められる Mobile Analytics Client SDK ログも取得できます。

### ロガーの使用例
{: #sample-logger-usage}

**注:** ロギング・フレームワークを使用する前に、{{site.data.keyword.mobileanalytics_short}} Client SDK を使用するようにアプリケーションを構成してください。

以下のコード・スニペットにロガーの使用例を示します。
```
// Configure Logger. Disabled by default;

Logger.isLogStorageEnabled = true

// Change the minimum log level (optional). Default is - LogLevel.debug

Logger.logLevelFilter = LogLevel.info

// Multiple log instances can be created to organize your logs

let logger1 = Logger.logger(name: "feature1Logger")
let logger2 = Logger.logger(name: "feature2Logger")

// Log messages with different levels

logger1.debug(message: "debug message for feature 1")

// logger1.debug message isn't logged as logLevelFilter is set to info

logger2.info(message: "info message for feature 2")

// Send logs to the Mobile Analytics Service

Logger.send(completionHandler: { (response: Response?, error: Error?) in
        if let response = response {
        logger.debug(message: "Status code: \(response.statusCode)")
        logger.debug(message: "Response: \(response.responseText)")
    }
    if let error = error {
        logger.error(message: "Error: \(error)")
    }
})
```
{: codeblock}

プライバシーに関する懸念から、リリース・モードでビルドされたアプリケーションのロガー出力を無効にすることができます。デフォルトでは、Logger クラスはログを Xcode コンソールに出力します。ターゲットのビルド設定で、リリース・ビルド構成の **Other Swift Flags** セクションに `-D RELEASE_BUILD` フラグを追加します。
{: tip}

## ステップ 8. ロケーション・データのロギング
{: #location-logging}

モバイル・デバイスのロケーションは、提供されている以下の API を使用してアプリからログに記録できます。
```
Analytics.logLocation();
```

この API によって、アプリはロケーションを (経度と緯度として) アプリ・セッション間にサーバーに送信できます。`location-logging` API を明示的に呼び出す代わりに、この SDK はアプリ・セッションごとにデバイス・ロケーションを送信します。ロケーションは、初期アプリ・セッション・コンテキストと、ユーザー・スイッチ・アプリ・セッション・コンテキスト (つまり、アプリ・セッション間のユーザーのスイッチ) に関して送信されます。ロケーション API は、SDK の初期設定時に有効でなければなりません。

`location-logging` API を呼び出すには、`collectLocation` パラメーターを SDK 初期設定時に `true` に設定します。
```
Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
```

## ステップ 9. ネットワーク要求の作成
{: #network-requests}

[ネットワーク要求を行う](/docs/mobile/sdk_network_request.html)ように {{site.data.keyword.mobileanalytics_short}} クライアント SDK を構成することができます。既に `BMSClient` と `BMSAnalytics` が初期化されていて、クライアント SDK がインポートされていることを確認してください。

## ステップ 10. 異常終了分析の報告
{: #report-crash-analytics}

分析およびログ情報を {{site.data.keyword.mobileanalytics_short}} に送信することによって、[アプリケーションの異常終了データ](app-monitoring.html#monitor-app-crash)を表示することができます。

`Analytics.send()` メソッドによって、**「異常終了」**ページの**「異常終了の概要」**表および**「異常終了」**表にデータが設定されます。グラフは、分析の初期設定および送信プロセスを使用して有効にすることができます。特別な構成は必要ありません。


`Logger.send()` メソッドによって、**「トラブルシューティング」**ページの**「異常終了の要約」**および**「異常終了の詳細」**表にデータが設定されます。以下のようにステートメントをアプリケーション・コードに追加することによって、アプリケーションがログを保管および送信してグラフにデータを設定できるようにする必要があります。
```
Logger.isLogStorageEnabled = true
Logger.logLevelFilter = LogLevel.Fatal // or greater
```
{: codeblock}

iOS の[ロガーの使用例](sdk.html##sample-logger-usage)を参照してください。

## ステップ 11. アクティブ・ユーザーのトラッキング
{: #trackingusers}

アプリケーションが 1 つのデバイスの固有のユーザーを認識できる場合は、アクティブ・ユーザーのユーザー名を {{site.data.keyword.mobileanalytics_short}} に渡すことで、アプリケーションをアクティブに使用しているユーザー数をトラッキングできます。

`hasUserContext=true` を指定して {{site.data.keyword.mobileanalytics_short}} を初期設定することで、ユーザー・トラッキングを有効にします。そうしないと、{{site.data.keyword.mobileanalytics_short}} はデバイス 1 つにつきユーザーを 1 人しかキャプチャーしません。

ユーザーのログイン時刻をトラッキングする場合は、以下のコードを追加します。
```
Analytics.userIdentity = "username"
```
{: codeblock}

## ステップ 12. アプリのテスト
{: #appid_testing}

すべて適切にセットアップされていますか? それでは、テストの段階に進みます。

1. アプリを開きます。Web アプリケーションの場合には、ブラウザーを使用します。iOS クライアント・アプリケーションの場合には、Xcode エミュレーターを使用します。
2. エミュレーターまたはデバイスでアプリケーションをコンパイルして実行します。
3. GUI を使用してアプリケーションへのサインイン・プロセスを実行します。
4. {{site.data.keyword.mobileanalytics_short}} コンソールに移動し、アプリケーションの使用分析を確認します。[アラートを設定](/docs/services/mobileanalytics/app-monitoring.html#alerts)して、[アプリの異常終了をモニターする](/docs/services/mobileanalytics/app-monitoring.html#monitor-app-crash)ことによって、アプリケーションをモニターすることもできます。

## 次の作業
{: #what-to-do-next notoc}

 - サービスの詳細を確認するには、[この資料](/docs/services/mobileanalytics/index.html#getting-started-tutorial)をご覧ください。

 - モバイル・サービスと {{site.data.keyword.Bluemix_notm}} を使用した作業方法については、[IBM Cloud におけるモバイル・アプリの概要](/docs/services/mobile/index.html)を参照してください。

 - スターター・キットは、{{site.data.keyword.cloud_notm}} の機能を素早く活用する方法の 1 つです。[モバイル開発者ダッシュボード](https://console.bluemix.net/developer/mobile/dashboard)に使用可能なすべてのスターター・キットが表示されます。コードをダウンロードし、アプリを実行します。

 - [Swagger UI](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/) を使用すると、REST API 資料を迅速に確認できます。
