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

# Incluindo Aprendizagem de Máquina em um Aplicativo
{: #overview}

Com o [Core ML](https://developer.apple.com/documentation/coreml){:new_window}, é possível integrar uma ampla variedade de tipos de modelo de aprendizado de máquina em seu app. Além de suportar deep learning extensivo com mais de 30 tipos de camada, ele também suporta modelos padrão, como combinações de árvores, SVMs e modelos lineares generalizados. Em vez de enviar dados remotamente para serem analisados, o Core ML utiliza tecnologias de baixo nível, como Metal e Accelerate, para aproveitar continuamente a CPU e a GPU e fornecer máximo desempenho e eficiência.

## Antes de começar
{: #before-you-begin}

Para usar o Core ML e o Watson Visualization, são necessários os seguintes componentes:

  * iOS 11.0 +
  * Xcode 9.0 +
  * Swift 4.0 +
  * Cartago

Para instalar o Carthage, use os seguintes comandos `brew`:
```
$ brew update $ brew install carthage
```
{: codeblock}

## Etapa 1. Treinando um modelo com  {{site.data.keyword.watson}}  {{site.data.keyword.visualrecognitionshort}}
{: #training-your-model}

Se um modelo atual não existir, o primeiro modelo localizado remotamente ou qualquer um que exista localmente será usado. As instruções de gif e de acompanhamento a seguir mostram como vincular seu serviço ao Watson Studio e treinar seu modelo.

![Core ML Model Walkthrough](images/CoreMLWalkthrough.gif)

### Configurando o serviço por meio do painel do App Core ML

1. Ative a Visual Recognition Tool no painel do Starter Kit selecionando **Ativar ferramenta**.
2. Comece a criar seu modelo selecionando **Criar modelo**.
2. Se um projeto ainda não estiver associado à instância do Visual Recognition criada, um projeto será criado. Caso contrário, vá para a seção **Criar modelo**.
3. Nomeie seu projeto e clique em  ** Criar **.
  **Dica**: se nenhum armazenamento estiver definido, clique em atualizar.

### Ligando um Serviço a um Projeto

1. Após a criação do projeto, o painel do projeto é exibido.
2. Acesse a guia de configurações, role para baixo para **Serviços associados**, selecione **Incluir serviço** -> **Watson**.
3. Na página de serviços do Watson, selecione **Visual Recognition**.
4. Selecione a guia **Existente** na página de informações de serviço e, em seguida, selecione a instância de serviço no painel.
5. Agora que seu serviço está ligado, é possível iniciar a criação de seu modelo selecionando **Ativos** no painel Projeto e, em seguida, clicando em **Incluir o modelo Visual Recognition**.

### Criando um Modelo

1. Na ferramenta de criação de modelo, modifique o nome do classificador, se desejar. Por padrão, o aplicativo usa o primeiro disponível, a menos que especificado. Se quiser usar um modelo específico, certifique-se de modificar o campo `defaultClassifierID` no Controlador de visualização principal.

2. Na barra lateral, faça upload dos cursos de treinamento de modelo em arquivos .zip. Em seguida, selecione cada conjunto de dados e inclua-os em seu modelo por meio do menu suspenso. Sinta-se à vontade para incluir mais classes que usam seus próprios conjuntos de imagens para aprimorar o classificador!

![Adding Classes](images/add_classes.png)

3. Selecione **Modelo de treino** e, em seguida, aguarde até que o modelo seja totalmente treinado.

Você está pronto! Agora, você está pronto para fazer download do modelo Core ML e integrá-lo a seu app usando o [{{site.data.keyword.watson}} Developer Cloud Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.

## Etapa 2. Fazendo Download e Construindo Dependências
{: #installing-dependencies}

1. Usando seu editor de texto favorito, crie um novo arquivo denominado **Cartfile** no diretório-raiz de seu projeto no qual o arquivo `.xcodeproj` está localizado.

2. Inclua uma linha para especificar o {{site.data.keyword.watson}} Developer Cloud Swift SDK como uma dependência, por exemplo:

  ```
  github "Watson-developer-cloud/swift-sdk"
  ```
  {: pre}

  Para um app de produção, talvez você também queira especificar um requisito de versão específico para evitar mudanças inesperadas de novas liberações do SDK.
  {: tip}

3. Use um terminal para navegar para o diretório-raiz de seu projeto e executar o Carthage:

  ```
  Atualização de Cartago $-- plataforma iOS
  ```
  {: pre}

  O {{site.data.keyword.watson}} Developer Cloud Swift SDK é, então, transferido por download e sua estrutura é construída na pasta `Carthage/Build/iOS` de seu projeto.

## Etapa 3. Incluindo classificação de imagem em seu app
{: #adding-image-classification}

```Swift
//
//  ViewController.swift
//  CoreMLImageClassificationExample
//
//  Created by Aaron Liberatore on 3/1/18.
//  Copyright © 2018 Aaron Liberatore. Todos os direitos reservados.
//

importar o VisualRecognitionV3 de importação do UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        initializeCoreMLExample()
    }

    func failureHandler (_ erro: Erro) {
        print(error)
    }

    func initializeCoreMLExample() {
        // Configuração

        // Your Watson Visual Recognition service api key
        let apiKey = "your-apiKey-here"

        // Name of the classifier to use
        let classifierID = "your-classifier-here"

        // Minimum confidence threshold for image recognition
        let threshold = 0.5

        // Initialize Service
        let visualRecognition = VisualRecognition(apiKey: apiKey, version: "03-01-2018")

        // Update or download your model
        visualRecognition.updateLocalModel(classifierID: classifierID,
                                           failure: failureHandler) {

            // Classify your image using your model                                         
            visualRecognition.classifyWithLocalModel(image: image,
                                                     classifierIDs: [classifierID],
                                                     threshold: threshold,
                                                     failure: self.failureHandler) { classifiedImages in

                 imprimir (classifiedImages)
             }            
        }
    }
}
```
{: codeblock}

## Etapa 4. Usando kits iniciadores
{: #coreml_starterkits}

Com kits do iniciador, é possível alavancar de forma rápida e fácil os recursos do {{site.data.keyword.cloud_notm}} e do Core ML. É possível incluir o {{site.data.keyword.visualrecognitionshort}} em qualquer app iOS do cliente ou backend do lado do servidor usando os kits do iniciador. O Custom Vision Model for Core ML with {{site.data.keyword.watson}} Starter Kit ilustra como criar um modelo {{site.data.keyword.visualrecognitionshort}} customizado e instanciá-lo como um modelo Core ML que pode ser executado localmente em seu dispositivo.

Para incluir o {{site.data.keyword.visualrecognitionshort}} em um kit do iniciador, conclua as etapas a seguir:

1. Selecione o [kit do iniciador](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} com o qual você deseja trabalhar.
2. Crie o projeto com os serviços padrão.
3. Clique em **Incluir recursos > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Faça download do projeto clicando em **Fazer download do código**. Para projetos iOS, as credenciais são inseridas no arquivo `BMSCredentials.plist` nos campos-chave correspondentes. Para projetos Swift do lado do servidor, é possível localizar essas credenciais no arquivo `config/local-dev.json`.

## Próximas etapas
{: #coreml_next}

Agora é possível analisar imagens usando modelos Core ML gerados customizados. Tente uma das opções a seguir para manter o ritmo:

* Inclua a lógica de nuvem. Inicie com  [ desenvolvendo apps serverless ](/docs/swift/backend/functions.html).
* Aproveite todos os recursos que o {{site.data.keyword.visualrecognitionshort}} tem para oferecer. Consulte a  [ documentação ](/docs/services/visual-recognition/index.html)  para obter mais detalhes.
