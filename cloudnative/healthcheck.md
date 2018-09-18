---

copyright:
  years: 2018
lastupdated: "2018-09-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Using a health check in your Swift app
{: #healthcheck}

Health checks provide a simple mechanism to determine whether a server-side application is behaving properly. Many deployment environments, such as [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) and [Kubernetes](https://www.ibm.com/cloud/container-service), can be configured to poll the health endpoints periodically to determine whether an instance of your service is ready to accept traffic.

## Health check overview
{: #overview}

Health checks are typically accessed over `HTTP`, and use standard return codes for indicating `UP` or `DOWN` status. Examples include returning `200` for `UP`, and `5xx` for `DOWN`. For example, a `503` return code is used when the application can’t handle requests, and a `500` is used when the server experiences an error condition. The return value of a health check is variable, but a minimal JSON response, like `{“status”: “UP”}` provides consistency.

Kubernetes defines both [liveness](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) and [readiness](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) probes:

* The liveness probe allows an application to indicate whether the process is to be restarted. A subset of considerations applies: a `liveness` check might fail if a memory threshold is reached, for example. If your app is running on Kubernetes, consider adding a `liveness` endpoint to ensure that your process is restarted when necessary.

* The readiness probe is used for automatic routing decisions. The success or failure indicates whether not the application can receive new work. This endpoint can include considerations for required downstream services. Consider caching the result of downstream service checks to minimize overall load on the system (for example, test database connectivity at most once per second).

Cloud Foundry uses only one health endpoint. If this check fails, Cloud Foundry restarts the process. But if it succeeds, Cloud Foundry assumes that the process can handle new work. This health check makes the single endpoint a combination between liveness and readiness.

## Adding health check to an existing Swift app
{: #add-healthcheck-existing}

The [Health](https://github.com/IBM-Swift/Health) library makes it easy add a health check to your Swift application. Health checks are extensible. For more information about [caching](https://github.com/IBM-Swift/Health#caching) to prevent DoS attacks or adding [custom checks](https://github.com/IBM-Swift/Health#implementing-a-health-check), see the [Health](https://github.com/IBM-Swift/Health) library.

To add the Health library to an existing Swift app, specify it in the *dependencies:* section of your `Package.swift` file, and adding it to all appropriate targets:
```swift
.package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
```
{: codeblock}

Then, add the following initialization code to your application:
```swift
import Health

let health = Health()
```
{: codeblock}

Then, add the route definition to define the health check endpoint:
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

Check the status of the app with a browser by accessing the `/health` endpoint. The code returns a payload `{"status": "UP"}`, as defined by the simple dictionary.

For alternative implementations, such as using **Codable** or the standard dictionary, see the [Health library examples](https://github.com/IBM-Swift/Health#usage).

## Accessing health check from server-side Swift Starter Kit apps
{: #healthcheck-starterkit}

When you generate a Kitura-based Swift app by using a Starter Kit, a basic health check endpoint, `/health`, is included by default. The endpoint uses the Codable protocol available in Swift 4, as supported by the [Health](https://github.com/IBM-Swift/Health) library.

Basic initialization code, such as the initialization of the Health object occurs in `Sources/Application.swift`. While the health check endpoint itself is provided by the `/Sources/Application/Routes/HealthRoutes.swift` file, and uses the following code:
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

The example uses the standard dictionary, which yields a payload such as `{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}` when you access the `/health` endpoint.
