---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: coreml swift, core ml swift, watson swift core, create model swift, image classification swift, version parameter swift, swift coreml watson

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando o Core ML com o Watson
{: #swift-coreml}

Com o [Core ML](https://developer.apple.com/documentation/coreml){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), é possível integrar uma ampla variedade de tipos de modelo de aprendizado de máquina em seu app. Além de suportar deep learning extensivo com mais de 30 tipos de camada, ele também suporta modelos padrão, como combinações de árvores, SVMs e modelos lineares generalizados. Em vez de enviar remotamente dados para análise,
o Core ML usa tecnologias de baixo nível, tais como o Metal e o Accelerate, para aproveitar facilmente a CPU
e a GPU a fim de fornecer desempenho e eficiência máximos.

É possível incluir o Core ML e o Watson Visual Recognition em um aplicativo Swift usando as
etapas a seguir para treinar e criar um modelo, fazer download, construir dependências e incluir
classificação de imagem.
{: shortdesc}

## Antes de começar
{: #prereqs-coreml}

Para usar o Core ML e o Watson Visual Recognition com o Swift, são necessários os componentes
a seguir:

* iOS 11.0 +
* Xcode 9.3 +
* Swift 4.1 +
* CocoaPods, Carthage, ou Swift Package Manager

É possível instalar o [SDK do Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") usando o [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), o [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ou o [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"). Usando o CocoaPods para gerenciar dependências, você obterá apenas as estruturas necessárias, em oposição a todo o SDK do Watson Swift. Se você for novo no CocoaPods, poderá instalá-lo facilmente:

```console
sudo gem install cocoapods
```
{: codeblock}

## Etapa 1. Treinando um modelo com  {{site.data.keyword.watson}}  {{site.data.keyword.visualrecognitionshort}}
{: #train-module-coreml}

Se um modelo atual não existir, o primeiro modelo que for descoberto remotamente ou qualquer
um que exista localmente será usado. O gif a seguir e as instruções de acompanhamento mostram como
vincular seu serviço ao Watson Studio e treinar seu modelo.

![Core ML Model Walkthrough](images/CoreMLWalkthrough.gif)

### Configurando o serviço por meio do painel do App Core ML
{: #service-coreml}

1. Inicie a Visual Recognition Tool no painel do seu Kit do iniciador selecionando a **ferramenta Ativar**.
2. Comece a criar seu modelo selecionando **Criar modelo**.
2. Se um projeto ainda não estiver associado à instância do Visual Recognition criada, um projeto será criado. Caso contrário, vá para a seção **Criar modelo**.
3. Nomeie seu projeto e clique em  ** Criar **.
  
  Se nenhum armazenamento estiver definido, clique em Atualizar.
  {: tip}

### Ligando um Serviço a um Projeto
{: #bind-service-coreml}

1. Após a criação do projeto, o painel do projeto é exibido.
2. Acesse a guia de configurações, role para baixo para **Serviços associados**, selecione **Incluir serviço** -> **Watson**.
3. Na página de serviços do Watson, selecione **Visual Recognition**.
4. Selecione a guia **Existente** na página de informações de serviço e,
em seguida, selecione sua instância de serviço no painel.
5. Agora que seu serviço está ligado, é possível iniciar a criação de seu modelo selecionando **Ativos** no painel Projeto e, em seguida, clicando em **Incluir o modelo Visual Recognition**.

### Criando um Modelo
{: #create-model-coreml}

1. Na ferramenta de criação de modelo, é possível atualizar o nome do classificador. Se quiser usar um modelo específico, certifique-se de modificar o campo `defaultClassifierID` no Controlador de visualização principal.

2. Na barra lateral, faça upload dos cursos de treinamento de modelo em arquivos `.zip` compactados. Em seguida, selecione cada conjunto de dados e inclua-os
em seu modelo no menu suspenso. Sinta-se à vontade para incluir mais classes que usam seus próprios conjuntos de imagens para aprimorar o classificador!

![Adding Classes](images/add_classes.png)

3. Selecione **Modelo de treino** e, em seguida, aguarde até que o modelo seja totalmente treinado.

Você está pronto! Agora, você está pronto para fazer download do seu modelo do Core ML e integrá-lo em seu app usando o [SDK do Watson Developer Cloud Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Etapa 2. Fazendo Download e Construindo Dependências
{: #install-depend-coreml}

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

## Etapa 3. Incluindo classificação de imagem em seu app
{: #add-image-coreml}

As amostras a seguir o ajudam a incluir os recursos do {{site.data.keyword.visualrecognitionshort}}
Core ML em seu aplicativo, geralmente no `ViewController.swift`. Usando os exemplos
a seguir, é possível estender as chamadas de modelo local para seu caso de uso.

1. Inclua uma instrução de importação para o Visual Recognition:
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. Passe a chave de API e a versão para inicializar o SDK:
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  Verifique a [documentação do parâmetro de versão](https://cloud.ibm.com/apidocs/visual-recognition#versioning){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ou use a data em que o serviço {site.data.keyword.visualrecognitionshort}} foi criado.
  {: tip}

3. Inclua o código a seguir para fazer download ou atualizar o modelo Core ML local com
seu classificador Watson:
  ```swift
  // Name of the classifier to use
  let classifierID = "your-classifier-ID-here"

  // Minimum confidence threshold for image recognition
        let threshold = 0.5

  // Update or download your model
  visualRecognition.updateLocalModel(classifierID: classifierID) { _, error in
      if let error = error {
          print(error)
      } else {
          print ("modelo atualizado com êxito")
      }                
  }
  ```
  {: codeblock}

4. Inclua o código a seguir para classificar uma imagem usando seu modelo Core ML local:
  ```swift
  let image = UIImage(named: "your-image-filename")
  let classifierID = "your-classifier-ID-here"
  let threshold = 0.5

  // Classify your image using your model
  visualRecognition.classifyWithLocalModel(image: image, classifierIDs: [classifierID], threshold: threshold) { classifiedImages, error in
      if let error = error {
          print(error)
    }
      guard let classifiedImages = classifiedImages? else {
          print("Failed to classify the image") return } print(classifiedImages) }
  ```
  {: codeblock}

5. Explore os outros [Recursos de classificação do Core ML](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") suportados pelo SDK do Watson.

## Etapa 4. Usando kits iniciadores
{: #coreml_starterkits}

Com os kits do iniciador, é possível usar rápida e facilmente os recursos do {{site.data.keyword.cloud_notm}} e do Core ML. É possível incluir o {{site.data.keyword.visualrecognitionshort}} em qualquer app iOS do cliente ou backend do lado do servidor usando os kits do iniciador. O Custom Vision Model for Core ML with {{site.data.keyword.watson}} Starter Kit ilustra como criar um modelo {{site.data.keyword.visualrecognitionshort}} customizado e instanciá-lo como um modelo Core ML que pode ser executado localmente em seu dispositivo.

Para incluir o {{site.data.keyword.visualrecognitionshort}} em um kit do iniciador, conclua as etapas a seguir:

1. Selecione o [kit do iniciador](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") com o qual você deseja trabalhar.
2. Crie o app com os serviços padrão.
3. Clique em **Incluir serviço > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Faça download do projeto clicando em **Fazer download do código**. Para projetos iOS, as credenciais são inseridas no arquivo `BMSCredentials.plist` nos campos-chave correspondentes. Para projetos Swift do lado do servidor, é possível localizar essas credenciais no arquivo `config/local-dev.json`.

## Próximas etapas
{: #coreml_next}

Agora, é possível analisar imagens usando seus próprios modelos Core ML gerados por customização. Tente uma das opções a seguir para manter o ritmo:

* Verifique o [SDK do Swift do {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") e explore os outros serviços suportados do Watson.
* Inclua a lógica de nuvem. Inicie com  [ desenvolvendo apps serverless ](/docs/swift/backend?topic=swift-serverless-dev-swift#serverless-dev-swift).
* Aproveite as vantagens de todos os recursos que o {{site.data.keyword.visualrecognitionshort}} oferece. Consulte a  [ documentação ](/docs/services/visual-recognition?topic=visual-recognition-index#index)  para obter mais detalhes.
