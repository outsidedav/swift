---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: swift security, add security swift, crypto swift, hyper protect swift, ios hyper protect, dbaas swift, swift key management, swift advanced security

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip} 

# 高度なセキュリティーを使用した構築
{: #security}

Swift 開発者は、{{site.data.keyword.cloud}} が提供する高度なセキュリティー・サービスを簡単に使用し、保存中、使用中、または転送中の鍵とデータを、業界で最高のセキュリティー・レベルで保護できます。
{: shortdesc}

簡単なアプローチとして、 {{site.data.keyword.cloud_notm}} HyperSecure プラットフォーム・サービスを {{site.data.keyword.cloud}} のすべての高度なセキュリティー・サービスで直接利用できます。 詳しくは、『[{{site.data.keyword.cloud_notm}}HyperSecure Platform Services の開始](/docs/services/hypersecure-platform/index.html#getting-started-with-ibm-cloud-hyper-protect-developer-starter-kits)』を参照してください。

## {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} の使用
{: #security-swift}

{{site.data.keyword.hscrypto}} は鍵とデータに対して業界で最高のセキュリティー・レベルでの暗号化を提供する試験サービスです。 IBM Z のセキュリティーと整合性をクラウドにもたらします。 銀行や金融サービスが利用する暗号化テクノロジーが、{{site.data.keyword.cloud_notm}} によってクラウド・ユーザーに提供されるようになりました。

{{site.data.keyword.hscrypto}} は、ネットワーク・アドレス可能ハードウェア・セキュリティー・モジュール (HSM) を提供して、PKCS#11 アプリケーション・プログラミング・インターフェース (API) を介し、安全でセキュリティーで保護された暗号化を提供します。 暗号化ハードウェアのセキュリティーで到達可能な最上位レベル、つまり FIPS 140-2 レベル 4 にアクセスできます。また、{{site.data.keyword.hscrypto}} は、IBM の暗号化コプロセッサーへのリモート・アクセスを可能にする IBM Advanced Crypto Service Provider (ACSP) ソリューションも使用できます。

{{site.data.keyword.hscrypto}} は {{site.data.keyword.keymanagementserviceshort}} サービスの鍵ストアでもあります。

{{site.data.keyword.hscrypto}} について詳しくは、『[{{site.data.keyword.cloud_notm}}{{site.data.keyword.hscrypto}} の開始](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started)』を参照してください。

## {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}} の使用
{: #swift-key-management}

{{site.data.keyword.keymanagementserviceshort}} は、{{site.data.keyword.cloud_notm}} サービス上のアプリの暗号鍵を作成するときに役立ちます。 鍵のライフサイクルを管理する際に、情報の盗難を防止するクラウド・ベースのハードウェア・セキュリティー・モジュール (HSM) によって鍵が保護されていることを理解していると役立ちます。 {{site.data.keyword.hscrypto}} と共に、鍵は FIPS 140-2 レベル 4 証明書の最高のセキュリティー・レベルで保護されます。

{{site.data.keyword.keymanagementserviceshort}} について詳しくは、『[{{site.data.keyword.keymanagementserviceshort}}の開始](/docs/services/key-protect?topic=key-protect-getting-started-tutorial#getting-started-tutorial)』を参照してください。

## {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS の使用
{: #hypersecure-dbaas}

{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS は試験的な {{site.data.keyword.cloud_notm}} サービスで、セキュアなデータベースをオンデマンドで作成します。 MongoDB データベースを素早く簡単にプロビジョンおよび管理するための、拡張可能なプラットフォームを提供します。

このサービスを使用すると、{{site.data.keyword.cloud_notm}} でデータベース・クラスターを作成できます。 作成するデータベース・クラスターごとに、1 つの 1 次データベース・インスタンスと、1 次のレプリカとして機能する 2 つの 2 次データベース・インスタンスがあります。 {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS のロジックは、データベース・クラスターで MongoDB データベースを作成してアクセスします。

### データとプライバシーの保護
{: #swift-securing-data}

{{site.data.keyword.IBM_notm}} は可用性が高くセキュアな環境でデータベースをホストします。
 * データは保存中も処理中も暗号化されます。
 * システム・ハードウェア、システム構成、およびデータベースのセットアップにより、高可用性が実現されます。
 * 基礎となるテクノロジーにより、{{site.data.keyword.IBM_notm}} や第三者がデータにアクセスできないようにします。

### アプリケーションへの {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS ロジックの追加
{: #swift-hyperprotect}

アプリケーションで MongoDB データベースを使用するには、『[データベースの使用を開始する](/docs/swift/hypersecure_dbaas?topic=swift-create-database-cluster#creating-a-highly-available-and-secure-database)』を参照してください。  

### {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS の詳細
{: #swift-learnmore}

{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS について詳しくは、『[IBM Cloud Hyper Protect DBaaS の開始](/docs/services/hyper-protect-dbaas?topic=hyper-protect-dbaas-gettingstarted#gettingstarted)』を参照してください。

## {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscontainers}} の使用
{: #swift-hscontainers}

{{site.data.keyword.hscontainers}} は、Docker と Kubernetes テクノロジーを結合させた強力なツール、直観的なユーザー・エクスペリエンス、標準装備のセキュリティーと分離機能を提供します。これらの機能を使用することで、コンピュート・ホストから成るクラスター内でコンテナー化アプリのデプロイメント、操作、スケーリング、モニタリングを自動化することができます。

{{site.data.keyword.hscontainers}} は、スポンサー・ユーザーのみが使用できます。 専用セキュリティー・サポートが必要な場合は、[IBM Z クライアント・フィードバック・プログラム](https://www-01.ibm.com/marketing/iwm/iwmdocs/web/cc/earlyprograms/zcustomer.shtml){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を使用してスポンサー・ユーザーとして登録し、アプリケーションを {{site.data.keyword.hscontainers}} クラスターにデプロイします。
{: tip}

スポンサー・ユーザーになる前に、{{site.data.keyword.containershort_notm}} を使用してアプリケーションを保護することができます。 詳しくは、『[{{site.data.keyword.containershort_notm}} の概要](/docs/containers?topic=containers-container_index#container_index)』を参照してください。
