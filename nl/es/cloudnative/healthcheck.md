---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: liveness probe swift, readiness probe swift, health swift, healthcheck swift, swift app status, kubernetes endpoint swift, health endpoint swift, swift health check

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilización de una comprobación de estado en la app Swift
{: #healthcheck}

Las comprobaciones de estado proporcionan un mecanismo simple para determinar si una aplicación del lado del servidor se está comportando correctamente. Los entornos de nube como [Kubernetes](https://www.ibm.com/cloud/container-service){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") y
[Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") se pueden configurar para que sondeen puntos finales de estado de manera periódica para determinar si una instancia del servicio está preparada para aceptar tráfico.
{: shortdesc}

## Visión general de la comprobación de estado
{: #overview}

Las comprobaciones de estado proporcionan un mecanismo simple para determinar si una aplicación del lado del servidor se está comportando correctamente. Normalmente se consumen a través de HTTP y utilizan códigos de retorno estándares para indicar el estado UP (activo) o DOWN(inactivo). El valor de retorno de una comprobación de estado es variable, pero típicamente es una respuesta JSON mínima, como `{"status": "UP"}`.

Kubernetes tiene una noción matizada del estado del proceso. Define dos análisis:

- Se utiliza una prueba de _**preparación**_ para indicar si el proceso puede manejar solicitudes (es direccionable).

  Kubernetes no direcciona el trabajo a un contenedor con una prueba de actividad anómala. Una prueba de actividad puede fallar si un servicio no ha terminado de inicializarse, o si está ocupado, sobrecargado o no puede procesar las solicitudes.

- Una prueba de _**actividad**_ se utiliza para indicar si el proceso debe reiniciarse.

  Kubernetes detiene y reinicia un contenedor con una prueba de actividad anómala. Utilice una prueba de actividad para limpiar procesos en un estado no recuperable, por ejemplo, si se ha agotado la memoria o si el contenedor no se ha detenido correctamente después de que un proceso interno haya fallado.

A modo de comparación, Cloud Foundry utiliza un punto final de estado. Si la comprobación falla, el proceso se reinicia, pero si se realiza correctamente, las solicitudes se direccionan a la misma. En este entorno, el punto final se realiza correctamente cuando el proceso está activo. Se ha configurado un tiempo de espera inicial para posponer la comprobación de estado hasta que el servicio haya finalizado la inicialización para evitar ciclos de reinicio.

La tabla siguiente proporciona una orientación sobre las respuestas que los puntos finales de estado singulares, de actividad y de preparación pueden proporcionar.

| Estado    | Preparación                   | Actividad                   | Estado                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | No es correcta y no se carga       | No es correcto y provoca un reinicio      | No es correcto y provoca un reinicio     |
| Iniciando | 503 - No disponible           | 200 - Correcto                   | Utilizar retraso para evitar la prueba   |
| Activo       | 200 - Correcto                    | 200 - Correcto                   | 200 - Correcto                  |
| Deteniendo | 503 - No disponible           | 200 - Correcto                   | 503 - No disponible         |
| Inactivo     | 503 - No disponible           | 503 - No disponible          | 503 - No disponible         |
| Con errores  | 500 - Error del servidor          | 500 - Error del servidor         | 500 - Error del servidor        |

## Adición de una comprobación de estado a una app de Swift existente
{: #existing-app}

La biblioteca [Health](https://github.com/IBM-Swift/Health){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") facilita la adición de una comprobación de estado a la aplicación Swift. Las comprobaciones de estado son ampliables. Para obtener más información sobre el [almacenamiento en memoria caché](https://github.com/IBM-Swift/Health#caching){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") para evitar ataques de denegación de servicio (DoS) o añadir [comprobaciones personalizadas](https://github.com/IBM-Swift/Health#implementing-a-health-check){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"), consulte la biblioteca [Health](https://github.com/IBM-Swift/Health){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

Para añadir la biblioteca de estado a una app Swift existente, consulte los pasos siguientes:

1. Especifíquela en la sección *dependencies:* del archivo `Package.swift` y añádela a todos los destinos adecuados:

    ```swift
    .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
    ```
    {: codeblock}

2. Añada el código de inicialización siguiente a la aplicación:

    ```swift
    import Health

    let health = Health()
    ```
    {: codeblock}

3. Añada la definición de ruta para definir el punto final de comprobación de estado:

    ```swift
    router.get("/health") { request, response, next in
        /* let status = health.status.toDictionary() */
        let status = health.status.toSimpleDictionary()
        if health.status.state == .UP {
            try response.send(json: status).end()
  } else {
            try response.status(.serviceUnavailable).send(json: status).end()
        }
    }
    ```
    {: codeblock}

4. Compruebe el estado de la app con un navegador accediendo al punto final `/health`. El código devuelve una carga útil `{"status": "UP"}`, tal como define el diccionario simple.

## Comprobación del estado de una app Swift de kit de inicio del lado del servidor
{: #healthcheck-starterkit}

Al generar una app Swift basada en Kitura utilizando un kit de inicio, se incluirá de forma predeterminada un punto final de comprobación de estado básico, `/health`. El punto final utiliza el protocolo Codable disponible en Swift 4, tal como se admite en la biblioteca [Health](https://github.com/IBM-Swift/Health){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

El código de inicialización básico, como la inicialización del objeto Health, se produce en `Sources/Application.swift`. El propio punto final de comprobación de estado se proporciona mediante el archivo `/Sources/Application/Routes/HealthRoutes.swift`, que utiliza el código siguiente:

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

El ejemplo utiliza el diccionario estándar, que produce una carga útil como:
```
{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}
```

al acceder al punto final `/health`.

## Recomendaciones para pruebas de actividad y preparación
{: #recommend-probes}

Las comprobaciones deben incluir la viabilidad de conexiones a servicios en sentido descendente en el resultado cuando no existe ninguna reserva aceptable cuando el servicio en sentido descendente no está disponible. Esto no implica llamar a la comprobación de estado que proporciona directamente el servicio en sentido descendente, puesto que la infraestructura realiza la comprobación. En su lugar, considere la posibilidad de verificar el estado de las referencias existentes que tiene la aplicación en los servicios en sentido descendente: puede ser una conexión JMS a WebSphere MQ o un consumidor o productor Kafka inicializado. Si comprueba la viabilidad de referencias internas en servicios en sentido descendente, almacene en memoria caché el resultado para minimizar el impacto que tiene la comprobación de estado en la aplicación.

Una prueba de actividad, por el contrario, puede tener en cuenta lo que se comprueba, ya que un error puede provocar una terminación inmediata del proceso. Un punto final HTTP simple que siempre devuelva `{"status": "UP"}` con el código de estado `200` es una elección razonable.

### Adición de soporte de preparación y actividad de Kubernetes a una app Swift
{: #kube-readiness-swift}

Para implementaciones alternativas, como por ejemplo utilizar **Codable** o el diccionario estándar, consulte los [ejemplos de la biblioteca Health](https://github.com/IBM-Swift/Health#usage){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"). Algunas de estas implementaciones simplifican la creación de comprobaciones de estado ampliables con soporte para la colocación en memoria caché de comprobaciones que se realizan en los servicios de reserva. En este caso, probablemente desee separar la prueba de actividad simple en el ejemplo de la comprobación de preparación, que es más detallada y potente.

## Configuración de pruebas de actividad y preparación en Kubernetes
{: #config-kube-readiness}

Declare las pruebas de actividad y preparación junto con el despliegue de Kubernetes. Ambas pruebas utilizan los mismos parámetros de configuración:

* El kubelet espera a `initialDelaySeconds` antes de la primera prueba.

* El kubelet prueba el servicio cada `periodSeconds` segundos. El valor predeterminado es 1.

* La prueba expira pasados `timeoutSeconds` segundos. El valor mínimo y valor predeterminado es 1.

* La prueba se realiza correctamente si es satisfactoria `successThreshold` veces tras un error. El valor predeterminado y mínimo es 1. El valor debe ser 1 en las pruebas de actividad.

* Cuando se inicia un pod y la prueba falla, Kubernetes intenta reiniciar el pod `failureThreshold` veces y luego abandona. El valor mínimo es 1 y el valor predeterminado es 3.
    - En una prueba de actividad, "abandonar" significa reiniciar el pod.
    - En una prueba de preparación, "abandonar" significa marcar el pod como `Unready` (no preparado).

Para evitar los ciclos de reinicio, establezca `livenessProbe.initialDelaySeconds` para que sea seguro más tiempo del que necesita el servicio para inicializarse. Puede utilizar un valor más corto para `readinessProbe.initialDelaySeconds`, para direccionar solicitudes al servicio en cuanto esté listo.

Consulte el siguiente ejemplo de archivo `yaml`:
```yaml
spec:
  containers:
  - name: ...
    image: ...
    readinessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 120
      timeoutSeconds: 5
    livenessProbe:
      httpGet:
        path: /liveness
        port: 8080
      initialDelaySeconds: 130
      timeoutSeconds: 10
      failureThreshold: 10
```

Para obtener más información, consulte cómo [Configurar pruebas de actividad y comprobación](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").
