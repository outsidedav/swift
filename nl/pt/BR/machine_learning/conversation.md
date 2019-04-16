---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: chatbot swift, virtual assistant swift, assistant swift, watson swift starter, assistantv2 swift, watson sdk swift, add chatbot swift, add assistant swift

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Incluindo um robô de bate-papo
{: #assistant}

É possível usar o serviço {{site.data.keyword.conversationshort}} para construir aplicativos que entendam a entrada de língua natural e respondam aos usuários com conversa do tipo "humano".

A lista a seguir descreve o fluxo de como a integração funciona:

1. Os usuários interagem com a interface implementada em seu app.
2. O seu app envia entrada do usuário para o {{site.data.keyword.conversationshort}} usando o {{site.data.keyword.watson}} Swift SDK.
3. O {{site.data.keyword.watson}} Swift SDK conecta-se a uma área de trabalho, que é um contêiner para o fluxo de diálogo e dados de treinamento.
4. A área de trabalho interpreta a entrada do usuário e direciona o fluxo da conversa, enviando uma resposta para o app.
5. Seu app exibe a resposta para o usuário.

## Antes de começar
{: #prereqs-chatbot}

Assegure-se de que você tenha os pré-requisitos a seguir:

* Uma instância do [serviço do {{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-getting-started#getting-started)
* iOS 10.0 +
* Xcode 9.3 +
* Swift 4.1 +
* CocoaPods, Carthage, ou Swift Package Manager

É possível instalar o [SDK do Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") usando o [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), o [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ou o [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"). Usando o CocoaPods para gerenciar dependências, você obterá apenas as estruturas necessárias, em oposição a todo o SDK do Watson Swift. Se você for novo no CocoaPods, poderá instalá-lo facilmente:

```console
sudo gem install cocoapods
```
{: codeblock}

## Etapa 1. Criando uma instância do Watson Assistant
{: #instance-watson-chatbot}

Provisem uma instância do serviço  {{site.data.keyword.conversationshort}} :

1. No catálogo do {{site.data.keyword.cloud_notm}}, selecione **{{site.data.keyword.conversationshort}}**. A tela de configuração de serviço é aberta.
2. Dê um nome à sua instância de serviço ou use o nome de pré-configuração.
3. Selecione um app no menu **Conectar** se quiser ligar sua instância a um app.
4. Selecione um plano de precificação e clique em **Criar**.
5. Selecione a guia **Credenciais** para visualizar as suas credenciais de serviço. Esses valores são usados para se conectar ao serviço por meio de seu app.
6. Clique na **ferramenta Ativar** para construir seu assistente de robô
de bate-papo. Siga as instruções na página de entrada para construir seu próprio robô de bate-papo
ou acesse a guia **Qualificações** e selecione **Criar novo**. Na
página **Incluir qualificação de diálogo**, escolha a guia **Usar a qualificação de amostra** e selecione a **Qualificação de amostra de atendimento ao cliente** para começar a usar rapidamente. É possível continuar a refinar essa qualificação ou
criar outras para serem usadas por seu app posteriormente.
7. Clique no menu em sua nova qualificação e selecione **Visualizar detalhes da
API**. Agora é possível ver o `ID da área de trabalho` que você precisa,
além das credenciais de serviço.

## Etapa 2. Fazendo Download e Construindo Dependências
{: #download-depend-chatbot}

O exemplo a seguir usa o AssistantV1. Para obter mais informações sobre a estrutura do AssistantV2, consulte a [documentação do Watson SDK AssistantV2](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV2/index.html){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

Usando seu editor de texto favorito, crie um novo `Podfile` no diretório raiz do seu projeto (onde o `.xcodeproj` está localizado) executando `pod init`. Em seguida, inclua uma linha para especificar a estrutura do {{site.data.keyword.conversationshort}} do SDK Watson Swift como uma dependência:

```pod
use_estruturas!

target 'MyApp' do
    pod 'IBMWatsonAssistantV1'
```
{: codeblock}

Para um app de produção, você também pode desejar especificar um [requisito de versão](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} específico![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") para evitar mudanças inesperadas de novas liberações do SDK do Watson Swift.

Com seu `Podfile` no local, agora será possível fazer download das dependências. Use um terminal para navegar para o diretório raiz do seu projeto, em seguida, execute o CocoaPods:

```console
pod install
```
{: codeblock}

O Cocoapods faz downloads da estrutura {{site.data.keyword.conversationshort}} e o constrói na pasta `Pods/` do seu projeto.

Para evitar falhas de construção do Pod, abra o arquivo que termina com `.xcworkspace` em vez de `.xcodeproj` quando abrir o projeto no Xcode.
{: tip}

## Etapa 3. Incluindo um assistente virtual em seu app
{: #virtual-assist-chatbot}

As amostras a seguir ajudam você a incluir os recursos do {{site.data.keyword.conversationshort}}
em seu aplicativo, geralmente no `ViewController.swift`. Usando os exemplos a seguir,
é possível estender as chamadas do Assistant para seu caso de uso.

1. Inclua uma instrução de importação para o {{site.data.keyword.conversationshort}},
por exemplo, `import Assistant`.
  ```swift
  Assistente de importação
  ```
  {: codeblock}

2. Instancie o serviço  {{site.data.keyword.conversationshort}} :
  ```swift
  let assistant = Assistant(version: "yyyy-mm-dd", apikey: "your-api-key-here")

  // save context to state to continue the conversation later
  var context: Context?
  ```
  {: codeblock}

  **Dica**: esse exemplo salva o contexto no estado. Para um melhor entendimento desse objeto e como adaptá-lo ao seu caso de uso, veja a [documentação da variável
de contexto](/docs/services/assistant?topic=assistant-dialog-runtime#context-variables). Verifique a [documentação do parâmetro de versão](https://cloud.ibm.com/apidocs/assistant#versioning){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ou use a data em que o serviço {site.data.keyword.conversationshort}} foi criado.

3. Inicialize a conversa. Dependendo de como o seu assistente é configurado, ele pode fornecer
uma resposta inicial para o usuário:
  ```swift
  // Start a conversation
  assistant.message(workspaceID: "your-workspace-ID-here) { response, error in
      if let error = error {
         print(error)
    }

      guard let message = response?.result else {
          print ("Falha ao obter a mensagem.")
          return } print("Conversation ID: \(message.context.conversationID!)")
      print ("Response: \ (message.output.text.juntou ())")

      // Set the context to state
      self.context = message.context
  }
  ```
  {: codeblock}

4. Envie uma mensagem e o contexto para o assistente:
  ```swift
  print ("Request: When are open?")
  let input = InputData(text: "When are you open?")

  assistant.message(workspaceID: workspace, input: input, context: self.context) { response, error in
      if let error = error {
         print(error)
    }

      guard let message = response?.result else {
          print ("Falha ao obter a mensagem.")
          return } print("Conversation ID: \(message.context.conversationID!)")
      print ("Response: \ (message.output.text.juntou ())")

      // Update the context
      self.context = message.context
  }
  ```
  {: codeblock}

Quando você executa seu app com esses exemplos com o assistente padrão, as mensagens a seguir
são exibidas no console:
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hello, I’m a demo customer care virtual assistant to show you the basics.  I can help with directions to my store, hours of operation and booking an in-store appointment.
Pedido: quando você abre?
Response: Our hours are Monday to Friday 10am to 8pm and Friday and Saturday 11am to 6pm.
```
{: screen}

5. Explore a [documentação do Assistente](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV1/index.html){: new_window} do SDK do Watson ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") para construir a funcionalidade de seu aplicativo.

## Usando kits iniciadores
{: #starterkits-chatbot}

Com kits do iniciador, é possível usar os recursos do {{site.data.keyword.cloud_notm}} de maneira rápida e fácil. É possível incluir o {{site.data.keyword.conversationshort}} em qualquer backend do lado do servidor usando os kits do iniciador. O robô de bate-papo para iOS com o kit do iniciador do Watson ilustra
como usar os recursos de deep learning do {{site.data.keyword.conversationshort}} incluindo uma
interface de língua natural em seu aplicativo, a qual automatiza interações com seus usuários.

1. Selecione o [kit do iniciador](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") com o qual você deseja trabalhar.
2. Crie o app com os serviços padrão.
3. Clique em **Incluir serviço > Watson > {{site.data.keyword.conversationshort}}**.
4. Faça download do projeto clicando em **Fazer download do código**. É possível localizar as credenciais de serviço no arquivo `config/local-dev.json`.

## Próximas etapas
{: #next-chatbot notoc}

Ótimo trabalho! Você incluiu um assistente de AI em seu app. Tente uma das opções a seguir para manter o ritmo:

* Verifique o [SDK do Swift do {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") e explore os outros serviços suportados do Watson.
* Tire proveito de todos os recursos que o [{{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-index#index) oferece.
* Visualize o código-fonte para o [app de amostra do Simple Chat](https://github.com/watson-developer-cloud/simple-chat-swift){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), demonstrando o SDK do Swift do {{site.data.keyword.watson}} no GitHub.
