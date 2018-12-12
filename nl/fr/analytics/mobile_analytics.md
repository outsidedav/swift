---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Collecte à l'aide de Mobile Analytics
{: #mobile_analytics}

{{site.data.keyword.mobileanalytics_short}} sur {{site.data.keyword.cloud_notm}} fournit aux développeurs, administrateurs et parties prenantes des informations sur les performances et l'utilisation des applications mobiles. Grâce au service {{site.data.keyword.mobileanalytics_short}}, vous pouvez :

 - **Obtenir un aperçu immédiat** - Voyez les mesures de performance et d'utilisation en direct.

 - **Implémenter en quelques minutes** - Créez une instance de service dans {{site.data.keyword.cloud_notm}}, ajoutez le logiciel SDK à votre projet, collez deux lignes de code dans votre application et vous êtes prêt à collecter des dizaines de mesures prédéfinies.

 - **Collecter les données de votre choix** - Instrumentez les applications à l'aide d'événements personnalisés, observez comment les utilisateurs interagissent avec votre application, suivez les dépenses et surveillez l'activité de l'application.

 - **Consulter un aperçu des métriques** - La console {{site.data.keyword.mobileanalytics_short}} propose des graphiques prêts à l'emploi, sans avoir à écrire de requêtes.

 - **Vous concentrer sur ce qui est important pour vous** - Filtrez les métriques par heure, adaptateur, application, version d'application, système d'exploitation, version de système d'exploitation ou modèle d'appareil.

 - **Identifier rapidement les problèmes** - Surveillez l'état des pannes. Définissez des déclencheurs d'alerte sur des mesures critiques et dirigez les alertes vers n'importe quel noeud final REST.

 - **Identifier et résoudre la cause profonde d'un problème** - Utilisez la journalisation personnalisée dans votre application, chargez les journaux de façon automatique et effectuez des recherches dans ces derniers depuis la console. Explorez en aval les événements de panne pour voir les traces de pile.

## Avant de commencer :

Tout d'abord, assurez-vous que vous respectez la configuration prérequise suivante :

 - iOS 8.0+ / watchOS 2.0+
 - Xcode 7.3, 8.0
 - Swift 2.2 - 3.0
 - CocoaPods ou Carthage

## Etape 1. Création d'une instance de {{site.data.keyword.mobileanalytics_short}}
{: #mobile_analytics_create}

1. Dans le catalogue {{site.data.keyword.cloud_notm}}, cliquez sur **Mobile** > **{{site.data.keyword.mobileanalytics_short}}**. L'écran de configuration du service s'ouvre.
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Cliquez sur **Créer**.
4. Dans le panneau de navigation, cliquez sur **Connexions** pour sélectionner une application et la lier à votre service. Vous pouvez lier l'instance de service à votre application ultérieurement si vous ne la liez pas pendant l'étape de création.

## Etape 2. Installation du logiciel SDK Swift iOS 

Le service fournit des logiciels SDK spécifiques à la plateforme afin de simplifier le développement d'application. Les logiciels SDK Swift d'{{site.data.keyword.cloud_notm}} Mobile Services peuvent être installés avec CocoaPods ou Carthage. Pour plus d'informations, consultez le site [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics).

Vous pouvez instrumenter votre application mobile à l'aide du logiciel SDK d'{{site.data.keyword.mobileanalytics_full}}. Le logiciel SDK Swift est disponible pour iOS et watchOS.

1. Vérifiez que Xcode est correctement configuré. Pour savoir comment configurer votre environnement de développement iOS, consultez le [site Web Apple Developer![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/support/xcode/){: new_window}. En savoir plus sur les [exigences relatives à Xcode![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#requirements){: new_window} pour le logiciel SDK client Swift Analytics.

2. Activez l'API d'emplacement en ajoutant une propriété dans le fichier `Info.plist` du dossier projet de votre application. Par exemple, entrez `Privacy - Location Usage Description` et indiquez une justification pour l'ajout de l'API d'emplacement, par exemple "L'application exige que le service de localisation soit activé".

Le logiciel SDK {{site.data.keyword.mobileanalytics_short}} est distribué avec [CocoaPods ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cocoapods.org/){: new_window} et [Carthage ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/Carthage/Carthage#getting-started){: new_window}, qui sont des gestionnaires de dépendance pour les projets Cocoa. CocoaPods et Carthage téléchargent automatiquement des artéfacts depuis des référentiels afin de les rendre disponibles pour votre application. Sélectionnez CocoaPods ou Carthage :

### CocoaPods
{: #cocoapods}

1. Suivez les [instructions du logiciel SDK Swift d'{{site.data.keyword.Bluemix_notm}} Mobile Services ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#cocoapods){: new_window} sur GitHub pour installer `BMSAnalytics` à l'aide de CocoaPods et l'ajouter à votre fichier Pod.

2. Après avoir installé le logiciel SDK pour client iOS, [importez et initialisez](sdk.html#initalize-ma-sdk) le logiciel SDK Analytics Client.   

### Carthage
{: #carthage}

Si vous n'utilisez pas CocoaPods, vous pouvez ajouter des infrastructures à votre projet à l'aide de [Carthage ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos){: new_window}.

1. Suivez les [instructions d'installation de Carthage![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#carthage){: new_window} sur GitHub pour installer `BMSAnalytics`.

2. Après avoir installé le logiciel SDK pour client iOS, importez puis initialisez le logiciel SDK Analytics Client.

## Etape 3. Initialisation du logiciel SDK

Avec {{site.data.keyword.mobileanalytics_short}}, vous pouvez collecter les catégories de données suivantes, chacune nécessitant un degré différent d'instrumentation :

**Données prédéfinies** :
Cette catégorie inclut l'utilisation générique et les informations sur l'unité qui concernent toutes les applications. Elle indique le volume, la fréquence ou la durée d'utilisation de l'application. Les données prédéfinies sont collectées automatiquement une fois que vous avez initialisé le logiciel SDK {{site.data.keyword.mobileanalytics_short}} dans votre application.

**Messages du journal d'application ** :
Cette catégorie permet au développeur d'ajouter des lignes de code au sein de l'application pour la consignation de messages personnalisés destinés à aider au développement et au débogage. Le développeur affecte un niveau de gravité/détail à chaque message de journal.

**Evénements personnalisés **:
Cette catégorie inclut les données que vous définissez vous-même et qui sont spécifiques à votre application. Ces données montrent des événements qui se produisent dans votre application, comme le fait de consulter des pages, de cliquer sur des boutons ou d'effectuer un achat dans l'application. Outre l'initialisation du logiciel SDK {{site.data.keyword.mobileanalytics_short}} dans votre application, vous devez aussi ajouter une ligne de code pour chaque événement personnalisé
devant faire l'objet d'un suivi.

## Etape 4. Identification de la valeur de clé d'API de vos données d'identification du service
{: #analytics-clientkey}

Identifiez votre valeur de **clé d'API** avant de configurer le logiciel SDK client. La clé d'API est obligatoire pour l'initialisation du logiciel SDK client.

1. Ouvrez votre tableau de bord de service {{site.data.keyword.mobileanalytics_short}}.
2. Développez **Afficher les données d'identification** pour révéler votre valeur de clé d'API. Vous avez besoin de la valeur de clé d'API lors de l'initialisation du logiciel SDK {{site.data.keyword.mobileanalytics_short}} Client.

## Etape 5.  Initialisation de votre application pour la collecte d'analyses
{: #initalize-ma-sdk}

Initialisez votre application pour qu'elle envoie des journaux au service {{site.data.keyword.mobileanalytics_short}}.

1. Importez le logiciel SDK client.

    Le logiciel SDK Swift est disponible pour iOS et watchOS. Importez les infrastructures `BMSCore` et `BMSAnalytics` en ajoutant les instructions `import` suivantes au début de votre fichier de projet `AppDelegate.swift` :
	```
	import BMSCore
	import BMSAnalytics
	```
	{: codeblock}  

2. Initialisez le logiciel SDK client {{site.data.keyword.mobileanalytics_short}} dans votre application.

	Tout d'abord, initialisez la classe `BMSClient` en plaçant le code d'initialisation dans la méthode `application (_:didFinishLaunchingWithOptions:)` de votre délégué d'application, ou dans un emplacement approprié pour votre projet.
	```
	BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Make sure that you point to your region
	```
	{: codeblock}

	Vous devez initialiser la classe `BMSClient` à l'aide du paramètre **bluemixRegion**. Dans l'initialiseur, la valeur **bluemixRegion** indique le déploiement {{site.data.keyword.Bluemix_notm}} que vous utilisez.

3. Initialisez Analytics en utilisant votre objet d'application et en lui attribuant le nom de votre application.

	Le nom que vous sélectionnez pour votre application (`your_app_name_here`) s'affiche dans la console {{site.data.keyword.mobileanalytics_short}} comme nom d'application. Le nom d'application est utilisé pour filtrer la recherche des journaux d'application dans votre tableau de bord. Lorsque vous utilisez le même nom d'application sur plusieurs plateformes (par exemple, iOS), vous pouvez voir tous les journaux de cette application sous le même nom, quelle que soit la plateforme à partir de laquelle les journaux ont été envoyés.

	Vous devez aussi connaître la valeur de la [clé d'API](#analytics-clientkey).
	```
	Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, collectLocation: true, deviceEvents: .lifecycle, .network)
	```
    {: codeblock}

4. L'application est maintenant initialisée et prête pour la collecte d'analyses. Ensuite, vous pouvez envoyer des données d'analyse au service {{site.data.keyword.mobileanalytics_short}}.		

## Etape 6. Collecte des analyses d'utilisation
{: #app-monitoring-gathering-analytics}

Vous pouvez configurer l'enregistrement des données d'analyse d'utilisation et leur envoi au service {{site.data.keyword.mobileanalytics_short}} par le logiciel SDK client de {{site.data.keyword.mobileanalytics_short}}.

Utilisez les API suivantes pour démarrer l'enregistrement et l'envoi des données d'analyse d'utilisation :
```
// Disable recording of usage analytics (eg: to save disk space)
// Recording is enabled by default

Analytics.isEnabled = false

// Enable recording of usage analytics

Analytics.isEnabled = true

// Send recorded usage analytics to the Mobile Analytics Service

Analytics.send(completionHandler: { (response: Response?, error: Error?) in
    if let response = response {

	  // Handle Analytics send success here.
    }
    if let error = error {
        // Handle Analytics send failure here.
    }
})
```
{: codeblock}

Exemple d'analyse d'utilisation pour consignation d'un événement :
```
// Log a custom analytics event
let eventObject = ["customProperty": "propertyValue"]
Analytics.log(metadata: eventObject)
```
{: codeblock}

## Etape 7. Utilisation du Journal d'événements

Le logiciel SDK client {{site.data.keyword.mobileanalytics_full}} fournit une infrastructure de journalisation similaire à d'autres infrastructures que vous pouvez connaître, telles que `java.util.logging` ou `log4j`. L'infrastructure de journalisation prend en charge plusieurs instances de journal d'événements par module, différents niveaux de journalisation, la capture de traces de pile pour une panne d'application, etc.

Vous pouvez également configurer les données consignées pour un stockage sur l'appareil où s'exécute l'application et envoyer les journaux d'appareil au service {{site.data.keyword.mobileanalytics_short}} plus tard.

L'infrastructure de journalisation du logiciel SDK client {{site.data.keyword.mobileanalytics_short}} prend en charge les niveaux de journalisation suivants, répertoriés du moins prolixe au plus prolixe et accompagnés de recommandations d'utilisation :

  * `FATAL` : Pour les pannes ou les blocages irrémédiables. Le niveau `FATAL` est réservé aux erreurs irrémédiables de journalisation, qui se matérialisent pour l'utilisateur sous la forme d'une panne de l'application.
  * `ERROR` : Pour les exceptions inattendues ou les erreurs de protocole de réseau inattendues.
  * `WARN` : Pour journaliser des avertissements d'utilisation qui ne sont pas considérés comme des erreurs critiques, par exemple, l'utilisation d'API obsolètes ou un ralentissement des réponses du réseau.
  * `INFO` : Pour communiquer des événements d'initialisation et d'autres données qui peuvent être importants, mais non urgents.
  * `DEBUG` : Pour communiquer des informations de débogage permettant aux développeurs de résoudre les défauts de l'application.

### Scénario de niveau de journalisation
{: #log-level-scenario}

Lorsque le niveau du journal d'événements a pour valeur `FATAL`, ce dernier capture les exceptions non interceptées, mais pas les journaux qui produisent l'événement de panne. Vous pouvez définir un niveau de journal plus prolixe, de sorte que les journaux susceptibles de produire une entrée `FATAL`, telle que `WARN` ou `ERROR`, soient également capturés.

Lorsque le niveau du consignateur est défini sur `DEBUG`, vous obtenez également les journaux SDK de Mobile Analytics Client, inclus lors de l'envoi des journaux.

### Exemple d'utilisation de journal d'événements
{: #sample-logger-usage}

**Remarque :** Veillez à ce que votre application soit configurée pour l'utilisation du logiciel SDK {{site.data.keyword.mobileanalytics_short}} Client avant d'utiliser l'infrastructure de journalisation.

Le fragment de code suivant est un exemple d'utilisation de Logger :
```
// Configure Logger. Disabled by default;

Logger.isLogStorageEnabled = true

// Change the minimum log level (optional). Default is - LogLevel.debug

Logger.logLevelFilter = LogLevel.info

// Multiple log instances can be created to organize your logs

let logger1 = Logger.logger(name: "feature1Logger")
let logger2 = Logger.logger(name: "feature2Logger")

// Log messages with different levels

logger1.debug(message: "debug message for feature 1")

// logger1.debug message isn't logged as logLevelFilter is set to info

logger2.info(message: "info message for feature 2")

// Send logs to the Mobile Analytics Service

Logger.send(completionHandler: { (response: Response?, error: Error?) in
        if let response = response {
        logger.debug(message: "Status code: \(response.statusCode)")
        logger.debug(message: "Response: \(response.responseText)")
    }
    if let error = error {
        logger.error(message: "Error: \(error)")
    }
})
```
{: codeblock}

Pour des raisons de confidentialité, vous pouvez désactiver la sortie du journal d'événements pour les applications qui sont générées en mode édition. Par défaut, la classe du journal d'événements imprime les journaux sur la console Xcode. Dans les paramètres de création de votre cible, ajoutez un indicateur `-D RELEASE_BUILD` à la section **Other Swift Flags** de la configuration de création d'édition.
{: tip}

## Etape 8. Consignation des données d'emplacement
{: #location-logging}

L'emplacement de l'appareil mobile doit être consigné depuis l'application à l'aide de l'API fournie :
```
Analytics.logLocation();
```

Cette API permet à l'application d'envoyer son emplacement (latitude et longitude, par exemple), au serveur entre deux sessions d'application. Pour les autres appels non explicites à l'API `location-logging`, le logiciel SDK envoie l'emplacement d'appareil pour chaque App-Session. L'emplacement est envoyé pour le contexte de session d'application initiale et le contexte de session d'application de changement d'utilisateur (qui est un changement d'utilisateur entre une session d'application ). L'API d'emplacement doit être activée dans l'initialisation du logiciel SDK.

Pour appeler l'API `location-logging`, définissez le paramètre `collectLocation` sur `true` dans l'initialisation du logiciel SDK :
```
Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
```

## Etape 9. Exécution d'une demande réseau
{: #network-requests}

Vous pouvez configurer le logiciel SDK client de {{site.data.keyword.mobileanalytics_short}} pour [effectuer une demande réseau](/docs/mobile/sdk_network_request.html). Vérifiez que vous avez bien initialisé et configuré `BMSClient` et `BMSAnalytics` et importé les logiciels SDK client.

## Etape 10. Génération de rapports relatifs à l'analyse des pannes
{: #report-crash-analytics}

Vous pouvez visualiser les [données de panne d'application ](app-monitoring.html#monitor-app-crash) en envoyant des informations d'analyse et de journal à {{site.data.keyword.mobileanalytics_short}}.

La méthode `Analytics.send()` remplit les tableaux **Vue d'ensemble des pannes** et **Pannes** de la page **Pannes**. Vous pouvez activer les graphiques à l'aide du processus d'initialisation et d'envoi pour analyse.

La méthode `Logger.send()` alimente les tables **Récapitulatif des pannes** et **Détails de la panne** sur la page **Traitement des incidents**. Vous devez activer votre application pour le stockage et l'envoi de journaux afin d'alimenter les graphiques par l'ajout d'une instruction dans votre code d'application :
```
Logger.isLogStorageEnabled = true
Logger.logLevelFilter = LogLevel.Fatal // or greater
```
{: codeblock}

Consultez l'[exemple d'utilisation du journal d'événements](sdk.html##sample-logger-usage) d'iOS.

## Etape 11. Suivi des utilisateurs actifs
{: #trackingusers}

Si votre application peut reconnaître des utilisateurs uniques sur un appareil, vous pouvez suivre le nombre d'utilisateurs qui utilisent de manière active l'application en transmettant le nom de l'utilisateur actif à {{site.data.keyword.mobileanalytics_short}}.

Activez le suivi des utilisateurs en initialisant {{site.data.keyword.mobileanalytics_short}} avec `hasUserContext=true`. Sinon, {{site.data.keyword.mobileanalytics_short}} ne capture qu'un seul utilisateur par appareil.

Ajoutez le code suivant pour savoir à quel moment l'utilisateur se connecte :
```
Analytics.userIdentity = "username"
```
{: codeblock}

## Etape 12. Test de votre application
{: #appid_testing}

Est-ce que tout est correctement configuré ? Il est temps de tester !

1. Ouvrez votre application. Si vous possédez une application Web, utilisez un navigateur. Si vous possédez une application client iOS, ouvrez-la avec l'émulateur Xcode.
2. Compilez et exécutez l'application sur votre émulateur ou appareil.
3. A partir de l'interface graphique utilisateur, suivez le processus de connexion à votre application.
4. Accédez à la console {{site.data.keyword.mobileanalytics_short}} pour voir les analyses d'utilisation de votre application. Vous pouvez également surveiller votre application en [définissant des alertes](/docs/services/mobileanalytics/app-monitoring.html#alerts) et en [surveillant les pannes d'application](/docs/services/mobileanalytics/app-monitoring.html#monitor-app-crash).

## Etapes suivantes
{: #what-to-do-next notoc}

 - Pour en savoir plus sur le service, lisez la [documentation](/docs/services/mobileanalytics/index.html#getting-started-tutorial).

 - Pour une présentation de l'utilisation des services mobiles et d'{{site.data.keyword.Bluemix_notm}}, voir [Mise en route avec les applications mobiles sur IBM Cloud](/docs/services/mobile/index.html).

 - Les kits de démarrage constituent l'un des moyens les plus rapides d'utiliser les fonctionnalités de {{site.data.keyword.cloud_notm}}. Vous pouvez voir tous les kits de démarrage disponibles dans le [tableau de bord Mobile Developer](https://console.bluemix.net/developer/mobile/dashboard). Téléchargez le code. Exécutez l'application.

 - Vous pouvez utiliser l'[Interface utilisateur swagger](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/) pour passer rapidement en revue la documentation d'API REST.
