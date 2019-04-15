---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: watson studio swift, machine learning swift, custom model swift, data set swift, predictive swift, watson api swift, generated model swift, dataset swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 使用定制生成的模型分析数据集
{: #dsx-overview}

Watson Studio 提供了环境和多种工具，用于通过以协作方式分析数据来解决业务问题。您可以选择分析、清理和组织数据所需的工具。了解如何摄入流式数据，或者创建、训练和部署机器学习模型。Watson Studio 可与各种不同的 {{site.data.keyword.cloud}} 服务以及 Watson Knowledge Catalog 集成，提供策略管理来控制资产，并提供目录来建立索引以查找这些资产。请在 https://dataplatform.ibm.com/ 上了解更多信息。

Watson Studio 围绕基于项目的体系结构进行构建，可对资源进行组织以解决业务问题。资源包括与云和内部部署数据存储的连接、数据文件、合作者和分析资产（如模型）。有关更多信息，请参阅 [Watson Studio 概述](https://datascience.ibm.com/docs/content/getting-started/overview-ws.html?context=analytics){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。

## {{site.data.keyword.DSX}} 的机器学习
{: #dsx-learning}

通过使用 {{site.data.keyword.DSX}}，可以对模型进行训练并部署这些模型，然后通过 API 来使用结果。随后，可以在 iOS 或 Swift 应用程序中使用这些 API。

通过 IBM Watson Machine Learning，在设置环境之后，可以创建模型，将其部署到云，并对其进行训练。有关更多信息，请参阅[使用 {{site.data.keyword.pm_full}} 和 {{site.data.keyword.DSX}}](https://datascience.ibm.com/docs/content/analyze-data/wml-ai.html?context=analytics){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 创建、部署和训练模型。

### 教程
{: #dsx-tutorials}

- [使用 {{site.data.keyword.pm_short}} 构建 Logistic 回归模型](https://datascience.ibm.com/docs/content/analyze-data/ml-example-log-regress.html?context=analytics){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")
- [使用 {{site.data.keyword.pm_short}} 构建朴素贝叶斯模型](https://datascience.ibm.com/docs/content/analyze-data/ml-example-naive-bayes.html?context=analytics){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")

## 设置 {{site.data.keyword.DSX}} 以用于 iOS 和 Swift
{: #dsx_ios}

1. 要更轻松地集成凭证，必须将 {{site.data.keyword.pm_short}} 实例添加到 iOS 应用程序或后端应用程序。为了方便访问，凭证会包含在项目仪表板中。

![应用程序中的 Machine Learning](images/ios-machinelearning-app.png)

2. 下载应用程序代码。
3. 初始化
  * 对于 iOS 项目，只需将 {{site.data.keyword.pm_short}} 服务添加到 iOS 项目，就会立即将凭证注入到应用程序中。
    要通过应用程序访问凭证，请复制并粘贴以下代码片段。此外，请确保将评分端点添加到应用程序，可以在模型的部署`实现`选项卡中找到该端点。

    ```swift
    /* The url to your model's scoring endpoint */
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"

    /* Your credentials */
    var machineLearningUsername: String!
    var machineLearningPassword: String!

    /* Machine Learning initialization */
    if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"),
       let dictionary = NSDictionary(contentsOfFile: contents),
       let username = dictionary["machinelearningUsername"] as? String,
       let password = dictionary["machinelearningPassword"] as? String {
          

           machineLearningUsername = username
           machineLearningPassword = password

    }
    ```
    {: codeblock}

  * 对于服务器端应用程序，手动将您的用户名和密码添加到应用程序，同时添加评分端点，可以在模型的部署`实现`选项卡中找到该端点。

    ```swift
    /* Your Machine Learning Credentials */
    let machineLearningUsername: String = "<your-ml-service-username>"
    let machineLearningPassword: String = "<your-ml-service-password>"

    /* The url to your model's scoring endpoint */
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"
    ```
    {: codeblock}

4. 检索访问令牌，并使用简单客户端 SDK 通过应用程序对数据集执行预测分析。

  ```swift
  public class MachineLearning {

      private let username: String
      private let password: String

      public init(username: String, password: String) {
          self.username = username
          self.password = password
      }

      public static func failure(_ error: Error) {
          print("Error:", error.localizedDescription)
      }

      public func retrieveToken(failure: @escaping (Error) -> Void = MachineLearning.failure, success: @escaping (String) -> Void) {

          guard var request = createRequest(url: "https://ibm-watson-ml.mybluemix.net/v3/identity/token") else {
              failure(NSError(domain: "Could not create url", code: 1, userInfo: nil))
              return
          }

          guard let authString = (username + ":" + password).data(using: .utf8)?.base64EncodedString() else {
              failure(NSError(domain: "Could not encode credentials", code: 1, userInfo: nil))
              return
          }

          request.setValue("Basic \(authString)", forHTTPHeaderField: "Authorization")

          execute(request, failure: failure) { dict in

              guard let token = dict["token"] as? String else {
                  failure(NSError(domain: "Invalid Response", code: 1, userInfo: nil))
                  return
              }

              success(token)
          }
      }

      public func retrieveScore(_ scoringUrl: String, token: String, payload: [String: Any], failure: @escaping (Error) -> Void  = failure, success: @escaping ([String: Any]) -> Void) {

          guard let data = try? JSONSerialization.data(withJSONObject: payload, options: .prettyPrinted) else {
              failure(NSError(domain: "Could not encode data", code: 1, userInfo: nil))
              return
          }

          guard var request = createRequest(method: "POST", url: scoringUrl, body: data) else {
              failure(NSError(domain: "Could not create url", code: 1, userInfo: nil))
              return
          }

          request.setValue("Bearer " + token, forHTTPHeaderField: "Authorization")
          request.setValue("application/json", forHTTPHeaderField: "Content-Type")

          execute(request, failure: failure, success: success)
      }

      public func createRequest(method: String = "GET", url: String, body: Data? = nil) -> URLRequest? {
          guard let url = URL(string: url) else {
              return nil
          }
          var req = URLRequest(url: url)
          req.httpMethod = method
          req.httpBody = body
          return req
      }

      private func execute(_ request: URLRequest, failure: @escaping (Error) -> Void, success: @escaping ([String: Any]) -> Void) {
          URLSession.shared.dataTask(with: request) { data, _, error in

              guard let data = data, error == nil else {
                  let error = error ?? NSError(domain: "No data was returned", code: 1, userInfo: nil)
                  failure(error)
                  return
              }

              guard let body = try? JSONSerialization.jsonObject(with: data, options: .allowFragments) as? [String: Any],
                    let dict = body else {
                  failure(NSError(domain: "Could not create dictionary", code: 1, userInfo: nil))
                  return
              }

              guard let resp = response as? HTTPURLResponse , resp.statusCode == 200 else {
                 let msg = (dict["errors"] as? [[String: Any]])?.first?["message"] as? String ?? "An error occurred"
                 let code = (response as? HTTPURLResponse)?.statusCode ?? 1
                 failure(NSError(domain: msg, code: code, userInfo: nil))
                 return
             }

              success(dict)

          }.resume()
      }
  }
  ```
  {: codeblock}

### 示例
{: #dsx-example}

**方案名称：**产品系列预测

**方案描述：**销售室外设备的公司构建并部署了一个模型，用于预测客户对其产品系列的兴趣。您的任务是针对部署的模型发出评分请求。

部署模型后，可以使用评分端点来执行预测分析。

```swift
// 要分析的数据
let examplePayload: [String: Any] = [
    "fields": ["GENDER", "AGE", "MARITAL_STATUS", "PROFESSION"],
    "values": [["M", 23, "Single", "Student"],["M", 55, "Single", "Executive"]]
]

// 您的服务凭证
let machineLearningUsername: String = "<your-ml-service-username>"
let machineLearningPassword: String = "<your-ml-service-password>"

// 您的评分端点
let scoringUrl: String = "<your-model-scoring-url>"

// 定义客户端
let client = MachineLearning(username: username, password: password)

// 检索访问令牌并对有效内容评分！
client.retrieveToken { token in
    client.retrieveScore(scoringUrl, token: token, payload: examplePayload) { dict in
        print(dict)
    }
}
```
{: codeblock}

## 后续步骤
{: #dsx_next notoc}

太棒了！现在，您可以使用定制生成的机器学习模型来分析数据集。请一鼓作气，在 [Data science and machine learning](https://www.ibm.com/analytics/data-science/machine-learning){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 中了解 {{site.data.keyword.pm_short}} 可以提供的功能的更多信息。

### 相关链接
{: #dsx-related}

* [{{site.data.keyword.pm_short}}](/docs/services/PredictiveModeling?topic=services/PredictiveModeling-WMLgettingstarted#using-machine-learning-with-data-science-experience)
* [{{site.data.keyword.DSX}}](https://datascience.ibm.com/){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")
* [{{site.data.keyword.DSX}} 文档](https://datascience.ibm.com/docs/content/getting-started/welcome-main.html?context=analytics){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")
