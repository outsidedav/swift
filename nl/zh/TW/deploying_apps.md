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

# 部署及整合 Swift 應用程式
{: #deploy_apps-swift}

您可以使用工具鏈或使用指令行介面，來部署您的 Swift 應用程式。工具鏈是一組工具整合。指令行介面是部署應用程式和服務實例的簡單方法。


如需相關資訊，請參閱[部署應用程式](/docs/apps?topic=creating-apps-create-deploy-app-cli#create-deploy-app-cli)。

## 開發無伺服器 Swift 應用程式
{: #serverless-swift}

若要以無伺服器開發的方式進行開發，您可以使用 IBM 的「函數即服務 (FaaS)」供應項目 {{site.data.keyword.openwhisk}}。{{site.data.keyword.openwhisk_short}} 可執行應用程式邏輯，以回應事件，或回應來自 Web 或行動應用程式透過 HTTP 的直接呼叫，而不需建立或管理伺服器。

{{site.data.keyword.openwhisk_short}} 會執行系統管理，例如，自動調整、可用性管理及維護，讓開發人員專注在撰寫應用程式邏輯。

如需相關資訊，請參閱[開發無伺服器應用程式](/docs/apps/deploying?topic=creating-apps-serverless#serverless)。

## 使用產生的 SDK 來整合後端服務
{: #backend_gensdk-swift}

「{{site.data.keyword.IBM_notm}} SDK 產生器」外掛程式會使用產生的 SDK，輕鬆地將後端服務整合至您的應用程式。當 REST API 發生變更時，您可以重新產生 SDK 並取代舊的 SDK，以升級 SDK。您也可以將 CLI 整合至 DevOps 管線，並確保每次建置應用程式時，SDK 永遠與 API 規格一致。

如需相關資訊，請參閱[使用產生的 SDK 來整合後端服務](/docs/swift/backend?topic=swift-sdkgen-cli#sdkgen-cli)。

## 部署至 Kubernetes 叢集
{: #deploy_kube-swift}

您可以學習如何使用 {{site.data.keyword.cloud_notm}} Kubernetes Service 來部署運用 Watson Tone Analyzer 的容器化應用程式。在提供的情境中，虛構的公關公司使用 {{site.data.keyword.cloud_notm}} 服務來分析其新聞稿，並收到對其訊息基調的意見。如需相關資訊，請參閱[將應用程式部署至 Kubernetes 叢集](/docs/containers?topic=containers-cs_apps_tutorial)指導教學。

## 部署至 Cloud Foundry
{: #swift-deploy-cf}

此選項可部署雲端原生應用程式，您不需要管理基礎架構。

如果您計劃將應用程式部署至 [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about)，則必須[準備 {{site.data.keyword.cloud_notm}} 帳戶](/docs/cloud-foundry?topic=cloud-foundry-prepare)。

如果您的帳戶有權存取 {{site.data.keyword.cfee_full_notm}}，則可以選取**[公用雲端](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)**或**[企業環境](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)**的部署者類型，您能夠使用此類型來建立及管理隔離環境，專供您的企業用來管理 Cloud Foundry 應用程式。

## 部署至虛擬伺服器
{: #virtual_deploy-swift}

虛擬服務針對所有工作負載類型，提供較高程度的透通性、可預測性及自動化。Terraform 用來有效率且安全地建置、變更及版本化您的基礎架構。

  VSI 部署適用於部分入門範本套件。若要使用此特性，請前往 [{{site.data.keyword.cloud_notm}} 儀表板](https://{DomainName})，然後按一下**應用程式**磚中的**建立應用程式**。
  {: note}

如需相關資訊，請參閱[部署至虛擬伺服器](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server)。
