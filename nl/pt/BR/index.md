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
{:tip: .tip}

# Tutorial Introdução
{: #set_up}

O {{site.data.keyword.cloud}} oferece soluções e serviços para fornecer a desenvolvedores e aplicativos do Swift a segurança, a AI e o valor exigido por seus clientes. Com um amplo portfólio de ofertas e SDKs, é possível alavancar esses serviços e trazer aplicativos de ponta para o mercado rapidamente. O guia de programação do Swift ensina como incluir serviços em um aplicativo Swift novo ou existente, seja ele um cliente iOS ou um Swift do lado do servidor.
{: shortdesc}

O tutorial a seguir é um ponto de entrada para mostrar como criar facilmente um app móvel Swift com o {{site.data.keyword.mobileanalytics_full}} usando um Starter Kit vazio do [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits). Por meio do console, você inclui o serviço {{site.data.keyword.mobileanalytics_short}}, faz download do código, executa o app iOS localmente no Xcode, configura e monitora o app.

## Etapa 1. Requisitos para Desenvolvedores
{: #dev-requirements}

Para iniciar o desenvolvimento do iOS no {{site.data.keyword.cloud_notm}}, certifique-se de atender aos requisitos a seguir.

### Sistema Operacional

É uma boa prática desenvolver apps do Swift usando o hardware suportado mais recente do MacOS e testando com as liberações mais recentes do iOS. Inscreva-se para uma conta do [Apple Developer](https://developer.apple.com/) para ativar o teste em um dispositivo físico.

### iOS e Xcode
{: #ios_and_xcode}

- Instale o  [ Xcode 8 + ](https://developer.apple.com/xcode/)  (ou superior).
- Implemente nos [dispositivos iOS 8](https://support.apple.com/downloads/ios) (ou superior).
- Revise as [Diretrizes de envio do App Store](https://developer.apple.com/app-store/guidelines/) antes de enviar apps para a Apple.

### SDKs e Gerenciamento de Dependência

As ferramentas a seguir asseguram que você possa instalar os SDKs nativos para trabalhar com os vários {{site.data.keyword.cloud_notm}} Services.

1. Instale o  [ CocoaPods ](https://cocoapods.org/)  para suportar  {{site.data.keyword.cloud_notm}}  SDKs:
  ```
  sudo jóias instalem cocoapods
  ```
  {: codeblock}
  
2. Instale o  [ Homebrew ](https://brew.sh/)  para ajudar a instalação do Cartago:
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

3. Instale o [Carthage](https://github.com/Carthage/Carthage) para suportar SDKs do Watson:
  ```
  cartilagem de instalação em cerveja
  ```
  {: codeblock}

## Etapa 2. Criando um app iOS Swift customizado
{: #create-ios-app}

1. Efetue login no [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits).
2. Clique em  ** Criar Aplicativo **.
3. Na página [Iniciador vazio](https://console.bluemix.net/developer/appledevelopment/create-app), é possível usar a configuração padrão ou atualizar os campos, conforme necessário. Assegure-se de que **iOS Swift** seja a linguagem selecionada. Clique em **Criar**.

## Etapa 3. Incluindo o serviço  {{site.data.keyword.mobileanalytics_short}}
{: #adding-services}

1. Na página Detalhes do app, clique no botão **Incluir recurso**.
2. Selecione  ** Remoto ** e clique em  ** Avançar **.
3. Selecione  ** {{site.data.keyword.mobileanalytics_short}} ** e clique em  ** Avançar **.
4. Clique em **Criar**.

## Etapa 4. Fazendo download do código e configurando os SDKs do cliente
{: #run-locally}

Para fazer download do código, clique em **Fazer download do código** em `Apps` > `Seu app`. O código transferido por download é fornecido com Client SDKs do **{{site.data.keyword.mobileanalytics_short}}** incluídos. Os Client SDKs estão disponíveis no CocoaPods e Carthage. Para esta solução, use CocoaPods.

1. Descompacte o arquivo ZIP do download. Em seguida, usando um terminal, navegue até a pasta descompactada.
  ```
  cd <Name of Project>
  ```
2. A pasta inclui um podfile com as dependências necessárias. Execute o comando a seguir para instalar as dependências (Client SDKs):
  ```
  instalação do pod
  ```
  {: codeblock}

## Etapa 5. Configurando o app para usar o {{site.data.keyword.mobileanalytics_short}}
{: #configure-analytics}

1. Abra `.xcworkspace` no Xcode e navegue para `AppDelegate.swift`.
  **Nota:** assegure-se de que você sempre abra a nova área de trabalho do Xcode, em vez do arquivo de projeto Xcode original: `MyApp.xcworkspace`.
   ![Abrir Xcode](images/Xcode.png)

  `BMSCore` é o Core SDK e é base para os Mobile Client SDKs. `BMSClient` é uma classe de BMSCore e inicializada no AppDelegate.swift. Junto com BMSCore, o SDK do {{site.data.keyword.mobileanalytics_short}} já foi importado para o projeto.
  
2. O código de inicialização de analítica já está incluído, conforme mostrado no fragmento a seguir:
  ```
  // Analytics client SDK is configured to record lifecycle events.
         	Analytics.initialize(appName:dictionary["appName"] as? String,
        			     apiKey: dictionary["analyticsApiKey"] as? String,
        	        	     deviceEvents: .lifecycle)

        	// Enable Logger (disabled by default) and set level to ERROR (DEBUG by default).
        	Logger.isLogStorageEnabled = true
        	Logger.logLevelFilter = .error
  ```
  {: codeblock}

  **Nota:** as credenciais de serviço fazem parte do arquivo `BMSCredentials.plist`.

3. Reunindo analítica de uso usando  ` logger `. Navegue para o arquivo `ViewController.swift` para ver o código a seguir.
  ```
   func didBecomeActive (_ notificação: Notificação) {
       Analytics.send()
       Logger.send()
    }
  ```
  {: codeblock}

   Para recursos avançados de Analítica e criação de log, consulte [Reunindo analítica de uso](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics) e [criação de log](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger).
   {:tip}

## Etapa 6. Monitorando o aplicativo com  {{site.data.keyword.mobileanalytics_short}}
O serviço {{site.data.keyword.mobileanalytics_short}} fornece uso essencial do aplicativo e insights de desempenho para desenvolvedores de aplicativos móveis e proprietários de aplicativos. Usando o {{site.data.keyword.mobileanalytics_short}}, os proprietários do aplicativo e os desenvolvedores podem entender o que está acontecendo no lado do usuário. Eles podem usar esse insight para construir aplicativos melhores que sejam relevantes para os usuários e que sobressaiam no verdadeiro mar de aplicativos móveis.

O serviço inclui o
{{site.data.keyword.mobileanalytics_short}} Console no qual
os desenvolvedores e proprietários de aplicativo podem monitorar o
desempenho do aplicativo móvel, ver estatísticas de uso e
procurar logs do dispositivo.

1. Abra o serviço `{{site.data.keyword.mobileanalytics_short}}` por meio do app móvel criado ou clique nos três pontos verticais próximos ao serviço e selecione `Open Dashboard`.
2. É possível ver Usuários em TEMPO REAL, Sessões e outros Dados de app desativando o `Demo Mode`. É possível filtrar as informações de analítica pelos critérios a seguir:
    * Data.
    * Aplicação.
    * Sistema Operacional.
    * Versão do app.
         ![{{site.data.keyword.mobileanalytics_short}}](images/mobile_analytics.png)
3. [Clique aqui](https://console.bluemix.net/docs/services/mobileanalytics/app-monitoring.html#monitoringapps) para configurar alertas, Monitorar travamentos do app e Monitorar solicitações de rede.

## Próximas etapas
{: #next-steps}

### Incluindo mais serviços
É possível incluir mais serviços em seu app iOS diretamente do console da web, como os serviços comumente usados a seguir:

* [ Incluindo o Serviço de Notificações Push ](/push/push_notifications.html)
* [ Incluindo Autenticação do Usuário com o ID do Aplicativo ](/authenticate/app_id.html)

### Utilizando as  {{site.data.keyword.cloud_notm}}  Ferramentas de Desenvolvedor
Também é possível aprender como desenvolver apps Swift usando as [ferramentas de desenvolvedor do {{site.data.keyword.cloud_notm}}](../cli/index.html), que oferecem uma abordagem de linha de comandos para criar, desenvolver e implementar aplicativos da web, móveis e de microsserviço de ponta a ponta.

