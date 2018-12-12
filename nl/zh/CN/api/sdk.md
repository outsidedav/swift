---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 使用生成的 SDK 将后端服务添加到应用程序
{: #sdk-cli}

可以在 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/bluemix_cli/get_started.html) 中安装 {{site.data.keyword.IBM}} SDK Generator 插件。

通过 {{site.data.keyword.IBM_notm}} SDK Generator 插件，您可以使用生成的 SDK 将后端服务集成到应用程序中。对 REST API 进行更改后，您可以重新生成 SDK，并替换旧 SDK 以轻松升级 SDK。然后，可以将 CLI 添加到 DevOps 管道中，并确保每次构建应用程序时 SDK 都与 API 规范一致。

REST API 定义必须有效，并且在实时服务器端点上或在系统上的本地文件中托管。

## 开始之前
{: #prereqs}

确保满足以下先决条件：

* [{{site.data.keyword.cloud_notm}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://bluemix.net){: new_window} 帐户。
* 符合 [Open API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.openapis.org/){: new_window} 规范的有效 API 定义。

## 安装 SDK 插件
{: #installation}

1. [安装 {{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/bluemix_cli/get_started.html)。

2. [安装 SDK 插件](/docs/cli/sdk/index.html)。
  ```
ibmcloud plugin install sdk-gen
	```
  {: codeblock}

## 生成 SDK
{: #commands}

通过输入以下命令生成 SDK：
```
ibmcloud sdk generate [arguments...] [command options]

```
{: codeblock}

### 自变量
{: #gen-args}

* **APP_NAME** - 当前空间中 Cloud Foundry 应用程序的名称。
* **OPENAPI_DOC_LOCATION** - 原始 REST API 定义 JSON 或 YAML 的 URL 或相对文件路径。
* **GENERATED_SDK_NAME**（可选）- 生成的 SDK 的名称。

### 选项
{: #gen-options}

**平台**（必需）：
  * `--ios` - 生成 iOS Swift SDK。
  * `--swift` - 生成 Swift 服务器 SDK。
  * `--js` - 生成 JavaScript SDK。

**位置**（必需）：

指定 `OPENAPI_DOC_LOCATION` 自变量的类型。

  * `-r` - 远程 URL。
  * `-f` - 文件名。
  * `-a` - 在 {{site.data.keyword.coud_notm}} 上运行的应用程序。
  * `-l` - 本地主机 URL。

**可选**：
  * `--output "YOUR_RELATIVE_PATH"` - 将生成的 SDK 放置在 `YOUR_RELATIONVE_PATH` 指定的目录中（如果存在现有 SDK，将覆盖现有 SDK。）
  * `--unzip` - 解压缩生成的 SDK（如果存在现有 SDK 工件，将覆盖现有数据）。

### 用法
{: #gen-usage}

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
{: #validating}

运行以下命令：`ibmcloud sdk validate [argument]`

### 自变量
{: #val-args}

* `APP_NAME` - 当前空间中 Cloud Foundry 应用程序的名称。
* `OPENAPI_DOC_LOCATION` - 原始 REST API 定义 JSON 或 YAML 的 URL 或相对文件路径。

### 用法
{: #val-usage}

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

