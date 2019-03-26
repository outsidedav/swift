---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-04"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Efetuando Login no Swift
{: #logging_swift}

Mensagens de log são sequências com informações contextuais sobre o estado e a atividade do microsserviço no momento em que a entrada de log é feita. Os logs são necessários para diagnosticar como e por que os serviços falham e desempenha uma função de suporte para as [métricas de app](/docs/swift/cloudnative/appmetrics.html) no monitoramento do funcionamento do aplicativo.

Dada a natureza transitória de processos em ambientes de nuvem, os logs devem ser coletados e enviados em outros lugares, geralmente em um local centralizado para análise. A maneira mais consistente de efetuar login nos ambientes de nuvem é enviar entradas de log para fluxos de saída padrão e de erro, o que deixa a infraestrutura para manipular o resto.

## Incluindo Criação de Log no app Swift
{: #logging-add}

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) é uma estrutura de criação de log leve e popular para Swift, além de fornecer muitos benefícios nativos, como a criação de log para saída padrão e diferentes níveis de log.

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) é o protocolo do criador de logs que fornece uma interface de criação de log comum para diferentes tipos de criadores de logs no Swift. O Kitura usa o `LoggerAPI` em toda sua implementação.

Para usar `HeliumLogger`, inclua o código a seguir na seção **dependências:** em seu `Package.swift` para todos os destinos apropriados:
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

## Criação de Log com kits do iniciador
{: #logging-starterkits}

Os apps Swift criados usando o {{site.data.keyword.cloud_notm}} App Service vêm com o `HeliumLogger`, por padrão. Executar o app nativamente ou em um ambiente de nuvem produz a seguinte saída:
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

Essas mensagens são localizadas em `stdout` localmente ou nos logs para implementações do [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) e do [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/), que são acessadas pelos `ibmcloud app logs --recent <APP_NAME>`  e  ` logs kubectl <deployment name>`.

No arquivo `/Sources/AppName/main.swift`, é possível ver o código a seguir:
```swift
HeliumLogger.use (LoggerMessageType.info)
```
{: codeblock}

O nível de log é explicitamente configurado como `.info` para registrar as mensagens de nível informativo, como os logs de inicialização do aplicativo.
{: tip}

## Próximas Etapas
{: #next-logging}

Saiba mais sobre como visualizar os logs em cada um dos nossos ambientes de implementação:
* [ Logs de Kubernetes ](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [ Logs do Cloud Foundry ](/docs/cli/reference/ibmcloud/bx_cli.html)
* [Cloud Foundry Enterprise Environment - Auditoria e criação de log](docs/cloud-foundry/auditing-logging.html)
* [ {{site.data.keyword.openwhisk}}  Logs & Monitoring ](/docs/openwhisk/openwhisk_logs.html)

Aprenda como implementar e usar um agregador de log:
* [ {{site.data.keyword.cloud_notm}}  Análise do Log ](/docs/services/CloudLogAnalysis/log_analysis_ov.html)
* [ {{site.data.keyword.cloud_notm}}  Pilha ELK privada ](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
