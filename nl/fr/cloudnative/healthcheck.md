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

# Utilisation d'un diagnostic d'intégrité dans votre application Swift
{: #healthcheck}

Les diagnostics d'intégrité fournissent un mécanisme simple pour déterminer si une application côté serveur se comporte de manière appropriée. Les environnements cloud tels que [Kubernetes](https://www.ibm.com/cloud/container-service) et [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) peuvent être configurés pour sonder périodiquement les noeuds finaux d'intégrité afin de déterminer si une instance de votre service est prête à accepter du trafic.
{: shortdesc}

## Présentation du diagnostic d'intégrité
{: #overview}

Les diagnostics d'intégrité fournissent un mécanisme simple pour déterminer si une application côté serveur se comporte de manière appropriée. Ils sont généralement consommés via HTTP et utilisent des codes retour standard pour indiquer l'état UP ou DOWN. La valeur de retour d'un diagnostic d'intégrité est variable, mais une réponse JSON minimale telle que `{"status": "UP"}` est typique.

Kubernetes a une notion nuancée de l'intégrité des processus. Il définit deux sondes :

- La sonde de _**préparation**_ est utilisée pour indiquer si le processus peut traiter les demandes (est routable).

  Kubernetes n'achemine pas le travail vers un conteneur si la sonde de préparation a échoué. Une sonde de préparation peut échouer si un service n'a pas terminé son initialisation, ou s'il est occupé, surchargé ou incapable de traiter les demandes.

- La sonde de _**vivacité**_ est utilisée pour indiquer si le processus doit être redémarré.

  Kubernetes arrête et redémarre un conteneur lorsqu'une sonde de vivacité échoue. Utilisez des sondes de vivacité pour nettoyer les processus se trouvant dans un état irrécupérable, par exemple, si la mémoire est épuisée, ou si le conteneur ne s'est pas arrêté correctement après un plantage de processus interne.

A titre de comparaison, Cloud Foundry utilise un seul noeud final d'intégrité. Si ce diagnostic échoue, le processus est redémarré, mais s'il réussit, les demandes sont acheminées vers lui. Dans cet environnement, le noeud final réussit très peu lorsque le processus est en cours. Un temps d'attente initial est configuré pour reporter la vérification de l'intégrité jusqu'à la fin de l'initialisation du service pour éviter les cycles de redémarrage.

Le tableau suivant donne des indications sur les réponses que les noeuds finaux peuvent fournir en terme de préparation, de vivacité et d'intégrité.

| Etat    | Préparation                   | Vivacité                   | Intégrité                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | Non-OK causes no load       | Non-OK causes restart      | Non-OK causes restart     |
| Starting | 503 - Unavailable           | 200 - OK                   | Use delay to avoid test   |
| Up       | 200 - OK                    | 200 - OK                   | 200 - OK                  |
| Stopping | 503 - Unavailable           | 200 - OK                   | 503 - Unavailable         |
| Down     | 503 - Unavailable           | 503 - Unavailable          | 503 - Unavailable         |
| Errored  | 500 - Server Error          | 500 - Server Error         | 500 - Server Error        |

## Ajout d'un diagnostic d'intégrité à une application Swift existante
{: #existing-app}

La bibliothèque [Health](https://github.com/IBM-Swift/Health) simplifie l'ajout d'un diagnostic d'intégrité à votre application Swift. Les diagnostics d'intégrité sont extensibles. Pour plus d'informations sur la [mise en cache](https://github.com/IBM-Swift/Health#caching) pour prévenir les attaques par déni de service ou sur l'ajout de [vérifications personnalisées](https://github.com/IBM-Swift/Health#implementing-a-health-check), consultez la bibliothèque [Health](https://github.com/IBM-Swift/Health).

Pour ajouter la bibliothèque Health à une application Swift existante, procédez somme suit :

1. Spécifiez-la dans la section *dependencies:* de votre fichier `Package.swift` et ajoutez-la à toutes les cibles appropriées :

    ```swift
    .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
    ```
    {: codeblock}

2. Ajoutez le code d'initialisation suivant à votre application :

    ```swift
    import Health

    let health = Health()
    ```
    {: codeblock}

3. Ajoutez la définition de routage pour définir le noeud final du diagnostic d'intégrité :

    ```swift
    router.get("/health") { request, response, next in
        /* let status = health.status.toDictionary() */
        let status = health.status.toSimpleDictionary()
        if health.status.state == .UP {
            try response.send(json: status).end()
  } else {
            try response.status(.serviceUnavailable).send(json: status).end()
        }
    }
    ```
    {: codeblock}

4. Vérifiez l'état de l'application avec un navigateur en accédant au noeud final `/health`. Le code renvoie le contenu `{"status": "UP"}`, comme défini par le dictionnaire simple.

## Vérification de l'intégrité d'une application Swift de kit de démarrage côté serveur
{: #healthcheck-starterkit}

Lorsque vous générez une application Swift basée sur Kitura à l'aide d'un kit de démarrage, un noeud final de diagnostic d'intégrité de base, `/health`, est inclus par défaut. Le noeud final utilise le protocole Codable disponible dans Swift 4, tel que pris en charge par la bibliothèque [Health](https://github.com/IBM-Swift/Health).

Le code d'initialisation de base, tel que pour l'initialisation de l'objet Health se trouve dans `Sources/Application.swift`. Le noeud final du diagnostic d'intégrité par lui-même est fourni par le fichier `/Sources/Application/Routes/HealthRoutes.swift` et utilise le code suivant :

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

## Recommandations pour les sondes de préparation et de vivacité
{: #recommend-probes}

Les vérifications de préparation doivent inclure la viabilité des raccordements aux services situés en aval dans leur résultat, s'il n'existe pas de solution de rechange acceptable lorsque le service en aval n'est pas disponible. Il ne s'agit pas d'appeler directement le diagnostic d'intégrité qui est fourni par le service en aval, car l'infrastructure le vérifie pour vous. Vous devez plutôt envisager de vérifier l'état des références existantes de votre application aux services en aval : il peut s'agir d'une connexion JMS à WebSphere MQ, ou d'un consommateur ou producteur Kafka initialisé. Si vous vérifiez la viabilité des références internes aux services en aval, mettez en cache le résultat pour minimiser l'impact de la vérification de santé sur votre application.

Une sonde de vivacité, en revanche, peut être utilisée délibérément sur ce qui est vérifié, car une défaillance entraîne l'arrêt immédiat du processus. Un noeud final HTTP simple qui renvoie toujours `{"status" : "UP"}` avec le code d'état `200` est un choix raisonnable.

### Ajout de la prise en charge de la préparation et de la vivacité Kubernetes à une application Swift
{: #kube-readiness-swift}

Pour des implémentations alternatives, telles que l'utilisation de **Codable** ou du dictionnaire standard, consultez les [exemples de bibliothèques Health](https://github.com/IBM-Swift/Health#usage). Certaines de ces implémentations simplifient la création de contrôles de santé extensibles avec une prise en charge des contrôles de mise en cache qui sont effectués sur les services de sauvegarde. Dans ce scénario, vous devriez séparer le test de vivacité simple du test d'état de préparation plus robuste et plus détaillé.

## Configuration des sondes de préparation et de vivacité dans Kubernetes
{: #config-kube-readiness}

Sondez la vivacité et l'état de préparation en même temps que votre déploiement Kubernetes. Les deux sondes utilisent les mêmes paramètres de configuration :

* Kubelet attend la durée définie par `initialDelaySeconds` avant la première sonde.

* Kubelet sonde le service toutes les `periodSeconds` secondes. La valeur par défaut est 1.

* La sonde expire au bout de `timeoutSeconds` secondes. La valeur minimale et la valeur par défaut sont de 1.

* La sonde réussit si elle est exécutée `successThreshold` fois après une panne. La valeur minimale et la valeur par défaut sont de 1. La valeur doit être 1 pour les sondes de vivacité.

* Lorsqu'un pod démarre et que la sonde échoue, Kubernetes essaie `failureThreshold` fois de redémarrer le pod et de renoncer. La valeur minimale est 1 et la valeur par défaut est 3.
    - Pour une sonde de vivacité, "renoncer" signifie redémarrer le pod.
    - Pour une sonde de préparation, "renoncer" signifie marquer le pod comme `Unready`.

Pour éviter les cycles de redémarrage, réglez `livenessProbe.initialDelaySeconds` sur une durée supérieure à la durée d'initialisation de votre service. Vous pouvez ensuite utiliser une valeur plus courte pour `readinessProbe.initialDelaySeconds` pour acheminer les demandes vers le service dès qu'il est prêt.

Exemple de fichier `yaml` :
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

Pour plus d'informations, consultez [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).
