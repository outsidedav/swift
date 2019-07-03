---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-19"

keywords: foodtrackerbackend, kitura swift, urlsession sdk, alamofire, restkit, kiturakit, kitura, xcode kitura, meals swift, rest api kitura, rest api swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Création d'une application avec Kitura
{: #kitura}

[Kitura](https://www.kitura.io){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") est une infrastructure Swift côté serveur destinée à développer des applications de back end et des applications Web iOS. Cette infrastructure crée des API REST qui peuvent être appelées à partir de l'application iOS en utilisant des logiciels SDK URLSession, tels que Alamofire, RestKit ou du logiciel SDK [KituraKit](https://github.com/ibm-swift/kiturakit){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") fourni par Kitura lui-même.

Kitura est capable d'intégrer tous les services et fonctionnalités fournis par {{site.data.keyword.cloud}}, y compris {{site.data.keyword.appid_short}}, {{site.data.keyword.mobilepushshort}} et {{site.data.keyword.mobileanalytics_short}}, ainsi que des bases de données, l'apprentissage automatique et d'autres services. Kitura peut ensuite être déployé et mis automatiquement à l'échelle à l'aide des plateformes Cloud Foundry ou de Docker (basés sur Kubernetes) dans {{site.data.keyword.cloud}}.

Kitura offre une [interface de ligne de commande](https://www.kitura.io/guides/kituracli/gettingstarted.html){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") `kitura` qui simplifie la création, la génération, le test et le déploiement des applications Kitura. Les applications qui sont générées à l'aide de l'interface CLI Kitura incluent une prise en charge complète pour le déploiement sur n'importe quel cloud prenant en charge les technologies Cloud Foundry, Docker et Kubernetes. Toutefois, si vous générez spécifiquement pour {{site.data.keyword.cloud_notm}}, il est recommandé d'utiliser la console de développement Apple d'IBM dans le navigateur ou d'utiliser {{site.data.keyword.dev_cli_notm}}. Par ailleurs, comme les deux méthodes partagent une technologie sous-jacente, la console de développement Apple et IBM Developer Tools créent un projet hébergé et un pipeline de déploiement pour vous, et ils mettent à disposition les services dont votre application a besoin.

## Avant de commencer
{: #prereqs-kitura}

Tout d'abord, assurez-vous que la configuration prérequise suivante est respectée :  

* iOS 11.0+  
* Xcode 9.0  
* Swift 4.0+  
* CocoaPods  

## Etape 1. Création d'une application Kitura à l'aide du navigateur
{: #create_kitura}

1. Accédez à la section Kits de démarrage de la console de développement Apple. Sélectionnez un module de démarrage prédéfini, par exemple **Swift for Backend for Frontend API** ou créez une application personnalisée à l'aide de la vignette **Créer une application**. Cliquez sur **Créer une application**.

2. Donnez un nom à votre application et sélectionnez l'emplacement où vous souhaitez la déployer. Si vous n'êtes pas sûr de l'endroit où l'application doit être déployée, utilisez les valeurs par défaut, car elles peuvent être modifiées ultérieurement.

3. Sélectionnez le langage Swift. Une application Swift côté serveur est créée. Sont également affichés les options Hôte et Domain, qui forme l'URL de l'application. Si vous n'êtes pas sûr, utilisez les valeurs par défaut et vous pouvez également fournir votre propre domaine personnalisé provenant d'un fournisseur de domaine où l'application doit être hébergée.

4. Cliquez sur **Créer une application**.

Une application est créée, mais elle n'utilise pas encore de services supplémentaires. Vous pouvez ajouter des services en cliquant sur le bouton **Ajout d'un service** ou **Télécharger le code** pour obtenir le code de l'application. Vous pouvez aussi facilement ajouter des services à une application existante.

## Etape 2. Ajout de services
{: #add_services-kitura}

1. Cliquez sur **Ajout d'un service** pour ajouter des services. Les catégories de service s'affichent. Par exemple, sélectionnez **Données** pour consulter les bases de données disponibles, et sélectionnez **Cloudant NoSQL DB**.
2. Sélectionnez un plan de tarification pour le service, par exemple Lite, et cliquez sur **Créer**.

Une instance du service est créée, laquelle fournit les données d'identification de l'application et dans certains cas, ajoute le code nécessaire à votre application pour inclure la connexion correcte au service. Vous pouvez désormais ajouter d'autres services en utilisant le bouton **Ajout d'un service** ou en cliquant sur **Télécharger le code** pour obtenir le code de l'application.

Une fois l'application téléchargée, vous pouvez commencer à l'utiliser.

## Etape 3. Développement de votre application avec Xcode
{: #develop_xcode-kitura}

Une fois votre code d'application téléchargé, vous pouvez le modifier et l'étoffer à l'aide de Xcode, puis envoyer votre application modifiée à des fins de déploiement dans le cloud.

1. Créez un projet Xcode.  
  Avant d'utiliser votre projet dans Xcode, vous devez créer un fichier projet Xcode à l'aide de la commande Swift Package Manager. Exécutez la commande suivante dans le répertoire racine de votre projet téléchargé où se trouve le fichier `Package.swift` :
  ```
  swift package generate-xcodeproj
  ```
  {: codeblock}

  Un projet Xcode est créé dans le même répertoire auquel vous pouvez ensuite accéder.

2. Définissez la cible Xcode pour le projet.  
  Pour lancer le serveur Kitura, vous devez éditer le schéma en cliquant sur la section **project_name-Package** dans la barre d'outils et en sélectionnant la cible **project_name** dans le menu. Vérifiez que l'appareil cible est défini sur **My Mac**.

3. Exécutez le serveur Kitura localement. 
  Cliquez sur **Exécuter** ou utilisez le raccourci clavier `⌘+R` pour démarrer le serveur Kitura. Une fois le serveur démarré, vous pouvez vérifier que les URL Kitura standard suivantes sont opérationnelles :
  * Surveillance Kitura : [http://localhost:8080/swiftmetrics-dash/](http://localhost:8080/swiftmetrics-dash/)
  * Vérification de l'intégrité Kitura : [http://localhost:8080/health](http://localhost:8080/health)

## Etape 4. Ajout d'API REST
{: #add_restapi-kitura}

Un squelette de serveur Kitura est créé, mais il ne fournit pas une API REST pouvant être utilisée par une application iOS. Ajoutez des API REST dans Kitura avec un codage minimal. Utilisez les étapes suivantes pour ajouter une API REST pour les requêtes `GET` sur `/meals`, qui est conçue pour retourner les objets `Meal` qui sont enregistrés par le serveur Kitura.

1. Ajoutez une structure `Meal` dans le fichier `Sources/Application/Application.swift` :  
  Avant la déclaration de la classe `App`, ajoutez les lignes suivantes pour créer une structure Meal conforme au protocole Codable :  
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
  Cliquez sur **Exécuter** ou utilisez le raccourci clavier `⌘+R`. Si vous y êtes invité, sélectionnez **Allow incoming network connections**.

6. Testez l'API REST en utilisant une requête `GET`, qui correspond à la manière dont les navigateurs Web demandent des données à un serveur. 

  Vous pouvez tester l'API REST à l'aide de l'URL suivante :  
  ```swift
  * `GET /meals`:	[http://localhost:8080/meals](http://localhost:8080/meals)
  ```
  {: codeblock}

  Un tableau vide est renvoyé `[]` car aucun objet `Meal` n'est actuellement stocké dans `mealStore`. 

Vous pouvez utiliser le tutoriel [FoodTracker Backend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), qui vous aide à créer un ensemble d'API REST pour stocker, extraire et supprimer des objets `Meal` dans une application iOS, notamment pour stocker les données dans une base de données.
{: tip}

## Etape 5. Installation de KituraKit dans votre application iOS
{: #install-kiturakit}

Les API REST générées par l'utilisation du serveur Kitura sont des API Web standard, utilisables depuis n'importe quelle application quelle que soit la bibliothèque client qui est utilisée ou le langage dans lequel est écrit le client. Cela signifie que vous pouvez utiliser Alamofire, RestKit ou URLSession pour établir des connexions au serveur. Kitura fournit également un connecteur client spécifique et optimisé afin de simplifier l'appel de ses API REST depuis iOS, au format de KituraKit. 

KituraKit fournit une image miroir des API du gestionnaire routeur utilisées dans Kitura, ce qui rend possible le partage des types Swift entre le client et le serveur avec peu d'effort. Cette fonction élimine le besoin de déclencher de manière explicite l'encode et le décodage JSON des données qui sont envoyées au serveur ou reçues de ce dernier.

Les étapes suivantes illustrent l'installation de KituraKit dans votre application iOS et son utilisation pour l'appel de l'API REST `GET /meals` créée à l'aide de Kitura :

1. Créez un fichier Pod à la racine de votre répertoire d'application iOS s'il n'existe pas déjà :
  ```
  pod init
  ```
  {: codeblock}

2. Editez le fichier Pod afin de définir une plateforme globale de iOS 11 pour votre projet en remplaçant la ligne suivante :
  ```pod
  # platform :ios, '9.0'
  ```
  {: codeblock}

  Par le code suivant :
	```pod
  platform :ios, '11.0'
  ```
  {: codeblock}

3. Ajoutez KituraKit au profil en ajoutant ce qui suit sous `# Pods for <application name>` :
  ```pod
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
  
  /* Connect to the Kitura server on http://localhost:8080 */
  guard let client = KituraKit(baseURL: "http://localhost:8080") else {
      print("Error creating KituraKit client")
      return
  }
  
  /* Make a request to `GET /meals`, expecting a [Meal] or a RequestError */
  client.get("/meals") { (meals: [Meal]?, error: RequestError?) in
      /* Check for an error */
      guard error == nil else {
          print("Error saving meal to Kitura: \(error!)")
          return
      }
      /* Check for meals */
      guard let meals = meals else {
          print("Received Meals!")
          return
      }
  }
  ```
  {: codeblock}

The Kitura server is called by using `client.get("/meals")` with a callback that matches the parameters from Kitura's completion handler.

You can use the [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon") tutorial, which helps you build a set of REST APIs for storing, fetching, and deleting `Meal` objects from an iOS application, including storing the data in a database.
{: tip}

## Next Steps
{: #next-kitura notoc}

Now that you have a Kitura server that provides a REST API that can be called by your iOS application, you're ready to deploy your server to {{site.data.keyword.cloud_notm}}. [Deployments](/docs/swift?topic=swift-deploy_apps-swift#deploy_apps-swift) can be done by using containers with Kubernetes, secure containers, Cloud Foundry, Cloud Foundry Enterprise Environment, or virtual instances.
