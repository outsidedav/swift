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

# 在 Swift 應用程式中使用性能檢查
{: #healthcheck}

性能檢查提供一個簡單的機制，可判定伺服器端應用程式是否適當地運作。許多部署環境（例如 [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) 及 [Kubernetes](https://www.ibm.com/cloud/container-service)）都可以配置成定期輪詢性能端點，以判定您的服務實例是否已備妥可以接受資料流量。

性能檢查一般是透過 HTTP 進行，並使用標準回覆碼來指出 `UP` 或 `DOWN` 狀態。範例包括：傳回 `200` 表示 `UP`，傳回 `5xx` 表示 `DOWN`。更明確地說，當應用程式無法處理要求，或者尚未啟動（服務無法使用）時，會使用 `503`，而當伺服器發現錯誤狀況時，則會使用 `500`。性能檢查的回覆值是可變的，但是最小 JSON 回應（例如 `{“status”: “UP”}`）則是一致的。

Cloud Foundry 使用一個性能端點來指出服務實例是否可以處理要求。在 Kubernetes 中，性能檢查端點等於 `readiness` 端點。您的應用程式必須定義這個端點，以協助判定自動遞送決策。在不接受撒回的事件中，必要下游服務的考量會影響此端點的成敗。如果您確實檢查下游服務，則快取結果有時會很有用，可將因為性能檢查而造成的系統整體負載降至最低。

Kubernetes 會定義額外的 [liveness](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) 端點，這容許應用程式指出是否要重新啟動處理程序。此處適用一個考量因素子集：一旦達到特定的記憶體使用率臨界值（舉例說明），`liveness` 檢查可能就會失敗。如果您的應用程式是在 Kubernetes 上執行，請考慮新增 `liveness` 端點，以確保處理程序在必要時重新啟動。

## 將性能檢查新增至現有 Swift 應用程式
{: #add-healthcheck-existing}

[性能](https://github.com/IBM-Swift/Health)程式庫可輕鬆地將性能檢查新增至您的 Swift 應用程式。性能檢查是可延伸的。如需[快取](https://github.com/IBM-Swift/Health#caching)以防止 DoS 攻擊或新增[自訂檢查](https://github.com/IBM-Swift/Health#implementing-a-health-check)的相關資訊，請參閱[效能](https://github.com/IBM-Swift/Health)程式庫。

若要將「性能」程式庫新增至現有 Swift 應用程式，請在 `Package.swift` 檔案的 *dependencies:* 區段中指定該程式庫，並確定要將它新增至所有使用它的目標中：
```swift
  .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
```
{: codeblock}

然後，將下列起始設定碼新增至您的應用程式：
```swift
import Health

let health = Health()
```
{: codeblock}

然後，新增路徑定義，以定義性能檢查端點：
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

使用瀏覽器，透過存取 `/health` 端點的方式，來檢查應用程式的狀態。程式碼會傳回有效負載 `{"status": "UP"}`，如簡單字典所定義。

若為替代的實作，例如，使用 **Codable** 或標準字典，請參閱[性能程式庫範例](https://github.com/IBM-Swift/Health#usage)。

## 透過伺服器端 Swift 入門範本套件應用程式存取性能檢查
{: #healthcheck-starterkit}

當您使用「入門範本套件」產生 Kitura 型 Swift 應用程式時，依預設會包括基本性能檢查端點 `/health`。此端點運用 Swift 4 中可用的 Codable 通訊協定，如[性能](https://github.com/IBM-Swift/Health)程式庫所支援。

基本起始設定碼（例如，起始設定「性能」物件）發生在 `Sources/Application.swift` 中，而性能檢查端點則由 `/Sources/Application/Routes/HealthRoutes.swift` 檔案提供，其中包含下列程式碼：
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
