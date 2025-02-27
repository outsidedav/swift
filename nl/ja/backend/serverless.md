---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: reduce cost swift, serverless swift, openwhisk swift, functions swift, faas swift, stateless swift, api reference swift, high availability swift, serverless ios

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip}

# サーバーレス開発
{: #serverless-dev-swift}

サーバーレスとは何でしょうか? サーバーレス開発パターンとは、サーバー・サイドのロジックがステートレス・コンテナー内で実行されるアプリケーション開発を指します。 このコンテナーは、イベントによってトリガーされる、一時的なものであり (単一の実行の間持続する)、サード・パーティーによって全面的に管理されます。 このパラダイム (Functions as a Service (FaaS) とも呼ばれる) では、開発者は明示的にサーバーの作成や管理を行わずにトリガーして実行できる、ステートレス機能を提供します。

サーバーレス・アーキテクチャーでは、サーバー・サイドの開発に必要なインフラストラクチャーとフレームワークを除外することにより、変更データに応じて実行されるコードを作成することに開発者が集中できるようになります。

IBM の FaaS オファリングである [{{site.data.keyword.openwhisk}}](https://{DomainName}/openwhisk){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") は、専門的なサーバー・サイドの知識を必要としない、シンプルなサーバー・サイドの開発エクスペリエンスを提供するように努めています。 サーバーレス・テクノロジーを利用すると、前もってリソースを作成しなくても、事実上どんなワークロード要求にも対応できる、拡張可能なバックエンド・ソリューションを迅速に開発できます。 負荷のパターンが予測できなかったり、サーバーのダウン時間が長かったりするアプリケーションの場合、{{site.data.keyword.openwhisk_short}} は、パフォーマンスを向上させる優れたクラウド・ソリューションとなります。また、使用した分だけ支払うシステムなので、コストの削減にも役立ちます。

## アーキテクチャーの変更点
{: #comparison-serverless}

FaaS に切り替えた場合のアーキテクチャー上の利点を理解するのに役立つように、データベースにリンクされた単純な iOS アプリケーションを使用して、従来のアーキテクチャーと FaaS アーキテクチャーを比較します。

従来のアーキテクチャーの場合、iOS アプリケーションは、ネットワークへの負荷が大きいタスクをオフロードしたり、中央サーバーにおいてリモートでデータを処理したりします。アプリケーション自体は、独自のサービスかストレージ・オプションによって接続されます。 クライアントへの負荷を最小限に抑え、ユーザー・ベース全体での同期化を提供するため、単一のサーバーに、要求の厳しいタスクが多数集中し、大きな負荷がかかります。

サーバーレス・アーキテクチャーはこの構造を変更して、以下のイメージのようにすることができます。

![サーバーレス・アーキテクチャー](./images/Architecture.png "サーバーレス・アーキテクチャー")

サーバーレス・アーキテクチャーでは、単一のサーバー内ですべての処理や認証のロジックを処理せずに、サーバー・サイドのロジックの多くをカプセル化する機能を利用し、一部のロジックをクライアント (および外部サービス) にオフロードします。

図式を参照すると、以下の要点が見えてきます。

1. クライアントが、App ID などの ID プロバイダーに対する認証を行います。
2. アクセス・トークンを含む FaaS バックエンド API に対する呼び出しを行います。
3. {{site.data.keyword.openwhisk_short}} を使用してバックエンドが実装されます。 Web アクションとして公開されるサーバーレス・アクションでは、実際の API へのアクセスのために、要求ヘッダーに入れたトークンが送信され、検証 (署名と有効期限日付) される必要があります。
4. クライアントがデータを送信すると、{{site.data.keyword.cloudant_short_notm}} 内にフィードバックが保管されます。
5. {{site.data.keyword.toneanalyzershort}} を使用して、フィードバックのテキストが処理されます。
6. 分析結果に基づいて、{{site.data.keyword.mobilepushshort}} によって通知がクライアントに返送されます。
7. クライアントが通知を受け取ります。

純粋なサーバーレス・モデルでは、ユーザーの状態を保管できないために、クライアントに余分な負担がかかることがよくあります。 クライアントと {{site.data.keyword.appid_short_notm}} の ID プロバイダー・サービスで許可が扱われます。

サーバーレス・アーキテクチャーは常に理想的とは限りませんが、適切なチームや使用法の条件下ではかなりのメリットがあります。 特定の例をいくつか確認して、最も一般的な[ユース・ケース](#use_cases)をいくつか学習してください。

## サーバーレスの利点
{: #benefits-serverless}

### コストの削減
{: #reduced-cost-serverless}

システム管理に関連した時間と金額のコストを外部委託することにより、従来型のバックエンド・サーバーに関連したコスト全体を削減します。 また、{{site.data.keyword.openwhisk_short}} は従来型のコンピューティング・テクノロジーと異なります。これはユーザーが、コードが要求を満たすためにかかる時間 (100 ミリ秒単位で切り上げ) に対してのみ支払うためです。 VM やコンテナーのような他のテクノロジーと比較して、相当のコスト削減を実現できる可能性があります。VM やコンテナーの場合は、100% 使用されない可能性や、クラウド・プロバイダーのシステムでメモリーが占有される可能性があります。

### 高可用性とスケーラビリティー
{: #ha-serverless}

サーバーレス・アーキテクチャーは本質的に、ほぼ一定の可用性を備えた即時のスケーラビリティーを提供します。

### 開発のスピードとシンプル化
{: #speed-serverless}

サーバーレス・パラダイムは、システム管理の必要をなくし、デプロイメント用の単純なインターフェースを提供することで、アプリケーション開発を加速させます。 開発者は、イベント・ドリブンの方式に反応して実行されるアクション・シーケンスによって、アプリを素早くビルドできます。

## ユース・ケースの例
{: #use-cases-serverless}

### モバイル・バックエンド
{: #mobile-backend-serverless}
![モバイル・バックエンド](./images/cloud-functions-rest-api-trigger.png "モバイル・バックエンド")

モバイル開発者は、サーバー・サイドのロジックに簡単にアクセスし、計算主体の作業をクラウド・プラットフォームに外部委託することができます。 サーバー・サイドの経験がなくても、iOS SDK を使用して、Swift などの言語で機能を実装し、簡単にサーバー・サイド機能を利用できます。

### データ処理
{: #data-processing-serverless}

![サーバーレス・データ処理](./images/cloud-functions-cloudant-trigger.png "サーバーレス・データ処理")

組み込みのトリガーにより、データ・ストア内のデータが更新されるたびにコードを実行できます。 また、機能的なサーバー・サイド・プログラミング・モデルにより、音声正規化、画像回転、鮮明化、ノイズ低減、サムネール生成、ビデオ・トランスコーディングなどの処理を簡単に自動化できます。

### コグニティブ・データ処理
{: #cognitive-serverless}

データが使用可能になるとすぐに分析できます。 ご使用の機能で IBM Watson のような強力なコグニティブ・サービスを活用して、画像やビデオ内のオブジェクトや人物を検出できます。

### スケジュールされたタスク
{: #tasks-serverless}

機能を定期的に実行し、cron のような構文に従ってスケジュールを定義して、アクションをいつ実行するか指定します。

## API リファレンス
{: #apiref-serverless notoc}

<!-- * [REST API Documentation](./openwhisk_reference.html#openwhisk_ref_restapi)-->
* [REST API](https://{DomainName}/apidocs){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")

## 関連リンク
{: #related-serverless notoc}

* [ディスカバー {{site.data.keyword.openwhisk_short}}](https://www.ibm.com/cloud/functions){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")
<!-- redirects to link above * [{{site.data.keyword.openwhisk_short}} on IBM developerWorks](https://developer.ibm.com/openwhisk/)-->
* [Apache OpenWhisk プロジェクトの Web サイト](http://openwhisk.incubator.apache.org/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")
* [サーバーレスに関する詳細](https://martinfowler.com/articles/serverless.html){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")
