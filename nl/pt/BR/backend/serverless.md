---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}

# Desenvolvimento do Serverless
{: #serverless}

O que é serverless? O padrão de desenvolvimento sem servidor refere-se ao desenvolvimento de aplicativo em que a lógica do lado do servidor é executada em contêineres sem estado que são acionados por evento, são efêmeros (duráveis para uma única execução) e totalmente gerenciados por um terceiro. Esse paradigma, também conhecido como Functions as a Service (FaaS), é onde o desenvolvedor fornece uma função sem estado que pode ser acionada e executada sem provisionar ou gerenciar explicitamente um servidor.

Ao separar a infraestrutura e as estruturas necessárias para o desenvolvimento do lado do servidor, a arquitetura sem servidor permite que os desenvolvedores se concentrem na construção de seu aplicativo e na gravação de código para que sejam executados de forma reativa para mudar dados.

A oferta FaaS da IBM, [{{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/), esforça-se para fornecer uma experiência de desenvolvimento sem interrupção, sem precisar de nenhum conhecimento especializado do lado do servidor. Ao usar a tecnologia sem servidor, é possível desenvolver rapidamente soluções escaláveis de backend para atender praticamente qualquer demanda de carga de trabalho sem a necessidade de provisionar recursos antecipadamente. Para aplicativos com padrões de carregamento imprevisíveis ou alto tempo de inatividade do servidor, o {{site.data.keyword.openwhisk_short}} pode ser uma excelente solução de nuvem com desempenho aprimorado e seu sistema "pague pelo que você usa" ajuda a diminuir os custos.

## Alterações Arquiteturais
{: #comparison}

Para ajudá-lo a entender os benefícios arquiteturais de se alternar para o FaaS, as arquiteturas tradicionais e do FaaS são comparadas usando um aplicativo iOS simples que é vinculado a um banco de dados.

Em uma arquitetura mais tradicional, o aplicativo iOS transfere tarefas intensivas de rede ou processa dados remotamente de forma centralizada, que se conecta por seus próprios serviços ou opções de armazenamento. Com um sistema tradicional, o trabalho pesado é colocado em um servidor singular que manipula a autenticação, processando tarefas intensivas para minimizar o estresse no cliente e fornecendo sincronização em sua base de usuários.

Uma arquitetura sem servidor pode alterar essa estrutura para parecer mais com a imagem a seguir.

![](./images/Architecture.png)  Figura 1. Arquitetura sem

Em vez de manipular todo o processamento e a lógica de autenticação dentro de um único servidor, uma arquitetura sem servidor alavanca as funções altamente escaláveis que encapsulam muito da lógica do lado do servidor e transfere alguma lógica para o cliente (e serviços externos).

Olhando para o esquemático, é possível ver os seguintes pontos:

1. O cliente autentica-se em um Provedor de identidade, como o ID do app.
2. Chamadas para a API de backend do FaaS, incluindo o token de acesso.
3. O backend é implementado com o  {{site.data.keyword.openwhisk_short}}. As ações sem servidor, que são expostas como ações da web, esperam que o token seja enviado no cabeçalho da solicitação e verifique sua validade (data de assinatura e de expiração) para fornecer acesso à API real.
4. Quando o cliente envia dados, o feedback é armazenado em um {{site.data.keyword.cloudant_short_notm}}.
5. O texto de feedback é processado com o  {{site.data.keyword.toneanalyzershort}}.
6. Com base no resultado da análise, uma notificação é enviada de volta para o cliente pelo {{site.data.keyword.mobilepushshort}}.
7. O cliente recebe a notificação.

Em um modelo puramente sem servidor, o cliente geralmente assume responsabilidades extras devido à incapacidade de armazenar o estado do usuário. Por exemplo, nesse caso, a autorização é manipulada pelo cliente e pelo serviço do provedor de identidade {{site.data.keyword.appid_short_notm}}.

Embora as arquiteturas sem servidor nem sempre sejam ideais, elas podem fornecer benefícios substanciais sob as condições corretas de uso e da equipe. Verifique alguns exemplos específicos para conhecer alguns dos [casos de uso](#use_cases) mais comuns.

## Benefícios do Server
{: #benefits}

### Custo Reduzido

A terceirização do tempo e do custo monetário associada à administração do sistema, reduz o custo geral associado aos servidores de backend tradicionais. Além disso, o {{site.data.keyword.openwhisk_short}} é diferente das tecnologias de computação tradicionais porque você paga apenas pelo tempo que seu código leva para satisfazer as solicitações, arredondado para os 100 ms mais próximos. Economias de custo consideráveis são possíveis em relação a outras tecnologias, como VMs e contêineres, que provavelmente não são 100% utilizados e ocupam memória no sistema do seu provedor de nuvem.

### Alta disponibilidade e escalabilidade

As arquiteturas sem servidor fornecem inerentemente escalabilidade instantânea com disponibilidade quase constante.

### Aceleração e desenvolvimento simplificado

Eliminando a necessidade de administração do sistema e fornecendo interfaces simples para implementação, o paradigma sem servidor acelera o desenvolvimento de aplicativo. Os desenvolvedores podem construir apps rapidamente com sequências de ações que são executadas em resposta a um mundo orientado a eventos.

## Casos de uso de exemplo
{: #use_cases}

### Backend Móvel
![](./images/cloud-functions-rest-api-trigger.png)

Os desenvolvedores de dispositivos móveis podem acessar facilmente a lógica do lado do servidor e terceirizar tarefas computacionalmente intensivas para uma plataforma de nuvem escalável. É possível implementar funções em linguagens como Swift e consumir facilmente funções do lado do servidor usando o iOS SDK sem a necessidade de experiência do lado do servidor.

### Processamento de Dados

![](./images/cloud-functions-cloudant-trigger.png)

É possível executar código sempre que os dados são atualizados em seu armazenamento de dados por meio de acionadores integrados. Também é possível automatizar processos facilmente, como normalização de áudio, rotação de imagem, aumento da nitidez, redução de ruído, geração de miniatura ou transcodificação de vídeo por meio de um modelo de programação funcional do lado do servidor.

### Processamento de Dados Cognitivo

É possível analisar dados assim que eles se tornam disponíveis. Deixe sua função aproveitar os serviços cognitivos poderosos, como o IBM Watson, para detectar objetos ou pessoas em imagens ou vídeos.

### Tarefas Planejadas

Execute suas funções periodicamente e defina planejamentos que sigam uma sintaxe semelhante a cron para especificar quando as ações devem ser executadas.

## Referência da API
{: #openwhisk_start_api notoc}

<!-- * [REST API Documentation](./openwhisk_reference.html#openwhisk_ref_restapi)-->
* [API REST](https://console.{DomainName}/apidocs/98)

## Links Relacionados
{: #general notoc}

* [ Descobrir:  {{site.data.keyword.openwhisk_short}} ](http://www.ibm.com/cloud-computing/bluemix/openwhisk/)
<!-- redirects to link above * [{{site.data.keyword.openwhisk_short}} on IBM developerWorks](https://developer.ibm.com/openwhisk/)-->
* [ website do projeto Apache OpenWhisk ](http://openwhisk.org)
* [ Mais sobre Serverless ](https://martinfowler.com/articles/serverless.html)
