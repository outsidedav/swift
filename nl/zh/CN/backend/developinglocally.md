---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 本地开发
{: #develop-locally}

通过在本地开发，可以轻松构建、运行和测试 Swift 应用程序。您可使用 {{site.data.keyword.dev_cli_short}} 通过标准命令来执行这些操作。 

可以使用 {{site.data.keyword.dev_cli_short}} 通过十几个命令来管理服务器端应用程序。在 [IBM Cloud Developer Tools CLI `ibmcloud dev` 命令](/docs/cli/idt/commands.html)中可了解有关各种命令的更多信息。

## 开始之前
{: #prereqs-local}

要在本地进行开发，必须安装 {{site.data.keyword.dev_cli_notm}}。执行以下命令来运行安装脚本:
```
curl -sL https://ibm.biz/idt-installer | bash
```
{: codeblock}

要了解有关配置和使用 {{site.data.keyword.dev_cli_notm}} 的更多信息，请参阅[设置 IBM Cloud Developer Tools CLI](/docs/cli/idt/setting_up_idt.html)。

## 检索服务凭证
{: #credentials-local}

通过 Git 克隆应用程序后，必须检索绑定到应用程序的服务的凭证，因为这些凭证并未存储在应用程序的 Git 存储库中。通过检索凭证，允许使用绑定服务。您可以在应用程序目录的根目录中运行以下命令来轻松下载凭证：
```
ibmcloud dev get-credentials
```
{: codeblock}

## 构建、运行和部署应用程序
{: #build-deploy-local}

1. **构建** - 现在可以构建应用程序，这是运行应用程序的先决条件。
  在应用程序目录的根目录中使用以下命令来构建应用程序：
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **运行** - 成功构建后，可以使用以下命令在本地容器中运行应用程序：
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  如果命令成功运行，将显示用于查看应用程序登录页面的本地主机和端口。

3. **部署** - 使用 `deploy` 命令将应用程序部署到 {{site.data.keyword.Bluemix_notm}}：
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}
