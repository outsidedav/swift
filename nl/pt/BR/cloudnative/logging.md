---

copyright:
  years: 2018
lastupdated: "2018-08-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Efetuando Login no Swift
{: #logging_swift}

Os logs são necessários para diagnosticar como e por que os serviços falham. Os logs não devem ser usados para monitorar o desempenho do aplicativo, pois as métricas são para isso, mas podem ser usados como uma origem para alertas, que podem então incluir mais detalhes do que você pode obter de métricas agregadas.

Um dos benefícios de trabalhar com infraestrutura em nuvem é que seu aplicativo pode parar de se preocupar com várias coisas, como gerenciar arquivos de log. Dada a natureza transitória de processos em ambientes de nuvem, os logs devem ser coletados e enviados para outros lugares, geralmente para um local centralizado para análise. A maneira mais consistente de se registrar em ambientes de nuvem é enviar entradas de log para fluxos de saída e de erro padrão e deixar que a infraestrutura cuide do resto.

À medida que seu aplicativo se desenvolve ao longo do tempo, a natureza do que você registra pode mudar. Ao usar um formato de log JSON, você obtém os benefícios a seguir:
* Os logs são indexáveis, o que facilita muito mais a procura de um corpo agregado de logs.
* Os logs são resilientes a mudanças, pois a análise não depende da posição de elementos em uma sequência.

O uso da criação de log formatada de JSON pode tornar os logs um pouco mais difíceis para você, humano, ler ao usar ferramentas de linha de comandos para buscar logs. É possível usar variáveis de ambiente para alternar qual formato de log será usado para que você possa ter logs de texto sem formatação para desenvolvimento local e depuração.

## Incluindo Criação de Log no app Swift

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) é uma estrutura de criação de log leve e popular para Swift, além de fornecer muitos benefícios nativos, como a criação de log para saída padrão e diferentes níveis de log.

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) é o protocolo do criador de logs que fornece uma interface de criação de log comum para diferentes tipos de criadores de logs no Swift. O Kitura usa o `LoggerAPI` em toda sua implementação.

Para alavancar o `HeliumLogger`, inclua o seguinte nas **dependências:** em seu `Package.swift`, certificando-se de incluí-lo em todos os destinos em que seja usado.
```swift
.package (url: "https: //github.com/IBM-Swift/HeliumLogger.git ", de:" 1.7.1 ")
```
{: codeblock}

Inclua o código de instrumentação a seguir para inicializar o `HeliumLogger` e configure-o como o criador de logs usado pelo `LoggerAPI`:
```swift
import HeliumLogger
import LoggerAPI

let logger = HeliumLogger(.verbose)
Log.logger = logger

Log.info ("Esta é uma mensagem de log informativa.")
```
{: codeblock}

No exemplo fornecido, o [nível de log](http://ibm-swift.github.io/HeliumLogger/) foi configurado explicitamente como `.verbose`, que é o padrão.

Para obter mais informações sobre como customizar as mensagens de log, consulte a [documentação de referência oficial da API do HeliumLogger](http://ibm-swift.github.io/HeliumLogger/).

## Registrando com StarterKits
{: #monitoring}

Os apps Swift criados usando o {{site.data.keyword.cloud_notm}} App Service vêm com o `HeliumLogger`, por padrão. Executar o app nativamente ou em um ambiente de nuvem produz a seguinte saída:
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

Essas mensagens são localizadas em `stdout` na execução local ou nos logs para implementações do [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) e do [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/), que são acessadas por `ibmcloud app logs --recent <APP_NAME>`  e  ` logs kubectl <deployment name>`, respectivamente.

No arquivo `/Sources/AppName/main.swift`, é possível ver o código a seguir:
```swift
HeliumLogger.use (LoggerMessageType.info)
```
{: codeblock}

O nível de log é configurado explicitamente como `.info` para registrar mensagens de nível informativo como os logs de inicialização do aplicativo.
{: tip}

## Próximas Etapas
{: #next_steps}

Saiba mais sobre como visualizar os logs em cada um dos nossos ambientes de implementação:
* [ Logs de Kubernetes ](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [ Logs do Cloud Foundry ](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [ {{site.data.keyword.openwhisk}}  Logs & Monitoring ](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

Usando um agregador de log:
* [ {{site.data.keyword.cloud_notm}}  Análise do Log ](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [ {{site.data.keyword.cloud_notm}}  Pilha ELK privada ](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
