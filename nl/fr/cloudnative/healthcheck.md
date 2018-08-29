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

# Utilisation d'un diagnostic d'intégrité dans votre appli Swift
{: #healthcheck}

Les diagnostics d'intégrité constituent un mécanisme simple permettant de déterminer si une application côté serveur se comporte ou non correctement. De nombreux environnements de déploiement, tels que [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) et [Kubernetes](https://www.ibm.com/cloud/container-service), peuvent être configurés pour une interrogation régulière des noeuds finaux d'intégrité afin de déterminer si une instance de votre service est ou non prête à accepter le trafic.

Les diagnostics d'intégrité sont généralement consommés via HTTP, et ils utilisent des codes retour standard pour indiquer l'état `UP` ou `DOWN`. Les exemples incluent le renvoi de la valeur `200` pour `UP` et la valeur `5xx` pour `DOWN`. Plus précisément, la valeur `503` est utilisée lorsque l'application ne peut pas traiter les demandes ou n'est pas encore démarrée (le service est indisponible), et la valeur `500` est utilisée lorsque le serveur détecte un cas d'erreur. La valeur renvoyée par un diagnostic d'intégrité est variable, mais une réponse JSON minimale, telle que `{“status”: “UP”}` assure la cohérence.

Cloud Foundry utilise un noeud final d'intégrité pour indiquer si une instance de service peut ou non traiter les demandes. Dans Kubernetes, un noeud final de diagnostic d'intégrité est équivalent à un noeud final `readiness`. Votre application doit définir ce noeud final afin de déterminer les décisions de routage automatiques. La réussite ou l'échec de ce noeud final peut dépendre des services en aval requis dans le cas où il n'existe pas de rétromigration acceptable. Si vous ne consultez pas les services aval, la mise en cache du résultat est parfois utile, afin de minimiser la charge sur le système dans l'ensemble en raison des diagnostics d'intégrité.

Kubernetes définit un noeud final [liveness](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) supplémentaire, qui permet à l'application d'indiquer si le processus doit ou non être redémarré. Un sous-ensemble de considérations s'applique ici : une vérification `liveness` peut échouer dès qu'un certain seuil d'utilisation de mémoire est atteint, par exemple. Si votre appli s'exécute sur Kubernetes, pensez à ajouter un noeud final `liveness` afin de garantir le redémarrage de votre processus si nécessaire.

## Ajout d'un diagnostic d'intégrité à une appli Swift existante
{: #add-healthcheck-existing}

La bibliothèque [Health](https://github.com/IBM-Swift/Health) simplifie l'ajout d'un diagnostic d'intégrité à votre application Swift. Les diagnostics d'intégrité sont extensibles. Pour plus d'informations sur la [mise en cache](https://github.com/IBM-Swift/Health#caching) pour prévenir les attaque par déni de service ou sur l'ajout de [vérifications personnalisées](https://github.com/IBM-Swift/Health#implementing-a-health-check), consultez la bibliothèque [Health](https://github.com/IBM-Swift/Health).

Pour ajouter la bibliothèque Health à une appli Swift existante, indiquez-la dans la section *dependencies:* de votre fichier `Package.swift`, afin de garantir son ajout aux cibles sur lesquelles il est utilisé :
```swift
  .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
```
{: codeblock}

Ensuite, ajoutez le code d'initialisation suivant à votre application :
```swift
import Health

let health = Health()
```
{: codeblock}

Ensuite, ajoutez la définition de route pour définir le noeud final du diagnostic d'intégrité :
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

Vérifiez l'état de l'appli avec un navigateur en accédant au noeud final `/health`. Le code renvoie un contenu `{"status": "UP"}`, comme défini par le dictionnaire simple.

Pour d'autres implémentations, telles que l'utilisation de **Codable** ou du dictionnaire standard, consultez les [exemples de bibliothèque Health](https://github.com/IBM-Swift/Health#usage).

## Accès aux applis du kit de démarrage Swift côté serveur
{: #healthcheck-starterkit}

Lorsque vous générez une appli Swift basée sur Kitura à l'aide d'un kit de démarrage, un noeud final de diagnostic d'intégrité de base, `/health`, est inclus par défaut. Ce noeud final optimise le protocole Codable disponible dans Swift 4, comme pris en charge par la bibliothèque [Health](https://github.com/IBM-Swift/Health).

Le code d'initialisation de base, comme l'initialisation de l'objet Health a lieu dans `Sources/Application.swift`, alors que le noeud final du diagnostic d'intégrité est fourni par le fichier `/Sources/Application/Routes/HealthRoutes.swift`, qui contient le code suivant :
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

L'exemple utilise le dictionnaire standard, qui génère un contenu tel que `{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}` lorsque vous accédez au noeud final `/health`.
