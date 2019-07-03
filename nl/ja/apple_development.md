---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: watson swift, swift developer, apple development, ios oveview, developer console, swift, apple console

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:tip .tip}

# {{site.data.keyword.cloud_notm}} での Apple 開発
{: #ios_dev}

{{site.data.keyword.cloud}} は、皆様の顧客が求めるセキュリティー、AI、価値を Swift 開発者およびアプリケーションが実現できるようにするためのソリューションとサービスを提供します。 オファリングと SDK の幅広いポートフォリオが用意されているので、これらのサービスを活用し、最先端のアプリケーションを迅速に市場に出すことができます。

## 1 つの開発プラットフォーム: IBM Cloud
{: #platform}

{{site.data.keyword.cloud_notm}} に組み込まれている開発者機能はさまざまなスキル・セットに適合し、アプリを作成、配信、実行、管理するための単一プラットフォームを提供します。 たとえば、示されているコグニティブ・アプリの中で、次の {{site.data.keyword.cloud_notm}} 機能に注目できます。

* [**{{site.data.keyword.cloud_notm}} 開発者コンソール**](/docs/apps?topic=creating-apps-getting-started)には、デジタル開発者やクラウド・ネイティブ開発者が実動用アプリのビルドを開始するのに役立つ {{site.data.keyword.cloud_notm}} の機能セットが含まれています。これには、サービスの自動プロビジョニング、および DevOps ツールチェーンへの「ワンクリック」デプロイメントが含まれます。

* [**IBM Watson Data Platform**](https://dataplatform.cloud.ibm.com/){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を使用すると、データ・コレクションの組み立て、データの調整、視覚化、洞察の発見、コグニティブ・アプリ用のモデルの作成を簡単に行うことができます。

* [**IBM Streaming Analytics**](/docs/services/StreamingAnalytics?topic=StreamingAnalytics-gettingstarted#gettingstarted) は、データ・ストリームの収集と分析に役立ちます。 これは、さまざまなタイプのデータ・ソースから到着する情報をリアルタイムで取り込み、分析し、相互に関連付けるために使用できる高機能の分析プラットフォームです。

* [**{{site.data.keyword.cloud_notm}} Continuous Delivery サービス**](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-getting-started)は、アプリの継続的デリバリーを自動化するための DevOps ツールチェーンをセットアップします。 ツールチェーンを簡単に拡張して、モニター、ロギング、追跡、アラートなどの管理機能を含めることができます。 また、[DevOps Insights サービス](/docs/services/DevOpsInsights?topic=DevOpsInsights-getting-started#getting-started)を使って高度な機械学習をツールチェーンに適用することができます。

{{site.data.keyword.cloud_notm}} プラットフォームは、この他にも多くの機能を備えており、包括的な開発プラットフォームとして使用できます。

## {{site.data.keyword.cloud_notm}} 機能の概要
{: #capabilities}

{{site.data.keyword.cloud_notm}} 開発者コンソールを使用してアプリのビルドを開始すると、「適切な」ビルドを数分で行えます。開発者コンソールの主な構成要素は以下のとおりです。

* {{site.data.keyword.cloud_notm}} プラットフォーム全体で検出される、トピックまたはチャネル中心の開発者コンソールからなるセット。
* さまざまな言語とアーキテクチャー・パターンで実動用スターター・アプリを生成する特定のユースケース・スターター・キット。
* サービスの自動プロビジョニング。
* 移植可能なアプリ構造を使用した、アプリ・コンポーネントの管理。
* ワンクリックによる [DevOps ツールチェーン](/docs/services/DevOpsInsights?topic=DevOpsInsights-getting-started#getting-started)作成。

実動用の高品質なアプリを迅速に作成するうえで {{site.data.keyword.cloud_notm}} 開発者コンソールがどのように役立つかを理解するには、以下の要素をより詳しく確認してください。

## 開発者コンソール
{: #developer_consoles}

{{site.data.keyword.cloud_notm}} 開発者コンソールには、対象となる領域 (Watson、セキュリティー、金融など) またはデジタル・チャネル (モバイルや Web アプリなど) が含まれています。[{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/dashboard){: new_window}![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を使用すると、Apple 開発者は {{site.data.keyword.cloud_notm}} プラットフォームを利用してアプリケーションやサービスを迅速に実験できます。 {{site.data.keyword.cloud_notm}} コンソールでメニュー・アイコンをクリックすると、これらのコンソールにアクセスできます。 たとえば、次の開発者コンソールをご覧ください。

* [Watson 開発者コンソール](https://{DomainName}/developer/watson/dashboard){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
* [モバイル開発者コンソール](https://{DomainName}/developer/mobile/dashboard){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
* [Web アプリ開発者コンソール](https://{DomainName}/developer/appservice/dashboard){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
* [セキュリティー開発者コンソール](https://{DomainName}/developer/security/dashboard){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
* [金融開発者コンソール](https://{DomainName}/developer/finance/dashboard){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")

<!--Cloud native development is the process of developing apps that are optimized to leverage capabilities engendered from running on the cloud.  Flexibility, portability, scaling, rapid development, continuous delivery, and a close coupling development and operations ("devops) are characteristics of cloud applications. The {{site.data.keyword.cloud}} developer console quickly gets you started building cloud native applications that are ready for team development and bound for production use.-->


<!--![Overview of elements of the {{site.data.keyword.cloud_notm}} developer console](images/elements_of_devex.png "Overview of elements of the {{site.data.keyword.cloud_notm}} developer console") <br> *Overview of elements of the {{site.data.keyword.cloud_notm}} developer console*-->

各開発者コンソールには、そのコンソールの領域に関連するスターター・キットがあり、一貫性のある直観的なワークフローを使って実動用のアプリを数分で作成できます。

## スターター・キットを使用したアプリの作成
{: #starterkit-apps}

Swift アプリを作成するために[スターター・キット](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro)を活用できます。これには、アプリを構成するコード、データ、サービス、ツールチェーンの関連付けが含まれます。
