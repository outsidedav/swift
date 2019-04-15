---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: watson swift, swift developer, apple development, ios oveview, developer consoles swift, apple console

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

 ![開発者のタイプ](images/IBM_Cloud_icon.png "IBM Cloud")

{{site.data.keyword.cloud_notm}} に組み込まれている開発者機能はさまざまなスキル・セットに適合し、アプリを作成、配信、実行、管理するための単一プラットフォームを提供します。 たとえば、示されているコグニティブ・アプリの中で、次の {{site.data.keyword.cloud_notm}} 機能に注目できます。

* [**{{site.data.keyword.cloud_notm}} Developer Experience**](/docs/overview?topic=overview-dev-journey#dev-journey) はサービスではありません。これは、デジタル開発者およびクラウド・ネイティブ開発者が実動用アプリを作成し始めるときに役立つ {{site.data.keyword.cloud_notm}} の機能セットです。 これには、サービスの自動プロビジョニング、および DevOps ツールチェーンへの「ワンクリック」デプロイメントが含まれます。

* [**IBM Watson Data Platform**](https://dataplatform.ibm.com){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を使用すると、データ・コレクションの組み立て、データの調整、視覚化、洞察の発見、コグニティブ・アプリ用のモデルの作成を簡単に行うことができます。

* [**IBM Streaming Analytics**](/docs/services/StreamingAnalytics?topic=StreamingAnalytics-gettingstarted#gettingstarted) は、データ・ストリームの収集と分析に役立ちます。 これは、さまざまなタイプのデータ・ソースから到着する情報をリアルタイムで取り込み、分析し、相互に関連付けるために使用できる高機能の分析プラットフォームです。

* [**{{site.data.keyword.cloud_notm}} Continuous Delivery サービス**](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-cd_getting_started#cd_getting_started)は、アプリの継続的デリバリーを自動化するための DevOps ツールチェーンをセットアップします。 ツールチェーンを簡単に拡張して、モニター、ロギング、追跡、アラートなどの管理機能を含めることができます。 また、[DevOps Insights サービス](/docs/services/DevOpsInsights?topic=DevOpsInsights-getting-started#getting-started)を使って高度な機械学習をツールチェーンに適用することができます。

{{site.data.keyword.cloud_notm}} プラットフォームは、この他にも多くの機能を備えており、包括的な開発プラットフォームとして使用できます。

## {{site.data.keyword.cloud_notm}} 機能の概要
{: #capabilities}

{{site.data.keyword.cloud_notm}} Developer Experience はサービスではありません。これは、「正しい」方法でアプリを数分で作成し始めるのに役立つ {{site.data.keyword.cloud_notm}} プラットフォーム内の機能セットです。 Developer Experience の主な構成要素は以下のとおりです。

* {{site.data.keyword.cloud_notm}} プラットフォーム全体で検出される、トピックまたはチャネル中心の開発者コンソールからなるセット。
* さまざまな言語とアーキテクチャー・パターンで実動用スターター・アプリを生成する特定のユースケース・スターター・キット。
* サービスの自動プロビジョニング。
* 移植可能なアプリ構造を使用した、アプリ・コンポーネントの管理。
* ワンクリックによる [DevOps ツールチェーン](/docs/services/DevOpsInsights?topic=DevOpsInsights-getting-started#getting-started)作成。

実動用の高品質なアプリを迅速に作成するうえで {{site.data.keyword.cloud_notm}} Developer Experience がどのように役立つかを理解するには、以下の要素をより詳しく確認してください。

## 開発者コンソール
{: #developer_consoles}

{{site.data.keyword.cloud_notm}} Developer Experience は、{{site.data.keyword.cloud_notm}} プラットフォームのさまざまなデベロッパー・コンソールに備わっています。 各コンソールは、1 つの分野 (Watson、セキュリティー、金融など) またはデジタル・チャネル (モバイルや Web アプリなど) を表します。 [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/dashboard){: new_window}![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を使用すると、Apple 開発者は {{site.data.keyword.cloud_notm}} プラットフォームを利用してアプリケーションやサービスを迅速に実験できます。{{site.data.keyword.cloud_notm}} コンソールでメニュー・アイコンをクリックすると、これらのコンソールにアクセスできます。 たとえば、次の開発者コンソールをご覧ください。

* [Watson 開発者コンソール](https://cloud.ibm.com/developer/watson/dashboard){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
* [モバイル開発者コンソール](https://cloud.ibm.com/developer/mobile/dashboard){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
* [Web アプリ開発者コンソール](https://cloud.ibm.com/developer/appservice/dashboard){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
* [セキュリティー開発者コンソール](https://cloud.ibm.com/developer/security/dashboard){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")
* [金融開発者コンソール](https://cloud.ibm.com/developer/finance/dashboard){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")

<!--Cloud native development is the process of developing apps that are optimized to leverage capabilities engendered from running on the cloud.  Flexibility, portability, scaling, rapid development, continuous delivery, and a close coupling development and operations ("devops) are characteristics of cloud applications. The {{site.data.keyword.cloud}} Developer Experience quickly gets you started building cloud native applications that are ready for team development and bound for production use.-->


<!--![Overview of elements of the {{site.data.keyword.cloud_notm}} Developer Experience](images/elements_of_devex.png "Overview of elements of the {{site.data.keyword.cloud_notm}} Developer Experience") <br> *Overview of elements of the {{site.data.keyword.cloud_notm}} Developer Experience*-->

各開発者コンソールには、そのコンソールの領域に関連するスターター・キットがあり、一貫性のある直観的なワークフローを使って実動用のアプリを数分で作成できます。

## スターター・キットを使用したアプリの作成
{: #starterkit-apps}

Swift アプリを作成するために[スターター・キット](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro)を活用できます。これには、アプリを構成するコード、データ、サービス、ツールチェーンの関連付けが含まれます。
