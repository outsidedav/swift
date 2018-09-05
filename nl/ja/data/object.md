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

# 静的コンテンツ用に Object Storage を使用する
{: #object}

Object Storage は、クラウド・コンピューティングにおける基本的なコンポーネントであり、Apple の開発者とアプリケーションに強力な機能を提供します。ファイル階層に情報を保管する場合 (例: ブロック・ストレージやファイル・ストレージ) とは異なり、オブジェクト・ストアは、バケットというコレクションに保管される、ファイルとそれらのメタデータのみで構成されます。定義上、それらのオブジェクトは変更不可能なため、イメージ、ビデオ、他の静的文書などのデータにとって最適です。頻繁に変更されるデータやリレーショナル・データの場合は、[NoSQL](/docs/swift/data/nosql.html)、[Cloudant](/docs/swift/data/cloudant.html)、[SQL](/docs/swift/data/sql.html) データベース・サービスを使用することができます。

{{site.data.keyword.cos_full_notm}} (COS) は、非構造化データの保管のために利用できる、柔軟でコスト効率が高く、スケーラブルなストレージ・システムです。データには、SDK または IBM ユーザー・インターフェースを使用してアクセスできます。{{site.data.keyword.cos_full_notm}} を使用すれば、Restful API と SDK によってサポートされるセルフサービス・ポータルを介して、非構造化データにアクセスできます。 

ニーズに応じて、以下のサービスのために {{site.data.keyword.cos_short}} を使用できます。

* コンテンツ・リポジトリー
* バックアップとリカバリー
* データ・アーカイブ
* ファイル・サービス
* データ転送

バケットを作成する際には、回復力レベル (cross-region または regional) を選択する必要があります。その選択に基づいて、データが分散され、複数の地理的ロケーションに保管されます。

## API

{{site.data.keyword.cos_full}} API は、オブジェクトを読み書きするための REST ベースの API です。この API は、アプリケーションを {{site.data.keyword.cloud_notm}} に簡単にマイグレーションできるように、S3 API のサブセットをサポートしています。{{site.data.keyword.cos_full}} を活用するために、任意の S3 SDK を使用できます。詳しくは、[{{site.data.keyword.cos_short}} API リファレンス](docs/services/cloud-object-storage/api-reference/about-compatibility-api.html#about-the-ibm-cloud-object-storage-api)を参照してください。

## セキュリティー
{: #security}

{{site.data.keyword.cos_full_notm}} は、高いセキュリティー性を備えています。最初に {{site.data.keyword.cos_full_notm}} サービスにアクセスできるのは、そのサービスを作成したバケット所有者およびオブジェクト所有者のみです。さらに、データにアクセスするユーザーの認証もサポートしています。バケット・ポリシーなどのアクセス制御メカニズムを利用すると、ユーザーとアプリケーションにアクセス権限を選択的に付与できます。{{site.data.keyword.cos_short}} へのデータ転送は、HTTPS プロトコルを使用することで SSL エンドポイントを介して安全に行うことができます。追加のセキュリティーが必要な場合は、サーバー・サイドの暗号化 (SSE) オプションまたはお客様が用意した鍵によるサーバー・サイドの暗号化 (SSE-C) オプションを使用することで、保存されているデータを暗号化できます。{{site.data.keyword.cos_full_notm}} は、SSE と SSE-C の両方の暗号化テクノロジーを提供しています。代わりの方法として、独自の暗号化ライブラリーを使用してデータを暗号化してから、Cloud Object Storage に保管することもできます。

以下のアクセス制御メカニズムを利用して、{{site.data.keyword.cos_full_notm}} でデータを保護することができます。

**Identity and Access Management (IAM) ポリシー**
IAM を利用すると、多数の従業員を擁する企業は、複数のユーザーを単一アカウントの下に作成して管理できます。企業は IAM ポリシーを使用することで、IAM ユーザーに、{{site.data.keyword.cos_short}} バケットを制御する権限を付与することができます。

**アクセス制御リスト (ACL)**
ACL を利用すると、個々のバケットに対する特定のアクセス権限 (READ や WRITE など) を、特定のユーザーに付与することができます。

Data at Rest (保存されたデータ) は、自動のプロバイダー・サイドの Advanced Encryption Standard (AES) 256 ビット暗号化と Secure Hash Algorithm (SHA)-256 ハッシュによって暗号化されます。Data in Motion (流れているデータ) は、組み込みのキャリア・グレードの Transport Layer Security (TLS)、Secure Sockets Layer (SSL)、または SNMPv3 with AES 暗号化を使用して保護されます。

### 暗号化

Data at Rest (保存されたデータ) は、自動のプロバイダー・サイドの Advanced Encryption Standard (AES) 256 ビット暗号化と Secure Hash Algorithm (SHA)-256 ハッシュによって暗号化されます。Data in Motion (流れているデータ) は、組み込みのキャリア・グレードの Transport Layer Security/Secure Sockets Layer (TLS/SSL) または SNMPv3 with AES 暗号化を使用して保護されます。

サーバー・サイドの暗号化は、お客様のデータのために常に ON です。イレージャー・コーディングや認証で必要なハッシュと比べて、暗号化は、Cloud Object Storage の処理コストの大きな部分を占めていません。

SSE-C が {{site.data.keyword.cos_full_notm}} でサポートされているため、暗号化のために独自の鍵を提供することができます。ただし、データを保管するための鍵の管理と提供、またデータの取得は、お客様の責任で行うことになります。

## 回復力

バケットを作成する際には、回復力レベル (cross-region または regional) を選択する必要があります。その選択に基づいて、データが分散され、複数の地理的ロケーションに保管されます。

Regional 回復力は、低遅延を求める場合に向いており、データは単一地域内の 3 つのセンターに分散されます。Cross region 回復力は、可用性がミッション・クリティカルな場合に向いており、データは 3 つ以上の地域に保管されます。Cross region は、地理的回復力を実現し、複数のエンドポイントにわたって使用できます。アプリケーションのニーズを検討して、これら 2 つの選択肢のいずれかを選んでください。

### 地域

{{site.data.keyword.cos_full_notm}} は、世界中のどこでも使用できます。バケットの作成時に、データを保管する場所を選択できます。IBM COS に保管される情報は、暗号化されます。そして、IBM Object Storage System で提供される分散ストレージ・テクノロジーを使用して、複数の地理的ロケーションに分散されます。 

regional または cross-region のどちらのオプションにするかを決定することに加えて、以下の要素を考慮してオブジェクト・ストアの地理的ロケーションを選択してください。

**ロケーションに関する考慮事項**
* 冗長性のために、運用地域から遠く離れたロケーション。
* 法的および規制上の要件を満たすロケーション。
* データ・アクセスの待ち時間の短縮。
* 価格設定モデルの考慮。

## ユース・ケースとストレージ・クラス

ユース・ケースに応じて、ニーズに合ったサービス・プランを選択することでコストを削減できます。オブジェクト・ストアへのアクセスが極めて少ないアーカイブ操作の場合は、頻繁にオブジェクトにアクセスする場合のような速度や耐久性は必要ありません。お客様のアプリケーションに対応するため、そうした相違点がストレージ・クラスのサポートおよび価格設定プランに反映されています。ストレージ・クラスはバケット・レベルで定義されるため、ニーズに合わせて異なるプランを組み合わせることもできます。新規バケットを、必要とするストレージ・クラスに設定して作成します。

価格設定について詳しくは、[{{site.data.keyword.cos_short}} Storage Class](/docs/services/cloud-object-storage/help/billing.html#ibm-cos-pricing) 資料を参照してください。

### サンプル・ストレージ・クラス

**Standard**
DevOps、コラボレーション、アクション・コンテンツのリポジトリーなど、頻繁にアクセスする必要のある非構造化データ向けのサービスです。

**Vault**
バックアップ、アーカイブ、コンプライアンス・ワークロードなど、データに頻繁にアクセスしないワークロード向けのサービスです。

**Cold Vault**
最小限のアクセス要件、履歴レコード・コンプライアンス、長期バックアップに最適なデプロイメント・オプションです。

**Flex** データ・アクセス要件が変化する場合に導入し、予期しないコストの変動から予算を守ります。
ストレージ・クラスはバケット・レベルで定義されます。新規バケットを、必要とするストレージ・クラスに設定して作成します。
