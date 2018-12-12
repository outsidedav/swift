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

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

Le service {{site.data.keyword.visualrecognitionfull}} permet à votre application d’étiqueter, de classifiez et d'entraîner de manière rapide et précise du contenu visuel à l'aide de l'apprentissage automatique. Ce service peut vous aider à classifier virtuellement tout contenu visuel, former votre propre modèle personnalisé en quelques minutes, et à détecter des visages.

## Fonctionnement
{: ##how-it-works}

1. Votre application choisit une sélection d'images à analyser.
2. Votre application envoie l'image au service {{site.data.keyword.visualrecognitionshort}} à l'aide du logiciel SDK Swift Watson.
3. Le service analyse l'image en utilisant l'analyse de classification pour identifier les scènes, les objets, les visages, etc.
4. L'analyse du service est renvoyée à votre application par le logiciel SDK Swift Watson.

## Avant de commencer
{: ###before-you-begin}

Tout d'abord, assurez-vous que vous respectez la configuration prérequise suivante :
<ul>
  <li>iOS 8.0+</li>
  <li>Xcode 9.0+</li>
  <li>Swift 3.2+ ou Swift 4.0+</li>
  <li>Carthage</li>
</ul>

Il est recommandé d'utiliser [Carthage](https://github.com/Carthage/Carthage) pour gérer les dépendances et de générer le logiciel SDK Swift Watson pour votre application. Si vous débutez avec Carthage, vous pouvez l'installer avec [Homebrew](http://brew.sh/) :

```bash
$ brew update
$ brew install carthage
```

## Etape 1. Création d'une instance de Visual Recognition
{: ###create-and-configure-an-instance-of-visual-recognition}

Mettez à disposition une instance du service {{site.data.keyword.visualrecognitionshort}} :

1. Dans le catalogue {{site.data.keyword.cloud_notm}}, sélectionnez **{{site.data.keyword.visualrecognitionshort}}**. L'écran de configuration du service s'ouvre.
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Sélectionnez une application dans la menu **Se connecter** si vous voulez lier votre instance à une application.
4. Sélectionnez un plan de tarification, puis cliquez sur **Créer**.
5. Sélectionnez l'onglet **Données d'identification** pour afficher les données d'identification du service. Ces valeurs sont utilisées pour la connexion au service à partir de votre application.

## Etape 2. Téléchargement et génération de dépendances
{: ###download-and-build-dependencies}

A l'aide de votre éditeur de texte préféré, créez un fichier nommé `Cartfile` dans le répertoire racine de votre projet (où se trouve votre fichier `.xcodeproj`). Ensuite, ajoutez une ligne pour indiquer le logiciel SDK Swift Watson en tant que dépendance :
```
github "watson-developer-cloud/swift-sdk"
```
{: pre}

Pour une application de production, vous pouvez aussi indiquer si vous le souhaitez, qu'une [version particulière est requise](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) pour éviter les modifications inattendues des nouvelles versions du logiciel SDK Swift Watson.

Une fois `Cartfile` installé, vous pouvez maintenant télécharger et générer les dépendances. Utilisez un terminal pour accéder au répertoire racine de votre projet, puis lancez Carthage :

```bash
carthage update --platform iOS
```
{: pre}

Carthage télécharge le logiciel SDK Swift Watson, puis il génère ses infrastructures dans le dossier `Carthage/Build/iOS` de votre projet.

## Etape 3. Ajout d'infrastructures à votre application
{: ###add-frameworks-to-your-app}

### Etapes de liaison de Visual Recognition :

A présent que les infrastructures du logiciel SDK Swift Watson sont générés par Carthage, vous devez lier l'infrastructure Visual Recognition à votre application.

1. Ouvrez votre application dans Xcode et sélectionnez votre projet dans le navigateur afin d'afficher ses paramètres.
2. Sélectionnez la cible de votre application et ouvrez l'onglet **Général**.
3. Faites défiler jusqu'à la section "Linked Frameworks and Libraries" et cliquez sur l'icône `+`.
4. Dans la nouvelle fenêtre qui s'affiche, cliquez sur **Add Other** et accédez au répertoire `Carthage/Build/iOS`. Sélectionnez **VisualRecognitionV3.framework** afin de le lier à votre application.

### Etapes de copie de Visual Recognition :

En plus de _lier_ l'infrastructure Visual Recognition, vous devez aussi la _copier_ dans l'application afin de la rendre accessible lors de l'exécution. Xcode propose différentes méthodes pour la copie ou l'imbrication d'une infrastructure, mais vous pouvez utiliser un script Carthage afin d'éviter un [bogue de soumission App Store](http://www.openradar.me/radar?id=6409498411401216) particulier.

1. Une fois les paramètres de la cible de votre application affichés dans Xcode, accédez à l'onglet **Build Phases**.
2. Cliquez sur l'icône `+` et sélectionnez **New Run Script Phase**.
3. Ajoutez la commande suivante pour exécuter le script phase : `/usr/local/bin/carthage copy-frameworks`.
4. Ajoutez l'infrastructure Visual Recognition à la liste **Fichiers d'entrée** : `$(SRCROOT)/Carthage/Build/iOS/VisualRecognitionV3.framework`.

Vous êtes maintenant prêt à commencer à utiliser le logiciel SDK Swift Watson dans votre application !

## Etape 4. Analyse des images dans votre application
{: #analyze-images-in-your-app}

1. Ouvrez le fichier `ViewController.swift` dans Xcode.

1. Ajoutez une instruction d'importation pour Visual Recognition:
    ```swift
    import VisualRecognitionV3
    ```
    {: pre}

1. Transmettez la clé et la version d'API (vous pouvez utiliser la date d'aujourd'hui) afin d'initialiser le logiciel SDK :
    ```swift
    let visualRecognition = VisualRecognition(apiKey: "your-api-key", version: "yyyy-mm-dd")
    ```
    {: pre}

1. Ajoutez le code suivant pour classifier une image:
    ```swift
    let url = "your-image-url"
    let failure = { (error: Error) in print(error) }
    visualRecognition.classify(url: url, failure: failure) { classifiedImages in
        print(classifiedImages)
    }
    ```
    {: pre}

## Utilisation des kits de démarrage
{: #recognition_starterkits}

Les [kits de démarrage](https://console.bluemix.net/developer/appledevelopment/starter-kits) sont l'un des moyens les plus rapides d'optimiser les fonctionnalités d'{{site.data.keyword.cloud_notm}}. Vous pouvez utiliser le service {{site.data.keyword.visualrecognitionshort}} en sélectionnant le kit de démarrage **Visual Recognition for iOS with Watson**. Ce service évalue et classe vos images. Téléchargez des images nouvelles ou existantes à partir de votre appareil mobile, et l'application Visual Recognition étiquette et classe rapidement le contenu d'image.

Pour démarrer :
1. Sélectionnez le kit de démarrage qui se trouve [ici](https://console.bluemix.net/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson).
2. Créez le projet avec les services par défaut.
3. Téléchargez le projet en cliquant sur **Télécharger le code**. Les données d'identification du service sont injectées dans le fichier `BMSCredentials.plist` des zones clé correspondantes.

## Etapes suivantes
{: #recognition_next}

Félicitations ! Visual Recognition est désormais disponible dans votre application. Poursuivez sur votre lancée en essayant l'une des options suivantes :
* Consultez le [logiciel SDK Swift Watson sur GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* Pour plus d'informations, voir [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/).

