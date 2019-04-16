---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-24"

keywords: watson swift, tone anaylzer swift, cocoapods swift, swift sdk install, starter kit watson

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

O serviço do {{site.data.keyword.ibmwatson}} {{site.data.keyword.toneanalyzershort}} permite que o seu app entenda emoções e sinais no texto. É possível usar o serviço para entender melhor as conversas de seu usuário ou ajudar os usuários a entenderem como sua comunicação escrita é percebida.

## Como Funciona
{: #how-it-works-tone}

1. Seu app escolhe uma seleção de texto para análise (por exemplo, uma mensagem de texto ou um feed do Twitter).
2. Seu app envia o texto para o serviço {{site.data.keyword.toneanalyzershort}} usando o Watson Swift SDK.
3. O serviço analisa o texto usando a análise linguística para identificar emoções e sinais.
4. A análise do serviço é retornada ao seu app por meio do [SDK do Watson Swift](https://github.com/watson-developer-cloud/swift-sdk).

## Antes de começar
{: #prereqs-tone}

Assegure-se de que você tenha os pré-requisitos a seguir:

* S.O. 10.0 +
* Xcode 9.3 +
* Swift 4.1 +
* CocoaPods, Carthage, ou Swift Package Manager

É possível instalar o [SDK do Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") usando o [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), o [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ou o [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"). Usando o CocoaPods para gerenciar dependências, você obterá apenas as estruturas necessárias, em oposição a todo o SDK do Watson Swift. Se você for novo no CocoaPods, poderá instalá-lo facilmente:

```bash
$sudo gem install cocoapods
```
{: codeblock}

## Etapa 1. Criando uma instância do Tone Analyzer
{: #create-instance-tone}

Provisem uma instância do serviço  {{site.data.keyword.toneanalyzershort}} :

1. No catálogo do {{site.data.keyword.cloud_notm}}, selecione {{site.data.keyword.toneanalyzershort}}. A tela de configuração de serviço é aberta.
2. Dê um nome à sua instância de serviço ou use o nome de pré-configuração.
3. Selecione um app no menu Conectar se quiser ligar sua instância a um app.
4. Selecione um plano de precificação e clique em **Criar**.
5. Selecione a guia **Credenciais** para visualizar as suas credenciais de serviço. Esses valores são usados para se conectar ao serviço por meio de seu app.

## Etapa 2. Fazendo Download e Construindo Dependências
{: #download-depend-tone}

Usando seu editor de texto favorito, crie um novo `Podfile` no diretório raiz do seu projeto (onde o `.xcodeproj` está localizado) executando `pod init`. Em seguida, inclua uma linha para especificar a estrutura do {{site.data.keyword.conversationshort}} do SDK Watson Swift como uma dependência:

```pod
use_estruturas!

target 'MyApp' do
    pod 'IBMWatsonToneAnalyzerV3'
```
{: codeblock}

Para um app de produção, você também pode desejar especificar um [requisito de versão](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} específico![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") para evitar mudanças inesperadas de novas liberações do SDK do Watson Swift.

Com seu `Podfile` no local, agora será possível fazer download das dependências. Use um terminal para navegar para o diretório raiz do seu projeto, em seguida, execute o CocoaPods:

```console
pod install
```
{: codeblock}

O Cocoapods faz downloads da estrutura {{site.data.keyword.toneanalyzershort}} e o constrói na pasta `Pods/` do seu projeto.

Para evitar falhas de construção do Pod, abra o arquivo que termina com `.xcworkspace` em vez de `.xcodeproj` quando abrir o projeto no Xcode.
{: tip}

## Etapa 3. Analisando texto em seu app
{: #analyze-text-tone}

As amostras a seguir ajudam você a incluir os recursos do {{site.data.keyword.toneanalyzershort}}
em seu aplicativo, geralmente no `ViewController.swift`. Usando os exemplos a seguir,
é possível estender as chamadas do Tone Analyzer para seu caso de uso.

1. Inclua uma instrução de importação para o Tone Analyzer:
  ```swift
  import ToneAnalyzer
  ```
  {: codeblock}

2. Instancie o serviço Tone Analyzer:
  ```swift
  let toneAnalyzer = ToneAnalyzer(version: "yyyy-mm-dd", apiKey: "your-api-key-here")
  ```
  {: codeblock}

  Verifique a [documentação do parâmetro de versão](https://cloud.ibm.com/apidocs/tone-analyzer#versioning){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ou use a data em que o serviço {site.data.keyword.conversationshort}} foi criado.
  {: tip}

3. Forneça o texto para análise e processe os resultados:
  ```swift
  // Text to analyze
  let review = """
      I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
  """

  // Analyze text
  let input = ToneInput(text: review)
  toneAnalyzer.tone(toneInput: .toneInput(input)) { response, error in
      if let error = error {
          print(error)
          return
      }

      guard let tones = response?.result?.documentTone.tones else {
          print("Failed to analyze the tone input")
          return
      }

      for tone in tones {
          print("\(tone.toneName): \(tone.score)")
              }
  }
  ```
  {: codeblock}

  Ao executar o código, você vê a análise a seguir no console:
  ```
  Sadness: 0.575803
Tentative: 0.867377
  ```
  {: screen}

4. Explore a [documentação do Tone Analyzer](https://watson-developer-cloud.github.io/swift-sdk/services/ToneAnalyzerV3/index.html){: new_window} do SDK do Watson ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") para construir a funcionalidade de seu aplicativo.

## Usando kits iniciadores
{: #tone_starterkits}

Os [kits do iniciador](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") são uma das maneiras mais rápidas de usar os recursos do {{site.data.keyword.cloud_notm}}. É possível usar o serviço {{site.data.keyword.toneanalyzershort}} selecionando o kit do iniciador **Tone Analyzer for iOS with Watson**. Esse serviço utiliza recursos deep learning para avaliar passagens de texto. O aplicativo Tone Analyzer identifica o sinal do locutor (feliz, triste, confiante e mais) à medida que se relaciona a várias categorias.

Para iniciar com esse kit do iniciador:

1. Selecione o kit do iniciador localizado [aqui](https://cloud.ibm.com/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").
2. Crie o projeto com os serviços padrão.
3. Faça download do projeto clicando em **Fazer download do código**. As credenciais de serviço são injetadas no arquivo `BMSCredentials.plist` nos campos-chaves correspondentes.

## Próximas etapas
{: #tone_next notoc}

Ótimo trabalho! O {{site.data.keyword.toneanalyzershort}}  agora está incluído em seu app. Tente uma das opções a seguir para manter o ritmo:

* Visualize o [SDK do Watson Swift no GitHub](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") e explore os outros serviços suportados do Watson.
* Para obter mais informações, consulte o [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").
