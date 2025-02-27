---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-07"

keywords: getting started swift, custom app, create app swift, stater kit swift, apple app swift, swift dependency, ios development

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Tutoriel de mise en route
{: #getting-started}

{{site.data.keyword.cloud}} propose des solutions et des services qui permettent aux développeurs Swift de créer des applications offrant la sécurité, l'intelligence artificielle et la valeur que vos clients exigent. Grâce à une gamme étendue d'offres et de logiciels SDK, vous pouvez utiliser ces services pour mettre rapidement sur le marché des applications de pointe. Ce guide de programmation Swift vous explique comment ajouter des services à une application Swift nouvelle ou existante, qu'il s'agisse d'une application Swift côté serveur ou d'une application client iOS.
{: shortdesc}

Le tutoriel ci-dessous montre comment créer rapidement une application mobile Swift avec {{site.data.keyword.mobileanalytics_full}} à l'aide d'un kit de démarrage vide depuis la [console de développement {{site.data.keyword.cloud_notm}} pour Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"). Depuis la console, vous pouvez ajouter le service {{site.data.keyword.mobileanalytics_short}}, télécharger le code, exécuter l'application iOS en local dans Xcode, configurer et surveiller l'application.

## Etape 1. Exigences pour les développeurs
{: #dev-requirements-swift}

Pour commencer le développement iOS sur {{site.data.keyword.cloud_notm}}, assurez-vous que les exigences suivantes sont respectées :

### Système d'exploitation
{: #swift-os-requirements}

Pour développer des applications Swift, il est recommandé d'utiliser le dernier matériel MacOS pris en charge, et d'effectuer les tests avec les versions iOS les plus récentes. Inscrivez-vous au compte [Apple Developer](https://developer.apple.com/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") pour pouvoir effectuer des tests sur une unité physique.

### iOS et Xcode
{: #ios_and_xcode}

- Installez [Xcode 8+](https://developer.apple.com/xcode/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") (ou version ultérieure).
- Déployez sur des [appareils iOS 8](https://support.apple.com/downloads/ios){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") (ou version ultérieure).
- Avant de soumettre des applications à Apple, consultez les [règles de soumission d'App Store](https://developer.apple.com/app-store/resources/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").

### Logiciels SDK et gestion des dépendances
{: #swift-sdk-management}

Les outils suivants garantissent que vous pouvez installer les logiciels SDK natifs pour qu'ils fonctionnent avec les différents services {{site.data.keyword.cloud_notm}}. Vous pouvez utiliser CocoaPods ou Carthage pour la gestion des dépendances.

* **Avec CocoaPods** - Installez [CocoaPods](https://cocoapods.org/) pour une prise en charge des logiciels SDK {{site.data.keyword.cloud_notm}} :
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}

* **Avec Carthage** - Certains logiciels SDK sont également disponibles via les gestionnaires de dépendances [Carthage](https://github.com/Carthage/Carthage){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") ou [Swift Package Manager](https://swift.org/package-manager/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"). Pour utiliser Carthage pour la gestion des dépendances, procédez comme suit :

  Installez [Homebrew](https://brew.sh/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") pour vous aider à installer Carthage :
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

  Installez Carthage :
  ```
  brew install carthage
  ```
  {: codeblock}

## Etape 2. Création d'une application Swift iOS personnalisée
{: #create-ios-app-swift}

1. Connectez-vous à [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
2. Cliquez sur **Créer une application**.
3. Sur la page [Empty Starter](https://{DomainName}/developer/appledevelopment/create-app){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"), vous pouvez utiliser la configuration par défaut, ou mettre à jour les zones si nécessaires. Assurez-vous que le langage **iOS Swift** est sélectionné. Cliquez sur **Créer**.

## Etape 3. Ajout du service {{site.data.keyword.cloudant_short_notm}}
{: #resources-swift}

Vous pouvez maintenant ajouter des services à votre application Swift. Pour ce tutoriel, ajoutez le service {{site.data.keyword.cloudant_short_notm}} à votre application Swift, qui crée une base de données de documents `JSON` entièrement gérée et distribuée. Cloudant optimise votre application grâce à son évolutivité, sa haute disponibilité et sa durabilité avec une infrastructure légère qui assure la sécurité et la synchronisation de vos données.

1. Depuis la page **Détails de l'application**, cliquez sur **Ajout d'un service**.
2. Sélectionnez **Données**, et cliquez sur **Suivant**.
3. Sélectionnez **Cloudant**, et cliquez sur **Suivant**.
4. Cliquez sur **Créer**.
5. Une fois que le service est créé, cliquez dessus pour le démarrer. Sur cette nouvelle page, sélectionnez **Lancer le tableau de bord Cloudant** pour commencer à créer une base de données et des documents JSON.  Vous pouvez également le faire à l'aide d'un programme.

## Etape 4. Téléchargement du code et configuration des logiciels SDK clients
{: #run-locally-swift}

Pour télécharger le code, cliquez sur **Télécharger le code** sous `Applications` > `Vos applications`. Le code téléchargé est inclus avec le [SDK SwiftCloudant](https://github.com/cloudant/swift-cloudant){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"), de même qu'un code d'initialisation de base. Les logiciels SDK clients sont disponibles sur CocoaPods et Swift Package Manager. Cette solution utilise CocoaPods.

1. Extrayez le code téléchargé, puis, à l'aide d'un terminal, accédez au dossier extrait.
  ```
  cd <Name of Project>
  ```

2. Le dossier contient un fichier pod avec les dépendances requises. Exécutez la commande suivante pour installer les dépendances (logiciels SDK clients) :
  ```
  pod install
  ```
  {: codeblock}

## Etape 5. Configuration de l'application pour utiliser votre nouvelle base de données
{: #config-db-swift}

1. Ouvrez le nom de fichier se terminant par `.xcworkspace` avec Xcode et naviguez vers `ViewController.swift`. `SwiftCloudant` est le logiciel SDK Cloudant de base. `CouchDBClient` est une classe de `SwiftCloudant` initialisée dans `ViewController.swift`.

  Ouvrez toujours le nouvel espace de travail Xcode au lieu du fichier de projet Xcode d'origine : `MyApp.xcworkspace`.
  {: tip}

2. Le code d'initialisation de base de données est déjà inclus, comme le montre le fragment suivant :
  ```swift
  /* Read the Cloudant credentials and intialize database connection */
  if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"), let dictionary = NSDictionary(contentsOfFile: contents) {
            let url = URL(string: dictionary["cloudantUrl"] as! String)
            let client = CouchDBClient(url:url!,
                                       username:dictionary["cloudantUsername"] as? String,
                                       password:dictionary["cloudantPassword"] as? String)
        }
  ```
  {: codeblock}

  Les données d'identification du service font partie du fichier `BMSCredentials.plist`.
  {: note}

## Etape 6. Réalisation de vos opérations de base de données
{: #build_ops-swift}

Maintenant que votre connexion à la base de données fonctionne et que le logiciel SDK est configuré, vous pouvez commencer à générer les [opérations de création, de lecture, de mise à jour et de destruction](/docs/swift/data?topic=swift-cloudant) de base pour votre scénario d'utilisation particulier.

## Etapes suivantes
{: #next-swift}

### Ajout de services supplémentaires
{: #moreresources-swift}

Vous pouvez ajouter d'autres services à votre application iOS directement depuis la console Web, tels que les services communs suivants :

* [Ajout du service Push Notifications](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate)
* [Ajout de l'authentification d'utilisateur avec un ID d'application](/docs/services/appid?topic=appid-getting-started)

### Utilisation des outils de développement {{site.data.keyword.cloud_notm}}
{: #devtools-swift}

Vous pouvez également apprendre à développer des applications Swift à l'aide des [outils de développement {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started), qui offrent une approche basée sur les lignes de commande pour créer, développer et déployer des applications Web, mobiles et de micro-services complètes.
