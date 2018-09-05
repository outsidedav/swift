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

# Registro en Swift
{: #logging_swift}

Es necesario que los registros diagnostiquen cómo y por qué fallan los servicios. Los registros no están pensados para utilizarse para supervisar el rendimiento de las aplicaciones de las que son necesarias las métricas, pero se pueden utilizar como un origen para las alertas, que luego pueden incluir más detalles de los que puede obtener de las métricas agregadas.

Una de las ventajas de trabajar con la infraestructura de nube es que la aplicación hace dejar de preocuparse sobre muchas cosas, como la gestión de archivos de registro. Dada la naturaleza transitoria de los procesos en entornos de nube, los registros deben recopilarse y enviarse a otro lugar, normalmente a una ubicación centralizada para su análisis. La forma más coherente de iniciar sesión en entornos de nube es enviar entradas de registro a la salida estándar y a las secuencias de error, y dejar que la infraestructura maneje el resto.

A medida que la aplicación evoluciona a lo largo del tiempo, la naturaleza de lo que puede registrar puede cambiar. Al utilizar un formato de registro JSON, obtendrá las ventajas siguientes:
* Los registros son indexables, lo que hace que la búsqueda de un cuerpo agregado de registros sea mucho más fácil.
* Los registros son resistentes al cambio, ya que el análisis no depende de la posición de los elementos en una serie.

El uso del registro con formato JSON puede hacer que los registros sean un poco más difíciles de leer para usted, el ser humano, al utilizar herramientas de línea de mandatos para captar registros. Puede utilizar variables de entorno para conmutar qué formato de registro se utiliza para que pueda tener registros de texto sin formato para el desarrollo y la depuración locales.

## Adición de registro a la app Swift

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) es una infraestructura de registro ligera y popular para Swift, y proporciona muchas ventajas nativas, como el registro en la salida estándar y en distintos niveles de registro.

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) es el protocolo de registrador que proporciona una interfaz de registro común para distintos tipos de registradores en Swift. Kitura utiliza la `LoggerAPI` a lo largo de su implementación.

Para aprovechar `HeliumLogger`, añada lo siguiente a las **dependencies:** en el archivo `Package.swift`, asegurándose de añadirlo a cualquier destino en el que se utilice.
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

En el ejemplo proporcionado, el [nivel de registro](http://ibm-swift.github.io/HeliumLogger/) se estableció explícitamente en `.verbose`, que es el valor predeterminado.

Para obtener más información sobre la personalización de los mensajes de registro, consulte la [documentación de referencia de la API HeliumLogger](http://ibm-swift.github.io/HeliumLogger/) oficial.

## Registro con StarterKits
{: #monitoring}

De forma predeterminada, las apps Swift creadas utilizando el {{site.data.keyword.cloud_notm}} App Service vienen con `HeliumLogger`. La ejecución de la app de forma nativa o en un entorno de nube genera la salida siguiente:
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

Estos mensajes se encuentran en `stdout` desde la ejecución local, o en los registros de los despliegues de [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) y [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/), a los que se accede mediante `ibmcloud app logs --recent <APP_NAME>` y `kubectl logs <deployment name>`, respectivamente.

En el archivo `/Sources/AppName/main.swift`, puede ver el código siguiente:
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

El nivel de registro se establece explícitamente en `.info` para registrar mensajes de nivel informativo como, por ejemplo, los registros de inicio de la aplicación.
{: tip}

## Pasos siguientes
{: #next_steps}

Obtenga más información sobre la visualización de los registros en cada uno de los entornos de despliegue:
* [Registros de Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [Registros de Cloud Foundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [{{site.data.keyword.openwhisk}} Registros y supervisión](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

Utilización de un agregador de registros:
* [{{site.data.keyword.cloud_notm}} Log Analysis ](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Pila de ELK privada](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
