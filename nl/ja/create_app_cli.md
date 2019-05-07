---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-19"

keywords: server-side swift, swift cli, swift dependency, swift commands app, create app swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# CLI を使用したサーバー・サイド Swift アプリの作成
{: #swift_cli}

{{site.data.keyword.cloud}} は、顧客が求めるセキュリティー、AI、価値を Swift 開発者およびアプリケーションが実現できるようにするためのソリューションとサービスを提供します。 オファリングと SDK の幅広いポートフォリオが用意されているので、これらのサービスを使用し、最先端のアプリケーションを迅速に市場に出すことができます。
{: shortdesc}

以下のガイドは、サーバー・サイド Swift アプリのビルド、ローカル実行、デプロイに役立つことを目的としています。 {{site.data.keyword.dev_cli_long}} を使用してこれらのアクションを標準コマンドで実行する方法について説明します。

{{site.data.keyword.dev_cli_short}} を使用すると、10 以上のコマンドを使用してサーバー・サイドのアプリケーションを管理できます。 `ibmcloud dev` コマンドの詳細については、[{{site.data.keyword.dev_cli_notm}}CLI](/docs/cli/idt?topic=cloud-cli-idt-cli#idt-cli) を参照してください。

## ステップ 1. 開発者向けの要件
{: #prereqs-swift-cli}

{{site.data.keyword.cloud_notm}} を始めるには、以下の要件を満たしていることを確認してください。

### オペレーティング・システム
{: #swift-cli-os-reqs}

MacOS がサポートする最新のハードウェアを使用し、最新の iOS リリースでテストすることで、ベスト・プラクティスに従った Swift アプリの開発を行います。 [Apple Developer](https://developer.apple.com/){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") アカウントに登録して、物理デバイス上でテストできるようにしてください。

### iOS および Xcode
{: #swift-cli-ios_xcode}

- [Xcode 8+ (以上) をインストールする](https://developer.apple.com/xcode/){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
- [iOS デバイス 8 (以上) にデプロイする](https://support.apple.com/downloads/ios){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
- Apple へのアプリ送信の前に [App Store Submission Guidelines](https://developer.apple.com/app-store/guidelines/){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を確認する

### SDK と依存関係の管理
{: #swift-cli-sdk-dependency}

以下のツールを使用すると、さまざまな {{site.data.keyword.cloud_notm}} サービスを扱うネイティブ SDK をインストールできます。

- [IBM Cloud SDK 用 CocoaPods のインストール](https://cocoapods.org/){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Carthage のインストールをサポートする Homebrew のインストール](https://brew.sh/){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Watson SDK 用 Carthage のインストール](https://github.com/Carthage/Carthage){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
  ```
  brew install carthage
  ```
  {: codeblock}

## ステップ 2. ローカル開発用ツールのインストール
{: #swift-cli-install-tools}

{{site.data.keyword.cloud}} には、{{site.data.keyword.cloud_notm}} のさまざまな側面を扱うのに役立つローカル CLI ツールが用意されています. 詳しくは、[{{site.data.keyword.dev_cli_long}} 情報](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)を参照してください。 これらのツールは、クラウド・デプロイメントの前にローカル Docker イメージ内の Swift バックエンドをテストするために使用できます。

* MacOS および Linux の場合は、次のコマンドを実行します。
  ```
  curl -sL http://ibm.biz/idt-installer | bash
  ```
  {: codeblock}

* Windows 10 の場合は、管理者として次のコマンドを実行します。
  ```
  Set-ExecutionPolicy Unrestricted; iex(New-Object Net.WebClient).DownloadString('http://ibm.biz/idt-win-installer')
  ```
  {: codeblock}

  Windows PowerShell アイコンを右クリックして、**「管理者として実行」**を選択します。
  {: tip}

## ステップ 3. Swift アプリの作成
{: #create-swift-app-cli}

1. {{site.data.keyword.dev_cli_short}} CLI から `ibmcloud dev create` コマンドを実行して、事前構成済みスターターを生成します。 
  ```
  ibmcloud dev create
  ```
  {: codeblock}

  プロジェクトを作成するためには、{{site.data.keyword.cloud_notm}} アカウントでログインする必要があります。 初めてのユーザーは、無料アカウントに[登録 ](https://cloud.ibm.com/registration){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") できます。 コマンド・ラインでログインするには、`ibmcloud login` コマンドを使用します。
  {: tip}

2. プロンプトが出されたら、次の例に示すように、オプション 1、次に 6、最後に 2 を選択します。
  ```
  ? Select a service type:                  
  1. Backend Service / Web App
  2. Mobile Client
  Enter a number> 1

  ? Select a language:
  1. Java - MicroProfile / Java EE
  2. Java - Spring
  3. Node
  4. Python - Django
  5. Python - Flask
  6. Swift
  Enter a number> 6

  ? Select a Starter Kit:
  1. Backend for Frontend - Swift Kitura - A starter for building 
  backend-for-frontend APIs in Swift, using the Kitura framework.
  2. Web App - Create Project
  3. Web App - Swift Kitura Basic - A starter that provides a basic web serving 
  application in Swift, using the Kitura framework.
  Enter a number> 2
  ```
  {: screen}

3. アプリケーションの名前を指定します。
  ```
  ? Enter a name for your application> swift_project
  ```
  {: screen}

4. OpenAPI 2.0 サポートを有効にすることを選択します。
  ```
  ? Enable your application based on an OpenAPI 2.0 Specification document? [y/n]> y
  ```
  {: screen}

  OpenAPI 2.0 サポートを有効にした場合は、OpenAPI 2.0 ドキュメントへのパスを URL として指定する必要があります。
  ```
  ? Path to OpenAPI 2.0 document as a url (both yaml and json formats supported)> http://hostname.domain.com/path/to/file.json

  Generating application ...
  ```
  {: screen}

## ステップ 4. アプリケーションのビルド、実行、デプロイ
{: #swift-cli-deploy}

{{site.data.keyword.dev_cli_short}} を使用してアプリケーションをビルド、実行、デプロイできるようになりました。

1. **ビルド**

  アプリケーションをビルドできるようになりました。これは、アプリケーションを実行するための前提条件です。 アプリをビルドするには、アプリケーション・ディレクトリーのルートで次のコマンドを使用します。
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **実行**

  ビルドが正常に完了したら、次のコマンドを使用してローカル・コンテナー内でアプリケーションを実行できます。
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  コマンドが正常に実行されると、アプリケーションのランディング・ページを表示するためのローカル・ホストとポートが表示されます。

3. **デプロイ**

  `deploy` コマンドを使用して、アプリケーションを {{site.data.keyword.cloud_notm}} にデプロイします。
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}

## 次のステップ
{: #swift-cli-next notoc}

{{site.data.keyword.cloud_notm}} Developer Console for Apple の使用方法を学びます。開発者はこれを使用することにより、さまざまなスターター・キットからアプリを作成し、重要な {{site.data.keyword.cloud_notm}} 最適化サービスを作成して接続した後、有効なコードを素早くダウンロードしたり、継続的デリバリーのためのセットアップを行ったりします。 ユーザーは、アプリのコードをダウンロードできるだけでなく、アプリを作成、表示、構成、管理できます。 Developer Console for Apple を使用することによって、新しいアプリで {{site.data.keyword.cloud_notm}} サービスを迅速に評価し、テストすることができます。

開始の準備ができましたか? 今すぐ [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") にアクセスして始めましょう。
{: tip}

詳しくは、[スターター・キットを使用した Swift アプリの作成](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro)を参照してください。
