---

copyright:
  years: 2018
lastupdated: "2018-11-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Tutoriel de mise en route
{: #set_up}

{{site.data.keyword.cloud}} propose des solutions et des services qui permettent aux développeurs Swift de créer des applications offrant la sécurité, l'intelligence artificielle et la valeur que vos clients exigent. Grâce à une gamme étendue d'offres et de logiciels SDK, vous pouvez utiliser ces services pour mettre rapidement sur le marché des applications de pointe. Ce guide de programmation Swift vous explique comment ajouter des services à une application Swift nouvelle ou existante, qu'il s'agisse d'une application Swift côté serveur ou d'une application client iOS.
{: shortdesc}

Le tutoriel ci-dessous vous montre comment créer rapidement une application mobile Swift avec {{site.data.keyword.mobileanalytics_full}} à l'aide d'un kit de démarrage vide à partir de la [console de développement {{site.data.keyword.cloud_notm}} pour Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits). Depuis la console, vous pouvez ajouter le service {{site.data.keyword.mobileanalytics_short}}, télécharger le code, exécuter l'application iOS en local dans Xcode, configurer et surveiller l'application.

## Etape 1. Exigences pour les développeurs
{: #dev-requirements}

Pour commencer le développement iOS sur {{site.data.keyword.cloud_notm}}, assurez-vous de respecter les exigences suivantes :

### Système d'exploitation

Pour développer des applications Swift, il est recommandé d'utiliser le dernier matériel MacOS pris en charge, et d'effectuer les tests avec les versions iOS les plus récentes. Inscrivez-vous à un compte [Apple Developer](https://developer.apple.com/) afin de pouvoir effectuer des tests sur une unité physique.

### iOS et Xcode
{: #ios_and_xcode}

- Installez [Xcode 8+](https://developer.apple.com/xcode/) (ou supérieur).
- Déployez sur des [unités iOS 8](https://support.apple.com/downloads/ios) (ou supérieur).
- Avant de soumettre une application à Apple, consultez les [règles de soumission d'App Store](https://developer.apple.com/app-store/guidelines/).

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

## Etape 2. Création d'une application Swift iOS personnalisée
{: #create-ios-app}

1. Connectez-vous à la [console de développement {{site.data.keyword.cloud_notm}} pour Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits).
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

Pour télécharger le code, cliquez sur **Télécharger le code** sous `Applications` > `Vos applications`. Le code téléchargé est fourni dans les logiciels SDK clients inclus dans **{{site.data.keyword.mobileanalytics_short}}**. Les logiciels SDK clients sont disponibles dans CocoaPods et Carthage. Pour cette solution, utilisez CocoaPods.

1. Décompressez le code téléchargé. Puis, à l'aide d'un terminal, accédez au dossier extrait.
  ```
  cd <Name of Project>
  ```
2. Le dossier contient un fichier pod avec les dépendances requises. Exécutez la commande suivante pour installer les dépendances (logiciels SDK clients) :
  ```
  pod install
  ```
  {: codeblock}

## Etape 5. Configuration de l'application pour l'utilisation de {{site.data.keyword.mobileanalytics_short}}
{: #configure-analytics}

1. Ouvrez `.xcworkspace` dans Xcode et accédez à `AppDelegate.swift`.
  **Remarque :** Assurez-vous de toujours ouvrir le nouvel espace de travail Xcode, au lieu du fichier projet Xcode d'origine : `MyApp.xcworkspace`.
   ![Open Xcode](images/Xcode.png)

  `BMSCore` est le logiciel SDK Core et est la base des logiciels SDK de client mobile. `BMSClient` est une classe de `BMSCore` initialisée dans `AppDelegate.swift`. Avec `BMSCore`, {{site.data.keyword.mobileanalytics_short}} SDK est déjà importé dans le projet.
  
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

3. Collectez les analyses d'utilisation à l'aide du `journal d'événements`. Accédez au fichier `ViewController.swift` pour voir le code suivant.
  ```
   func didBecomeActive(_ notification: Notification) {
       Analytics.send()
       Logger.send()
    }
  ```
  {: codeblock}

   Pour connaître les fonctions de journalisation et d'analyse avancées, consultez [Collecte des analyses d'utilisation](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics) et [journalisation](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger).
   {:tip}

## Etape 6. Surveillance de l'application avec {{site.data.keyword.mobileanalytics_short}}
Le service {{site.data.keyword.mobileanalytics_short}} fournit aux développeurs d'applications mobiles et aux propriétaires d'applications des informations clés sur l'utilisation des applications et sur les performances. En utilisant {{site.data.data.keyword.mobileanalytics_short}}, les propriétaires d'applications et les développeurs peuvent comprendre ce qui se passe du côté utilisateur. Ils peuvent utiliser ces connaissances pour créer de meilleures applications qui sont pertinentes pour les utilisateurs et qui se démarquent dans la véritable marée des applications mobiles.

Le service inclut la console {{site.data.data.keyword.mobileanalytics_short}} où les développeurs et les propriétaires d'applications peuvent surveiller les performances des applications mobiles, voir les statistiques d'utilisation et les journaux des périphériques de recherche.

1. Ouvrez le service `{{site.data.keyword.mobileanalytics_short}}` depuis l'application mobile que vous avez créée ou cliquez sur les trois points verticaux en regard du service, puis sélectionnez `Ouvrir le tableau de bord`.
2. Vous pouvez voir les utilisateur, les sessions et d'autres données d'application en temps réel en désactivant `Mode démonstration`. Vous pouvez filtrer les informations d'analyses à l'aide des critères suivants :
    * Date.
    * Application.
    * Systèmes d'exploitation.
    * Version de l'application.
         ![{{site.data.keyword.mobileanalytics_short}}](images/mobile_analytics.png)
3. [Cliquez ici](https://console.bluemix.net/docs/services/mobileanalytics/app-monitoring.html#monitoringapps) pour définir des alertes, surveiller les pannes d'application et surveiller les demandes réseau.

## Etapes suivantes
{: #next-steps}

### Ajout de services supplémentaires
Vous pouvez ajouter d'autres services à votre application iOS directement depuis la console Web, tels que les services communément utilisés suivants :

* [Ajout du service Notifications push](/docs/services/mobilepush/index.html)
* [Ajout de l'authentification d'utilisateur avec un ID d'application](/docs/services/appid/index.html)

### Utilisation des outils de développement {{site.data.keyword.cloud_notm}}
Vous pouvez également apprendre à développer des applications Swift en utilisant les [outils de développement {{site.data.keyword.cloud_notm}}](../cli/index.html), qui offrent une approche basée sur les lignes de commandes pour créer, développer et déployer des applications Web, mobiles et de micro-services complètes.

