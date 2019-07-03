---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-07"

keywords: develop swift, swift local, service credentials swift, developer tools swift, swift cli, ibmcloud build swift, ibmcloud swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# ローカルでの開発
{: #develop-locally}

ローカルに開発すると、Swift アプリのビルド、実行、テストが簡単になります。 標準的なコマンドを使用してこれらのアクションを実行するには、{{site.data.keyword.dev_cli_short}} を使用します。 

{{site.data.keyword.dev_cli_short}} を使用すると、10 以上のコマンドを使用してサーバー・サイドのアプリケーションを管理できます。 各種コマンドについて詳しくは、[{{site.data.keyword.dev_cli_notm}}CLI プラグイン (`ibmcloud dev`) コマンド](/docs/cli/idt?topic=cloud-cli-idt-cli)を参照してください。

## 始める前に
{: #prereqs-local}

ローカルで開発するには、{{site.data.keyword.dev_cli_notm}} をインストールする必要があります。 以下のコマンドを実行して、インストール・スクリプトを実行します。
```
curl -sL https://ibm.biz/idt-installer | bash
```
{: codeblock}

{{site.data.keyword.dev_cli_notm}} の構成と使用法の詳細については、[{{site.data.keyword.dev_cli_notm}} CLI のセットアップ](/docs/cli?topic=cloud-cli-getting-started)を参照してください。

## サービス資格情報の取得
{: #credentials-local}

Git からアプリケーションを複製した後に、そのアプリケーションにバインドされているサービスの資格情報を取得する必要があります。そのアプリケーションに関する Git リポジトリーにはこの資格情報は保管されないからです。 資格情報を取得すると、バインドされたサービスの使用が許可されます。 アプリケーション・ディレクトリーのルートで以下のコマンドを実行すると、資格情報を簡単にダウンロードできます。
```
ibmcloud dev get-credentials
```
{: codeblock}

## アプリケーションのビルド、実行、デプロイ
{: #build-deploy-local}

1. **ビルド** - アプリケーションをビルドできるようになりました。ビルドはアプリケーションを実行するための前提条件です。
  アプリをビルドするには、アプリケーション・ディレクトリーのルートで次のコマンドを使用します。
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **実行** - 正常にビルドした後に、以下のコマンドを使用してローカル・コンテナー内でアプリケーションを実行できます。
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  コマンドが正常に実行されると、アプリケーションのランディング・ページを表示するためのローカル・ホストとポートが表示されます。

3. **デプロイ** -  `deploy` コマンドを使用して、アプリケーションを {{site.data.keyword.cloud_notm}} にデプロイします。
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}
