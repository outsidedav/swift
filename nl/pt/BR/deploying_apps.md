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

# Implementando e integrando apps Swift
{: #deploy_apps-swift}

É possível implementar os apps Swift com uma cadeia de ferramentas ou usando a interface da linha de comandos. Uma cadeia de ferramentas é um conjunto de integrações de ferramentas. A interface da linha de comandos é uma maneira simples de implementar seus apps e instâncias de serviço.

Para obter mais informações, consulte  [ Implementando apps ](/docs/apps/dep-app-tool.html#deploying-apps).

## Desenvolvendo aplicativos de Swift sem servidor
{: #serverless-swift}

Para desenvolver em um desenvolvimento sem servidor, é possível usar a oferta Functions as a Service (FaaS) da IBM, {{site.data.keyword.openwhisk}}. O {{site.data.keyword.openwhisk_short}} permite que a lógica do aplicativo seja executada em resposta a eventos ou chamadas diretas por meio de apps móveis ou da web sobre HTTP sem criar ou gerenciar servidores.

O {{site.data.keyword.openwhisk_short}} executa administração do sistema como Auto-scaling, gerenciamento de disponibilidade e manutenção, permitindo que os desenvolvedores permaneçam focados na gravação da lógica do aplicativo.

Para obter mais informações, consulte [Desenvolvendo apps sem servidor](/docs/apps/deploying/functions.html#serverless).

## Integrando serviços de backend com um SDK gerado
{: #backend_gensdk-swift}

O plug-in {{site.data.keyword.IBM_notm}} SDK Generator integra facilmente os serviços de backend a seu app com um SDK gerado. Quando uma mudança em uma API de REST ocorre, é possível gerar novamente o SDK e substituir o antigo para fazer upgrade do SDK. Também é possível integrar a CLI a um pipeline do DevOps e assegurar-se de que o SDK esteja
sempre consistente com a especificação de API toda vez que o app é construído.

Para obter mais informações, consulte [Integrando serviços de backend com um SDK gerado](/docs/swift/backend/cli_sdkgen.html#sdk-cli).

## Implementando em um Cluster do Kubernetes
{: #deploy_kube-swift}

É possível saber como usar o serviço Kubernetes do {{site.data.keyword.cloud_notm}} para implementar um app Node.js conteinerizado que alavanca o Watson Tone Analyzer. No cenário fornecido, uma firma PR fictícia usa o serviço {{site.data.keyword.cloud_notm}} para analisar seus press releases e receber feedback sobre o sinal de suas mensagens. Para obter mais informações, veja o tutorial [Implementando apps nos
clusters do Kubernetes](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial).

## Implementando no Cloud Foundry
{: #swift-deploy-cf}

Essa opção implementa o seu app nativo de nuvem sem você precisar gerenciar a infraestrutura subjacente.

Se você planeja implementar o seu app no [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry/index.html#about), deverá [preparar a sua conta do {{site.data.keyword.cloud_notm}}](/docs/cloud-foundry/prepare-account.html#prepare).

Se a sua conta tiver acesso ao {{site.data.keyword.cfee_full_notm}}, será possível selecionar um tipo de implementador do **[Public Cloud](/docs/cloud-foundry-public/about-cf.html#about-cf)** ou do **[Enterprise Environment](/docs/cloud-foundry-public/cfee.html#cfee)**, que é possível usar para criar e gerenciar ambientes isolados para hospedar aplicativos do Cloud Foundry exclusivamente para a sua empresa.

## Implementando em um Virtual Server
{: #virtual_deploy-swift}

Um serviço virtual entrega um grau mais alto de transparência, de previsibilidade e de automação para todos os tipos de carga de trabalho. O Terraform é usado para construir, mudar e criar versões de sua infraestrutura de maneira segura e eficiente.

Para obter mais informações, consulte [Implementando em um Virtual Server](/docs/apps/vsi-deploy.html#vsi-deploy).
