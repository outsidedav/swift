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

# Utilizzo di un controllo di integrità nella tua applicazione Swift
{: #healthcheck}

I controlli di integrità forniscono un semplice meccanismo per determinare se un'applicazione lato server sta funzionando correttamente o meno. Molti ambienti di sviluppo, come [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) e [Kubernetes](https://www.ibm.com/cloud/container-service), possono essere configurati per eseguire periodicamente il polling degli endpoint di integrità per determinare se un'istanza del tuo servizio è pronta ad accettare traffico o meno.

I controlli di integrità sono di norma consumati su HTTP e utilizzano dei codici di ritorno standard per indicare gli stati `UP` o `DOWN`. Degli esempi includono la restituzione di `200` per `UP` e di `5xx` per `DOWN`. Per essere specifici, viene utilizzato `503` quando l'applicazione non può gestire le richieste o non è stata ancora avviata (il servizio non è disponibile) e viene utilizzato `500` quando il server riscontra una condizione di errore. Il valore di ritorno di un controllo di integrità è variabile ma una risposta JSON minima, come `{“status”: “UP”}` fornisce congruenza.

Cloud Foundry utilizza un singolo endpoint di integrità per indicare se un'istanza del servizio può gestire le richieste o meno. In Kubernetes, un endpoint di controllo di integrità equivale a un endpoint `readiness`. La tua applicazione deve definire questo endpoint per facilitare la determinazione delle decisioni di instradamento automatiche. L'esito positivo o negativo di questo endpoint può includere la considerazione per i servizi downstream richiesti nel caso in cui non sia presente un fallback accettabile. Se si esegue il controllo dei servizi downstream, la memorizzazione nella cache del risultato è a volte utile, per ridurre al minimo il carico sul sistema nel suo complesso a causa dei controlli di integrità.

Kubernetes definisce un endpoint [liveness](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) supplementare, che consente all'applicazione di indicare se il processo deve essere riavviato o meno. Qui si applica un sottoinsieme di considerazioni: un controllo `liveness` potrebbe non riuscire dopo che viene raggiunta una specifica soglia di utilizzo della memoria, ad esempio: Se la tua applicazione è in esecuzione su Kubernetes, prendi in considerazione l'aggiunta di un endpoint `liveness` per assicurarti che il processo venga riavviato quando necessario.

## Aggiunta di un controllo di integrità a un'applicazione Swift esistente
{: #add-healthcheck-existing}

La libreria [Health](https://github.com/IBM-Swift/Health) facilita l'aggiunta di un controllo di integrità alla tua applicazione Swift. I controlli di integrità sono estensibili. Per ulteriori informazioni sulla [memorizzazione in cache](https://github.com/IBM-Swift/Health#caching) per prevenire attacchi di tipo DoS o sull'aggiunta di [controlli personalizzati](https://github.com/IBM-Swift/Health#implementing-a-health-check), vedi la libreria [Health](https://github.com/IBM-Swift/Health).

Per aggiungere la libreria Health a un'applicazione Swift esistente, specificala nella sezione *dependencies:* del tuo file `Package.swift`, assicurandoti di aggiungerla a qualsiasi destinazione dove se ne fa uso.
```swift
  .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
```
{: codeblock}

Aggiungi quindi il seguente codice di inizializzazione alla tua applicazione:
```swift
import Health

let health = Health()
```
{: codeblock}

Aggiungi quindi la definizione di instradamento per definire l'endpoint di controllo di integrità:
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

Controlla lo stato dell'applicazione con un browser accedendo all'endpoint `/health`. Il codice restituisce un payload `{"status": "UP"}`, come definito dal semplice dizionario.

Per delle implementazioni alternative, quali l'utilizzo di **Codable** o del dizionario standard, vedi gli [esempi della libreria Health](https://github.com/IBM-Swift/Health#usage).

## Accesso al controllo di integrità dalle applicazioni kit starter Swift lato server
{: #healthcheck-starterkit}

Quando generi un'applicazione Swift basata su Kitura utilizzando un kit starter, un endpoint di controllo di integrità di base `/health`, viene incluso per impostazione predefinita. L'endpoint si avvale del protocollo Codable disponibile in Swift 4, come supportato dalla libreria [Health](https://github.com/IBM-Swift/Health).

Il codice di inizializzazione di base, quale l'inizializzazione dell'oggetto Health, si verifica in `Sources/Application.swift`, mentre l'endpoint di controllo di integrità viene fornito dal file `/Sources/Application/Routes/HealthRoutes.swift`, che contiene il seguente codice:
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

L'esempio utilizza il dizionario standard, che produce un payload quale `{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}` quando accedi all'endpoint `/health`.
