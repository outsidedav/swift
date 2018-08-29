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

# Création d'une appli avec Kitura
{: #kitura}

[Kitura](http://www.kitura.io) est une infrastructure Swift côté serveur pour la génération d'applications de back end et d'applications Web iOS. Cette infrastructure crée des API REST qui peuvent être appelées depuis l'application iOS à l'aide de logiciels SDK URLSession tels que Alamofire, RestKit ou du logiciel SDK [KituraKit](https://github.com/ibm-swift/kiturakit) fourni par Kitura lui-même.

Kitura est en mesure de s'intégrer à l'ensemble des services et fonctions qui sont fournis par {{site.data.keyword.cloud}}, y compris {{site.data.keyword. appid_short}}, {{site.data.keyword.mobilepushshort}} et {{site.data.keyword.mobileanalytics_short}}, ainsi que des bases de données, l'apprentissage automatique et d'autres services. Kitura peut ensuite être déployé et mis automatiquement à l'échelle à l'aide des plateformes Cloud Foundry ou de Docker (basés sur Kubernetes) dans {{site.data.keyword.cloud}}.

Kitura fournit une [interface de ligne de commande](http://www.kitura.io/en/starter/gettingstarted.html) `kitura` qui simplifie la création, la génération et le test, ainsi que le déploiement d'applications Kitura. Les applications qui sont générées à l'aide de l'interface CLI Kitura incluent une prise en charge complète pour le déploiement sur n'importe quel cloud prenant en charge les technologies Cloud Foundry, Docker et Kubernetes. Toutefois, si vous générez de manière spécifique pour {{site.data.keyword.cloud}}, nous vous recommandons d'utiliser la console de développement IBM Apple dans le navigateur ou d'utiliser {{site.data.keyword.dev_cli_notm}}. Par ailleurs, comme les deux méthodes partagent une technologie sous-jacente, la console de développement Apple et IBM Developer Tools créent un projet hébergé et une pipeline de déploiement pour vous, et ils mettent à disposition les services dont votre application a besoin.

## Avant de commencer

Tout d'abord, assurez-vous que vous respectez la configuration prérequise suivante :  

* iOS 11.0+  
* Xcode 9.0  
* Swift 4.0+  
* Cocoapods  

## Etape 1. Création d'un projet Kitura à l'aide du navigateur

1. Accédez à la section Kits de démarrage de la console de développement Apple. Sélectionnez un kit prédéfini, comme "Swift for Backend for Frontend API" ou créez un projet personnalisé à l'aide la vignette **Créer un projet**. Cliquez sur **Créer un projet**.
2. Donnez un nom à votre projet et sélectionnez l'emplacement où vous souhaitez déployer votre projet. Si vous ne savez pas où l'application doit être déployée, utilisez les valeurs par défaut, elles peuvent être modifiées plus tard.
3. Sélectionnez le langage Swift. Un projet Swift côté serveur est créé. Sont également affichés les options Hôte et Domain, qui forme l'URL de l'application. En cas d'incertitude, utilisez les valeurs par défaut et vous pouvez aussi fournir votre propre domaine personnalisé d'une fournisseur de domaine où l'application doit résider.
4. Vous pouvez fournit une définition OpenAPI (également appelée Swagger) pour l'API REST que vous voulez générer. Si vous disposez d'une telle définition, une API REST est créée et elle inclut les fonctions de gestionnaire correspondantes dans Kitura. Si vous n'avez pas de définition OpenAPI, Kitura vous permet de créer simplement des API REST de façon manuelle à l'aide de ses API Router.
5. Cliquez sur **Créer un projet**.

Un projet est créé, mais il n'utilise pas encore des services ou des fonctions supplémentaires. Vous pouvez ajouter des services à l'aide du bouton **Ajouter une ressource** ou en cliquant sur le bouton **Télécharger le code** pour obtenir le code du projet. Vous pouvez aussi ajouter facilement des services à un projet existant.

## Step 2. Ajout de services

1. Cliquez sur le bouton **Ajouter une ressource** pour ajouter des services. Un panneau de catégories de services s'affiche. Par exemple, sélectionnez **Données** pour consulter les bases de données disponibles, et sélectionnez **Cloudant NoSQL DB**.
2. Sélectionnez un plan de tarification pour le service, par exemple Lite, et cliquez sur **Créer**.

Une instance du service est créée et elle vous fournit les données d'identification du service pour l'application, et dans certains cas elle ajoute le code nécessaire à votre projet pour inclure la connexion correcte au service. Vous pouvez désormais ajouter d'autres services à l'aide du bouton **Ajouter une ressource** ou en cliquant sur le bouton **Télécharger le code** pour obtenir le code du projet. 

Une fois le projet téléchargé, vous pouvez commencer à gérer votre application.

## Etape 3. Développement de votre application avec Xcode
Une fois votre projet téléchargé, vous pouvez le modifier et le développer à l'aide de Xcode, puis envoyer par téléchargement votre application modifiée pour le déploiement dans le cloud.

1. Créez un projet Xcode.  
  Avant d'utiliser votre projet dans Xcode, vous devez créer un fichier projet Xcode à l'aide de la commande Swift Package Manager. Exécutez la commande suivante dans le répertoire racine de votre projet téléchargé (qui contient un fichier `Package.swift`) :
  ```
  swift package generate-xcodeproj
  ```
  {: codeblock}

  Un projet Xcode est créé dans le même répertoire auquel vous pouvez ensuite accéder.

2. Définissez la cible Xcode pour le projet.  
  Pour lancer le serveur Kitura, vous devez éditer le schéma en cliquant sur la section **project_name-Package** dans la barre d'outils et en sélectionnant la cible **project_name** dans le menu. Vérifiez que l'appareil cible est défini sur **My Mac**.

3. Lancez le serveur Kitura en local  
  Cliquez sur le bouton **Exécuter** ou utilisez le raccourci de touche `⌘+R` pour démarrer le serveur Kitura. Une fois le serveur démarré, vous pouvez vérifier que les URL Kitura standard suivantes sont opérationnelles :
  * Surveillance Kitura : [http://localhost:8080/swiftmetrics-dash/]()
  * Vérification de l'intégrité Kitura : [http://localhost:8080/health]()

## Etape 5. Ajout d'API REST
Un serveur Kitura squelette est créé, mais il ne fournit aucune API REST pouvant être utilisée par une application iOS. Ajoutez des API REST dans Kitura avec un codage minimal. Les étapes suivantes illustrent l'ajout d'une API REST pour la demande `GET` sur `/meals`, qui est conçue pour renvoyer les objets `Meal` stockés par le serveur Kitura.

1. Ajoutez une structure `Meal` dans le fichier `Sources/Application/Application.swift` :  
  Avant la déclaration de la classe App, ajoutez ce qui suite pour créer une structure Meal conforme au protocole Codable :  
  ```swift
	struct Meal: Codable {    
	    var name: String
	    var photo: Data
	    var rating: Int
	}
  ```
  {: codeblock}

2. Ajoutez un dictionnaire dans le fichier `Sources/Application/Application.swift` afin de stocker des objets `Meal` en ajoutant `let cloudEnv = CloudEnv()` à la section de code suivante :
 
  ```swift
  private var mealStore: [String: Meal] = [:]
  ```
  {: codeblock}

3. Ajoutez un gestionnaire pour les demandes `GET` sur `/meals` dans le fichier `Sources/Application/Application.swift` en ajoutant le code suivant dans la fonction `postInit()` :  

  ```swift
  router.get("/meals", handler: loadHandler)
  ```
  {: codeblock}

4. Implémentez la fonction loadHandler dans le fichier `Sources/Application/Application.swift` en ajoutant le code suivant en tant qu'autre fonction dans la classe `App` :  

  ```swift
  func loadHandler(completion: ([Meal]?, RequestError?) -> Void ) {
      let meals: [Meal] = self.mealStore.map({ $0.value })
    completion(meals, nil)
  }
  ```
  {: codeblock"}

Vous disposez maintenant d'une API REST pour les demandes `GET` sur `/meals` qui répond avec un tableau `Meals` ou une erreur.

5. Exécutez le projet.  
  Appuyez sur le bouton **Run** ou utilisez le raccourci de touche `⌘+R`. Si vous y êtes invité, sélectionnez **Allow incoming network connections**.

6. Testez l'API REST.
  Si vous utilisez une demande `GET`, qui correspond à la façon dont les navigateurs Web demandent les données d'un serveur, vous pouvez tester l'API REST à l'aide de l'URL suivante :  
  ```
  * `GET /meals`:	[http://localhost:8080/meals]()
  ```

  Un tableau vide est renvoyé `[]` car aucun objet `Meal` n'est actuellement stocké dans `mealStore`. 

Vous pouvez utiliser le tutoriel [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend), lequel vous aidera à générer un jeu d'API REST pour le stockage, l'extraction et la suppression d'objets `Meal` d'une application iOS, y compris le stockage des données dans une base de données.
{: tip}

## Etape 6. Installation de KituraKit dans votre application iOS
Les API REST générées par l'utilisation du serveur Kitura sont des API Web standard, utilisables depuis n'importe quelle application quelle que soit la bibliothèque client qui est utilisée ou le langage dans lequel est écrit le client. Cela signifie que vous pouvez utiliser Alamofire, RestKit ou URLSession pour établir des connexions au serveur. Kitura fournit également un connecteur client spécifique et optimisé afin de simplifier l'appel de ses API REST depuis iOS, au format de KituraKit. 

KituraKit fournit une image miroir des API du gestionnaire routeur utilisées dans Kitura, ce qui rend possible le partage des types Swift entre le client et le serveur avec peu d'effort. Cette fonction élimine le besoin de déclencher de manière explicite l'encode et le décodage JSON des données qui sont envoyées au serveur ou reçues de ce dernier.

Les étapes suivantes illustrent l'installation de KituraKit dans votre application iOS et son utilisant pour l'appel de l'API REST `GET /meals` créée à l'aide de Kitura :

1. Créez un fichier Pod à la racine du répertoire de votre application iOS si ce n'est déjà fait :
  ```
  pod init
  ```
  {: codeblock}

2. Editez le fichier Pod afin de définir une plateforme globale de iOS 11 pour votre projet en remplaçant la ligne suivante :
  ```
  # platform :ios, '9.0'
  ```
  {: codeblock}

  Par le code suivant :
	```
  platform :ios, '11.0'
  ```
  {: codeblock}

3. Ajoutez KituraKit au profil en ajoutant ce qui suit sous `# Pods for <application name>` :
  ```
  pod 'KituraKit', :git => 'https://github.com/IBM-Swift/KituraKit.git', :branch => 'pod'
  ```
  {: codeblock}

4. Sauvegardez le fichier Pod et installez KituraKit à l'aide de la commande suivante :
  ```
  pod install
  ```
  {: codeblock}

5. Rouvrez l'application dans Xcode en ouvrant l'espace de travail (et non le projet). Vous pouvez maintenant ajouter la même définition `Meal` à votre application iOS telle qu'utilisée sur le serveur Kitura :
  ```swift
  struct Meal: Codable {    
      var name: String
      var photo: Data
      var rating: Int
  }
  ```
  {: codeblock}

  And use the following code to make a connection to the Kitura server:
  
  ```swift
  import KituraKit
  
  // Connect to the Kitura server on http://localhost:8080
  guard let client = KituraKit(baseURL: "http://localhost:8080") else {
      print("Error creating KituraKit client")
      return
  }
  
  // Make a request to `GET /meals`, expecting a [Meal] or a RequestError
  client.get("/meals") { (meals: [Meal]?, error: RequestError?) in
      // Check for an error
      guard error == nil else {
          print("Error saving meal to Kitura: \(error!)")
          return
      }
      // Check for meals
      guard let meals = meals else {
          print("Received Meals!")
          return
      }
  }
  ```
  {: codeblock}

The Kitura server is called by using `client.get("/meals")` with a callback that matches the parameters from Kitura's completion handler.

You can use the [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend) tutorial, which helps you build a set of REST APIs for storing, fetching, and deleting `Meal` objects from an iOS application, including storing the data in a database.
{: tip}

## Next Steps
Now that you have a Kitura server that provides a REST API that can be called by your iOS application, you are ready to deploy your server to {{site.data.keyword.cloud_notm}}. Deployments can be done by using Containers with Kubernetes, Secure Containers, or Cloud Foundry.
