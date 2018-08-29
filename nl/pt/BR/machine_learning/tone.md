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

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

O serviço IBM Watson Tone Analyzer permite que o seu app entenda as emoções e os sinais no texto. É possível usar o serviço para entender melhor as conversas de seu usuário ou ajudar os usuários a entenderem como sua comunicação escrita é percebida.

## Como Funciona
{: ##how-it-works}

1. Seu app escolhe uma seleção de texto para análise (por exemplo, uma mensagem de texto ou um feed do Twitter).
2. Seu app envia o texto para o serviço {{site.data.keyword.toneanalyzershort}} usando o Watson Swift SDK.
3. O serviço analisa o texto usando a análise linguística para identificar emoções e sinais.
4. A análise do serviço é retornada para seu app por meio do Watson Swift SDK.

## Antes de começar
{: ###before-you-begin}

Primeiro, verifique se os pré-requisitos a seguir estão prontos para execução:
<ul>
  <li>iOS 8.0 +</li>
  <li>Xcode 9.0 +</li>
  <li>Swift 3.2 + ou Swift 4.0 +</li>
  <li>Cartago</li>
</ul>

Recomenda-se usar o [Carthage](https://github.com/Carthage/Carthage) para gerenciar dependências e construir o Watson Swift SDK para seu aplicativo. Se você for novo no Carthage, poderá instalá-lo com [Homebrew](http://brew.sh/):

```bash
$ brew update $ brew install carthage
```
{: codeblock}

## Etapa 1. Criando uma instância do Tone Analyzer
{: #create-and-configure-an-instance-of-tone-analyzer}

Provisem uma instância do serviço  {{site.data.keyword.toneanalyzershort}} :

1. No catálogo do {{site.data.keyword.cloud_notm}}, selecione {{site.data.keyword.toneanalyzershort}}. A tela de configuração de serviço é aberta.
2. Dê um nome à sua instância de serviço ou use o nome de pré-configuração.
3. Selecione um app no menu Conectar se quiser ligar sua instância a um app.
4. Selecione um plano de precificação e clique em **Criar**.
5. Selecione a guia **Credenciais** para visualizar as suas credenciais de serviço. Esses valores são usados para se conectar ao serviço por meio de seu app.

## Etapa 2. Fazendo Download e Construindo Dependências
{: ###download-and-build-dependencies}

Usando seu editor de texto favorito, crie um novo arquivo chamado `Cartfile` no diretório-raiz de seu projeto (no qual o arquivo `.xcodeproj` está localizado). Em seguida, inclua uma linha para especificar o SDK do Watson Swift como uma dependência:

  ```
  github "Watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

Para um app de produção, talvez você também queira especificar um [requisito de versão](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) específico para evitar mudanças inesperadas de novas liberações do Watson Swift SDK.

Com o `Cartfile` em vigor, agora é possível fazer download e construir as dependências. Use um terminal para navegar para o diretório raiz de seu projeto e, em seguida, execute o Carthage:
  
  ```bash
  Atualização de Cartago $-- plataforma iOS
  ```
  {: codeblock}

O Carthage faz download do SDK do Watson Swift e constrói as suas estruturas na pasta `Carthage/Build/iOS` de seu projeto.

## Etapa 3. Incluindo estruturas em seu app
{: ###add-frameworks-to-your-app}

### Etapas do Link Tone Analyzer

Agora que as estruturas do Watson Swift SDK estão construídas pelo Carthage, deve-se vincular a estrutura do Tone Analyzer a seu app.

1. Abra seu app no Xcode e, em seguida, selecione seu projeto para abrir suas configurações.
2. Selecione o destino do app e, em seguida, abra a **guia Geral**.
3. Role para baixo para a seção "Estruturas e bibliotecas vinculadas" e clique no ícone `+`.
4. Na janela que aparece, escolha **Incluir outro...** e navegue para o diretório `Carthage/Build/iOS`. Selecione **ToneAnalyzerV3.framework** para vinculá-lo a seu app.

### Copiar etapas do Tone Analyzer

Além de _vincular_ a estrutura do Tone Analyzer, deve-se também _copiá-la_ para o app para torná-la acessível no tempo de execução. O Xcode tem várias maneiras diferentes de copiar ou integrar uma estrutura, mas é possível usar um script do Carthage para evitar um determinado [bug de envio da App Store](http://www.openradar.me/radar?id=6409498411401216).

1. Com as configurações do destino do seu app abertas no Xcode, navegue para a guia **Fases de construção**.
2. Clique no ícone `+` e, em seguida, selecione **Nova fase do script de execução**.
3. Inclua o comando a seguir na fase de script de execução: `/usr/local/bin/carthage copy-frameworks`.
4. Inclua a estrutura do Tone Analyzer na lista "Arquivos de entrada": `$(SRCROOT)/Carthage/Build/iOS/ToneAnalyzerV3.framework`.

Agora você está pronto para começar a trabalhar com o SDK do Watson Swift em seu app.

## Etapa 4. Analisando texto em seu app
{: #analyze-text-in-your-app}

1. Abra o arquivo  ` ViewController.swift `  em Xcode.
2. Inclua uma instrução de importação para o Tone Analyzer:
    ```swift
    import ToneAnalyzerV3
    ```
    {: codeblock}

3. Crie uma função vazia chamada `toneAnalyzerExample` e, em seguida, chame-a por meio de `viewDidLoad`.
4. Inclua o código a seguir na função `toneAnalyzerExample`. Certifique-se de atualizar o nome de usuário e a senha do serviço.
  ```swift
  importar UIKit de importação ToneAnalyzerV3

  class ViewController: UIViewController {

      override func viewDidLoad() {
          super.viewDidLoad()
          toneAnalyzerExample()
      }

      func toneAnalyzerExample () {

          // instantiate service
          let toneAnalyzer = ToneAnalyzer(
              username: "your-username-here",
              password: "your-password-here",
              version: "yyyy-mm-dd"
          )

          // text to analyze
          let review = """
              I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
          """

          // analyze text
          let toneInput = ToneInput(text: review)
          let failure = { (error: Error) in print(error) }
          toneAnalyzer.tone(toneInput: toneInput, contentType: "application/json", failure: failure) { analysis in
              for tone in analysis.documentTone.tones! {
                  print("\(tone.toneName): \(tone.score)")
              }
          }
      }
  }
  ```
  {: codeblock}

Ao executar o app, você vê a seguinte análise no console:
```
Sadness: 0.575803
Tentative: 0.867377
```
{: screen}

## Usando kits iniciadores
{: #tone_starterkits}

Os [Starter Kits](https://console.bluemix.net/developer/appledevelopment/starter-kits) são uma das maneiras mais rápidas de alavancar os recursos do {{site.data.keyword.cloud_notm}}. É possível usar o serviço {{site.data.keyword.toneanalyzershort}} selecionando o kit do iniciador **Tone Analyzer for iOS with Watson**. Esse serviço utiliza recursos deep learning para avaliar passagens de texto. O aplicativo Tone Analyzer identifica o sinal do locutor (feliz, triste, confiante e mais) à medida que se relaciona a várias categorias.

Para iniciar com esse kit do iniciador:

1. Selecione o kit iniciador localizado  [ aqui ](https://console.bluemix.net/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson).
2. Crie o projeto com os serviços padrão.
3. Faça download do projeto clicando em **Fazer download do código**. As credenciais de serviço são injetadas no arquivo `BMSCredentials.plist` nos campos-chaves correspondentes.

## Próximas etapas
{: #tone_next}

Ótimo trabalho! O {{site.data.keyword.toneanalyzershort}} agora está incluído em seu app. Tente uma das opções a seguir para manter o ritmo:

* Visualize o [SDK do Watson Swift no GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* Para obter mais informações, consulte [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/).

