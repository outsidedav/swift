---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 사용자 정의 생성 모델로 데이터 분석
{: #dsx-overview}

Watson Studio는 협업해서 데이터를 분석하여 비즈니스 문제점을 해결할 수 있는 환경 및 도구를 제공합니다. 데이터를 분석, 정리 및 구성하기 위해 필요한 도구를 선택할 수 있습니다. 스트리밍 데이터를 수집하거나 기계 학습 모델을 작성, 훈련 및 배치하는 방법에 대해 알아보십시오. Watson Studio는 광범위한 {{site.data.keyword.cloud}} 서비스 및 Watson Knowledge Catalog와 통합되며, Watson Knowledge Catalog는 자산 제어를 위한 정책 관리 기능 및 자산을 찾기 위한 색인에 대한 카탈로그를 제공합니다. 자세한 내용은 https://dataplatform.ibm.com/ 을 참조하십시오.

Watson Studio는 비즈니스 문제점을 해결하기 위해 리소스를 구성하는 프로젝트 기반 아키텍처에 따라 구조화됩니다. 리소스에는 클라우드 및 온프레미스 데이터 저장소에 대한 연결, 데이터 파일, 협업자 및 분석 자산(예: 모델)이 포함됩니다. 자세한 내용은 https://datascience.ibm.com/docs/content/getting-started/overview-ws.html?context=analytics 를 참조하십시오.

## {{site.data.keyword.DSX}}용 기계 학습
{: #dsx-learning}

{{site.data.keyword.DSX}}를 사용하면 모델을 훈련하고 배치한 후 API 사용을 통해 결과를 이용할 수 있습니다. 그런 다음 iOS 또는 Swift 애플리케이션에서 API를 사용할 수 있습니다.

IBM Watson Machine Learning을 사용하면 환경을 설정한 후 모델을 작성하고, 클라우드에 배치하고, 훈련할 수 있습니다. 자세한 정보는 [{{site.data.keyword.pm_full}} 및 {{site.data.keyword.DSX}}를 사용하여 모델 작성, 배치 및 훈련](https://datascience.ibm.com/docs/content/analyze-data/wml-ai.html?context=analytics)을 참조하십시오.

### 튜토리얼
{: #dsx-tutorials}

- [{{site.data.keyword.pm_short}}을 사용하여 로지스틱 회귀분석(logistic regression) 모델 빌드](https://datascience.ibm.com/docs/content/analyze-data/ml-example-log-regress.html?context=analytics)
- [{{site.data.keyword.pm_short}}을 사용하여 순수 베이즈(naive Bayes) 모델 빌드](https://datascience.ibm.com/docs/content/analyze-data/ml-example-naive-bayes.html?context=analytics)

## iOS 및 Swift를 사용하여 {{site.data.keyword.DSX}} 설정
{: #dsx_ios}

1. 인증 정보를 좀 더 쉽게 통합하려면 {{site.data.keyword.pm_short}} 인스턴스를 iOS 앱 또는 백엔드 앱에 추가해야 합니다. 접근성을 용이하게 하기 위해 인증 정보가 프로젝트 대시보드에 포함됩니다.

![앱의 기계 학습](images/ios-machinelearning-app.png)

2. 앱 코드를 다운로드하십시오.
3. 초기화하십시오.
  * iOS 프로젝트의 경우 {{site.data.keyword.pm_short}} 리소스를 iOS 프로젝트에 추가하기만 하면 인증 정보가 앱에 즉시 삽입됩니다.
    애플리케이션에서 인증 정보에 액세스하려면 다음 코드 스니펫을 복사하여 붙여넣으십시오. 또한 모델의 배치 `implementation` 탭 내부에서 찾을 수 있는 앱에 스코어링 엔드포인트를 추가해야 합니다.

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

  * 서버 측 애플리케이션의 경우 `implementation` 탭 내부에서 찾을 수 있는 애플리케이션에 사용자 이름 및 비밀번호를 수동으로 추가하십시오.

    ```swift
    /* Your Machine Learning Credentials */
    let machineLearningUsername: String = "<your-ml-service-username>"
    let machineLearningPassword: String = "<your-ml-service-password>"

    /* The url to your model's scoring endpoint */
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"
    ```
    {: codeblock}

4. 액세스 토큰을 검색하고 단순 클라이언트 SDK를 사용하여 애플리케이션에서 데이터 세트에 대한 예측 분석을 수행하십시오.

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

### 예
{: #dsx-example}

**시나리오 이름:** 제품 라인 예측

**시나리오 설명:** 실외 장비를 판매하는 회사는 제품 라인에 대한 고객의 관심사를 예측하기 위해 모델을 빌드하고 배치합니다. 태스크는 배치 모델에 대한 스코어 요청을 작성하는 것입니다.

모델이 배치되면 스코어링 엔드포인트를 사용하여 예측 분석을 수행할 수 있습니다.

```swift
// The data you want to have analyzed
let examplePayload: [String: Any] = [
    "fields": ["GENDER", "AGE", "MARITAL_STATUS", "PROFESSION"],
    "values": [["M", 23, "Single", "Student"],["M", 55, "Single", "Executive"]]
]

// Your service credentials
let machineLearningUsername: String = "<your-ml-service-username>"
let machineLearningPassword: String = "<your-ml-service-password>"

// Your scoring end-point
let scoringUrl: String = "<your-model-scoring-url>"

// Define your client
let client = MachineLearning(username: username, password: password)

// Retrieve your access token and score your payload!
client.retrieveToken { token in
    client.retrieveScore(scoringUrl, token: token, payload: examplePayload) { dict in
        print(dict)
    }
}
```
{: codeblock}

## 다음 단계
{: #dsx_next}

잘 하셨습니다! 이제 사용자 정의 생성 기계 학습 모델을 사용하여 데이터 세트를 분석할 수 있습니다. [Data science and machine learning](https://www.ibm.com/analytics/data-science/machine-learning)을 통해 {{site.data.keyword.pm_short}}에서 제공하는 기능에 대해 자세히 알아보면서 계속 진행하십시오.

### 관련 링크
{: #dsx-related}

* [{{site.data.keyword.pm_short}}](/docs/services/PredictiveModeling/index.html#using-machine-learning-with-data-science-experience)
* [{{site.data.keyword.DSX}}](https://datascience.ibm.com/)
* [{{site.data.keyword.DSX}} 문서](https://datascience.ibm.com/docs/content/getting-started/welcome-main.html?context=analytics)
