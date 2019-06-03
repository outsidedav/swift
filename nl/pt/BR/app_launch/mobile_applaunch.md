---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: swift app service, swift versioning, audience swift, messages swift, in-app swift, engagement swift, create feature swift, feature control swift, swift feature release

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:gif: data-image-type='gif'}

# Gerenciando Liberações de Recursos do
{: #mobile_applaunch}

O {{site.data.keyword.engage_full}} permite que os desenvolvedores construam apps envolventes controlando o alcance e a apresentação de recursos de app que podem fornecer métricas mensuráveis. O serviço ajuda os desenvolvedores a remover o acoplamento que existe hoje entre o lançamento do recurso do app e as atualizações do app para a produção. Agora é possível publicar recursos sem expô-los à produção para liberar gradualmente novas versões de um app de maneira controlada. Com o serviço {{site.data.keyword.engage_short}}, os proprietários de app têm controle total sobre o lançamento do recurso para um segmento de destino.

O serviço {{site.data.keyword.engage_short}} define um recurso, cria um público baseado em plataformas de dispositivo (incluindo atributos de público customizados) e, finalmente, define um engajamento que coreografa a sincronização e o posicionamento do recurso. Depois que os SDKs são usados, junto aos atributos de recurso e de métricas incorporados no aplicativo, o serviço começa a medir as experiências do público. Agora é possível usar o app com base nessas informações para criar engajamentos do cliente customizados em várias categorias de usuários do app.

![Visão geral do Cognitive Engage](images/process_app_launch.png) Figura 1. Visão geral do ciclo de vida do serviço {{site.data.keyword.engage_short}}

Consulte os seguintes recursos do serviço {{site.data.keyword.engage_short}}:

- Acelerar o recurso de implementação

  Acelerar a entrega de recursos para seu app por meio de liberação controlada, minimizando os riscos. Libere recursos para um subconjunto de segmentos de público e tome decisões maiores de lançamento ou de recuperação com base em feedback em tempo real. Separe os lançamentos de recursos de ciclos de liberação regulares.

- Segmentar público

  Os segmentos de usuário podem ser definidos com base em atributos demográficos, contextuais e comportamentais. Também é possível lançar recursos para determinada porcentagem da base do usuário inteira. Os principais indicadores de desempenho podem ser definidos para cada recurso e código do lado do cliente para medir os resultados.

- Adaptar com base no contexto do aplicativo

  O comportamento do aplicativo, a interface com o usuário e as notificações podem ser customizados para segmentos de público específicos. Por exemplo, o plano de fundo do app pode mudar com base na localização do usuário. Essa personalização do usuário resulta em maior engajamento de usuários com o aplicativo.

- A / B recursos de teste

  Obtenha confiança por experimentação. Duas variantes de recursos de aplicativos podem ser comparadas entre si apresentando ambas ao mesmo tempo. As decisões podem ser tomadas com base em dados permanentes.

- Aumentar o engajamento do cliente**

  Promover o engajamento do usuário. As notificações podem ser destinadas a todos os usuários do aplicativo ou a um conjunto específico de usuários e dispositivos. Um planejamento mensagem pode ser definido. A interação com o usuário reproduz um papel vital nos relacionamentos do cliente.

## Antes de começar
{: #prereqs-applaunch}

Primeiro, assegure-se de que tenha os seguintes pré-requisitos prontos para execução:

 - iOS 10 +
 - Xcode 9
 - Swift 3.2-4
 - CocoaPods ou Carthage

## Etapa 1. Criando uma instância do  {{site.data.keyword.engage_short}}
{: ##app_launch_create}

1. No catálogo do {{site.data.keyword.cloud_notm}}, clique em **Mobile** > **Ativação do app**. A tela de configuração de serviço é aberta.
2. Dê um nome à sua instância de serviço ou use o nome de pré-configuração.
3. Clique em **Criar**.
4. Na área de janela de navegação, clique em **Conexões** para selecionar um app e ligá-lo a seu serviço. É possível ligar a instância de serviço ao seu app posteriormente se ele não estiver vinculado durante a criação.

## Etapa 2. Inicializando seu app
{: #initialize-applaunch}

O serviço fornece SDKs específicos de plataforma para simplificar o desenvolvimento de aplicativo. Os SDKs do {{site.data.keyword.cloud_notm}} Mobile Services Swift podem ser instalados com o CocoaPods ou o Carthage.

1. Clique em **Configurações**.
2. Instale o  [ SDK ](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-applaunch). Para obter mais informações, consulte o arquivo `README` que inclui as etapas de instalação e os conceitos técnicos.
3. Copie as chaves de configuração para inicializar o app. Use o segredo do app, o GUID do app e o segredo do cliente para configurar o app e criar engajamentos.

## Etapa 3. Criando um Recurso
{: #create-feature-applaunch}

O serviço {{site.data.keyword.engage_short}} cria e testa respostas para recursos.

![Feature Details](images/feature_creation_animated.gif){: gif}

Para criar um recurso, conclua as etapas a seguir: 
1. Na área de janela de navegação, clique em **Recursos** > **Criar novo recurso**.
2. Atualize o formulário Criar novo recurso e métricas com um nome de recurso e uma descrição. Também é possível definir as propriedades do recurso e incluir métricas para medir o impacto de seu engajamento. Clique em **Edição em massa** para incluir múltiplas propriedades editando o JSON.
3. Clique em **Criar**. O novo recurso agora aparece no painel Recursos.
4. Ative o recurso depois de ele ser desenvolvido.
5. Para permitir que um recurso seja usado como engajamento, clique no Recurso criado.
6. Na janela Detalhes do recurso, escolha Atualizar status de seu recurso para **Pronto**.
7. Clique em  ** Atualizar Status **.
8. Atualize seu app para incluir os atributos e os códigos de recurso recém-criados no app iOS.
9. O recurso agora está pronto para ser usado.

A janela Detalhes do recurso pode exportar o recurso como um arquivo JSON, que pode ser usado no aplicativo cliente para carregar os valores padrão.

## Etapa 4. Criando um público
{: #audience-applaunch}

![Create Audience](images/create_audience_animated.gif){: gif}

Para criar um público, conclua as etapas a seguir:

### Criando um **atributo de público**:
{: #audience-attrib-applaunch}

1. Clique em **Público** > **Criar Atributo**.
2. Forneça os seguintes valores:
  * **Nome**: forneça um nome apropriado para o atributo.
  * **Descrição**: uma breve descrição sobre o atributo.
  * ** Tipo **: Escolha o tipo de atributo.
  * **Valores permitidos**: insira os valores de atributo que você gostaria de usar.

  É possível optar por criar mais de um atributo de público, conforme listado na imagem a seguir, com base em seu requisito.

### Criando um **público**:
{: #audience-create-applaunch}

1. Clique em  ** Criar Audiência **.
2. Forneça um nome apropriado e uma descrição na janela Novo público.
3. Selecione um atributo e clique em  ** Incluir **.
4. Escolha as opções necessárias dos atributos listados.
5. Clique em  ** Salvar **.

Agora é possível criar um Engagement.

## Etapa 5. Criando um Engagement
{: #engagement-applaunch}

Um Engajamento é uma instanciação de um Recurso com propriedades inicializadas e a anexação de um dos públicos predefinidos. É possível criar um engajamento usando **Controle de recurso** ou **Sistema de mensagens no app**.

### Ativando Recurso de Controle de Recurso
{: #feature-control-applaunch}

Por meio desse engajamento, um proprietário de app pode controlar a visibilidade de um recurso ativando ou desativando-o no tempo de execução. Um recurso pode ser ativado ou desativado para todos os usuários do aplicativo ou para um conjunto específico de usuários e dispositivos.

As apresentações do recurso podem ser planejadas e coordenadas definindo-se uma data e hora de início ou encerramento. Também é possível escolher um dia específico no qual um recurso definido deve ser ativado ou desativado.

![GIF animado](images/create_engagement.gif){: gif}

Conclua as etapas a seguir para criar um engajamento usando o Controle de recurso:

1. É possível criar um engajamento usando um dos métodos a seguir:
  - Clique em **Engajamentos** na área de janela de navegação.
  - Selecione **Criar engajamentos** no novo Recurso criado.
  - Na área de janela de navegação, clique em **Visão geral** > **Criar novo engajamento**.

  A janela Novo Engagement aparece.

2. Forneça um nome e uma descrição para o seu novo engajamento. Assegure-se de que você dê um nome exclusivo ao engajamento e não um que já esteja listado em Engajamentos.
  - ** Selecione o Tipo de Compromisso **  como  ** Controle de Recurso **.
  - Para executar um experimento controlado com mais de uma variante do recurso, selecione **Teste A/B** em **Selecionar tipo de experimentação**. Clique em  ** Avançar **.

3. Selecione o Recurso que você criou. Também é possível optar por incluir e definir as variantes que você possa querer experimentar. Clique em  ** Avançar **.

4. Selecione um público. Clique em  ** Avançar **.

5. Defina um acionador escolhendo Horário e a data ou hora de **Início** e uma data ou hora de **Encerramento**. Clique em  ** Salvar **.

  O novo engajamento agora aparece na janela Detalhes de engajamento.

Agora é possível medir o [desempenho](/docs/services/app-launch?topic=app-launch-applaunch_type#applaunch_type) de seu engajamento.

### Ativando o recurso de sistema de mensagens no app
{: #app-message-applaunch}

Por meio desse engajamento, um proprietário de app pode enviar notificações para os usuários do app enquanto eles estão usando ativamente o aplicativo.

As mensagens podem ser destinadas a todos os usuários do aplicativo ou para um conjunto específico de usuários e dispositivos. Para cada mensagem enviada para o serviço, o público desejado recebe uma notificação.

As mensagens no app podem ser planejadas definindo-se uma data e hora de início ou encerramento. Também é possível planejá-las com base em um evento. Essas mensagens são mais customizadas, pois se baseiam em insights da análise de dados sobre as opções, as interações, os dispositivos e os logs de aplicativo do usuário e muito mais.

Consulte os seguintes exemplos de mensagem no app:

- Enviar mensagens customizadas.
- Enviar mensagens para usuários que desativaram as notificações push.
- Solicitar feedback ou envolver usuários em uma conversa.
- Enviar mensagens relevantes com base no que o usuário está procurando.
- Acione clientes ativos e leais.
- Informar os usuários sobre as atualizações do app (ativação de um novo recurso).

![GIF animado](images/in-app-engagement_animated.gif){: gif}

Conclua as etapas a seguir para criar um engajamento que use a opção Sistema de mensagens:

1. É possível criar um engajamento usando um dos métodos a seguir:
  - Clique em **Engajamentos** na área de janela de navegação.
  - Selecione **Criar engajamento** no novo Recurso criado.
  - Na área de janela de navegação, clique em **Visão geral** > **Criar novo engajamento**.

  A janela Novo Engagement aparece.

2. Forneça um nome e uma descrição para o seu novo engajamento. Assegure-se de fornecer um nome exclusivo ao engajamento, e não um que já esteja listado em Engajamentos.
  - **Selecionar Tipo de Compromisso** como **Em-Mensagens App**
  - Para executar um experimento controlado com múltiplas variantes do recurso de sistema de mensagens, selecione **Teste A/B** em **Selecionar tipo de experimento**. Clique em  ** Avançar **.

3. Insira as propriedades de mensagem e clique em **Avançar**.

4. **Selecione o público** e a porcentagem de público que deseja alcançar. Clique em  ** Avançar **.

5. Defina um acionador selecionando uma **Data e hora de início/encerramento**.

6. Selecione **Evento** e clique em **Avançar**.

7. Mapeie os elementos para as métricas que você deseja medir. Selecione o elemento e insira os detalhes da métrica. Clique em  ** Salvar **.

  O novo engajamento agora aparece na janela Detalhes de engajamento.

Agora é possível medir o [desempenho](/docs/services/app-launch?topic=app-launch-applaunch_type#applaunch_type) de seu engajamento.

## Links Rápidos
{: #links-applaunch notoc}

Verifique os links a seguir para obter insight e entender os recursos do {{site.data.keyword.engage_short}}:

 - Experimente o [Serviço de ativação do app](https://cloud.ibm.com/catalog/services/app-launch){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").
 - [Blogs e vídeos](/docs/services/app-launch?topic=app-launch-blogs-and-videos#blogs-and-videos)
 - Para obter mais informações, consulte [Ativação de app: tutorial de introdução](/docs/services/app-launch?topic=app-launch-gettingstartedtemplate#gettingstartedtemplate).
