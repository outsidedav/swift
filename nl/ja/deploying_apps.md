---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: deploy swift app, deploy swift, serverless swift, deploy swift cloud foundry, swift kubernetes, swift virtual server

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Swift アプリのデプロイと統合
{: #deploy_apps-swift}

Swift アプリは、ツールチェーンやコマンド・ライン・インターフェースを使用してデプロイできます。 ツールチェーンは、ツール統合の集合です。 コマンド・ライン・インターフェースは、アプリとサービス・インスタンスをデプロイするための簡単な方法です。

詳しくは、[アプリのデプロイ](/docs/apps?topic=creating-apps-create-deploy-app-cli#create-deploy-app-cli)を参照してください。

## サーバーレス Swift アプリの開発
{: #serverless-swift}

サーバーレス開発を行うため、IBM の Functions as a Service (FaaS) オファリングである {{site.data.keyword.openwhisk}} を使用できます。 {{site.data.keyword.openwhisk_short}} を使用すると、サーバーの作成や管理を行うことなく、HTTP 経由での Web アプリまたはモバイル・アプリからの直接呼び出しまたはイベントへの応答としてアプリケーション・ロジックを実行できます。

{{site.data.keyword.openwhisk_short}} が自動スケーリング、可用性管理、保守などのシステム管理を実行するため、開発者はアプリケーション・ロジックの作成に注力できます。

詳しくは、[サーバーレス・アプリの開発](/docs/apps/deploying?topic=creating-apps-serverless#serverless)を参照してください。

## 生成された SDK によるバックエンド・サービスの統合
{: #backend_gensdk-swift}

{{site.data.keyword.IBM_notm}} SDK Generator プラグインは、生成された SDK によって、容易にバックエンド・サービスをアプリに統合します。 REST API が変更された場合は、SDK を再生成し、古い SDK を置き換えることで、SDK をアップグレードできます。 また、CLI を DevOps パイプラインに統合することによって、アプリをビルドするたびに、SDK が API スペックに常に整合していることを確認することができます。

詳しくは、[生成された SDK によるバックエンド・サービスの統合](/docs/swift/backend?topic=swift-sdkgen-cli#sdkgen-cli)を参照してください。

## Kubernetes クラスターへのデプロイ
{: #deploy_kube-swift}

{{site.data.keyword.cloud_notm}} Kubernetes Service を使用して、Watson Tone Analyzer を活用したコンテナー化アプリをデプロイする方法を学ぶことができます。 用意されているシナリオでは、架空の PR 会社が {{site.data.keyword.cloud_notm}} サービスを使用して自社のプレス・リリースを分析し、自社のメッセージのトーンに関するフィードバックを受け取ります。 詳しくは、チュートリアル: [Kubernetes クラスターへのアプリのデプロイ](/docs/containers?topic=containers-cs_apps_tutorial)を参照してください。

## Cloud Foundry へのデプロイ
{: #swift-deploy-cf}

このオプションでは、基礎となるインフラストラクチャーの管理を必要とせずに、クラウド・ネイティブ・アプリをデプロイします。

アプリを [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about) にデプロイする計画の場合は、[{{site.data.keyword.cloud_notm}} アカウントを準備する](/docs/cloud-foundry?topic=cloud-foundry-prepare)必要があります。

ご使用のアカウントで {{site.data.keyword.cfee_full_notm}} にアクセスできる場合、デプロイヤー・タイプの **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** または **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)** のいずれかを選択できます。後者を使用すれば、お客様の会社専用に Cloud Foundry アプリケーションをホストするための分離された環境を作成して管理することができます。

## 仮想サーバーへのデプロイ
{: #virtual_deploy-swift}

仮想サービスでは、あらゆるワークロード・タイプのために、高いレベルの透過性、予測性、自動化を提供します。 Terraform を使用して、インフラストラクチャーの構築、変更、バージョン管理を安全かつ効率的に行うことができます。

  いくつかのスターター・キットで VSI デプロイメントを使用できます。この機能を使用するには、[{{site.data.keyword.cloud_notm}} ダッシュボード](https://{DomainName})に移動し、**「アプリ」**タイルで**「アプリの作成」**をクリックします。
  {: note}

詳しくは、[仮想サーバーへのデプロイ](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server)を参照してください。
