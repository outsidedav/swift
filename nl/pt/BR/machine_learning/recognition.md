---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: swift visual recognition, train swift, cocoapods swift, swift sdk install, starter kit watson swift, image swift classify, machine learning swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

O serviço {{site.data.keyword.visualrecognitionfull}} permite que seu app identifique, classifique e treine de forma rápida e precisa o conteúdo visual usando aprendizado de máquina. O serviço pode ajudar a classificar virtualmente qualquer conteúdo visual, treinar seu próprio modelo customizado em minutos e detectar rostos.

## Como Funciona
{: #how-it-works-recognition}

1. Seu app escolhe uma seleção de imagens para análise.
2. Seu app envia a imagem para o serviço {{site.data.keyword.visualrecognitionshort}} usando o Watson Swift SDK.
3. O serviço analisa a imagem usando a análise de classificação para identificar cenas, objetos, rostos e mais.
4. A análise do serviço é retornada para seu app pelo Watson Swift SDK.

## Antes de começar
{: #prereqs-recognition}

Assegure-se de que você tenha os pré-requisitos a seguir:

* iOS 10.0 +
* Xcode 9.3 +
* Swift 4.1 +
* CocoaPods, Carthage, ou Swift Package Manager

É possível instalar o [SDK do Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") usando o [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), o [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ou o [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"). Usando CocoaPods (https://cocoapods.org/) para gerenciar dependências, você obtém somente as estruturas de que precisa, ao invés do SDK Watson Swift inteiro. Se você for novo no CocoaPods, poderá instalá-lo facilmente:
```console
sudo gem install cocoapods
```
{: codeblock}

## Etapa 1. Criando uma instância do Visual Recognition
{: #create-instance-recognition}

Provisem uma instância do serviço  {{site.data.keyword.visualrecognitionshort}} :

1. No catálogo do {{site.data.keyword.cloud_notm}}, selecione **{{site.data.keyword.visualrecognitionshort}}**. A tela de configuração de serviço é aberta.
2. Dê um nome à sua instância de serviço ou use o nome de pré-configuração.
3. Selecione um app no menu **Conectar** se quiser ligar sua instância a um app.
4. Selecione um plano de precificação e clique em **Criar**.
5. Selecione a guia **Credenciais** para visualizar as suas credenciais de serviço. Esses valores são usados para se conectar ao serviço por meio de seu app.

## Etapa 2. Fazendo Download e Construindo Dependências
{: #download-depend-recognition}

Usando seu editor de texto favorito, crie um novo `Podfile` no diretório raiz do seu projeto (onde o `.xcodeproj` está localizado) executando `pod init`. Em seguida, inclua uma linha para especificar a estrutura do {{site.data.keyword.visualrecognitionshort}} do SDK Watson Swift como uma dependência:

```pod
use_estruturas!

target 'MyApp' do pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

Para um app de produção, você também pode desejar especificar um [requisito de versão](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} específico![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") para evitar mudanças inesperadas de novas liberações do SDK do Watson Swift.

Com seu `Podfile` no local, agora será possível fazer download das dependências. Use um terminal para navegar para o diretório raiz do seu projeto, em seguida, execute o CocoaPods:

```console
pod install
```
{: codeblock}

O Cocoapods faz downloads da estrutura {{site.data.keyword.visualrecognitionshort}} e o constrói na pasta `Pods/` do seu projeto.

Para evitar falhas de construção do Pod, abra o arquivo que termina com `.xcworkspace` em vez de `.xcodeproj` quando abrir o projeto no Xcode.
{: tip}

## Etapa 3. Analisando imagens em seu app
{: #analyze-images-recognition}

As amostras a seguir ajudam você a incluir os recursos do {{site.data.keyword.visualrecognitionshort}}
em seu aplicativo, geralmente no `ViewController.swift`. Usando os exemplos a seguir,
você pode estender as chamadas do Visual Recognition para o seu caso de uso.

1. Inclua uma instrução de importação para o Visual Recognition:
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. Passe a chave API e a versão (é possível usar a data de hoje) para inicializar o SDK:
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  É possível verificar a [documentação do parâmetro de versão](https://cloud.ibm.com/apidocs/visual-recognition#versioning){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ou usar a data em que o serviço {site.data.keyword.conversationshort}} foi criado.
  {: tip}

3. Inclua o código a seguir para classificar uma imagem:
  ```swift
  let url = "your-image-url"
  visualRecognition.classify(url: url) { response, error in
      if let error = error {
          print(error)
    }
      guard let classifiedImages = response?.result else {
          print("Failed to classify the image") return } print(classifiedImages) }
  ```
  {: codeblock}

Há diversos métodos de classificação que são suportados pela estrutura do Visual Recognition. Explore a [Documentação do Visual Recognition do](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} SDK do Watson ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") para descobrir qual melhor se adequa ao seu aplicativo.
{: tip}

## Usando kits iniciadores
{: #recognition_starterkits}

Os [kits do iniciador](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") são uma das maneiras mais rápidas de usar os recursos do {{site.data.keyword.cloud_notm}}. É possível usar o serviço {{site.data.keyword.visualrecognitionshort}} selecionando o kit do iniciador **Visual Recognition for iOS with Watson**. Esse serviço avalia e classifica suas imagens. Faça upload de imagens novas ou existentes de seu dispositivo móvel e o app Visual Recognition identificará e classificará rapidamente o conteúdo da imagem.

Para iniciar:
1. Selecione o kit do iniciador localizado [aqui](https://cloud.ibm.com/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").
2. Crie o projeto com os serviços padrão.
3. Faça download do projeto clicando em **Fazer download do código**. As credenciais de serviço são injetadas no arquivo `BMSCredentials.plist` nos campos-chaves correspondentes.

## Próximas etapas
{: #recognition_next notoc}

Ótimo trabalho! O Visual Recognition está agora disponível para seu app. Tente uma das opções a seguir para manter o ritmo:
* Verifique o [SDK do Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") e explore os outros serviços suportados do Watson.
* Para obter mais informações, consulte o [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").
