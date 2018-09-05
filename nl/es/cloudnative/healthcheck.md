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

# Utilización de una comprobación de estado en la app Swift
{: #healthcheck}

Las comprobaciones de estado proporcionan un mecanismo simple para determinar si una aplicación del lado del servidor se está comportando correctamente. Muchos entornos de despliegue, como [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) y [Kubernetes](https://www.ibm.com/cloud/container-service), se pueden configurar para sondear los puntos finales de estado de forma periódica para determinar si una instancia del servicio está lista para aceptar tráfico.

Las comprobaciones de estado normalmente se consumen a través de HTTP, y utilizan códigos de retorno estándares para indicar el estado `UP` o `DOWN`. Entre los ejemplos se incluye volver a `200` para `UP`, y `5xx` para `DOWN`. Para ser específico, se utiliza un `503` cuando la aplicación no puede manejar solicitudes o no se ha iniciado todavía (el servicio no está disponible), y se utiliza un `500` cuando el servidor encuentra una condición de error. El valor de retorno de una comprobación de estado es variable, pero una respuesta JSON mínima, como `{“status”: “UP”}`, proporciona coherencia.

Cloud Foundry utiliza un punto final de estado para indicar si una instancia de servicio puede manejar o no las solicitudes. En Kubernetes, un punto final de comprobación de estado es equivalente a un punto final de `readiness`. La aplicación debe definir este punto final para ayudar a determinar las decisiones de direccionamiento automáticas. El éxito o el fracaso de este punto final puede incluir la consideración de los servicios en sentido descendente necesarios en el caso de que no haya retroceso aceptable. Si comprueba los servicios en sentido descendente, el almacenamiento en memoria caché del resultado a veces es útil, para minimizar la carga en el sistema en general debido a comprobaciones de estado.

Kubernetes define un punto final de [liveness](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) adicional, que permite a la aplicación indicar si el proceso se debe reiniciar o no. Aquí se aplica un subconjunto de consideraciones: una comprobación de `liveness` puede fallar una vez que se alcance un determinado umbral de utilización de memoria, por ejemplo. Si la app se está ejecutando en Kubernetes, considere la posibilidad de añadir un punto final `liveness` para asegurarse de que el proceso se reinicia cuando sea necesario.

## Adición de comprobación de estado a una app Swift existente
{: #add-healthcheck-existing}

La biblioteca [Health](https://github.com/IBM-Swift/Health) facilita la adición de una comprobación de estado a la aplicación Swift. Las comprobaciones de estado son ampliables. Para obtener más información sobre el [almacenamiento en memoria caché](https://github.com/IBM-Swift/Health#caching) para evitar ataques de DoS o añadir [comprobaciones personalizadas](https://github.com/IBM-Swift/Health#implementing-a-health-check), consulte la biblioteca [Health](https://github.com/IBM-Swift/Health).

Para añadir la biblioteca Health a una app Swift existente, especifíquela en la sección *dependencies:* del archivo `Package.swift`, asegurándose de añadirla a cualquier destino en el que se utilice:
```swift
  .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
```
{: codeblock}

A continuación, añada el código de inicialización siguiente a la aplicación:
```swift
import Health

let health = Health()
```
{: codeblock}

A continuación, añada la definición de ruta para definir el punto final de comprobación de estado:
```
router.get("/health") { request, response, next in
  // let status = health.status.toDictionary()
  let status = health.status.toSimpleDictionary()
  if health.status.state == .UP {
    try response.send(json: status).end()
  } else {
    try response.status(.serviceUnavailable).send(json: status).end()
  }
}
```
{: codeblock}

Compruebe el estado de la app con un navegador accediendo al punto final `/health`. El código devuelve una carga útil `{"status": "UP"}`, tal como define el diccionario simple.

Para implementaciones alternativas, como por ejemplo utilizar **Codable** o el diccionario estándar, consulte los [ejemplos de la biblioteca Health](https://github.com/IBM-Swift/Health#usage).

## Acceso a la comprobación de estado desde las apps del kit de iniciación de Swift del lado del servidor
{: #healthcheck-starterkit}

Al generar una app Swift basada en Kitura utilizando un kit de iniciación, se incluirá de forma predeterminada un punto final de comprobación de estado básico, `/health`. El punto final aprovecha el protocolo Codable disponible en Swift 4, tal como se admite en la biblioteca [Health](https://github.com/IBM-Swift/Health).

El código de inicialización básico, como la inicialización del objeto Health, se produce en `Sources/Application.swift`, mientras que el punto final de comprobación de estado se proporciona mediante el archivo `/Sources/Application/Routes/HealthRoutes.swift`, que contiene el código siguiente:
```swift
import LoggerAPI
import Health
import KituraContracts

func initializeHealthRoutes(app: App) {

    app.router.get("/health") { (respondWith: (Status?, RequestError?) -> Void) -> Void in
        if health.status.state == .UP {
            respondWith(health.status, nil)
        } else {
            respondWith(nil, RequestError(.serviceUnavailable, body: health.status))
        }
    }

}
```
{: codeblock}

El ejemplo utiliza el diccionario estándar, que genera una carga útil como por ejemplo `{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}` al acceder al punto final `/health`.
