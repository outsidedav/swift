---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-28"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 生成された SDK を使用したアプリへのバックエンド・サービスの追加
{: #sdk-cli}

{{site.data.keyword.IBM}} SDK Generator プラグインは、[{{site.data.keyword.cloud_notm}} CLI](/docs/cli/index.html) にインストールできます。

{{site.data.keyword.IBM_notm}} SDK Generator プラグインを使用すると、生成された SDK によって、バックエンド・サービスをアプリに統合できます。 REST API が変更された場合は、SDK を再生成し、古い SDK を置き換えることで、SDK を容易にアップグレードできます。 そして、CLI を DevOps パイプライン内に追加することによって、アプリをビルドするたびに、SDK と API 仕様との整合性を常に維持することができます。

REST API 定義は有効でなければならず、かつ稼働中のサーバー・エンドポイントまたはシステム上のローカル・ファイルでホストされる必要があります。

## 始める前に
{: #prereqs-sdk-cli}

以下の前提条件を満たしていることを確認してください。

* [{{site.data.keyword.cloud_notm}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://cloud.ibm.com){: new_window} アカウント。
* [Open API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.openapis.org/){: new_window} 仕様に準拠した有効な API 定義。

## SDK プラグインのインストール
{: #install-sdk-cli}

1. [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/index.html) をインストールします。

2. [SDK プラグインをインストール](/docs/cli/sdk/index.html)します。
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## SDK の生成
{: #commands-sdk-cli}

次のように入力して、SDK を生成します。
```
ibmcloud sdk generate [arguments...] [command options]
```
{: codeblock}

### 引数
{: #gen-args-sdk-cli}

* **APP_NAME** - 現在のスペースにある Cloud Foundry アプリの名前。
* **OPENAPI_DOC_LOCATION** - ロー REST API 定義の JSON または YAML への URL または相対ファイル・パス。
* **GENERATED_SDK_NAME** (オプション) - 生成された SDK の名前。

### オプション
{: #gen-options-sdk-cli}

**Platform** (必須):
  * `--ios` - iOS Swift SDK を生成します。
  * `--swift` - Swift Server SDK を生成します。
  * `--js` - JavaScript SDK を生成します。

**Location** (必須):

`OPENAPI_DOC_LOCATION` 引数のタイプを指定します。

  * `-r` - リモート URL。
  * `-f` - ファイル名。
  * `-a` - {{site.data.keyword.cloud_notm}} 上で稼働するアプリ。
  * `-l` - ローカル・ホスト URL。

**オプション**:
  * `--output "YOUR_RELATIVE_PATH"` - 生成された SDK を `YOUR_RELATIVE_PATH` で指定されたディレクトリーに置きます (既存の SDK がある場合は上書きします)。
  * `--unzip` - 生成された SDK を解凍します (SDK 成果物がある場合には既存のデータを上書きします)。

### 使用法
{: #gen-usage-sdk-cli}

{{site.data.keyword.cloud_notm}} で稼働中の Cloud Foundry アプリから SDK を生成するには、アプリの名前を CLI のパラメーターに使用します。 以下のコマンドでは、アプリの名前を `SDK_Name` に使用します。

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

Open API 定義ファイルへの URL、またはローカルの JSON ファイルまたは YAML ファイルから SDK を生成するには、以下のコマンドを使用します。

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}


## Open API 定義の検証
{: #validating-sdk-cli}

次のコマンドを実行します。`ibmcloud sdk validate [argument]`

### 引数
{: #val-args-sdk-cli}

* `APP_NAME` - 現在のスペースにある Cloud Foundry アプリの名前。
* `OPENAPI_DOC_LOCATION` - ロー REST API 定義の JSON または YAML への URL または相対ファイル・パス。

### 使用法
{: #val-usage-sdk-cli}

{{site.data.keyword.cloud_notm}} で稼働中の Cloud Foundry アプリの API スペックを検証するには、アプリの名前を CLI のパラメーターに使用します。
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

API 仕様文書の URL、あるいはローカルの JSON ファイルまたは YAML ファイルから SDK を検証するには、次のコマンドを使用します。
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

