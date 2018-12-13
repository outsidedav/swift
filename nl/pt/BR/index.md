---

copyright:
  years: 2018
lastupdated: "2018-11-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Tutorial Introdução
{: #set_up}

O {{site.data.keyword.cloud}} oferece soluções e serviços para permitir que os desenvolvedores do Swift construam aplicativos que estejam integrados à segurança, à IA e ao valor que seus clientes demandam. Com um amplo portfólio de ofertas e SDKs, é possível usar esses serviços e trazer aplicativos de última geração para o mercado rapidamente. Esta programação Swift explica como incluir serviços em um aplicativo Swift novo ou existente, seja Swift iOS do lado do cliente ou do servidor.
{: shortdesc}

O tutorial a seguir mostra como criar facilmente um app móvel Swift com o {{site.data.keyword.mobileanalytics_full}} usando um kit do iniciador vazio por meio do [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits). Por meio do console, você inclui o serviço {{site.data.keyword.mobileanalytics_short}}, faz download do código, executa o app iOS localmente no Xcode, configura e monitora o app.

## Etapa 1. Requisitos para Desenvolvedores
{: #dev-requirements}

Para iniciar o desenvolvimento do iOS no {{site.data.keyword.cloud_notm}}, certifique-se de atender aos requisitos a seguir.

### Sistema Operacional

A melhor prática para desenvolver apps Swift é usar o hardware suportado pela MacOS mais recente e testar com as liberações mais recentes do iOS. Inscreva-se para uma conta do [Apple Developer](https://developer.apple.com/) para ativar o teste em um dispositivo físico.

### iOS e Xcode
{: #ios_and_xcode}

- Instale o  [ Xcode 8 + ](https://developer.apple.com/xcode/)  (ou superior).
- Implemente nos [dispositivos iOS 8](https://support.apple.com/downloads/ios) (ou superior).
- Revise as [Diretrizes de envio de armazenamento de app](https://developer.apple.com/app-store/guidelines/) antes de enviar os apps para a Apple.

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

1. Descompacte o arquivo ZIP do download. Em seguida, usando um terminal, navegue para a pasta extraída.
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

  `BMSCore` é o Core SDK e é base para os Mobile Client SDKs. `BMSClient` é uma classe de `BMSCore` inicializada em `AppDelegate.swift`. Juntamente com o `BMSCore`, o SDK do {{site.data.keyword.mobileanalytics_short}} já foi importado para o projeto.
  
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

   Para recursos de analítica avançada e criação de log, consulte [Reunindo análise de dados de uso](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics) e [Criação de log](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger).
   {:tip}

## Etapa 6. Monitorando o aplicativo com  {{site.data.keyword.mobileanalytics_short}}
O serviço {{site.data.keyword.mobileanalytics_short}} fornece insights chave de desempenho e de uso de aplicativo para desenvolvedores de aplicativos móveis e proprietários de aplicativos. Usando o {{site.data.keyword.mobileanalytics_short}}, os proprietários do aplicativo e os desenvolvedores podem entender o que está acontecendo no lado do usuário. Eles podem usar esse insight para construir aplicativos melhores que sejam relevantes para os usuários e que sobressaiam no verdadeiro mar de aplicativos móveis.

O serviço inclui o {{site.data.keyword.mobileanalytics_short}} Console no qual os desenvolvedores e os proprietários de aplicativos podem monitorar o desempenho do aplicativo remoto, consulte as estatísticas de uso e os logs do dispositivo de procura.

1. Abra o serviço `{{site.data.keyword.mobileanalytics_short}}` por meio do app móvel que você criou ou clique nos três pontos verticais ao lado do serviço e selecione `Abrir painel`.
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

* [Incluindo o serviço de notificações de push](/docs/services/mobilepush/index.html)
* [Incluindo autenticação do usuário com o ID do app](/docs/services/appid/index.html)

### Usando o {{site.data.keyword.cloud_notm}} Developer tools
Também é possível aprender como desenvolver apps Swift usando as ferramentas do desenvolvedor do [{{site.data.keyword.cloud_notm}}](../cli/index.html), que oferecem uma abordagem de linha de comandos para criar, desenvolver e implementar aplicativos completos da web, de dispositivo móvel e de microsserviço.

