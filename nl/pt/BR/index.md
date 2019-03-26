---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-31"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Tutorial Introdução
{: #getting_started_swift}

O {{site.data.keyword.cloud}} oferece soluções e serviços para permitir que os desenvolvedores do Swift construam aplicativos que estejam integrados à segurança, à IA e ao valor que seus clientes demandam. Com um amplo portfólio de ofertas e SDKs, é possível usar esses serviços e trazer aplicativos de última geração para o mercado rapidamente. Esta programação Swift explica como incluir serviços em um aplicativo Swift novo ou existente, seja Swift iOS do lado do cliente ou do servidor.
{: shortdesc}

O tutorial a seguir mostra como criar facilmente um app móvel Swift com o {{site.data.keyword.mobileanalytics_full}} usando um kit do iniciador vazio por meio do [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits). Por meio do console, você inclui o serviço {{site.data.keyword.mobileanalytics_short}}, faz download do código, executa o app iOS localmente no Xcode, configura e monitora o app.

## Etapa 1. Requisitos para Desenvolvedores
{: #dev-requirements-swift}

Para iniciar o desenvolvimento do iOS no {{site.data.keyword.cloud_notm}}, certifique-se de atender aos requisitos a seguir.

### Sistema Operacional
{: #swift-os-requirements}

A melhor prática para desenvolver apps Swift é usar o hardware suportado pela MacOS mais recente e testar com as liberações mais recentes do iOS. Inscreva-se para uma conta do [Apple Developer](https://developer.apple.com/) para ativar o teste em um dispositivo físico.

### iOS e Xcode
{: #ios_and_xcode}

- Instale o  [ Xcode 8 + ](https://developer.apple.com/xcode/)  (ou superior).
- Implemente nos [dispositivos iOS 8](https://support.apple.com/downloads/ios) (ou superior).
- Revise as [Diretrizes de envio de armazenamento de app](https://developer.apple.com/app-store/guidelines/) antes de enviar os apps para a Apple.

### SDKs e Gerenciamento de Dependência
{: #swift-sdk-management}

As ferramentas a seguir asseguram que você possa instalar os SDKs nativos para trabalhar com os vários {{site.data.keyword.cloud_notm}} Services. É possível usar o CocoaPods ou o Carthage para gerenciamento de dependências.

* **Usando o CocoaPods** - Instale o [CocoaPods](https://cocoapods.org/) para suportar SDKs
do {{site.data.keyword.cloud_notm}}:
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}

* **Usando o Carthage** - Alguns SDKs também estão disponíveis por meio
dos gerenciadores de dependência [Carthage](https://github.com/Carthage/Carthage)
ou [Swift Package Manager](https://swift.org/package-manager/). Para usar o
Carthage para gerenciamento de dependência, execute as etapas a seguir:

  Instale o  [ Homebrew ](https://brew.sh/)  para ajudar a instalação do Cartago:
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

  Instale o Carthage:
  ```
  brew install carthage
  ```
  {: codeblock}

## Etapa 2. Criando um app iOS Swift customizado
{: #create-ios-app-swift}

1. Efetue login no [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits).
2. Clique em  ** Criar Aplicativo **.
3. Na página [Iniciador vazio](https://cloud.ibm.com/developer/appledevelopment/create-app), é possível usar a configuração padrão ou atualizar os campos, conforme necessário. Assegure-se de que **iOS Swift** seja a linguagem selecionada. Clique em **Criar**.

## Etapa 3. Incluindo o serviço  {{site.data.keyword.cloudant_short_notm}}
{: #resources-swift}

Agora é possível incluir serviços em seu aplicativo Swift. Para este tutorial, inclua o
serviço {{site.data.keyword.cloudant_short_notm}} em seu app Swift, que cria um banco
de dados de documentos `JSON` totalmente gerenciado e distribuído. O Cloudant
potencializa seu app com escalabilidade, alta disponibilidade e durabilidade em uma estrutura leve
que mantém seus dados seguros e em sincronia.

1. Na página Detalhes do app, clique no botão **Incluir recurso**.
2. Selecione  ** Dados ** e clique em  ** Avançar **.
3. Selecione  ** Cloudant ** e clique em  ** Avançar **.
4. Clique em **Criar**.
5. Depois que o serviço for criado, clique nele para iniciá-lo. Nessa nova página, selecione
**Ativar o Cloudant Dashboard** para iniciar a criação de
documentos JSON e de um banco de dados. Como alternativa, isso pode ser feito programaticamente.

## Etapa 4. Fazendo download do código e configurando os SDKs do cliente
{: #run-locally-swift}

Para fazer download do código, clique em **Fazer download do código** em `Apps` > `Seu app`. O código transferido por download vem com o [SDK do SwiftCloudant](https://github.com/cloudant/swift-cloudant) incluído, bem como
algum código básico de inicialização. Os SDKs do cliente estão disponíveis no CocoaPods e no Swift
Package Manager. Essa solução usa CocoaPods.

1. Extraia o código transferido por download. Em seguida, usando um terminal, navegue para a pasta extraída.
  ```
  cd <Name of Project>
  ```

2. A pasta inclui um podfile com as dependências necessárias. Execute o comando a seguir para instalar as dependências (Client SDKs):
  ```
  pod install
  ```
  {: codeblock}

## Etapa 5. Configurar o app para usar seu novo banco de dados
{: #config-db-swift}

1. Abra o nome do arquivo que termina em `.xcworkspace` com o Xcode e navegue
até `ViewController.swift`. ` SwiftCloudant `  é o núcleo do Cloudant SDK principal. `CouchDBClient` é uma classe de `SwiftCloudant` e inicializada
no `ViewController.swift`.

  Sempre abra a nova área de trabalho Xcode, em vez do arquivo de projeto Xcode
original: `MyApp.xcworkspace`.
  {: tip}

2. O código de inicialização do banco de dados já está incluído conforme mostrado no
fragmento a seguir:
  ```swift
  /* Read the Cloudant credentials and intialize database connection */
  if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"), let dictionary = NSDictionary(contentsOfFile: contents) {
            let url = URL (string: dictionary [ "cloudantUrl" ] como! String)
            let client = CouchDBClient(url:url!,
                                       username:dictionary["cloudantUsername"] as? String,
                                       password:dictionary["cloudantPassword"] as? String)
        }
  ```
  {: codeblock}

  As credenciais de serviço fazem parte do arquivo `BMSCredentials.plist`.
  {: note}

## Etapa 6. Construir suas operações de banco de dados
{: #build_ops-swift}

Agora que você tem uma conexão com o banco de dados em funcionamento e um SDK configurado,
é possível começar a construir as [operações de
criação, leitura, atualização e destruição](./data/cloudant.html#basic-operations) básicas para seu caso de uso específico.

## Próximas etapas
{: #next-swift}

### Incluindo mais serviços
{: #moreresources-swift}

É possível incluir mais serviços em seu app iOS diretamente do console da web, como os serviços comumente usados a seguir:

* [Incluindo o serviço de notificações de push](/docs/services/mobilepush/index.html)
* [Incluindo autenticação do usuário com o ID do app](/docs/services/appid/index.html)

### Usando o {{site.data.keyword.cloud_notm}} Developer tools
{: #devtools-swift}

Também é possível aprender a desenvolver apps Swift usando as [ferramentas do {{site.data.keyword.cloud_notm}} Developer](/docs/cli/index.html),
que oferecem uma abordagem de linha de comandos para criar, desenvolver e implementar aplicativos da
web, de dispositivos móveis e de microsserviço completos.
