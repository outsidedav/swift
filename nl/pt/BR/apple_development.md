---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: watson swift, swift developer, apple development, ios oveview, developer console, swift, apple console

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:tip .tip}

# Desenvolvimento da Apple no {{site.data.keyword.cloud_notm}}
{: #ios_dev}

O {{site.data.keyword.cloud}} oferece soluções e serviços para fornecer a desenvolvedores e aplicativos do Swift a segurança, a AI e o valor exigido por seus clientes. Com um amplo portfólio de ofertas e SDKs, é possível alavancar esses serviços e trazer os seus aplicativos de ponta para o mercado rapidamente.

## Uma plataforma de desenvolvimento: IBM Cloud
{: #platform}

Os recursos do desenvolvedor que são integrados no {{site.data.keyword.cloud_notm}} se alinham a vários conjuntos de habilidades e fornecem uma plataforma para produzir, entregar, executar e gerenciar o seu app. Por exemplo, por meio do aplicativo cognitivo que é mencionado, os seguintes recursos do {{site.data.keyword.cloud_notm}} são de interesse:

* O [**console do desenvolvedor do {{site.data.keyword.cloud_notm}}**](/docs/apps?topic=creating-apps-getting-started) inclui um conjunto de recursos no {{site.data.keyword.cloud_notm}} que ajuda os desenvolvedores digitais e do Cloud Native a iniciarem a construção de apps prontos para produção. Ele inclui a prestação de serviços automática e a implementação de "um clique" para uma cadeia de ferramentas do DevOps.

* A [**IBM Watson Data Platform**](https://dataplatform.cloud.ibm.com/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") torna simples a montagem de coletas de dados, refinar os dados e, em seguida, visualizar, descobrir insights e construir modelos para uso em seu app cognitivo.

* O [**IBM Streaming Analytics**](/docs/services/StreamingAnalytics?topic=StreamingAnalytics-gettingstarted#gettingstarted) ajuda a argumentar e analisar fluxos de dados. É uma plataforma de analítica avançada que pode ser usada para alimentar, analisar e correlacionar informações à medida que chegam por meio de tipos diferentes de origens de dados em tempo real.

* O [**{{site.data.keyword.cloud_notm}} serviço Continuous Delivery**](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-getting-started) configura uma cadeia de ferramentas do devops para automatizar a entrega contínua de seu app. É possível aprimorar facilmente a cadeia de ferramentas para incluir funções de gerenciamento como monitoramento, criação de log, rastreamento e alerta. É possível até mesmo aplicar aprendizado de máquina avançado à sua cadeia de ferramentas usando o [serviço DevOps Insights](/docs/services/DevOpsInsights?topic=DevOpsInsights-getting-started#getting-started).

A plataforma {{site.data.keyword.cloud_notm}} oferece muito mais recursos e pode ser usada como sua plataforma de desenvolvimento abrangente.

## Visão geral de recursos do {{site.data.keyword.cloud_notm}}
{: #capabilities}

O console do desenvolvedor do {{site.data.keyword.cloud_notm}} ajuda você a iniciar a construção de apps de maneira "correta" em minutos. Elementos essenciais do console do desenvolvedor são:

* Um conjunto de consoles de desenvolvedor centrados no tópico ou no canal, localizados ao longo da plataforma {{site.data.keyword.cloud_notm}}.
* Kits do iniciador de caso de uso específico que produzem apps iniciadores prontos para produção em vários idiomas e padrões arquiteturais.
* Provisionamento automático de serviços.
* Gerencie os componentes do app usando uma estrutura de aplicativo móvel.
* Um-clique na criação de uma  [ cadeia de ferramentas devops ](/docs/services/DevOpsInsights?topic=DevOpsInsights-getting-started#getting-started).

Para entender como o console do desenvolvedor do {{site.data.keyword.cloud_notm}} pode ajudá-lo a construir rapidamente apps prontos para produção de alta qualidade, consulte os elementos a seguir em mais detalhes.

## Consoles do desenvolvedor
{: #developer_consoles}

O console do desenvolvedor do {{site.data.keyword.cloud_notm}} inclui áreas de interesse (como Watson, Segurança ou Finanças) ou um canal digital (como Apps móveis ou da web). O [{{site.data.keyword.cloud_notm}}Developer Console for Apple](https://{DomainName}/developer/appledevelopment/dashboard){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") foi desenvolvido para que os desenvolvedores da Apple possam experimentar rapidamente os aplicativos e os serviços, apoiados pela plataforma do {{site.data.keyword.cloud_notm}}. É possível acessar esses consoles clicando no ícone do menu no console do {{site.data.keyword.cloud_notm}}. Por exemplo, consulte os seguintes consoles do desenvolvedor:

* [Console do desenvolvedor do Watson](https://{DomainName}/developer/watson/dashboard){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")
* [Console do desenvolvedor do Mobile](https://{DomainName}/developer/mobile/dashboard){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")
* [Console do desenvolvedor de aplicativo da web](https://{DomainName}/developer/appservice/dashboard){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")
* [Console do desenvolvedor de segurança](https://{DomainName}/developer/security/dashboard){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")
* [Console do desenvolvedor financeiro](https://{DomainName}/developer/finance/dashboard){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")

<!--Cloud native development is the process of developing apps that are optimized to leverage capabilities engendered from running on the cloud.  Flexibility, portability, scaling, rapid development, continuous delivery, and a close coupling development and operations ("devops) are characteristics of cloud applications. The {{site.data.keyword.cloud}} developer console quickly gets you started building cloud native applications that are ready for team development and bound for production use.-->


<!--![Overview of elements of the {{site.data.keyword.cloud_notm}} developer console](images/elements_of_devex.png "Overview of elements of the {{site.data.keyword.cloud_notm}} developer console") <br> *Overview of elements of the {{site.data.keyword.cloud_notm}} developer console*-->

Cada console do desenvolvedor fornece Starter Kits relevantes para a área de foco desse console, oferecendo um fluxo de trabalho consistente e intuitivo para criar um app funcional pronto para produção em minutos.

## Criando apps com kits do iniciador
{: #starterkit-apps}

É possível alavancar os [Kits do iniciador](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro) para criar apps do Swift, que contêm a associação de código, dados, serviços e cadeias de ferramentas que compõem o seu app.
