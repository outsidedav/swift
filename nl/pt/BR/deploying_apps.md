---

copyright:
  years: 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Implementando e integrando apps Swift
{: #deploy_apps}

É possível implementar os apps Swift com uma cadeia de ferramentas ou usando a interface da linha de comandos. Uma cadeia de ferramentas é um conjunto de integrações de ferramentas. A interface da linha de comandos é uma maneira simples de implementar seus apps e instâncias de serviço.

Para obter mais informações, consulte  [ Implementando apps ](../apps/dep-app-tool.html).

## Desenvolvendo aplicativos de Swift sem servidor
{: #serverless}

Para facilitar o desenvolvimento sem servidor, é possível usar a oferta Functions as a Service (FaaS) da IBM, {{site.data.keyword.openwhisk}}. O {{site.data.keyword.openwhisk_short}} permite que a lógica do aplicativo seja executada em resposta a eventos ou chamadas diretas de apps da web ou móveis sobre HTTP sem fornecimento ou gerenciamento de servidores.

O {{site.data.keyword.openwhisk_short}} executa administração do sistema, como auto-scaling, gerenciamento de disponibilidade e manutenção, permitindo que os desenvolvedores permaneçam focados na gravação de lógica do aplicativo.

Para obter mais informações, consulte [Desenvolvendo apps sem servidor](../apps/deploying/functions.html).

## Integrando serviços de backend com um SDK gerado
{: #backend_gensdk}

O plug-in {{site.data.keyword.IBM_notm}} SDK Generator integra facilmente os serviços de backend a seu app com um SDK gerado. Quando ocorrer uma mudança em uma API de REST, será possível gerar novamente o SDK e substituir o antigo por um upgrade de SDK sem interrupção. Também é possível integrar a CLI em um pipeline devops e assegurar que o SDK seja sempre consistente com a especificação de API toda vez que o app for construído.

Para obter mais informações, consulte [Integrando serviços de backend com um SDK gerado](/docs/swift/backend/cli_sdkgen.html).

## Implementando em um Cluster do Kubernetes
{: #deploy_kube}

É possível saber como usar o serviço Kubernetes do {{site.data.keyword.cloud_notm}} para implementar um app Node.js conteinerizado que alavanca o Watson Tone Analyzer. No cenário fornecido, uma firma PR fictícia usa o serviço {{site.data.keyword.cloud_notm}} para analisar seus press releases e receber feedback sobre o sinal de suas mensagens. Para obter mais informações, consulte o tutorial [Implementando apps nos clusters](../containers/cs_tutorials_apps.html).

## Implementando em um Virtual Server
{: #virtual_deploy}

Um serviço virtual entrega um grau mais alto de transparência, de previsibilidade e de automação para todos os tipos de carga de trabalho. O Terraform é usado para construir, mudar e criar versões de sua infraestrutura de maneira segura e eficiente.

Para obter mais informações, consulte [Implementando em um Virtual Server](../apps/vsi-deploy.html).
