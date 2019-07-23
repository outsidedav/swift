---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-21"

keywords: push swift, swift notifications, push notifications swift, sending push swift, configure service instance swift, provider credentials swift

subcollection: swift

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

{{site.data.keyword.mobilepushshort}} サービスは、MobileFirst Services のスターター・ボイラープレートの一部として、または {{site.data.keyword.cloud_notm}} [専用サービス](/docs/dedicated?topic=dedicated-dedicated#dedicated)として、使用することができます。 また、SDK (Software Development Kit) と [REST API ](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/notifications/rest-apis/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を使用して、クライアント・アプリケーションをさらに開発することもできます。

![プッシュの概要](images/push_notification_lifecycle.jpg "プッシュの概要")

## 始める前に
{: #prereqs-push}

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
{: #get_creds-push}

Push Notification サービスをセットアップするには、Apple Push Notification Service (APN) から、必要な資格情報を取得する必要があります。 ここに示すステップを実行して、[APN 資格情報を取得および構成します ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1){: new_window}。


## ステップ 3. サービス・インスタンスの構成
{: #config-resource-push}

{{site.data.keyword.mobilepushshort}} サービスを使用して通知を送信するには、作成した `.p12` 鍵ストアをアップロードします。この鍵ストアには、アプリケーションの構築と公開に必要な、秘密鍵と SSL 証明書があります。 REST API を使用して APN 証明書をアップロードすることもできます。

`.cer` ファイルがキー・チェーン・アクセスに配置された後に、それをコンピューターにエクスポートして`.p12` 証明書を作成します。

APN の使用について詳しくは、[iOS Developer Library: Local and Push Notification Programming Guide ](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")を参照してください。

Push Notification サービス・コンソールで APN をセットアップするには、以下のステップを実行します。

1. {{site.data.keyword.mobilepushshort}} サービス・コンソールで**「構成」**を選択します。
2. **「モバイル」**オプションを選択し、**「APN プッシュ資格情報 (APNs Push Credentials)」**フォームの情報を更新します。
3. 次のいずれかのオプションを選択します。
	- **「モバイル」**オプションの場合
		1. **「サンドボックス」**(開発) または**「実動」**(配布) を選択し、作成した `p.12` 証明書をアップロードします。
		  ![{{site.data.keyword.mobilepushshort}} コンソール](images/wizard.jpg "プッシュ通知のモバイル構成")

		2. **「パスワード」**フィールドに、`.p12` 証明書ファイルに関連付けられているパスワードを入力し、次に**「保存」**をクリックします。
	- **「Web」**オプションの場合
		- 「Safari Push (Safari Push)」セクションで、必要な情報を指定してフォームを更新します。
		- **Web サイト名 (Website Name)**: 通知センターで提供される Web サイト名。
		- **Web サイト・プッシュ ID (Website Push ID)**: Web サイトのプッシュ ID の反転ドメイン・ストリングを使用して更新します。 例えば、`web.com.example.www` です。
		- **Web サイト URL (Website URL)**: プッシュ通知をサブスクライブする必要がある Web サイトの URL を指定します。 例えば、`https://www.example.com` です。
		- **許可されたドメイン (Allowed Domains)**: (オプション・パラメーター) ユーザーからの許可を要求する Web サイトのリスト。 URL は、必ずコンマ区切りの値で指定します。 情報を指定しない場合、Web サイト URL の中の値が使用されます。
		- **URL 形式文字列 (URL Format String)**: 通知がクリックされたときに解決される URL。 例えば、`https://www.example.com` です。 その URL で http または https のスキーマが使用されていることを確認してください。
		-**Safari Web プッシュ証明書 (Safari web push certificate)**: `.p12` 証明書をアップロードし、パスワードを指定します。
4. **「保存」**をクリックします。

  ![{{site.data.keyword.mobilepushshort}} コンソール](images/push_configure_safari.jpg "プッシュ通知の Web 構成")

## ステップ 4. サービス・クライアント SDK のセットアップ
{: #service-client-push}

iOS アプリケーションがデバイスへのプッシュ通知を受信できるようにするには、{{site.data.keyword.mobilepushshort}} サービス用の iOS SDK を構成する必要があります。

{{site.data.keyword.cloud_notm}} Mobile Services Swift SDK は、Cocoapods と Carthage のどちらにもインストールできます。 詳しくは、[https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。

## ステップ 5. 通知の送信
{: #send-notify-push}

アプリケーションの開発が完了したら、基本的なプッシュ通知を送信できます。

基本的なプッシュ通知を送信するには、以下の手順を実行します。

1. **「通知の送信 (Send Notifications)」**を選択し、**「送信先 (Send To)」**オプションを選択することでメッセージを構成します。 サポートされるオプションは、**「タグ指定によるデバイス (Device by Tag)」**、**「デバイス ID」**、**「ユーザー ID」**、**「iOS デバイス (iOS devices)」**、**「Web 通知 (Web Notifications)」**、および**「すべてのデバイス」**です。
**注**: **「すべてのデバイス」**オプションを選択すると、{{site.data.keyword.mobilepushshort}}をサブスクライブしているすべてのデバイスが通知を受け取ることになります。

  ![「通知の送信 (Send Notifications)」画面](images/tag_notification.jpg "「通知の送信 (Send Notifications)」画面")

2. **「メッセージ」**フィールドで、メッセージを作成します。 必要に応じてオプションの設定を構成してください。
3. **「送信」**をクリックします。
3. デバイスまたはブラウザーが通知を受信したことを確認します。

  次の画面キャプチャーは、デバイスのフォアグラウンドでプッシュ通知を処理する、アラート・ボックスを示しています。
  
  ![フォアグラウンド・プッシュ通知 (Android)](images/Android_Screenshot.jpg "フォアグラウンド通知アラート")

  次の画面キャプチャーは、バックグラウンドでのプッシュ通知を示しています。
  
  ![バックグラウンド・プッシュ通知 (iOSd)](images/background.png "バックグラウンド通知アラート")

### オプションの設定
{: #optional-push}

iOS デバイスに通知を送信するための {{site.data.keyword.mobilepushshort}} 設定をカスタマイズできます。 以下の任意指定のカスタマイズ・オプションがサポートされます。

- **バッジ (Badge)**: アプリケーション・バッジに表示される数値を示します。 デフォルト値はゼロ (0) で、この場合、バッジは表示されません。
- **音 (Sound)**: 通知の受信時に音声クリップを再生するかどうかを示します。 デフォルト、またはアプリにバンドルされているサウンド・リソースの名前がサポートされています。
- **追加のペイロード (Additional payload)**: 通知用のカスタム・ペイロードの値を指定します。

[対話式通知](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#interactive-notifications){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")および[リッチ・メディア通知](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enabling-rich-media-notifications){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を有効にすることを選択することもできます。

## ステップ 6. 配信された通知のモニター
{: #monitor-push}

{{site.data.keyword.mobilepushshort}} サービスは、送信されるメッセージの状況の検査に役立つモニタリング・ユーティリティーを提供します。 モニタリング・ユーティリティーを構成するには、[iOS アプリケーションのモニタリングの有効化](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enable-monitoring){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。

## 次のステップ
{: #next-push notoc}

 - サービスについての詳細情報を参照し、すべての機能を活用するには、[ドキュメンテーション](/docs/services/mobilepush?topic=mobile-pushnotification-overview-push)を参照してください。

 - モバイル・サービスと {{site.data.keyword.cloud_notm}} の作業の概要については、[Getting started with Mobile apps on {{site.data.keyword.cloud_notm}}](/docs/services/mobile?topic=mobile-about) を参照してください。

 - スターター・キットは、{{site.data.keyword.cloud_notm}} の機能を素早く使用する方法の 1 つです。 [モバイル開発者ダッシュボード](https://{DomainName}/developer/mobile/dashboard){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") で、使用可能なスターター・キットを確認できます。 コードをダウンロードして アプリを実行します。

 - [Swagger UI](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/notifications/rest-apis/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を使用すると、REST API 資料を迅速に確認できます。
