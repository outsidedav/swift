---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 部署及整合 Swift 應用程式
{: #deploy_apps-swift}

您可以使用工具鏈或使用指令行介面，來部署您的 Swift 應用程式。工具鏈是一組工具整合。指令行介面是部署應用程式和服務實例的簡單方法。


如需相關資訊，請參閱[部署應用程式](/docs/apps/dep-app-tool.html#deploying-apps)。

## 開發無伺服器 Swift 應用程式
{: #serverless-swift}

若要以無伺服器開發的方式進行開發，您可以使用 IBM 的「函數即服務 (FaaS)」供應項目 {{site.data.keyword.openwhisk}}。{{site.data.keyword.openwhisk_short}} 可執行應用程式邏輯，以回應事件，或回應來自 Web 或行動應用程式透過 HTTP 的直接呼叫，而不需建立或管理伺服器。

{{site.data.keyword.openwhisk_short}} 會執行系統管理，例如，自動調整、可用性管理及維護，讓開發人員專注在撰寫應用程式邏輯。

如需相關資訊，請參閱[開發無伺服器應用程式](/docs/apps/deploying/functions.html#serverless)。

## 使用產生的 SDK 來整合後端服務
{: #backend_gensdk-swift}

「{{site.data.keyword.IBM_notm}} SDK 產生器」外掛程式會使用產生的 SDK，輕鬆地將後端服務整合至您的應用程式。當 REST API 發生變更時，您可以重新產生 SDK 並取代舊的 SDK，以升級 SDK。您也可以將 CLI 整合至 DevOps 管線，並確保每次建置應用程式時，SDK 永遠與 API 規格一致。

如需相關資訊，請參閱[使用產生的 SDK 來整合後端服務](/docs/swift/backend/cli_sdkgen.html#sdk-cli)。

## 部署至 Kubernetes 叢集
{: #deploy_kube-swift}

您可以學習如何使用 {{site.data.keyword.cloud_notm}} Kubernetes Service 來部署運用 Watson Tone Analyzer 的容器化 Node.js 應用程式。在提供的情境中，虛構的公關公司使用 {{site.data.keyword.cloud_notm}} 服務來分析其新聞稿，並收到對其訊息基調的意見。如需相關資訊，請參閱[將應用程式部署至 Kubernetes 叢集](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial)指導教學。

## 部署至 Cloud Foundry
{: #swift-deploy-cf}

此選項可部署雲端原生應用程式，您不需要管理基礎架構。

如果您計劃將應用程式部署至 [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry/index.html#about)，則必須[準備 {{site.data.keyword.cloud_notm}} 帳戶](/docs/cloud-foundry/prepare-account.html#prepare)。

如果您的帳戶有權存取 {{site.data.keyword.cfee_full_notm}}，則可以選取**[公用雲端](/docs/cloud-foundry-public/about-cf.html#about-cf)**或**[企業環境](/docs/cloud-foundry-public/cfee.html#cfee)**的部署者類型，您能夠使用此類型來建立及管理隔離環境，專供您的企業用來管理 Cloud Foundry 應用程式。

## 部署至虛擬伺服器
{: #virtual_deploy-swift}

虛擬服務針對所有工作負載類型，提供較高程度的透通性、可預測性及自動化。Terraform 用來有效率且安全地建置、變更及版本化您的基礎架構。

如需相關資訊，請參閱[部署至虛擬伺服器](/docs/apps/vsi-deploy.html#vsi-deploy)。
