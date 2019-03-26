---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 生成済み SDK を使用してバックエンド・サービスをアプリに統合する
{: #sdkgen-cli}

{{site.data.keyword.IBM}} SDK ジェネレーター・プラグインを [{{site.data.keyword.cloud_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli/index.html){: new_window}にインストールできます。

この {{site.data.keyword.IBM_notm}} SDK ジェネレーター・プラグインは、生成された SDK を使用して、バックエンド・サービスをアプリに統合します。 REST API が変更された場合は、SDK を再生成し、古い SDK を置き換えることで、SDK を容易にアップグレードできます。 また、CLI を DevOps パイプラインに統合することによって、アプリをビルドするたびに、SDK が API スペックに常に整合していることを確認することができます。

REST API 定義は有効でなければならず、かつ稼働中のサーバー・エンドポイントまたはシステム上のローカル・ファイルでホストされる必要があります。

## 始める前に
{: #prereqs-sdkgen}

以下のものが存在することを確認します。

* [{{site.data.keyword.Bluemix_notm}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://cloud.ibm.com){: new_window} アカウント
* [Open API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.openapis.org/){: new_window} 仕様に準拠した有効な API 定義

## SDK プラグインのインストール
{: #install-sdkgen}

1. [{{site.data.keyword.Bluemix}} CLI](/docs/cli/reference/bluemix_cli/get_started.html) をインストールします。

2. [プラグイン ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli/index.html#install_plug-in){: new_window} をインストールします。

  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## SDK の生成
{: #commands-sdkgen}

次のように入力して SDK を生成します: `ibmcloud sdk generate [引数 ...] [コマンド・オプション]`

### 引数
{: #gen-args-sdkgen}

* `APP_NAME` - 現在のスペースにある Cloud Foundry アプリの名前
* `OPENAPI_DOC_LOCATION` - ロー REST API 定義の JSON または YAML への URL または相対ファイル・パス
* `GENERATED_SDK_NAME` (オプション) - 生成された SDK の名前

### オプション
{: #gen-options-sdkgen}

* `PLATFORM` (必須)
   * `--ios` - iOS Swift SDK を生成します
   * `--swift` - Swift Server SDK を生成します
   * `--js` - JavaScript SDK を生成します
* `LOCATION` (必須) - `OPENAPI_DOC_LOCATION` のタイプを指定します
   * `-r` - リモート URL
   * `-f` - ファイル
   * `-a` - {{site.data.keyword.Bluemix_notm}} 上で稼働するアプリ
   * `-l` - ローカル・ホスト URL
* `--output "YOUR_RELATIVE_PATH"` (オプション) - 生成された SDK を、`YOUR_RELATIVE_PATH` で指定されたディレクトリーに置きます (既存の SDK は上書きされます)。
* `--unzip` (オプション) - 生成された SDK を解凍します (既存の SDK は上書きされます)。

### 使用法
{: #gen-usage-sdkgen}

{{site.data.keyword.Bluemix_notm}} で稼働中の Cloud Foundry アプリから SDK を生成するには、アプリの名前を CLI のパラメーターに使用できます。 以下のコマンドでは、アプリの名前を `SDK_Name` に使用します。

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
{: #validating-sdkgen-sdkgen}

次のコマンドを実行します。
```
ibmcloud sdk validate [引数]
```
{: codeblock}

### 引数
{: #val-args-sdkgen}

* `APP_NAME` - 現在のスペースにある Cloud Foundry アプリの名前
* `OPENAPI_DOC_LOCATION` - ロー REST API 定義の JSON または YAML への URL または相対ファイル・パス

### 使用法
{: #val-usage-sdkgen}

{{site.data.keyword.Bluemix_notm}} で稼働中の Cloud Foundry アプリの API スペックを検証するには、アプリの名前を CLI のパラメーターとして使用します。
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

API 仕様文書の URL、あるいはローカルの JSON ファイルまたは YAML ファイルから SDK を検証するには、次のコマンドを使用します。
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}
