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

# Using a health check in your Swift app
{: #healthcheck}

Health checks provide a simple mechanism to determine whether or not a serverside application is behaving properly. You can add a health check endpoint to your existing server-side Swift application quickly to aid in monitoring the wellbeing of your app.

Health checks are typically consumed over `HTTP`. Many deployment environments, such as Cloud Foundry or Kubernetes, and infrastructure cloud services like load balancers can be configured to poll the health endpoint periodically. Standard return codes are used for checking the UP or DOWN status, returning 200 for UP, and 5xx for DOWN. Following convention, the response, or payload, is returned in a `JSON` format. You can also cache the health check endpoint at a configurable interval to prevent DoS on your application.

## Adding health check to an existing Swift app
{: #add-healthcheck-existing}

The [Health](https://github.com/IBM-Swift/Health) library makes it easy add a health check to your Swift application. Health checks are extensible. For more information about [caching](https://github.com/IBM-Swift/Health#caching) to prevent DoS attacks or adding [custom checks](https://github.com/IBM-Swift/Health#implementing-a-health-check), see the [Health](https://github.com/IBM-Swift/Health) library.

To add the Health library to an existing Swift app, specify it in the *dependencies:* section of your `Package.swift` file, making sure to add it to any targets where it is used:
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

Check the status of the app with a browser by accessing the `/health` endpoint. The code returns a payload `["status": "UP"]`, as defined by the simple dictionary.

For alternate implementations, such as using **Codable** or the standard dictionary, see the [Health library examples](https://github.com/IBM-Swift/Health#usage).

## Accessing health check from server-side Swift Starter Kit apps
{: #healthcheck-starterkit}

When you generate a Kitura-based Swift app by using a Starter Kit, a basic health check endpoint (`/health`) is included by default. The endpoint leverages the Codable protocol available in Swift 4, as supported by the [Health](https://github.com/IBM-Swift/Health) library.

Basic initialization code, such as the initialization of the Health object occurs in `Sources/Application.swift`, while the health check endpoint is provided by the `/Sources/Application/Routes/HealthRoutes.swift` file, which contains the following code:
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

The example uses the standard dictionary, which yields a payload such as `["status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"]` when you access the `/health` endpoint.
