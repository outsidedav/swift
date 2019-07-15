---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: watson studio swift, machine learning swift, custom model swift, data set swift, predictive swift, watson api swift, generated model swift, dataset swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Analisi delle serie di dati con modelli generati personalizzati
{: #dsx-overview}

Watson Studio ti fornisce l'ambiente e gli strumenti per risolvere i problemi aziendali mediante l'analisi dei dati in modo collaborativo. Puoi scegliere gli strumenti di cui hai bisogno per analizzare, ripulire e organizzare i dati. Impara a inserire dati di streaming o a creare, eseguire il training di, e distribuire modelli di machine learning. Watson Studio si integra con un'ampia gamma di servizi {{site.data.keyword.cloud}} e Watson Knowledge Catalog, che fornisce la gestione delle politiche per controllare gli asset e i cataloghi da indicizzare per individuarli. [Ulteriori informazioni](https://dataplatform.cloud.ibm.com/){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").

Watson Studio è strutturato intorno ad un'architettura basata sui progetti, che organizza le tue risorse per risolvere un problema di business. Le risorse includono connessioni ad archivi dati cloud e in loco, file di dati, collaboratori e asset di analisi come i modelli. Per ulteriori informazioni, consulta [Watson Studio overview](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/overview-ws.html){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").

## Machine learning per {{site.data.keyword.DSX}}
{: #dsx-learning}

Utilizzando {{site.data.keyword.DSX}}, è possibile eseguire il training dei modelli e distribuirli e utilizzare quindi i risultati servendosi delle API. Queste API possono quindi essere utilizzate nelle tue applicazioni iOS o Swift.

Con IBM Watson Machine Learning, dopo che hai configurato il tuo ambiente, puoi creare modelli, distribuirli al cloud ed eseguirne il training. Per ulteriori informazioni, vedi il documento relativo alla [creazione, distribuzione e formazione dei modelli con {{site.data.keyword.pm_full}} e {{site.data.keyword.DSX}}](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/wml-ai.html){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").

### Esercitazioni
{: #dsx-tutorials}

## Configurazione di {{site.data.keyword.DSX}} con iOS e Swift
{: #dsx_ios}

1. Per semplificare l'integrazione delle credenziali, devi aggiungere l'istanza {{site.data.keyword.pm_short}} alla tua applicazione iOS o alla tua applicazione backend. Per maggiore facilità di accesso, le credenziali sono incluse nel dashboard del progetto.

![Dettagli applicazione](images/ios-machinelearning-app.png "Dettagli applicazione")

2. Scarica il codice dell'applicazione.
3. Inizializzazione
  * Per un progetto iOS, semplicemente aggiungendo il servizio {{site.data.keyword.pm_short}} al tuo progetto iOS, le credenziali vengono istantaneamente inserite nella tua applicazione.
    Per accedere alle credenziali dalla tua applicazione, copia e incolla il seguente frammento di codice. Assicurati inoltre di aggiungere l'endpoint di punteggio alla tua applicazione, che è disponibile nella scheda `implementation` della distribuzione del tuo modello.

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

  * Per l'applicazione lato server, aggiungi manualmente il tuo nome utente e la tua password alla tua applicazione nonché l'endpoint di punteggio, che è disponibile nella scheda `implementation` della distribuzione del tuo modello.

    ```swift
    /* Your Machine Learning Credentials */
    let machineLearningUsername: String = "<your-ml-service-username>"
    let machineLearningPassword: String = "<your-ml-service-password>"

    /* The url to your model's scoring endpoint */
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"
    ```
    {: codeblock}

4. Richiama i token di accesso ed esegui l'analisi predittiva sui dataset dalla tua applicazione con un semplice SDK client.

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

### Esempio
{: #dsx-example}

**Nome dello scenario: ** Previsione linea di prodotti

**Descrizione dello scenario:** un'azienda che vende attrezzature da esterno crea e distribuisce un modello per prevedere l'interesse del cliente alla sua linea di prodotti. La tua attività consiste nell'effettuare richieste di punteggio rispetto al modello distribuito.

Una volta distribuito il modello, puoi eseguire l'analisi predittiva utilizzando l'endpoint di punteggio.

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

## Passi successivi
{: #dsx_next notoc}

Ottimo lavoro. Ora puoi analizzare le serie di dati utilizzando i modelli di machine learning generati personalizzati. Non fermarti ora e continua scoprendo di più sulle funzioni che {{site.data.keyword.pm_short}} ha da offrire in [Data science and machine learning](https://www.ibm.com/analytics/machine-learning){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").

### Link correlati
{: #dsx-related}

* [{{site.data.keyword.pm_short}}](/docs/services/PredictiveModeling?topic=PredictiveModeling-WMLgettingstarted)
* [{{site.data.keyword.DSX}}](https://www.ibm.com/cloud/watson-studio){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")
* [Documentazione di {{site.data.keyword.DSX}}](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/welcome-main.html){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")
