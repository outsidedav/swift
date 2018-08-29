---

copyright:
  years: 2018
lastupdated: "2018-08-07"

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
{: #before-you-begin}

Assegure-se de que tenha os seguintes pré-requisitos em vigor:

  * Uma instância do  [ {{site.data.keyword.conversationshort}}  serviço ](/docs/services/conversation/getting-started.html)
  * iOS 8.0 +
  * Xcode 9.0 +
  * Swift 3.2 + ou Swift 4.0 +
  * Cartago

Use o [Carthage](https://github.com/Carthage/Carthage){:new_window} para gerenciar dependências e construir o {{site.data.keyword.watson}} Swift SDK para seu aplicativo. Se você for novo no Carthage, poderá instalá-lo com [Homebrew](http://brew.sh/){:new_window}:

```bash
$ brew update $ brew install carthage
```

## Fazendo Download e Construindo Dependências
{: #download-and-build-dependencies}

1. Usando seu editor de texto favorito, crie um novo arquivo denominado `Cartfile` no diretório-raiz de seu projeto (no qual o arquivo `.xcodeproj` está localizado).

2. Inclua uma linha para especificar o Watson Swift SDK como uma dependência:
  ```
  github "Watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

  Para um app de produção, é possível especificar um [requisito de versão](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement){:new_window} para evitar mudanças inesperadas de novas liberações do {{site.data.keyword.watson}} Swift SDK.
  {: tip}

3. Use um terminal para navegar para o diretório-raiz de seu projeto e, em seguida, execute o Carthage:
  ```bash
  atualização de cartago -- plataforma iOS
  ```
  {: codeblock}

  O {{site.data.keyword.watson}} Swift SDK é então transferido por download e sua estrutura é construída na pasta `Carthage/Build/iOS` de seu projeto.

## Incluindo estruturas em seu app
{: #add-frameworks-to-your-app}

Agora que a estrutura do {{site.data.keyword.watson}} Swift SDK foi construída, deve-se vincular e copiar a estrutura do {{site.data.keyword.conversationshort}} para seu app.

1. Abra seu app no Xcode e selecione seu projeto no Navegador para abrir suas configurações.
2. Selecione o destino do app e abra a guia **Geral**.
3. Role para a seção Estruturas e bibliotecas vinculadas e clique no ícone `+`.
4. Na nova janela exibida, clique em **Incluir outro** e navegue para o diretório `Carthage/Build/iOS`.
5. Selecione `AssistantV1.framework` para vinculá-lo a seu app.

Além de vincular a estrutura do {{site.data.keyword.conversationshort}}, deve-se também copiá-la para o app para torná-la acessível no tempo de execução. Um script do Carthage é então usado para evitar um [erro de envio específico do App Store](http://www.openradar.me/radar?id=6409498411401216){:new_window}.

1. Com as configurações do destino do seu app abertas no Xcode, navegue para a guia **Fases de construção**.
2. Clique no ícone `+` e selecione **Nova fase do script de execução**.
3. Inclua o comando `/usr/local/bin/carthage copy-frameworks` na fase do script de execução.
4. Inclua a estrutura do {{site.data.keyword.conversationshort}} na lista **Arquivos de entrada**:
  ```
  $(SRCROOT) /Carthage/Build/iOS/AssistantV1.framework
  ```
  {: codeblock}

## Incluindo um assistente virtual em seu app
{: #add-a-virtual-assistant-to-your-app}

1. Abra seu  ` ViewController.swift `  em Xcode.
2. Inclua uma instrução de importação para o {{site.data.keyword.conversationshort}}, por exemplo, `import AssistantV1`.
3. Crie uma função vazia chamada `assistantExample` e, em seguida, chame-a por meio de `viewDidLoad`.
4. Inclua o código a seguir para a função `assistantExample`. Certifique-se de atualizar seu nome de usuário, sua senha e seu ID da área de trabalho.

```swift
//
//  ViewController.swift
//  WatsonAssistantExample
//
//  Created by Glenn R. Fisher on 3/1/18.
//  Copyright © 2018 Glenn R. Fisher. Todos os direitos reservados.
//

importar AssistantV1 de importação do UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        assistantExample()
    }

    func assistantExample () func {
        // Assistant credentials
        let username = "your-username-here"
        let password = "your-password-here"
        let workspace = "your-workspace-id-here"

        // instantiate service
        let assistant = Assistant(username: username, password: password, version: "2018-03-01")

        // start a conversation
        assistant.message(workspaceID: workspace) { response in
            print("Conversation ID: \(response.context.conversationID!)")
            print ("Resposta: \ (response.output.text.juntou ())")

            // continue assistant
            print("Request: turn the radio on")
            let input = InputData(text: "turn the radio on")
            let request = MessageRequest(input: input, context: response.context)
            assistant.message(workspaceID: workspace, request: request) { response in
                print("Response: \(response.output.text.joined())")
            }
        }
    }
}
```
{: codeblock}

Ao executar seu app, as mensagens a seguir são exibidas no console:
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hi. It looks like a nice drive today. What would you like me to do?
Request: turn the radio on
Response: Sure thing! Which genre would you prefer? Jazz is my personal favorite..
```
{: screen}

## Usando kits iniciadores
{: #conversation_starterkits}

Com kits do iniciador, é possível alavancar de forma rápida e fácil os recursos do {{site.data.keyword.cloud_notm}}. É possível incluir o {{site.data.keyword.conversationshort}} em qualquer backend do lado do servidor usando os kits do iniciador. O Chatbot for iOS com o Watson Starter Kit ilustra como usar os recursos deep learning do {{site.data.keyword.conversationshort}} incluindo uma interface de idioma natural em seu aplicativo que automatiza as interações com os usuários finais.

1. Selecione o [kit do iniciador](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} com o qual você deseja trabalhar.
2. Crie o projeto com os serviços padrão.
3. Clique em **Incluir recursos > Watson >  {{site.data.keyword.conversationshort}} **.
4. Faça download do projeto clicando em **Fazer download do código**. É possível localizar as credenciais de serviço no arquivo `config/local-dev.json`.

## Próximas etapas
{: #assistant_next}

Ótimo trabalho! Você incluiu um assistente de AI em seu app. Tente uma das opções a seguir para manter o ritmo:

* Efetue check-out do  [ {{site.data.keyword.watson}}  Swift SDK ](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.
* Aproveite todos os recursos que o [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) tem para oferecer.
* Visualize o código-fonte do [app de amostra Simple Chat](https://github.com/watson-developer-cloud/simple-chat-swift){:new_window}, demonstrando o {{site.data.keyword.watson}} Swift SDK no GitHub.
