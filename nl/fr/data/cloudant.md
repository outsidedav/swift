---

copyright:
  years: 2017-2018
lastupdated: "2018-11-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Stockage de documents dans {{site.data.keyword.cloud_notm}}
{: #cloudant}

{{site.data.keyword.cloudantfull}} est une base de données de type DBaaS (database as a service) qui stocke les données en tant que de documents JSON. Elle est créée en gardant à l'esprit l'évolutivité, la haute disponibilité et la durabilité. Elle est aussi très simple à configurer pour une utilisation dans les applications Swift. Elle est fournie avec un large éventail d'options d'indexation telles que MapReduce, {{site.data.keyword.cloudant_short_notm}} Query, la recherche en texte intégral et le traitement des données géospatiales. Ses fonctions de réplication facilitent la synchronisation des données entre les clusters de base de données, les ordinateurs de bureau et les périphériques
mobiles. 
{: shortdesc}

Pour en savoir plus sur les différentes utilisations de {{site.data.keyword.cloudant_short_notm}}, voir [Concepts de base relatifs à {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/basics/index.html#cloudant-nosql-db-basics).

## Avant de commencer

Tout d'abord, assurez-vous que vous respectez la configuration prérequise suivante :
 * CocoaPods (version 1.1.0 ou ultérieure)
 * iOS (version 9 ou ultérieure)
 * MacOS (version 10.11.5 ou ultérieure)
 * Xcode (version 9.0.1 ou ultérieure)

Le logiciel SDK [{{site.data.keyword.cloudant_short_notm}} pour Swift ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudant/swift-cloudant) est généré avec Swift 3.2. Si vous prévoyez d'utiliser {{site.data.keyword.cloudant_short_notm}} avec Kitura, consultez la [bibliothèque Kitura-CouchDB ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/IBM-Swift/Kitura-CouchDB), qui est générée avec Swift 4.0.
{: tip}

## Etape 1. Création d'une instance de {{site.data.keyword.cloudant_short_notm}}

Pour créer une instance du service, consultez le tutoriel [Création d'une instance de base de données Cloudant NoSQL ![Icône de lien externe](../images/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/services/Cloudant/tutorials/create_service.html#creating-a-cloudant-nosql-db-instance-on-ibm-cloud){:new_window}.

## Etape 2. Installation du logiciel SDK

### Installation du logiciel SDK Swift iOS

Utilisez le logiciel SDK Swift Cloudant SDK pour simplifier le codage de votre application. Ce logiciel SDK doit être installé dans votre code d'application.

1. Ouvrez votre répertoire projet Xcode existant sur le `Fichier pod`.
2. Sous votre cible projets, ajoutez une dépendance pour le pod `SwiftCloudant`. Assurez-vous que la commande `use_frameworks!` figure également sous votre cible comme illustré dans l'exemple suivant.
    ```
    target '<yourTarget>' do
      use_frameworks!
        pod 'SwiftCloudant', :git => 'https://github.com/cloudant/swift-cloudant.git'
    end
    ```
    {: screen}

3. Téléchargez la dépendance `SwiftCloudant`.
    ```
    pod install
    ```
    {: codeblock}

### Installation du logiciel SDK Swift côté serveur

Pour une utilisation avec le Gestionnaire de package Swift pour le développement côté serveur, ajoutez la ligne suivante à vos dépendances dans votre `Package.swift` :
```swift
.Package(url: "https://github.com/cloudant/swift-cloudant.git")
```
{: codeblock}

## Etape 3. Initialisation du logiciel SDK

Une fois le logiciel SDK initialisé dans votre application, vous pouvez commencer à utiliser {{site.data.keyword.cloudant_short_notm}} pour stocker les données.

1.  Ajoutez l'importation ci-dessous dans votre fichier `AppDelegate.swift` ou fichier Swift côté serveur.
    ```
    import SwiftCloudant
    ```
    {: codeblock}

2. Initialisez la connexion à la base de données.
    ```swift
    let cloudantURL = NSURL(string: "https://username.cloudant.com")!
    let client = CouchDBClient(url: cloudantURL, username: "username", password: "password")
    let dbName = "database"
    ```
    {: codeblock}

### Opérations de base
Ces opérations de base illustrent les principales actions permettant de créer, de lire et de supprimer vos documents à l'aide du client initialisé.

#### Créer un document
```swift
let create = PutDocumentOperation(id: "doc1", body: ["hello": "world"], databaseName: dbName) {(response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while creating a document. Error: \(error)")
    } else {
        print("Created document \(response?["id"]) with revision id \(response?["rev"])")
    }
}
client.add(operation: create)
```
{: codeblock}

#### Lire un document
```swift
let read = GetDocumentOperation(id: "doc1", databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while reading a document. Error: \(error)")
    } else {
        print("Read document: \(response)")
    }   
}
client.add(operation: read)
```
{: codeblock}

#### Supprimer un document
```swift
let delete = DeleteDocumentOperation(id: "doc1",
    revision: "1-revisionidhere",
    databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while deleting a document. Error: \(error)")
    } else {
        print("Document deleted")
    }   
}
client.add(operation: delete)
```
{: codeblock}

## Etape 4. Test de votre application
{: #cloudant_testing}

Est-ce que tout est correctement configuré ? Il est temps de tester !

1. Exécutez votre application, en veillant à démarrer l'initialisation et les opérations respectives, comme la création d'un document.
2. Retournez à l'instance de service {{site.data.keyword.cloudant_short_notm}} préalablement créée dans votre navigateur Web et ouvrez le tableau de bord de service.
3. Sélectionnez la base de données qui est utilisée, afin de voir les documents dans le tableau de bord.

Vous rencontrez des problèmes ? Consultez la [Référence d'API {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/api/index.html#api-reference-overview).

## Etapes suivantes
{: #cloudant_next notoc}

Félicitations ! Vous avez ajouté un niveau de persistance sécurisé à votre application. Poursuivez sur votre lancée en essayant l'une des options suivantes :

* Consultez le code source du [logiciel SDK pour Swift {{site.data.keyword.cloudant_short_notm}}![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudant/swift-cloudant).
* Les kits de démarrage constituent l'un des moyens les plus rapides pour utiliser les fonctionnalités d'{{site.data.keyword.cloud_notm}}. Le kit de démarrage **Infinite Scrolling with Cloudant NoSQL for iOS** illustre comment étendre un `ViewController` pour afficher les données à l'aide de la pagination. Ce modèle d'application est commun pour les développeurs iOS et il s'agit d'un bon exemple pour illustrer les possibilités de {{site.data.keyword.cloudant_short_notm}}. Vous pouvez voir les kits de démarrage disponibles dans le [tableau de bord Mobile Developer](https://console.bluemix.net/developer/mobile/dashboard). Téléchargez le code. Exécutez l'application.
* Pour découvrir et bénéficier de toutes les fonctions offertes par {{site.data.keyword.cloudant_short_notm}}, [consultez les documentations](/docs/services/Cloudant/index.html) ! 
