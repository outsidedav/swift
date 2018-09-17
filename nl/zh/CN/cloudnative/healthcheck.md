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

# 在 Swift 应用程序中使用运行状况检查
{: #healthcheck}

运行状况检查提供了简单的机制来确定服务器端应用程序是否在正常运行。许多部署环境（例如，[Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) 和 [Kubernetes](https://www.ibm.com/cloud/container-service)）可以配置为定期轮询运行状况端点，以确定服务实例是否已准备好接受流量。

运行状况检查通常通过 HTTP 使用，并使用标准返回码来指示 `UP` 或 `DOWN` 状态。例如，返回 `200` 表示 `UP`，返回 `5xx` 表示 `DOWN`。具体而言，当应用程序无法处理请求或尚未启动（服务不可用）时，将使用 `503`；当服务器遇到错误条件时，将使用 `500`。运行状况检查的返回值是变量，但最低 JSON 响应（如 `{“status”: “UP”}`）提供了一致性。

Cloud Foundry 使用一个运行状况端点来指示服务实例是否可以处理请求。在 Kubernetes 中，运行状况检查端点等同于 `readiness` 端点。应用程序必须定义此端点以帮助确定自动路由决策。此端点的成功或失败可能包括考虑万一没有可接受的回退时所需的下游服务。如果确实检查下游服务，那么对结果进行高速缓存有时很有用，可最大程度地减少由于运行状况检查而导致的系统上的总体负载。

Kubernetes 定义了一个额外的 [liveness](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) 端点，允许应用程序指示进程是否要重新启动。下面是适用的一部分注意事项：例如，达到特定内存利用率阈值后，`liveness` 检查可能会失败。如果应用程序正在 Kubernetes 上运行，请考虑添加 `liveness` 端点，以确保在必要时重新启动进程。

## 向现有 Swift 应用程序添加运行状况检查
{: #add-healthcheck-existing}

通过 [Health](https://github.com/IBM-Swift/Health) 库，可以轻松地将运行状况检查添加到 Swift 应用程序。运行状况检查可扩展。有关[高速缓存](https://github.com/IBM-Swift/Health#caching)以防止 DoS 攻击或添加[定制检查](https://github.com/IBM-Swift/Health#implementing-a-health-check)的更多信息，请参阅 [Health](https://github.com/IBM-Swift/Health) 库。

要将 Health 库添加到现有 Swift 应用程序，请在 `Package.swift` 文件的 *dependencies:* 部分中指定该库，确保将其添加到使用该库的任何目标：
```swift
  .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
```
{: codeblock}

然后，将以下初始化代码添加到应用程序：
```swift
import Health

let health = Health()
```
{: codeblock}

接着，添加路径定义以定义运行状况检查端点：
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

使用浏览器通过访问 `/health` 端点来检查应用程序的状态。代码会返回有效内容 `{"status": "UP"}`，如简单字典所定义。

有关备用实现（例如，使用 **Codable** 或标准字典）的信息，请参阅 [Health 库示例](https://github.com/IBM-Swift/Health#usage)。

## 通过服务器端 Swift 入门模板工具包应用程序访问运行状况检查
{: #healthcheck-starterkit}

使用入门模板工具包生成基于 Kitura 的 Swift 应用程序时，缺省情况下会包含基本运行状况检查端点 `/health`。该端点利用 Swift 4 中提供的 Codable 协议，[Health](https://github.com/IBM-Swift/Health) 库中支持此协议。

基本初始化代码（例如，初始化 Health 对象）在 `Sources/Application.swift` 中执行，而运行状况检查端点由 `/Sources/Application/Routes/HealthRoutes.swift` 文件提供，其中包含以下代码：
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

该示例使用标准字典，在访问 `/health` 端点时，会生成有效内容，例如 `{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}`。
