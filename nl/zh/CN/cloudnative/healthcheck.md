---

copyright:
  years: 2018
lastupdated: "2018-11-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 在 Swift 应用程序中使用运行状况检查
{: #healthcheck}

运行状况检查提供了一种用于确定服务器端应用程序是否运行正常的简单机制。您可以将云环境（如 [Kubernetes](https://www.ibm.com/cloud/container-service) 和 [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry)）配置为定期轮询运行状况端点，以确定您的服务实例是否已准备好接受流量。
{: shortdesc}

## 运行状况检查概述
{: #overview}

运行状况检查提供了一种用于确定服务器端应用程序是否运行正常的简单机制。通常会通过 HTTP 来进行运行状况检查，并使用标准返回码来指示 UP 或 DOWN 状态。运行状况检查的返回值是可变的，通常是最短的 JSON 响应，例如 `{"status": "UP"}`。

Kubernetes 提供了细致入微的进程运行状况检查。它定义了两个探测器：

- _**就绪情况**_探测器用于指示进程能否处理请求（可否路由）。

  Kubernetes 不会将工作路由到就绪情况探测器失败的容器。如果服务未完成初始化，或者处于忙碌状态、超负荷或无法处理请求，那么就绪情况探测器可能会失败。

- _**活跃度**_探测器用于指示是否要重新启动进程。

  Kubernetes 会停止并重新启动活跃度探测器失败的容器。在某些情况下，您可以使用活跃度探测器来清除处于不可恢复状态的进程，例如，内存耗尽，或容器在内部进程崩溃后未正确停止。

相比之下，Cloud Foundry 使用一个运行状况端点。如果此检查失败，那么会重新启动进程；如果成功，那么会将请求路由到进程。在此环境中，当进程处于活动状态时，端点的成功程度最低。为了避免反复重新启动，您可以对初始等待时间进行配置，以延迟运行状况检查，直到服务完成初始化为止。

下表提供了就绪情况、活跃度和单一运行状况端点可提供的响应的相关信息。

| 状态     | 就绪情况                    | 活跃度                      | 运行状况                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | 非正常，不装入              | 非正常，重新启动            | 非正常，重新启动            |
| 正在启动 | 503 - 不可用                | 200 - 正常                  | 使用延迟来避免测试          |
| 启动     | 200 - 正常                  | 200 - 正常                  | 200 - 正常                  |
| 正在停止 | 503 - 不可用                | 200 - 正常                  | 503 - 不可用                |
| 关闭     | 503 - 不可用                | 503 - 不可用                | 503 - 不可用                |
| 出错     | 500 - 服务器错误            | 500 - 服务器错误            | 500 - 服务器错误            |

## 向现有 Swift 应用程序添加运行状况检查
{: #add-healthcheck-existing}

通过 [Health](https://github.com/IBM-Swift/Health) 库，可以轻松地将运行状况检查添加到 Swift 应用程序。运行状况检查可扩展。有关[高速缓存](https://github.com/IBM-Swift/Health#caching)以防止 DoS 攻击或添加[定制检查](https://github.com/IBM-Swift/Health#implementing-a-health-check)的更多信息，请参阅 [Health](https://github.com/IBM-Swift/Health) 库。

要将 Health 库添加到现有 Swift 应用程序中，请参阅以下步骤：

1. 在 `Package.swift` 文件的 *dependencies:* 部分中指定该库，然后将其添加到所有适当的目标：

    ```swift
  .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
```
    {: codeblock}

2. 将以下初始化代码添加到应用程序中：

    ```swift
import Health



    let health = Health()
```
    {: codeblock}

3. 添加路由定义，以定义运行状况检查端点：

    ```swift
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

4. 使用浏览器通过访问 `/health` 端点来检查应用程序的状态。代码会返回有效内容 `{"status": "UP"}`，如简单字典所定义。

## 检查服务器端 Swift 入门模板工具包应用程序的运行状况
{: #healthcheck-starterkit}

使用入门模板工具包生成基于 Kitura 的 Swift 应用程序时，缺省情况下会包含基本运行状况检查端点 `/health`。该端点使用 Swift 4 中提供的 Codable 协议，而 [Health](https://github.com/IBM-Swift/Health) 库支持此协议。

基本初始化代码（例如，初始化 Health 对象）在 `Sources/Application.swift` 中执行。运行状况检查端点本身由 `/Sources/Application/Routes/HealthRoutes.swift` 文件提供，其中包含以下代码：

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

## 针对就绪情况和活跃度探测器的建议

当下游服务不可用时，如果不存在可接受的回退，那么就绪情况检查必须在其结果中包含与下游服务进行连接的可行性。这并不意味着要调用下游服务直接提供的运行状况检查，因为基础架构会为您执行该项检查。请考虑验证应用程序对下游服务的现有引用的运行状况：可能是与 WebSphere MQ 的 JMS 连接，也可能是已初始化的 Kafka 使用者或生产者。如果您需要检查对下游服务的内部引用的可行性，请对结果进行高速缓存，以最小化运行状况检查对应用程序的影响。

相比之下，您需要慎重考虑活跃度探测器的检查内容，因为一旦失败即会立即终止进程。较为合理的做法是使用一个简单的 HTTP 端点，让它始终返回状态码为 `200` 的 `{"status": "UP"}`。

### 向 Swift 应用程序添加对 Kubernees 就绪情况和活跃度的支持

有关备用实施方案（例如，使用 **Codable** 或标准字典）的信息，请参阅 [Health 库示例](https://github.com/IBM-Swift/Health#usage)。某些备用实施方案简化了对可扩展运行状况检查的创建，并且支持高速缓存对后备服务执行的检查。在此场景中，您需要将简单的活跃度测试与更稳健的详细就绪情况检查分隔开。

## 在 Kubernees 中配置就绪情况和活跃度探测器

您可以将活跃度和就绪情况探测器与您的 Kubernetes 部署一起声明。这两个探测器使用的配置参数相同：

* 在进行首次探测之前，kubelet 会等待 `initialDelaySeconds`。

* kubelet 会每 `periodSeconds` 秒探测一次服务。缺省值为 1。

* 探测会在 `timeoutSeconds` 秒之后超时。缺省值和最小值都是 1。

* 如果探测在失败后成功 `successThreshold` 次，那么表示探测成功。缺省值和最小值都是 1。对于活跃度探测器，该值必须为 1。

* 如果 pod 启动但探测失败，Kubernetes 会尝试 `failureThreshold` 次来重新启动 pod，然后放弃。最小值为 1，缺省值为 3。
    - 对于活跃度探测器，“放弃”意味着重新启动 pod。
    - 对于就绪情况探测器，“放弃”意味着将 pod 标记为`未就绪`。

为了避免反复重新启动，请将 `livenessProbe.initialDelaySeconds` 设置为比服务初始化所需时间更长的时间。然后，对 `readinessProbe.initialDelaySeconds` 使用较小的值，以便在准备就绪后立即将请求路由到服务。

请参阅以下 `yaml` 示例：
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

有关更多信息，请参阅 [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)。
