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

# Coletando analítica móvel
{: #mobile_analytics}

O {{site.data.keyword.mobileanalytics_short}} no {{site.data.keyword.cloud_notm}} fornece aos desenvolvedores, administradores de TI e interessados nos negócios insight sobre a execução de seus apps móveis e como estão sendo usados. Com o serviço  {{site.data.keyword.mobileanalytics_short}} , é possível:

 - **Obter insight imediato** - Consulte as métricas de desempenho e de uso em tempo real.

 - **Implementar em minutos** - Crie uma instância de serviço no {{site.data.keyword.cloud_notm}}, inclua o SDK no projeto, cole duas linhas de código no aplicativo e você estará pronto para coletar dezenas de métricas predefinidas.

 - **Coletar dados que você deseja** - Instrumente apps com eventos customizados, descubra como os usuários estão interagindo com seu app, rastreie compras e monitore a atividade do app.

 - **Ter uma visão rápida das métricas** - O console do {{site.data.keyword.mobileanalytics_short}} oferece gráficos prontos, sem a necessidade de gravar consultas.

 - **Concentrar-se no que é importante para você** - Filtre métricas por tempo, adaptador, aplicativo, versão do aplicativo, S.O., versão do S.O. ou modelo de dispositivo.

 - **Descobrir problemas rapidamente** - Monitore o status do travamento. Configure acionadores de alerta sobre métricas críticas e roteie alertas para qualquer terminal REST.

 - **Solucionar problemas na causa raiz** - Use a criação de log customizada em seu aplicativo e faça upload automaticamente dos logs e procure-os no console. Realize drill down em eventos de travamento para ver os rastreios de pilha.

## Antes de começar

Primeiramente, certifique-se de que tenha os pré-requisitos a seguir prontos para execução:

 - iOS 8.0+/watchOS 2.0+
 - Xcode 7.3, 8.0
 - Swift 2.2-3.0
 - Cocoapods ou Cartago

## Etapa 1. Criando uma instância do  {{site.data.keyword.mobileanalytics_short}}
{: #mobile_analytics_create}

1. No catálogo do {{site.data.keyword.cloud_notm}}, clique em **Remoto** > **{{site.data.keyword.mobileanalytics_short}}**. A tela de configuração de serviço é aberta.
2. Dê um nome à sua instância de serviço ou use o nome de pré-configuração.
3. Clique em **Criar**.
4. Na área de janela de navegação, clique em **Conexões** para selecionar um app e ligá-lo ao seu serviço. É possível ligar a instância de serviço ao seu app posteriormente se ele não estiver vinculado durante a criação.

## Etapa 2. Instalando o SDK do iOS Swift

O serviço fornece SDKs específicos de plataforma para simplificar o desenvolvimento de aplicativo. Os SDKs do {{site.data.keyword.cloud_notm}} Mobile Services Swift podem ser instalados com Cocoapods ou Carthage. Para obter mais informações, consulte  [ https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics ](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics).

É possível instrumentar seu aplicativo móvel usando o SDK do {{site.data.keyword.mobileanalytics_full}}. O Swift SDK está disponível para iOS e watchOS.

1. Assegure-se de ter configurado corretamente o Xcode. Para aprender a configurar seu ambiente de desenvolvimento iOS, consulte o [website do Apple Developer ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.apple.com/support/xcode/){: new_window}. Leia sobre os [Requisitos do Xcode ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#requirements){: new_window} for Client SDK Swift Analytics.

2. Ative a API de localização incluindo uma propriedade no arquivo `Info.plist` na pasta do projeto de seu app. Por exemplo, `Privacy - Location Usage Description` e forneça uma justificativa adequada para incluir a API de localização, como "O app requer que o serviço de localização seja ativado".

O {{site.data.keyword.mobileanalytics_short}} SDK é distribuído com [CocoaPods ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cocoapods.org/){: new_window} e [Carthage ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/Carthage/Carthage#getting-started){: new_window}, que são gerenciadores de dependência para projeto Cocoa. O CocoaPods e o Carthage fazem download automaticamente de artefatos de repositórios para disponibilizá-los para seu aplicativo. Selecione CocoaPods ou Carthage:

### CocoaPods
{: #cocoapods}

1. Siga as [instruções do Swift SDK do {{site.data.keyword.Bluemix_notm}} Mobile Services ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#cocoapods){: new_window} no GitHub para instalar o `BMSAnalytics` usando o Cocoapods e incluí-lo no Podfile.

2. Depois de instalar o iOS Client SDK, [importe e inicialize](sdk.html#initalize-ma-sdk) o Analytics Client SDK.   

### Cartago
{: #carthage}

Se você não estiver usando o CocoaPods, será possível incluir estruturas no projeto usando o [o Carthage ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos){: new_window}.

1. Siga as [instruções de instalação do Carthage ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#carthage){: new_window} no GitHub para instalar o `BMSAnalytics`.

2. Depois de instalar o iOS Client SDK, importe e, em seguida, inicialize o Analytics Client SDK.

## Etapa 3. Inicializando o SDK

Ao usar o {{site.data.keyword.mobileanalytics_short}}, é possível coletar as categorias de dados a seguir, cada uma das quais requer um grau diferente de instrumentação:

**Dados pré-definidos**:
essa categoria inclui informações genéricas de uso e de dispositivo que se aplicam a todos os aplicativos. Ela indica o volume, a frequência ou a duração de uso do aplicativo. Os dados predefinidos são coletados automaticamente depois de inicializar o SDK do {{site.data.keyword.mobileanalytics_short}} em seu aplicativo.

**Mensagens de log do aplicativo**:
essa categoria permite que o desenvolvedor inclua linhas de código em todo o aplicativo que podem registrar mensagens customizadas para ajudar no desenvolvimento e na depuração. O desenvolvedor designa um nível de severidade/verbosidade a cada mensagem de log.

**Eventos customizados**:
essa categoria inclui dados que você define sozinho e que são específicos para seu app. Esses dados representam eventos que ocorrem no aplicativo, como visualizações da página, toques do botão ou compras do aplicativo. Além de inicializar o SDK do {{site.data.keyword.mobileanalytics_short}} no aplicativo, deve-se incluir uma linha de código para cada evento customizado que você desejar rastrear.

## Etapa 4. Identificando o valor da chave API de suas credenciais de serviço
{: #analytics-clientkey}

Identifique o valor da **Chave API** antes
de configurar o SDK do cliente. A chave API é necessária para inicializar o SDK do cliente.

1. Abra o painel de serviço do  {{site.data.keyword.mobileanalytics_short}} .
2. Expanda **Visualizar credenciais** para revelar o valor de sua chave API. Você precisa do valor da chave API ao inicializar o {{site.data.keyword.mobileanalytics_short}} Client SDK.

## Etapa 5.  Inicializando seu aplicativo para coletar analítica
{: #initalize-ma-sdk}

Inicialize seu aplicativo para permitir o envio de logs para o serviço {{site.data.keyword.mobileanalytics_short}}.

1. Importe o SDK do Cliente.

    O Swift SDK está disponível para iOS e watchOS. Importe as estruturas `BMSCore` e `BMSAnalytics` ao incluir as instruções de `importação` a seguir no início do seu arquivo de
projeto `AppDelegate.swift`:
	```
	importar BMSAnalytics BMSCore de importação
	```
	{: codeblock}  

2. Inicialize o SDK do cliente
{{site.data.keyword.mobileanalytics_short}} em seu
aplicativo.

	Primeiramente, inicialize a classe `BMSClient` colocando o código de inicialização no método `application(_:didFinishLaunchingWithOptions:)` de seu representante de aplicativo ou em um local que funcione melhor para seu projeto.
	```
	BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Make sure that you point to your region
	```
	{: codeblock}

	Deve-se inicializar o `BMSClient` com
o parâmetro **bluemixRegion**. No inicializador, o valor **bluemixRegion** especifica qual implementação do {{site.data.keyword.Bluemix_notm}} está sendo usada.

3. Inicialize o Analytics usando seu objeto de aplicativo e
dando a ele seu nome do seu aplicativo.

	O nome que você seleciona para o seu aplicativo (`your_app_name_here`) é exibido no console do
{{site.data.keyword.mobileanalytics_short}} como o nome do aplicativo. O nome do aplicativo é usado como um filtro para procurar logs do aplicativo no painel. Ao usar o mesmo nome de aplicativo nas plataformas (por exemplo, iOS), é possível ver todos os logs desse aplicativo com o mesmo nome, independentemente de qual plataforma os logs foram enviados.

	Você precisa também do valor [Chave API](#analytics-clientkey).
	```
	Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, collectLocation: true, deviceEvents: .lifecycle, .network)
	```
    {: codeblock}

4. O aplicativo agora está inicializado e pronto para coletar analítica. Em seguida, é possível enviar dados de analítica para o serviço {{site.data.keyword.mobileanalytics_short}}.		

## Etapa 6. Reunindo analítica de uso
{: #app-monitoring-gathering-analytics}

É possível configurar o {{site.data.keyword.mobileanalytics_short}} client SDK para registrar a analítica de uso e enviar os dados registrados para o serviço {{site.data.keyword.mobileanalytics_short}}.

Use as APIs a seguir para iniciar a gravação e o envio da análise de uso:
```
// Disable recording of usage analytics (eg: to save disk space)
// Recording is enabled by default

Analytics.isEnabled = false

// Ativar gravação de analítica de uso

Analytics.isEnabled = true

// Send recorded usage analytics to the Mobile Analytics Service

Analytics.send (completionHandler: { in
    if let response = response {

	  // Handle Analytics send success here.
    }
    if let error = error {
        // Manipular a falha de envio de analítica aqui.
    }
})
```
{: codeblock}

Amostra de análise de uso para registrar um evento:
```
// Log a custom analytics event
let eventObject = ["customProperty": "propertyValue"]
Analytics.log(metadata: eventObject)
```
{: codeblock}

## Etapa 7. Utilizando o Logger

O {{site.data.keyword.mobileanalytics_full}} Client SDK fornece uma estrutura de criação de log que é semelhante a outras estruturas de log com as quais você pode estar familiarizado, tais como, `java.util.logging` ou `log4j`. A estrutura de criação de log suporta múltiplas instâncias do criador de logs por pacote, níveis diferentes de log, captura de rastreios de pilha para um travamento do aplicativo e muito mais.

Também é possível configurar os dados registrados a serem armazenados no dispositivo no qual o aplicativo está em execução e enviar os logs do dispositivo para o Serviço {{site.data.keyword.mobileanalytics_short}} posteriormente.

A estrutura de criação de log do SDK do cliente do {{site.data.keyword.mobileanalytics_short}} suporta os níveis de log a seguir, que são listados do menos ao mais detalhado, com as diretrizes de uso recomendadas:

  * `FATAL` - Usar para travamentos ou interrupções irrecuperáveis. O nível `FATAL` é reservado para registrar erros irrecuperáveis, que aparecem para os usuários como um travamento do aplicativo
  * `ERROR` - Usar para exceções inesperadas ou erros de protocolo de rede inesperados
  * `WARN` - Usar para registrar avisos de uso que não são considerados erros críticos, como uso de APIs descontinuadas ou resposta de rede lenta
  * `INFO` - Usar para relatar eventos de inicialização e outros dados que podem ser importantes, mas não urgentes
  * `DEBUG` - Usar para relatar instruções de depuração para ajudar os desenvolvedores a resolver defeitos do aplicativo

### Cenário de nível de log
{: #log-level-scenario}

Quando o nível do criador de logs estiver configurado como `FATAL`, o criador de logs capturará exceções de não captura, mas não capturará logs que levam ao evento de travamento. É possível configurar um nível do criador de logs mais detalhado para assegurar que os logs que podem levar a uma entrada do criador de logs `FATAL`, como `WARN` e `ERROR`, também sejam capturados.

Quando o nível do criador de logs estiver configurado
como `DEBUG`, você também obterá os logs do SDK
do cliente Mobile Analytics, que serão incluídos ao enviar logs.

### Uso do Logger de Amostra
{: #sample-logger-usage}

**Nota:** certifique-se de que o seu aplicativo esteja configurado para usar o {{site.data.keyword.mobileanalytics_short}} Client SDK antes de usar a estrutura de criação de log.

Os fragmentos de código a seguir mostram o uso do criador de logs de amostra:
```
// Configure o Logger. Desativado por padrão;

Logger.isLogStorageEnabled = true

// Change the minimum log level (optional). O padrão é-LogLevel.debug

Logger.logLevelFilter = LogLevel.info

// Multiple log instances can be created to organize your logs

let logger1 = Logger.logger(name: "feature1Logger")
let logger2 = Logger.logger(name: "feature2Logger")

// Registrar mensagens com níveis diferentes

logger1.debug (mensagem: "mensagem de depuração para o recurso 1")

// logger1.debug message isn't logged as logLevelFilter is set to info

logger2.info (mensagem: "mensagem de informação para o recurso 2")

// Send logs to the Mobile Analytics Service

Logger.send(completionHandler: { (response: Response?, error: Error?) in
        if let response = response {
        logger.debug(message: "Status code: \(response.statusCode)")
        logger.debug(message: "Response: \(response.responseText)")
    }
    if let error = error {
        logger.error( mensagem: "Erro: \ (erro) ")
    }
})
```
{: codeblock}

Por questões de privacidade, é possível desativar a saída do Criador de Logs de aplicativos construídos no modo de liberação. Por padrão, a classe de Criador de logs imprime logs no console Xcode. Nas configurações de construção para seu destino, inclua uma sinalização `-D RELEASE_BUILD` na seção **Outra sinalizações do Swift** da configuração de construção da liberação.
{: tip}

## Etapa 8. Registrando Dados do Local
{: #location-logging}

O local do dispositivo móvel pode ser registrado usando o app por meio desta API fornecida:
```
Analytics.logLocation ();
```

A API permite que o app envie seu local (como latitude e longitude) para o servidor entre as sessões do aplicativo. Além de chamadas explícitas para a API `location-logging`, o SDK envia a localização de dispositivo para cada Sessão de app. O local é enviado para o contexto de sessão de app inicial e para o contexto de sessão do app do comutador do usuário (ou seja, um comutador de um usuário entre uma sessão de app). A API de localização deve ser ativada na inicialização do SDK.

Para chamar a API `location-logging`, configure o parâmetro `collectLocation` como `true` na inicialização do SDK:
```
Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
```

## Etapa 9. Fazendo uma Solicitação de Rede
{: #network-requests}

É possível configurar o SDK do cliente do {{site.data.keyword.mobileanalytics_short}} para [fazer uma solicitação de rede](/docs/mobile/sdk_network_request.html). Certifique-se
de já ter inicializado `BMSClient` e `BMSAnalytics`, além de ter
importado os SDKs do cliente.

## Etapa 10. Relatório analítico de travamento
{: #report-crash-analytics}

É possível ver [dados de travamento de aplicativo](app-monitoring.html#monitor-app-crash) enviando informações de analítica e de log para
{{site.data.keyword.mobileanalytics_short}}.

O método `Analytics.send()` preenche as tabelas **Visão geral de travamento** e **Travamentos**
na página **Travamentos**. Os gráficos são ativados usando a inicialização e o processo de envio para analítica; nenhuma configuração especial é necessária.

O método `Logger.send()` preenche as tabelas **Resumo de travamento** e **Detalhes do travamento** na página **Resolução de problemas**. Deve-se ativar seu aplicativo para armazenar e enviar logs para preencher os gráficos incluindo uma instrução no código de aplicativo:
```
Logger.isLogStorageEnabled = true
Logger.logLevelFilter = LogLevel.Fatal // or greater
```
{: codeblock}

Consulte a [amostra de uso do criador de logs](sdk.html##sample-logger-usage) do iOS.

## Etapa 11. Rastreando Usuários Ativos
{: #trackingusers}

Se o seu aplicativo puder reconhecer usuários exclusivos em um dispositivo, será possível rastrear quantos usuários estão usando ativamente o aplicativo, transmitindo o nome de usuário do usuário ativo para o {{site.data.keyword.mobileanalytics_short}}.

Ative o rastreamento de usuário inicializando {{site.data.keyword.mobileanalytics_short}} com
`hasUserContext=true`. Caso contrário, o {{site.data.keyword.mobileanalytics_short}} irá capturar apenas um usuário por dispositivo.

Inclua o código a seguir para controlar quando o usuário efetua login:
```
Analytics.userIdentity = "username"
```
{: codeblock}

## Etapa 12. Testando seu aplicativo
{: #appid_testing}

Está tudo acertado corretamente? Hora de testá-lo!

1. Abra seu app. Se você tiver um aplicativo da web, use um navegador. Se tiver um aplicativo cliente iOS com o emulador Xcode.
2. Compile e execute o aplicativo em seu emulador ou dispositivo.
3. Usando a GUI, percorra o processo de assinatura em seu aplicativo.
4. Acesse o console do {{site.data.keyword.mobileanalytics_short}} para ver a analítica de uso para seu aplicativo. Também é possível monitorar seu aplicativo [configurando alertas](/docs/services/mobileanalytics/app-monitoring.html#alerts) e [monitorando travamentos do app](/docs/services/mobileanalytics/app-monitoring.html#monitor-app-crash).

## O que Fazer a Seguir
{: #what-to-do-next notoc}

 - Para saber mais sobre o serviço, leia toda a [documentação](/docs/services/mobileanalytics/index.html#getting-started-tutorial).

 - Para obter uma introdução ao trabalho com serviços móveis e o {{site.data.keyword.Bluemix_notm}}, consulte [Introdução aos apps móveis no IBM Cloud](/docs/services/mobile/index.html).

 - Os Starter Kits são uma das maneiras mais rápidas de alavancar os recursos do {{site.data.keyword.cloud_notm}}. Visualize todos os kits do iniciador disponíveis no [Painel do desenvolvedor do dispositivo móvel](https://console.bluemix.net/developer/mobile/dashboard). Faça download do código. Execute o app.

 - É possível usar o [Swagger UI](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/) para revisar rapidamente a documentação da API de REST.
