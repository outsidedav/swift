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

# 本端開發
{: #develop-locally}

藉由本端開發，您可以輕鬆地建置、執行及測試 Swift 應用程式。使用 {{site.data.keyword.dev_cli_short}}，可以利用標準指令來執行這些動作。 

您可以使用 {{site.data.keyword.dev_cli_short}}，利用許多指令來管理您的伺服器端應用程式。在 [{{site.data.keyword.dev_cli_notm}} CLI `ibmcloud dev` 指令](/docs/cli/idt?topic=cloud-cli-idt-cli)中，可進一步瞭解各種指令。

## 開始之前
{: #prereqs-local}

若要在本端開發，您必須安裝 {{site.data.keyword.dev_cli_notm}}。請執行下列指令，以執行安裝 Script：
```
curl -sL https://ibm.biz/idt-installer | bash
```
{: codeblock}

請參閱[設定 {{site.data.keyword.dev_cli_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started)，以進一步瞭解 {{site.data.keyword.dev_cli_notm}} 的配置與用法。

## 擷取服務認證
{: #credentials-local}

從 Git 複製您的應用程式之後，您必須擷取連結至您應用程式之服務的認證，因為這些認證並未儲存在您應用程式的 Git 儲存庫中。擷取認證，即可使用連結服務。在應用程式目錄的根目錄中執行下列指令，即可輕鬆地下載認證：
```
ibmcloud dev get-credentials
```
{: codeblock}

## 建置、執行及部署應用程式
{: #build-deploy-local}

1. **建置** - 您現在可以建置您的應用程式，這是執行應用程式的必要條件。在應用程式目錄的根目錄中使用下列指令，以建置您的應用程式：
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **執行** - 成功建置之後，您可以使用下列指令，在本端容器中執行您的應用程式：
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  如果指令執行順利，則會顯示本端主機及埠，以檢視您應用程式的登入頁面。

3. **部署** - 使用 `deploy` 指令，將您的應用程式部署至 {{site.data.keyword.cloud_notm}}：
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}
