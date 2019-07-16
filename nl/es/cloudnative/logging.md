---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: swift logging, ios logging, debug swift, add logging swift, heliumlogger swift, loggerapi swift, logger swift, starter kit swift logger

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Registro en Swift
{: #logging_swift}

Los mensajes de registro son series con información contextual sobre el estado y la actividad del microservicio en el momento en que se realiza la entrada de registro. Los registros son necesarios para diagnosticar cómo y por qué fallan los servicios, y desempeñan un rol de soporte a las [métricas de app](/docs/swift/cloudnative?topic=swift-metrics#metrics) en la supervisión del estado de la aplicación.

Dada la naturaleza transitoria de los procesos en entornos de nube, los registros deben recopilarse y enviarse a otro lugar, normalmente a una ubicación centralizada para su análisis. La forma más coherente de iniciar sesión en entornos de nube es enviar entradas de registro a la salida estándar y a las secuencias de error, que deja que la infraestructura maneje el resto.

## Adición de registro a la app Swift
{: #logging-add}

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") es una infraestructura de registro ligera y popular para Swift, y proporciona muchas ventajas nativas, como el registro en la salida estándar y en distintos niveles de registro.

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") es el protocolo de registrador que proporciona una interfaz de registro común para distintos tipos de registradores en Swift. Kitura utiliza la `LoggerAPI` a lo largo de su implementación.

Para utilizar `HeliumLogger`, añada el siguiente código a la sección **dependencies:** del archivo `Package.swift` para todos los destinos adecuados:
```swift
.package(url: "https://github.com/IBM-Swift/HeliumLogger.git", from: "1.7.1")
```
{: codeblock}

Añada el código de instrumentación siguiente para inicializar `HeliumLogger` y establecerlo como el registrador utilizado por `LoggerAPI`:
```swift
import HeliumLogger
import LoggerAPI

let logger = HeliumLogger(.verbose)
Log.logger = logger

Log.info("This is an informational log message.")
```
{: codeblock}

En el ejemplo proporcionado, el [nivel de registro](http://ibm-swift.github.io/HeliumLogger/){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") se estableció explícitamente en `.verbose`, que es el valor predeterminado.

Para obtener más información sobre la personalización de los mensajes de registro, consulte la [documentación de referencia de la API HeliumLogger](http://ibm-swift.github.io/HeliumLogger/){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") oficial.

## Funcionamiento de los registros con kits de inicio
{: #logging-starterkits}

De forma predeterminada, las apps Swift creadas utilizando {{site.data.keyword.cloud_notm}} App Service vienen con `HeliumLogger`. La ejecución de la app de forma nativa o en un entorno de nube genera la salida siguiente:
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

Estos mensajes se encuentran en `stdout` (salida estándar) de manera local, o en los registros de los despliegues de Cloud Foundry y Kubernetes, a los que se accede mediante
`[ibmcloud app logs --recent <APP_NAME>]`(/docs/cli/reference?topic=cloud-cli-ibmcloud_commands_apps#ibmcloud_app_logs) y
`[kubectl logs <deployment name>`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

En el archivo `/Sources/AppName/main.swift`, puede ver el código siguiente:
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

El nivel de registro se establece explícitamente en `.info` para registrar mensajes de nivel informativo como, por ejemplo, los registros de inicio de la aplicación.
{: tip}

## Pasos siguientes
{: #next-logging notoc}

Obtenga más información sobre la visualización de los registros en cada uno de los destinos de despliegue:
* [Registros de Kubernetes](https://kubernetes.io/docs/concepts/cluster-administration/logging/#basic-logging-in-kubernetes){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")
* [Registros de Cloud Foundry](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
* [Cloud Foundry Enterprise Environment: Auditoría y registro](/docs/cloud-foundry?topic=cloud-foundry-auditing-logging#auditing-logging)
* [Registros y supervisión de {{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-logs)

Más información sobre cómo implementar y utilizar un agregador de registros:
* [{{site.data.keyword.cloud_notm}} Log Analysis](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_analysis_ov#log_analysis_ov)
* [Pila ELK de {{site.data.keyword.cloud_notm}} privado](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")
