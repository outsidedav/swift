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

# Analisando conjuntos de dados com modelos gerados customizados
{: #dsx-overview}

O Watson Studio fornece o ambiente e as ferramentas para resolver seus problemas de negócios, analisando dados de forma colaborativa. É possível escolher as ferramentas que você precisa para analisar, purificar e organizar dados. Aprenda a alimentar dados de fluxo ou a criar, treinar e implementar modelos de aprendizado de máquina. O Watson Studio integra-se a uma ampla variedade de serviços do {{site.data.keyword.cloud}} e ao Watson Knowledge Catalog, que fornece gerenciamento de política para controlar ativos e catálogos para indexação para localizá-los. Saiba mais em https://dataplatform.ibm.com/.

O Watson Studio é estruturado em torno de uma arquitetura baseada em projeto, que organiza seus recursos para resolver um problema de negócios. Os recursos incluem conexões com armazenamentos de dados na nuvem e no local, arquivos de dados, colaboradores e recursos analíticos como modelos. Para obter mais informações, consulte a [Visão geral do Watson Studio](https://datascience.ibm.com/docs/content/getting-started/overview-ws.html?context=analytics){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Aprendizado de máquina para o {{site.data.keyword.DSX}}
{: #dsx-learning}

Ao usar o {{site.data.keyword.DSX}}, é possível treinar modelos e implementá-los e, em seguida, consumir os resultados usando APIs. Essas APIs podem, então, ser usadas nos aplicativos iOS ou Swift.

Com o IBM Watson Machine Learning, depois de configurar seu ambiente, é possível criar modelos, implementá-los na nuvem e treiná-los. Para obter mais informações, consulte [Criar, implementar e treinar modelos com o {{site.data.keyword.pm_full}} e o {{site.data.keyword.DSX}}](https://datascience.ibm.com/docs/content/analyze-data/wml-ai.html?context=analytics){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

### Tutoriais
{: #dsx-tutorials}

- [Construa um modelo de regressão logístico com o {{site.data.keyword.pm_short}}](https://datascience.ibm.com/docs/content/analyze-data/ml-example-log-regress.html?context=analytics){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")
- [Construa um modelo de Bayes ingênuo com o {{site.data.keyword.pm_short}}](https://datascience.ibm.com/docs/content/analyze-data/ml-example-naive-bayes.html?context=analytics){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")

## Configurando o  {{site.data.keyword.DSX}}  com o iOS e o Swift
{: #dsx_ios}

1. Para facilitar a integração das credenciais, deve-se incluir a instância do {{site.data.keyword.pm_short}} no app iOS ou de backend. Para facilitar a acessibilidade, suas credenciais são incluídas no painel do projeto.

![Machine Learning in your App](images/ios-machinelearning-app.png)

2. Faça download do código do aplicativo.
3. Inicialização
  * Para um projeto do iOS, simplesmente incluindo o serviço do {{site.data.keyword.pm_short}} em seu projeto do iOS, as credenciais serão imediatamente injetadas em seu app.
    Para acessar as credenciais por meio do aplicativo, copie e cole o fragmento de código a seguir. Além disso, certifique-se de incluir o terminal de pontuação no app, que pode ser localizado na guia `implementation` de implementação do modelo.

    ```swift
    /* The url to your model's scoring endpoint */
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"

    /* Your credentials */
    var machineLearningUsername: String!
    var machineLearningPassword: Cadeia!

    /* Machine Learning initialization */
    if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"),
       let dictionary = NSDictionary(contentsOfFile: contents),
       let username = dictionary["machinelearningUsername"] as? String,
       let password = dictionary["machinelearningPassword"] as? Cadeia {

           machineLearningUsername = username
           machineLearningPassword = password

    }
    ```
    {: codeblock}

  * Para o aplicativo do lado do servidor, inclua manualmente seu nome de usuário e senha no aplicativo, bem como o terminal de pontuação, que pode ser localizado na guia `implementation` de implementação do modelo.

    ```swift
    /* Your Machine Learning Credentials */
    let machineLearningUsername: String = "<your-ml-service-username>"
    let machineLearningPassword: String = "<your-ml-service-password>"

    /* The url to your model's scoring endpoint */
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"
    ```
    {: codeblock}

4. Recupere os tokens de acesso e execute a análise preditiva em conjuntos de dados de seu aplicativo com um SDK simples do cliente.

  ```swift
  public class MachineLearning {

      private let username: String
      private let password: String

      public init (nome de usuário: String, password: String) {
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

          executar (solicitação, falha: falha) {

              do guarda let token = dict [ "token" ] como? Cadeia else {
                  failure(NSError(domain: "Invalid Response", code: 1, userInfo: nil))
                  return
              }

              success(token)
          }
      }

      public func retrieveScore(_ scoringUrl: String, token: String, payload: [String: Any], failure: @escaping (Error) -> Void  = failure, success: @escaping ([String: Any]) -> Void) {

          guarda let data = try? JSONSerialization.data (withJSONObject: carga útil, opções: .prettyPrinted) else {
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

      public func createRequest(method: String = "GET", url: String, body: Data? = nil)-> URLRequest? {
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
                  erro de let = erro?? NSError(domain: "No data was returned", code: 1, userInfo: nil)
                  failure(error)
                  return
              }

              guard let body = try? JSONSerialization.jsonObject (com: dados, opções: .allowFragments) como? [String: Any],
                    let dict = body else {
                  failure(NSError(domain: "Could not create dictionary", code: 1, userInfo: nil))
                  return
              }

              guard let resp = response as? HTTPURLResponse , resp.statusCode == 200 else {
                 msg = (dict [ "errors" ] como? [ [ String: Any ] ]) ?.first? [ "mensagem " ] como? Sequência? "An error occurred"
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

### Exemplo
{: #dsx-example}

**Nome do cenário:** Predição da linha de produto

**Descrição do cenário:** uma empresa que vende equipamentos para uso ao ar livre, constrói e implementa um modelo para prever o interesse do cliente em sua linha de produto. Sua tarefa é fazer solicitações de escore com relação ao modelo
implementado.

Depois que o modelo é implementado, é possível executar a análise preditiva usando o terminal de pontuação.

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

## Próximas etapas
{: #dsx_next notoc}

Ótimo trabalho! Agora é possível analisar conjuntos de dados usando modelos de aprendizado de máquina gerados customizados. Mantenha o ritmo aprendendo mais sobre os recursos que o {{site.data.keyword.pm_short}} tem a oferecer em [Ciência de dados e aprendizado de máquina](https://www.ibm.com/analytics/data-science/machine-learning){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

### Links Relacionados
{: #dsx-related}

* [{{site.data.keyword.pm_short}}](/docs/services/PredictiveModeling?topic=services/PredictiveModeling-WMLgettingstarted#using-machine-learning-with-data-science-experience)
* [{{site.data.keyword.DSX}}](https://datascience.ibm.com/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")
* [Documentação do {{site.data.keyword.DSX}}](https://datascience.ibm.com/docs/content/getting-started/welcome-main.html?context=analytics){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")
