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

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

O serviço {{site.data.keyword.visualrecognitionfull}} permite que seu app identifique, classifique e treine de forma rápida e precisa o conteúdo visual usando aprendizado de máquina. O serviço pode ajudar a classificar virtualmente qualquer conteúdo visual, treinar seu próprio modelo customizado em minutos e detectar rostos.

## Como Funciona
{: ##how-it-works}

1. Seu app escolhe uma seleção de imagens para análise.
2. Seu app envia a imagem para o serviço {{site.data.keyword.visualrecognitionshort}} usando o Watson Swift SDK.
3. O serviço analisa a imagem usando a análise de classificação para identificar cenas, objetos, rostos e mais.
4. A análise do serviço é retornada para seu app pelo Watson Swift SDK.

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

## Etapa 1. Criando uma instância do Visual Recognition
{: ###create-and-configure-an-instance-of-visual-recognition}

Provisem uma instância do serviço  {{site.data.keyword.visualrecognitionshort}} :

1. No catálogo do  {{site.data.keyword.cloud_notm}} , selecione  ** {{site.data.keyword.visualrecognitionshort}} **. A tela de configuração de serviço é aberta.
2. Dê um nome à sua instância de serviço ou use o nome de pré-configuração.
3. Selecione um app no menu **Conectar** se quiser ligar sua instância a um app.
4. Selecione um plano de precificação e clique em **Criar**.
5. Selecione a guia **Credenciais** para visualizar as suas credenciais de serviço. Esses valores são usados para se conectar ao serviço por meio de seu app.

## Etapa 2. Fazendo Download e Construindo Dependências
{: ###download-and-build-dependencies}

Usando seu editor de texto favorito, crie um arquivo chamado `Cartfile` no diretório-raiz de seu projeto (no qual o arquivo `.xcodeproj` está localizado). Em seguida, inclua uma linha para especificar o SDK do Watson Swift como uma dependência:
```
github "Watson-developer-cloud/swift-sdk"
```
{: pre}

Para um app de produção, é possível especificar um [requisito de versão](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) específico para evitar mudanças inesperadas de novas liberações do Watson Swift SDK.

Com o `Cartfile` em vigor, agora é possível fazer download e construir as dependências. Use um terminal para navegar para o diretório raiz de seu projeto e, em seguida, execute o Carthage:

```bash
atualização de cartago -- plataforma iOS
```
{: pre}

O Carthage faz download do SDK do Watson Swift e constrói as suas estruturas na pasta `Carthage/Build/iOS` de seu projeto.

## Etapa 3. Incluindo estruturas em seu app
{: ###add-frameworks-to-your-app}

### Etapas de Reconhecimento Visual do Link:

Agora que as estruturas do Watson Swift SDK foram construídas pelo Carthage, deve-se vincular a estrutura do Visual Recognition a seu app.

1. Abra seu app no Xcode e selecione seu projeto para abrir suas configurações.
2. Selecione o destino do app e, em seguida, abra a guia **Geral**.
3. Role para baixo para a seção "Estruturas e bibliotecas vinculadas" e clique no ícone `+`.
4. Na janela que aparece, escolha **Incluir outro...** e navegue para o diretório `Carthage/Build/iOS`. Selecione **VisualRecognitionV3.framework** para vinculá-la a seu app.

### Copie as etapas do Visual Recognition:

Além de _vincular_ a estrutura do Visual Recognition, deve-se também _copiá-la_ para o app para torná-la acessível no tempo de execução. O Xcode tem várias maneiras diferentes de copiar ou integrar uma estrutura, mas é possível usar um script do Carthage para evitar um determinado [bug de envio da App Store](http://www.openradar.me/radar?id=6409498411401216).

1. Com as configurações do destino do seu app abertas no Xcode, navegue para a guia **Fases de construção**.
2. Clique no ícone `+` e, em seguida, selecione **Nova fase do script de execução**.
3. Inclua o comando a seguir na fase de script de execução: `/usr/local/bin/carthage copy-frameworks`.
4. Inclua a estrutura do Visual Recognition na lista **Arquivos de entrada**: `$(SRCROOT)/Carthage/Build/iOS/VisualRecognitionV3.framework`.

Agora você está pronto para começar a trabalhar com o SDK do Watson Swift em seu app.

## Etapa 4. Analisando imagens em seu app
{: #analyze-images-in-your-app}

1. Abra o arquivo  ` ViewController.swift `  em Xcode.

1. Inclua uma instrução de importação para o Visual Recognition:
    ```swift
    import VisualRecognitionV3
    ```
    {: pre}

1. Passe a chave API e a versão (é possível usar a data de hoje) para inicializar o SDK:
    ```swift
    let visualRecognition = VisualRecognition(apiKey: "your-api-key", version: "yyyy-mm-dd")
    ```
    {: pre}

1. Inclua o código a seguir para classificar uma imagem:
    ```swift
    let url = "your-image-url"
    let failure = { (error: Error) in print(error) }
    visualRecognition.classify(url: url, failure: failure) { classifiedImages in
        print(classifiedImages)
    }
    ```
    {: pre}

## Usando kits iniciadores
{: #recognition_starterkits}

Os [Starter Kits](https://console.bluemix.net/developer/appledevelopment/starter-kits) são uma das maneiras mais rápidas de alavancar os recursos do {{site.data.keyword.cloud_notm}}. É possível usar o serviço {{site.data.keyword.visualrecognitionshort}} selecionando o kit do iniciador **Visual Recognition for iOS with Watson**. Esse serviço avalia e classifica suas imagens. Faça upload de imagens novas ou existentes de seu dispositivo móvel e o app Visual Recognition identificará e classificará rapidamente o conteúdo da imagem.

Para iniciar:
1. Selecione o kit iniciador localizado  [ aqui ](https://console.bluemix.net/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson).
2. Crie o projeto com os serviços padrão.
3. Faça download do projeto clicando em **Fazer download do código**. As credenciais de serviço são injetadas no arquivo `BMSCredentials.plist` nos campos-chaves correspondentes.

## Próximas etapas
{: #recognition_next}

Ótimo trabalho! O Visual Recognition está agora disponível para seu app. Tente uma das opções a seguir para manter o ritmo:
* Visualize o [SDK do Watson Swift no GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* Para obter mais informações, consulte [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/).

