---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: swift starter kit, apple developer console, download code swift, app details swift, create swift app

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Criando apps do Swift com kits do iniciador
{: #starterkits-intro}

O {{site.data.keyword.cloud_notm}} Developer Console for Apple permite que os desenvolvedores da Apple criem apps por meio de vários kits do iniciador, provisionem e conectem serviços essenciais otimizados pelo {{site.data.keyword.cloud_notm}} e, em seguida, façam download rapidamente do código de trabalho ou configurem para entrega contínua. Os usuários são capazes de criar, visualizar, configurar e gerenciar o seu app, bem como fazer download do código do seu app. O uso de kits do iniciador ajuda a avaliar e testar rapidamente os serviços do {{site.data.keyword.cloud_notm}} com um aplicativo totalmente novo.

Pronto para saltar? Visite o [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") agora para começar.
{: tip}

## O que é um kit do iniciador?
{: #starterkits-what}

Com o {{site.data.keyword.cloud_notm}} Developer Experience, é possível escolher entre vários kits de iniciador. Os kits do iniciador instruem o {{site.data.keyword.cloud_notm}} a montar dinamicamente um app de produção de estrutura básica, na linguagem de sua escolha, pronto para implementação na nuvem. Cada kit do iniciador incorpora uma linguagem, uma estrutura e um padrão para um caso de uso específico do mundo real que permite reutilizar código, em vez de reinventá-lo.

Os kits do iniciador estão prontos para produção e concentram-se em demonstrar uma implementação de padrão chave usando um tempo de execução (por exemplo, Swift). Em alguns casos, os kits do iniciador oferecem uma experiência do usuário simples para destacar a integração do serviço. Em outros casos, os kits do iniciador representam uma implementação customizável de um caso de uso sofisticado.

Os kits do iniciador contêm instruções que permitem que o {{site.data.keyword.cloud_notm}} produza automaticamente apps colocados em andaimes com código móvel e especifique serviços a serem provisionados automaticamente quando você criar um app por meio do kit do iniciador.

## Usando o  {{site.data.keyword.cloud_notm}}  Developer Console para Apple
{: #starterkits-journey}

O {{site.data.keyword.cloud_notm}} Developer Console for Apple fornece um caminho sem interrupção para construir um app iniciador do Swift para seu caso de uso específico. Vamos examinar as etapas que podem ser executadas em sua jornada.

### Tela Visão geral
{: #overview_screen}

A tela Visão geral fornece conteúdo customizado para um conjunto de casos de uso, como Watson, Weather e mais. Na tela de visão geral, é possível ver a documentação, acessar recursos educacionais, procurar serviços, ver kits do iniciador apresentados ou vincular a uma coleção maior de kits do iniciador. Clique em `Starter Kits` na área de navegação esquerda para entrar na visualização Starter Kits.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - Tela Visão geral](images/overview_screen.png "Tela Visão geral") <br> * {{site.data.keyword.cloud_notm}}  Tela do Console do Developer para Visão Geral da Apple *

### Visualização de kits do iniciador
{: #starter_kits_view}

A visualização de kits do iniciador mostra a coleção de kits do iniciador específica para uma área de caso de uso. É possível clicar em vários links em um cartão do kit do iniciador para ver demos e mais informações. Selecione um kit do iniciador para ir para a visualização Criar app.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - Visualização Starter Kits](images/starter_kits_screen.png "Visualização Starter Kits") <br> *Visualização Starter Kits do {{site.data.keyword.cloud_notm}} Developer Console for Apple*

### Criar visualização de app
{: #create_new_app_view}

Na visualização **Criar app**, é possível nomear o seu app, assim como fornecer informações de implementação e roteamento. À direita, também é possível ver os serviços que são provisionados automaticamente ao criar seu app, junto a planos de precificação e termos para cada um. Clique em `Create` para ir para a visualização Detalhes do app. Se você não estiver conectado ao {{site.data.keyword.cloud_notm}}, será necessário fazer isso agora.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - Visualização Criar novo app](images/create_new_project_screen.png "Visualização Criar novo app") <br> *Visualização Criar novo app do {{site.data.keyword.cloud_notm}} Developer Console for Apple*

## Visualização de detalhes do aplicativo
{: #app_details_view}

A visualização Detalhes do app exibe uma lista de serviços configurados para o seu app. Para cada item da lista, é possível ver o nome do serviço, links para outras informações e um botão de **ações** com três pontos alinhados verticalmente. As opções do botão de **ações** são para remover serviços de um app, abrir painel para serviço e excluir serviço. A remoção de uma instância de serviço remove a associação a esse app e não exclui a instância de serviço. Além disso, as credenciais de serviço são consolidadas nessa visualização, portanto, não é necessário visitar cada visualização de instância de serviço individual para obtê-la.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - Visualização Detalhes do app](images/project_details_screen.png "Visualização Detalhes do app") <br> *Visualização Detalhes do app do {{site.data.keyword.cloud_notm}} Developer Console for Apple*

Usando a página **Detalhes do app**, é possível incluir serviços novos ou existentes em seu app que não faziam parte do Kit do iniciador original. Clique em **Incluir serviço** para incluir serviços. Os serviços disponíveis dependem do tipo de app e dos serviços disponíveis em uma região; portanto, nem todos os serviços estão disponíveis para associação a todos os apps.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - Diálogo Incluir recurso](images/add_resource_screen.png "Diálogo Incluir recurso") <br> *Diálogo Incluir recurso do {{site.data.keyword.cloud_notm}} Developer Console for Apple*

### Fazendo download do seu código

* Na página _Detalhes do app_, é possível acessar o seu código selecionando **Fazer download do código** para gerar e fazer download do código para o seu app.

### Visualização Lista de Aplicativos
{: #app_list_view}

É possível listar todos os seus apps criados por meio da visualização de _Lista de apps_. É possível renomear ou excluir seus apps daí. Clique na linha de um nome do app para retornar para a visualização Detalhes do app.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple - Visualização Lista de apps](images/project_list_screen.png "Visualização Lista de apps") <br> *Visualização Lista de apps do {{site.data.keyword.cloud_notm}} Developer Console for Apple*

Para obter mais informações, visite o [{{site.data.keyword.cloud_notm}} Developer Console for Apple Learning Resources](https://cloud.ibm.com/developer/appledevelopment/learning-resources){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").
{: tip}
