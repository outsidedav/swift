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

# 在 Swift 應用程式中使用性能檢查
{: #healthcheck}

性能檢查提供一種簡單機制，可判定伺服器端應用程式是否適當地運作。雲端環境（例如 [Kubernetes](https://www.ibm.com/cloud/container-service) 及 [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry)）可以配置為定期輪詢性能端點，以判斷服務實例是否準備好接受資料流量。
{: shortdesc}

## 性能檢查概觀
{: #overview}

性能檢查提供一種簡單機制，可判定伺服器端應用程式是否適當地運作。它們通常是透過 HTTP 耗用，並使用標準回覆碼來指出 UP 或 DOWN 狀態。性能檢查的回覆值是可變的，但是最小 JSON 回應（例如 `{"status": "UP"}`）是相同的。

Kubernetes 具有細緻入微的處理程序性能記號。它定義兩個探測：

- _**readiness**_ 探測用來指出處理程序是否可以處理要求（可遞送）。

  Kubernetes 不會將工作遞送至無法通過 readiness 探測的容器。readiness 探測可能失敗的情況包括服務未完成起始設定，或是因為其他因素而忙碌、超載或無法處理要求。

- _**liveness**_ 探測用來指出處理程序是否要重新啟動。

  Kubernetes 會停止並重新啟動具有失敗 liveness 探測的容器。使用 liveness 探測可清除處於無法復原狀態的處理程序，例如耗盡記憶體，或是內部處理程序當機之後容器未適當地停止。

作為比較的附註，Cloud Foundry 使用一個性能端點。如果這項檢查失敗，處理程序會重新啟動，但如果成功，要求便會遞送給它。在此環境中，端點至少會在處理程序存活時成功。已配置了起始等待時間，以便延遲性能檢查，直到服務完成起始設定為止，以避免重新啟動的循環。

下表提供 readiness、liveness 及單一性能端點可以提供之回應的指引。

|狀態| Readiness                   | Liveness                   |性能|
|----------|-----------------------------|----------------------------|---------------------------|
|          | 非 OK 會導致不載入| 非 OK 會導致重新啟動| 非 OK 會導致重新啟動|
| Starting | 503 - 無法使用| 200 - OK                   |使用延遲以避免測試|
| Up       | 200 - OK                   | 200 - OK                   | 200 - OK                   |
| Stopping | 503 - 無法使用| 200 - OK                   | 503 - 無法使用|
| Down     | 503 - 無法使用| 503 - 無法使用| 503 - 無法使用|
| Errored  | 500 - 伺服器錯誤| 500 - 伺服器錯誤| 500 - 伺服器錯誤|

## 將性能檢查新增至現有 Swift 應用程式
{: #add-healthcheck-existing}

[性能](https://github.com/IBM-Swift/Health)程式庫可輕鬆地將性能檢查新增至您的 Swift 應用程式。性能檢查是可延伸的。如需[快取](https://github.com/IBM-Swift/Health#caching)以防止 DoS 攻擊或新增[自訂檢查](https://github.com/IBM-Swift/Health#implementing-a-health-check)的相關資訊，請參閱[效能](https://github.com/IBM-Swift/Health)程式庫。

若要將「性能」檔案庫新增至現有的 Swift 應用程式，請參閱下列步驟：

1. 在 `Package.swift` 檔案的 *dependencies:* 區段中指定它，並將它新增至所有適當的目標：

    ```swift
    .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
    ```
    {: codeblock}

2. 將下列起始設定碼新增至您的應用程式：

    ```swift
    import Health

    let health = Health()
    ```
    {: codeblock}

3. 新增路徑定義，以定義性能檢查端點：

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

4. 使用瀏覽器，透過存取 `/health` 端點的方式，來檢查應用程式的狀態。程式碼會傳回有效負載 `{"status": "UP"}`，如簡單字典所定義。

## 檢查伺服器端 Swift 入門範本套件應用程式的性能
{: #healthcheck-starterkit}

當您使用「入門範本套件」產生 Kitura 型 Swift 應用程式時，依預設會包括基本性能檢查端點 `/health`。此端點使用 Swift 4 中可用的 Codable 通訊協定，如[性能](https://github.com/IBM-Swift/Health)程式庫所支援。

基本起始設定碼（例如，起始設定「性能」物件）發生在 `Sources/Application.swift` 中。性能檢查端點本身由 `/Sources/Application/Routes/HealthRoutes.swift` 檔案提供，並使用下列程式碼：

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

此範例使用標準字典，當您存取 `/health` 端點時，該字典會產生有效負載，例如 `{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}`。

## readiness 及 liveness 探測的建議

如果下游服務無法使用時沒有可接受的撤回措施存在，就緒檢查必須在其結果中包含下游服務連線的可行性。這並不表示直接呼叫下游服務所提供的性能檢查，因為基礎架構會為您檢查。相反地，請考慮驗證您應用程式對下游服務之現有參照的性能：這可能是與 WebSphere MQ 的 JMS 連線，或已起始設定的 Kafka 消費者或生產者。如果您檢查了對下游服務之內部參照的可行性，請將結果予以快取，將性能檢查對您應用程式的影響降至最低。

相反地，liveness 探測對於檢查的對象可能很慎重，因為失敗會導致立即終止處理程序。簡單的 http 端點、一律傳回 `{"status": "UP"}` 與狀態碼 `200`，便是一個合理的選項。

### 新增 Kubernetes 就緒及存活的支援給 Swift 應用程式

若為替代的實作，例如，使用 **Codable** 或標準字典，請參閱[性能程式庫範例](https://github.com/IBM-Swift/Health#usage)。這些實作中有一部分能簡化可延伸性能檢查的建立，因為它們能支援快取針對支援服務所執行的檢查。在此情境中，建議您區分簡單的存活測試，與較強大而詳細的就緒檢查。

## 在 Kubernetes 中配置 readiness 和 liveness 探測

請與 Kubernetes 部署同時宣告 liveness 和 readiness 探測。兩種探測都使用相同的配置參數：

* kubelet 在第一個探測之前等待 `initialDelaySeconds`。

* kubelet 每隔 `periodSeconds` 秒探測一次服務。預設值為 1。

* 探測會在 `timeoutSeconds` 秒之後逾時。預設及最小值為 1。

* 如果在失敗之後成功 `successThreshold` 次，探測便成功。預設及最小值為 1。liveness 探測的值必須為 1。

* 當 Pod 啟動而探測失敗時，Kubernetes 會嘗試重新啟動 Pod `failureThreshold` 次，然後放棄。最小值為 1，預設值為 3。
    - 針對 liveness 探測，「放棄」表示會重新啟動 Pod。
    - 針對 readiness 探測，「放棄」表示會將 Pod 標示為 `Unready`。

為了避免重新啟動循環，請將 `livenessProbe.initialDelaySeconds` 設為比起始設定服務更長，且長度必須足夠安全。然後您便可以針對 `readinessProbe.initialDelaySeconds` 使用較短的值，只要服務就緒便將要求遞送給服務。

請參閱下列 `yaml` 範例：
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

如需相關資訊，請參閱如何 [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)。
