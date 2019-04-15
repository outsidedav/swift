---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-14"

keywords: cloudant swift, store data swift, dbaas swift, cloudant instance swift, initialize sdk swift, create document swift, read document swift, delete document swift

subcollection: swift

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

Pour en savoir plus sur les différentes utilisations de {{site.data.keyword.cloudant_short_notm}}, voir [Concepts de base relatifs à {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/basics?topic=cloudant-ibm-cloudant-basics#cloudant-nosql-db-basics).

## Avant de commencer
{: #prereqs-cloudant}

Tout d'abord, assurez-vous que la configuration prérequise suivante est respectée :
 * CocoaPods (version 1.1.0 ou ultérieure)
 * iOS (version 9 ou ultérieure)
 * MacOS (version 10.11.5 ou ultérieure)
 * Xcode (version 9.0.1 ou ultérieure)

Le logiciel SDK [{{site.data.keyword.cloudant_short_notm}} pour Swift ](https://github.com/cloudant/swift-cloudant){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") est généré avec Swift 3.2. Si vous prévoyez d'utiliser {{site.data.keyword.cloudant_short_notm}} avec Kitura, consultez la [bibliothèque Kitura-CouchDB ](https://github.com/IBM-Swift/Kitura-CouchDB){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), qui est générée avec Swift 4.0.
{: tip}

## Etape 1. Création d'une instance de {{site.data.keyword.cloudant_short_notm}}
{: #create-instance-cloudant}

Voir [Création d'une instance IBM Cloudant sur le tutoriel {{site.data.keyword.cloud_notm}}](/docs/services/Cloudant/tutorials?topic=cloudant-creating-an-ibm-cloudant-instance-on-ibm-cloud#creating-an-ibm-cloudant-instance-on-ibm-cloud){:new_window} pour créer une instance du service.

## Etape 2. Installation du logiciel SDK
{: #install-sdk-cloudant}

### Installation du logiciel SDK Swift iOS
{: #install-swift-sdk-cloudant}

Utilisez le logiciel SDK Swift Cloudant SDK pour simplifier le codage de votre application. Ce logiciel SDK doit être installé dans votre code d'application.

1. Ouvrez votre répertoire projet Xcode existant sur le `Fichier pod`.
2. Sous votre cible projets, ajoutez une dépendance pour le pod `SwiftCloudant`. Assurez-vous que la commande `use_frameworks!` figure également sous votre cible, comme illustré dans l'exemple suivant.
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
{: #install-serverside-cloudant}

Pour une utilisation avec le Gestionnaire de package Swift pour le développement côté serveur, ajoutez la ligne suivante à vos dépendances dans votre `Package.swift` :
```swift
.Package(url: "https://github.com/cloudant/swift-cloudant.git")
```
{: codeblock}

## Etape 3. Initialisation du logiciel SDK
{: #initialize-cloudant}

Une fois le logiciel SDK initialisé dans votre application, vous pouvez commencer à utiliser {{site.data.keyword.cloudant_short_notm}} pour stocker les données.

1.  Ajoutez l'importation ci-dessous dans votre fichier `AppDelegate.swift` ou fichier Swift côté serveur.
    ```swift
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
{: #basic-operations-cloudant}

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

Vous rencontrez des problèmes ? Consultez la section [Référence d'API {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/api?topic=cloudant-ibm-cloudant-basics#api-reference-overview).

## Etapes suivantes
{: #cloudant_next notoc}

Félicitations ! Vous avez ajouté un niveau de persistance sécurisé à votre application. Continuez sur votre lancée en essayant l'une des options suivantes :

* Affichez le code source du [logiciel SDK pour Swift {{site.data.keyword.cloudant_short_notm}}](https://github.com/cloudant/swift-cloudant){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").
* Les kits de démarrage constituent l'un des moyens les plus rapides d'utiliser les fonctionnalités d'{{site.data.keyword.cloud_notm}}. Le kit de démarrage **Infinite Scrolling with Cloudant NoSQL for iOS** illustre comment étendre un `ViewController` pour afficher les données à l'aide de la pagination. Ce modèle d'application est commun pour les développeurs iOS et il s'agit d'un bon exemple pour illustrer les possibilités de {{site.data.keyword.cloudant_short_notm}}. Affichez les kits de démarrage disponibles dans le [tableau de bord Mobile Developer ](https://cloud.ibm.com/developer/mobile/dashboard){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"). Téléchargez le code. Exécutez l'application.
* Pour découvrir et bénéficier de toutes les fonctions offertes par {{site.data.keyword.cloudant_short_notm}}, [consultez les documentations](/docs/services/Cloudant?topic=cloudant-ibm-cloudant-basics#ibm-cloudant-basics) !
