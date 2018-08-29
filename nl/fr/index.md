---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Tutoriel de mise en route
{: #set_up}

{{site.data.keyword.cloud}} propose des solutions et des services qui permettent aux développeurs et aux applications Swift de bénéficier de fonctions de sécurité, d'intelligence artificielle et de la valeur exigée par vos clients. Avec une gamme étendue d'offres et de logiciels SDK, vous pouvez optimiser ces services et mettre rapidement sur le marché vos applications de pointe. Le guide de programmation Swift vous indique comment ajouter des services à une application Swift nouvelle ou existante application, qu'il s'agisse d'un client iOS ou Swift côté serveur .
{: shortdesc}

Le tutoriel suivant est un point d'entrée pour vous montrer comment créer rapidement une appli mobile Swift avec {{site.data.keyword.mobileanalytics_full}} à l'aide d'un kit de démarrage vide de la [{{site.data.keyword.cloud_notm}} console de développement pour Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits). Depuis la console, vous pouvez ajouter le service {{site.data.keyword.mobileanalytics_short}}, télécharger le code, exécuter l'appli iOS en local dans Xcode, configurer et surveiller l'appli.

## Etape 1. Exigences pour les développeurs
{: #dev-requirements}

Pour démarrer avec le développement iOS sur {{site.data.keyword.cloud_notm}}, assurez-vous de respecter les exigences suivantes :

### Système d'exploitation

Une meilleure pratique consiste à développer les applis Swift à l'aide du dernier niveau de matériel MacOS pris en charge, et à effectuer des tests avec les versions iOS les plus récentes. Inscrivez-vous pour un compte [Apple Developer](https://developer.apple.com/) afin de pouvoir effectuer des tests sur une unité physique.

### iOS et Xcode
{: #ios_and_xcode}

- Installez [Xcode 8+](https://developer.apple.com/xcode/) (ou supérieur).
- Déployez sur des [unités iOS 8](https://support.apple.com/downloads/ios) (ou supérieur).
- Passez en revue les [Règles de soumission d'App Store](https://developer.apple.com/app-store/guidelines/) avant de soumettre des applis à Apple.

### Logiciels SDK et gestion des dépendances

Les outils suivants garantissent que vous pouvez installer les logiciels SDK natifs pour qu'ils fonctionnent avec les différents services {{site.data.keyword.cloud_notm}}.

1. Installez [CocoaPods](https://cocoapods.org/) pour la prise en charge des logiciels SDK {{site.data.keyword.cloud_notm}} :
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
2. Installez [Homebrew](https://brew.sh/) pour faciliter l'installation de Carthage :
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

3. Installez [Carthage](https://github.com/Carthage/Carthage) pour la prise en charge des logiciels SDK Watson :
  ```
  brew install carthage
  ```
  {: codeblock}

## Etape 2. Création d'une appli Swift iOS personnalisée
{: #create-ios-app}

1. Connectez-vous à la [{{site.data.keyword.cloud_notm}} console de développement pour Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits).
2. Cliquez sur **Créer une application**.
3. Sur la page [Empty Starter](https://console.bluemix.net/developer/appledevelopment/create-app), vous pouvez utiliser la configuration par défaut, ou mettre à jour les zones si nécessaires. Assurez-vous que le langage **iOS Swift** est sélectionné. Cliquez sur **Créer**.

## Etape 3. Ajout du service {{site.data.keyword.mobileanalytics_short}}
{: #adding-services}

1. Depuis la page Détails de l'application, cliquez sur le bouton **Ajouter une ressource**.
2. Sélectionnez **Mobile**, puis cliquez sur **Suivant**.
3. Sélectionnez **{{site.data.keyword.mobileanalytics_short}}**, puis cliquez sur **Suivant**.
4. Cliquez sur **Créer**.

## Etape 4. Téléchargement du code et configuration des logiciels SDK clients
{: #run-locally}

Pour télécharger le code, cliquez sur **Télécharger le code** sous `Applis` > `Votre appli`. Le code téléchargé est fourni avec les logiciels SDK clients **{{site.data.keyword.mobileanalytics_short}}** inclus. Les logiciels SDK clients sont disponibles dans CocoaPods and Carthage. Pour cette solution, utilisez CocoaPods.

1. Décompressez le code téléchargé. Ensuite, à l'aide d'un terminal, accédez au dossier décompressé.
  ```
  cd <Name of Project>
  ```
2. Le dossier contient un fichier pod avec les dépendances requises. Exécutez la commande suivante pour installer les dépendances (logiciels SDK clients) :
  ```
  pod install
  ```
  {: codeblock}

## Etape 5. Configuration de l'appli pour l'utilisation de {{site.data.keyword.mobileanalytics_short}}
{: #configure-analytics}

1. Ouvrez `.xcworkspace` dans Xcode et accédez à `AppDelegate.swift`.
  **Remarque :** Assurez-vous de toujours ouvrir le nouvel espace de travail Xcode, au lieu du fichier projet Xcode d'origine : `MyApp.xcworkspace`.
   ![Open Xcode](images/Xcode.png)

  `BMSCore` est le logiciel SDK Core et est la base des logiciels SDK de client mobile. `BMSClient` est une classe de BMSCore et initialisée dans AppDelegate.swift. Avec BMSCore, le logiciel SDK {{site.data.keyword.mobileanalytics_short}} est déjà importé dans le projet.
  
2. Le code d'initialisation d'analyse est déjà inclus comme illustré dans le fragment suivant :
  ```
  // Analytics client SDK is configured to record lifecycle events.
         	Analytics.initialize(appName:dictionary["appName"] as? String,
        			     apiKey: dictionary["analyticsApiKey"] as? String,
        	        	     deviceEvents: .lifecycle)

        	// Enable Logger (disabled by default) and set level to ERROR (DEBUG by default).
        	Logger.isLogStorageEnabled = true
        	Logger.logLevelFilter = .error
  ```
  {: codeblock}

  **Remarque :** Les données d'identification du service font partie du fichier `BMSCredentials.plist`.

3. Collecte des analyses d'usage à l'aide du `journal d'événements`. Accédez au fichier `ViewController.swift` pour voir le code suivant.
  ```
   func didBecomeActive(_ notification: Notification) {
       Analytics.send()
       Logger.send()
    }
  ```
  {: codeblock}

   Pour les fonctions d'analyse et de journalisation avancées, voir [Collecte des analyses d'utilisation](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics) et [journalisation](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger).
   {:tip}

## Etape 6. Surveillance de l'appli avec {{site.data.keyword.mobileanalytics_short}}
Le service {{site.data.keyword.mobileanalytics_short}} fournit des données d'utilisation de l'application et de performance pour les développeurs d'application mobile et les propriétaires d'application. Grâce à {{site.data.keyword.mobileanalytics_short}}, les propriétaires et les développeurs d'application peuvent comprendre ce qui se passe côté utilisateur. Ils peuvent utiliser ces données pour générer de meilleures applications pertinentes pour les utilisateurs, et qui se différencient dans l'immense gamme d'applications mobiles.

Le service inclut la console {{site.data.keyword.mobileanalytics_short}} qui permet aux propriétaires et aux développeurs d'applications de surveiller les performances des applications mobiles, de consulter les statistiques d'utilisateur et d'effectuer des recherches dans les journaux de périphériques.  

1. Ouvrez le service `{{site.data.keyword.mobileanalytics_short}}` depuis l'appli mobile que vous avez créée ou cliquez sur les trois points verticaux en regard du service et sélectionnez `Ouvrir le tableau de bord`.
2. Vous pouvez voir les utilisateur, les sessions et d'autres données d'appli en temps réel en désactivant `Mode démonstration`. Vous pouvez filtrer les informations d'analyses à l'aide des critères suivants :
    * Date.
    * Application.
    * Systèmes d'exploitation.
    * Version de l'appli.
         ![{{site.data.keyword.mobileanalytics_short}}](images/mobile_analytics.png)
3. [Cliquez ici](https://console.bluemix.net/docs/services/mobileanalytics/app-monitoring.html#monitoringapps) pour définir des alertes, surveiller les pannes d'appli et surveiller les demandes réseau.

## Etapes suivantes
{: #next-steps}

### Ajout de services supplémentaires
Vous pouvez ajouter d'autres services à votre appli iOS directement depuis la console Web, tels que les services communément utilisés suivants :

* [Ajout du service Notifications push](/push/push_notifications.html)
* [Ajout de l'authentification d'utilisateur avec un ID d'appli](/authenticate/app_id.html)

### Utilisation des outils de développement {{site.data.keyword.cloud_notm}}
Vous pouvez également apprendre comment développer des applis Swift à l'aide des [outils de développement {{site.data.keyword.cloud_notm}}](../cli/index.html), lesquels proposent une ligne de commande pour la création, le développement et le déploiement d'applications Web, mobile, et microservice de bout en bout.

