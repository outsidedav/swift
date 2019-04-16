---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: swift logging, ios logging, debug swift, add logging swift, heliumlogger swift, loggerapi swift, logger swift, starter kit swift logger

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Efetuando Login no Swift
{: #logging_swift}

Mensagens de log são sequências com informações contextuais sobre o estado e a atividade do microsserviço no momento em que a entrada de log é feita. Os logs são necessários para diagnosticar como e por que os serviços falham e desempenha uma função de suporte para as [métricas de app](/docs/swift/cloudnative?topic=swift-metrics#metrics) no monitoramento do funcionamento do aplicativo.

Dada a natureza transitória de processos em ambientes de nuvem, os logs devem ser coletados e enviados para outros lugares, geralmente para um local centralizado para análise. A maneira mais consistente de efetuar login nos ambientes de nuvem é enviar entradas de log para fluxos de saída padrão e de erro, o que deixa a infraestrutura para manipular o resto.

## Incluindo a criação de log no app do Swift
{: #logging-add}

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") é uma estrutura de criação de log leve, popular, para Swift e fornece muitos benefícios nativos, como criação de log para níveis de saída padrão e de log diferentes.

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") é o protocolo do criador de logs que fornece uma interface de criação de log comum para diferentes tipos de criadores de logs no Swift. O Kitura usa o `LoggerAPI` em toda sua implementação.

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

No exemplo fornecido, o [nível de log](http://ibm-swift.github.io/HeliumLogger/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") foi configurado explicitamente como `.verbose`, que é o padrão.

Para obter mais informações sobre a customização das mensagens de log, consulte a [documentação de referência da API do HeliumLogger](http://ibm-swift.github.io/HeliumLogger/){: new_window} oficial ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Efetuando login com kits do iniciador
{: #logging-starterkits}

Os apps Swift criados usando o {{site.data.keyword.cloud_notm}} App Service vêm com o `HeliumLogger`, por padrão. Executar o app nativamente ou em um ambiente de nuvem produz a seguinte saída:
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

Essas mensagens são localizadas em `stdout` localmente ou nos logs para implementações do [CloudFoundry](/docs/cli/reference/bluemix_cli?topic=cloud-cli-ibmcloud_cli#ibmcloud_app_logs) e do [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/){: new_window}![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), que são acessadas por `ibmcloud app logs --recent <APP_NAME>`  e  ` logs kubectl <deployment name>`.

No arquivo `/Sources/AppName/main.swift`, é possível ver o código a seguir:
```swift
HeliumLogger.use (LoggerMessageType.info)
```
{: codeblock}

O nível de log é explicitamente configurado como `.info` para registrar as mensagens de nível informativo, como os logs de inicialização do aplicativo.
{: tip}

## Próximas etapas
{: #next-logging notoc}

Saiba mais sobre como visualizar os logs em cada um dos destinos de implementação:
* [Logs do Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")
* [ Logs do Cloud Foundry ](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
* [Cloud Foundry Enterprise Environment - Auditoria e criação de log](/docs/cloud-foundry?topic=cloud-foundry-auditing-logging#auditing-logging)
* [{{site.data.keyword.openwhisk}} Logs & Monitoramento](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#openwhisk_logs)

Aprenda como implementar e usar um agregador de log:
* [{{site.data.keyword.cloud_notm}} Log Analysis](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_analysis_ov#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Pilha do ELK privado](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")
