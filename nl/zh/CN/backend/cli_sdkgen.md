---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-28"

keywords: generated sdk swift, devops pipeline swift, sdk plug-in swift, open api swift, sdkgen swift, ibmcloud sdk swift, swift backend service

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用生成的 SDK 将后端服务集成到应用程序
{: #sdkgen-cli}

可以在 [{{site.data.keyword.cloud}} CLI ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli){: new_window} 中安装 {{site.data.keyword.IBM}} SDK Generator 插件。

此 {{site.data.keyword.IBM_notm}} SDK Generator 插件使用生成的 SDK 将后端服务集成到应用程序。对 REST API 进行更改后，您可以重新生成 SDK，并替换旧 SDK 以轻松升级 SDK。还可以将 CLI 集成到 DevOps 管道中，并确保每次构建应用程序时 SDK 都与 API 规范一致。

REST API 定义必须有效，并且在实时服务器端点上或在系统上的本地文件中托管。

## 开始之前
{: #prereqs-sdkgen}

确保您具有：

* [{{site.data.keyword.cloud_notm}}](http://cloud.ibm.com){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 帐户
* 符合 [Open API ](https://www.openapis.org/){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 规范的有效 API 定义

## 安装 SDK 插件
{: #install-sdkgen}

1. [安装 {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-install-ibmcloud-cli#install-ibmcloud-cli)。

2. 安装 `sdk-gen` 插件：
  ```
ibmcloud plugin install sdk-gen
	```
  {: codeblock}

您可以安装 [{{site.data.keyword.dev_cli_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#install_plug-in)，其中包括基本的 {{site.data.keyword.cloud_notm}} CLI，也包括 `sdk-gen` 插件以及其他有用的本地工具。
{: tip}

## 生成 SDK
{: #commands-sdkgen}

通过输入以下命令生成 SDK：`ibmcloud sdk generate [arguments...] [command options]`

### 自变量
{: #gen-args-sdkgen}

* `APP_NAME` - 当前空间中 Cloud Foundry 应用程序的名称
* `OPENAPI_DOC_LOCATION` - 原始 REST API 定义 JSON 或 YAML 的 URL 或相对文件路径
* `GENERATED_SDK_NAME`（可选）- 生成的 SDK 的名称

### 选项
{: #gen-options-sdkgen}

* `PLATFORM`（必需）
   * `--ios` - 生成 iOS Swift SDK
   * `--swift` - 生成 Swift 服务器 SDK
   * `--js` - 生成 JavaScript SDK
* `LOCATION`（必需）- 为 `OPENAPI_DOC_LOCATION` 指定类型
   * `-r` - 远程 URL
   * `-f` - 文件
   * `-a` - 在 {{site.data.keyword.cloud_notm}} 上运行的应用程序
   * `-l` - 本地主机 URL
* `--output "YOUR_RELATIVE_PATH"`（可选）- 将生成的 SDK 放在 `YOUR_RELATIVE_PATH` 指定的目录中（现有 SDK 将会被覆盖）。
* `--unzip`（可选）- 解压缩生成的 SDK（现有 SDK 将会被覆盖）。

### 用法
{: #gen-usage-sdkgen}

要从在 {{site.data.keyword.cloud_notm}} 中运行的 Cloud Foundry 应用程序生成 SDK，可以将应用程序的名称用作 CLI 的参数。以下命令使用应用程序名称作为 `SDK_Name`。

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

要通过指向 Open API 定义文件或者本地 JSON 或 YAML 文件的 URL 生成 SDK，请使用以下命令。

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}

## 验证 Open API 定义
{: #validating-sdkgen-sdkgen}

运行以下命令：
```
ibmcloud sdk validate [argument]
```
{: codeblock}

### 自变量
{: #val-args-sdkgen}

* `APP_NAME` - 当前空间中 Cloud Foundry 应用程序的名称
* `OPENAPI_DOC_LOCATION` - 原始 REST API 定义 JSON 或 YAML 的 URL 或相对文件路径

### 用法
{: #val-usage-sdkgen}

要验证在 {{site.data.keyword.cloud_notm}} 中运行的 Cloud Foundry 应用程序的 API 规范，可以将应用程序的名称用作 CLI 的参数。
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

要通过指向 API 规范文档或者本地 JSON 或 YAML 文件的 URL 验证 SDK，请使用以下命令：
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}
