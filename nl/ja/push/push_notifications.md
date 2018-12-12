---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.mobilepushshort}} の送信
{: #push_notifications}

{{site.data.keyword.cloud}} で {{site.data.keyword.mobilepushshort}} サービスを使用して、モバイル・デバイスおよび Web アプリケーションにライブ通知を送信することによって、Swift アプリを強化します。

 - 通知は、すべてのアプリケーション・ユーザーに配信したり、選択した一連のユーザーまたはデバイスに配信したりすることができます。
 - 対話式通知とサイレント通知の両方をサポートします。
 - お客様は、通知の対象として特定のタグまたはトピックへのサブスクライブを選択できます。
 - アプリ所有者が、通知を受信するように登録されているデバイスの数や、送信された通知の数を分析することを可能にします。

{{site.data.keyword.mobilepushshort}} サービスは、MobileFirst Services のスターター・ボイラープレートの一部として、または {{site.data.keyword.cloud_notm}} [専用サービス](/docs/dedicated/index.html)として、使用することができます。 また、SDK (Software Development Kit) と [REST API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobile.{DomainName}/imfpush/){: new_window}を使用して、クライアント・アプリケーションをさらに開発することもできます。

![プッシュの概説](images/push_notification_lifecycle.jpg) 図 1. {{site.data.keyword.mobilepushshort}} サービスのライフサイクルの概要

## 始めに

まず、以下の前提条件が整っていることを確認してください。

 - iOS 8.0 以上
 - Xcode 7.3、8.0
 - Swift 2.3 - 4.0
 - CocoaPods または Carthage

## ステップ 1. {{site.data.keyword.mobilepushshort}} のインスタンスの作成
{: #push_create}

1. {{site.data.keyword.cloud_notm}} カタログで、**「モバイル」**>**「{{site.data.keyword.mobilepushshort}}」**をクリックします。 サービス構成画面が開きます。
2. サービス・インスタンスに名前を付けます。または、事前設定された名前を使用します。
3. **「作成」**をクリックします。
4. ナビゲーション・ペインで、**「接続」**をクリックしてアプリを選択し、サービスにバインドします。 作成時にアンバインドの状態のままにする場合、後ほど、アプリにサービス・インスタンスをバインドできます。


## ステップ 2. 通知プロバイダーの資格情報の取得
{: #get_creds}

Push Notification サービスをセットアップするには、Apple Push Notification Service (APN) から、必要な資格情報を取得する必要があります。 ここに示すステップを実行して、[APN 資格情報を取得および構成します ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/services/mobilepush/push_step_1.html#push_step_1_ios){: new_window}。


## ステップ 3. サービス・インスタンスの構成
{: #enable-push-ios-notifications}

{{site.data.keyword.mobilepushshort}} サービスを使用して通知を送信するには、作成した `.p12` 鍵ストアをアップロードします。この鍵ストアには、アプリケーションの構築と公開に必要な、秘密鍵と SSL 証明書があります。REST API を使用して APN 証明書をアップロードすることもできます。

`.cer` ファイルがキー・チェーン・アクセスに配置された後に、それをコンピューターにエクスポートして`.p12` 証明書を作成します。

APN の使用について詳しくは、[iOS Developer Library: Local and Push Notification Programming Guide ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1){: new_window}を参照してください。

Push Notification サービス・コンソールで APN をセットアップするには、以下のステップを実行します。

1. {{site.data.keyword.mobilepushshort}} サービス・コンソールで**「構成」**を選択します。
2. **「モバイル」**オプションを選択し、**「APN プッシュ資格情報 (APNs Push Credentials)」**フォームの情報を更新します。
3. 次のいずれかのオプションを選択します。
	- **「モバイル」**オプションの場合
		1. **「サンドボックス」**(開発) または**「実動」**(配布) を選択し、作成した `p.12` 証明書をアップロードします。
![{{site.data.keyword.mobilepushshort}} コンソールの設定](images/wizard.jpg)

		2. **「パスワード」**フィールドに、`.p12` 証明書ファイルに関連付けられているパスワードを入力し、次に**「保存」**をクリックします。
	- **「Web」**オプションの場合
		- 「Safari Push (Safari Push)」セクションで、必要な情報を指定してフォームを更新します。
		- **Web サイト名 (Website Name)**: 通知センターで提供される Web サイト名。
		- **Web サイト・プッシュ ID (Website Push ID)**: Web サイトのプッシュ ID の反転ドメイン・ストリングを使用して更新します。 例: web.com.acmebanks.www
		- **Web サイト URL (Website URL)**: プッシュ通知をサブスクライブする必要がある Web サイトの URL を指定します。例: https://www.acmebanks.com
		- **許可されたドメイン (Allowed Domains)**: (オプション・パラメーター) ユーザーからの許可を要求する Web サイトのリスト。 URL は、必ずコンマ区切りの値で指定します。 情報を指定しない場合、Web サイト URL の中の値が使用されます。
		- **URL 形式文字列 (URL Format String)**: 通知がクリックされたときに解決される URL。 例: ["https://www.acmebanks.com"]。 その URL で http または https のスキーマが使用されていることを確認してください。
		-**Safari Web プッシュ証明書 (Safari web push certificate)**: `.p12` 証明書をアップロードし、パスワードを指定します。
4. **「保存」**をクリックします。
![{{site.data.keyword.mobilepushshort}} コンソール](images/push_configure_safari.jpg)

## ステップ 4. サービス・クライアント SDK のセットアップ

iOS アプリケーションがデバイスへのプッシュ通知を受信できるようにするには、{{site.data.keyword.mobilepushshort}} サービス用の iOS SDK を構成する必要があります。

{{site.data.keyword.cloud_notm}} Mobile Services Swift SDK は、Cocoapods と Carthage のどちらにもインストールできます。 詳しくは、[https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application) を参照してください。


## ステップ 5. 通知の送信

アプリケーションの開発が完了したら、基本的なプッシュ通知を送信できます。

基本的なプッシュ通知を送信するには、以下の手順を実行します。

1. **「通知の送信 (Send Notifications)」**を選択し、**「送信先 (Send To)」**オプションを選択することでメッセージを構成します。 サポートされるオプションは、**「タグ指定によるデバイス (Device by Tag)」**、**「デバイス ID」**、**「ユーザー ID」**、**「iOS デバイス (iOS devices)」**、**「Web 通知 (Web Notifications)」**、および**「すべてのデバイス」**です。
**注**: **「すべてのデバイス」**オプションを選択すると、{{site.data.keyword.mobilepushshort}}をサブスクライブしているすべてのデバイスが通知を受け取ることになります。

	![通知画面](images/tag_notification.jpg)

2. **「メッセージ」**フィールドで、メッセージを作成します。 必要に応じてオプションの設定を構成してください。
3. **「送信」**をクリックします。
3. デバイスまたはブラウザーが通知を受信したことを確認します。

次の画面キャプチャーは、デバイスのフォアグラウンドでプッシュ通知を処理する、アラート・ボックスを示しています。
	![Android でのフォアグラウンド・プッシュ通知](images/Android_Screenshot.jpg)

次の画面キャプチャーは、バックグラウンドでのプッシュ通知を示しています。
	![Android でのバックグラウンド・プッシュ通知](images/background.png)

### オプションの設定
{: #push_step_4_ios}

iOS デバイスに通知を送信するための {{site.data.keyword.mobilepushshort}} 設定をカスタマイズできます。 以下の任意指定のカスタマイズ・オプションがサポートされます。

- **バッジ (Badge)**: アプリケーション・バッジに表示される数値を示します。 デフォルト値はゼロ (0) で、この場合、バッジは表示されません。
- **音 (Sound)**: 通知の受信時に音声クリップを再生するかどうかを示します。 デフォルト、またはアプリにバンドルされているサウンド・リソースの名前がサポートされています。
- **追加のペイロード (Additional payload)**: 通知用のカスタム・ペイロードの値を指定します。

[対話式通知](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#interactive-notifications)および[リッチ・メディア通知](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enabling-rich-media-notifications)を有効にすることを選択することもできます。

## ステップ 6. 配信された通知のモニター
{: #push_step_4_monitor}

{{site.data.keyword.mobilepushshort}} サービスは、送信されるメッセージの状況の検査に役立つモニタリング・ユーティリティーを提供します。 モニタリング・ユーティリティーを構成するには、[iOS アプリケーションのモニタリングの有効化](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enable-monitoring)を参照してください。

## 次のステップ

 - サービスについての詳細情報を参照し、すべての機能を活用するには、[ドキュメンテーション](/docs/services/mobilepush/c_overview_push.html#overview-push)を参照してください。

 - モバイル・サービスと {{site.data.keyword.cloud_notm}} の作業の概要については、[Getting started with Mobile apps on {{site.data.keyword.cloud_notm}}](/docs/services/mobile/index.html) を参照してください。

 - スターター・キットは、{{site.data.keyword.cloud_notm}} の機能を素早く使用する方法の 1 つです。 [モバイル開発者ダッシュボード](https://console.bluemix.net/developer/mobile/dashboard)にある使用可能なスターター・キットをご覧ください。 コードをダウンロードし、 アプリを実行してください。

 - [Swagger UI](https://mobile.ng.bluemix.net/imfpush/) を使用することにより、REST API ドキュメンテーションを素早く確認することができます。
