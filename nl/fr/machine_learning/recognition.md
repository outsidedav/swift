---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: swift visual recognition, train swift, cocoapods swift, swift sdk install, starter kit watson swift, image swift classify, machine learning swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

Le service {{site.data.keyword.visualrecognitionfull}} permet à votre application d’étiqueter, de classifier et d'entraîner de manière rapide et précise du contenu visuel à l'aide de l'apprentissage automatique. Ce service peut vous aider à classifier virtuellement tout contenu visuel, à former votre propre modèle personnalisé en quelques minutes, et à détecter des visages.

## Fonctionnement
{: #how-it-works-recognition}

1. Votre application choisit une sélection d'images à analyser.
2. Votre application envoie l'image au service {{site.data.keyword.visualrecognitionshort}} à l'aide du logiciel SDK Watson Swift.
3. Le service analyse l'image en utilisant l'analyse de classification pour identifier les scènes, les objets, les visages, etc.
4. L'analyse du service est renvoyée à votre application par le logiciel SDK Watson Swift.

## Avant de commencer
{: #prereqs-recognition}

Assurez-vous que la configuration prérequise suivante est respectée :

* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage ou Swift Package Manager

Vous pouvez installer le [logiciel SDK Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") avec [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), ou [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"). Si vous utilisez CocoaPods (https://cocoapods.org/) pour gérer les dépendances, vous obtenez uniquement les infrastructures dont vous avez besoin, par opposition au SDK Watson Swift complet. Si vous ne connaissez pas encore CocoaPods, vous pouvez l'installer facilement :
```console
sudo gem install cocoapods
```
{: codeblock}

## Etape 1. Création d'une instance de Visual Recognition
{: #create-instance-recognition}

Mettez à disposition une instance du service {{site.data.keyword.visualrecognitionshort}} :

1. Dans le catalogue {{site.data.keyword.cloud_notm}}, sélectionnez **{{site.data.keyword.visualrecognitionshort}}**. L'écran de configuration du service s'ouvre.
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Sélectionnez une application dans le menu **Se connecter** si vous voulez lier votre instance à une application.
4. Sélectionnez un plan de tarification, puis cliquez sur **Créer**.
5. Sélectionnez l'onglet **Données d'identification** pour afficher les données d'identification du service. Ces valeurs sont utilisées pour la connexion au service à partir de votre application.

## Etape 2. Téléchargement et génération des dépendances
{: #download-depend-recognition}

A l'aide de l'éditeur de texte de votre choix, créez un nouveau fichier `Podfile` dans le répertoire racine de votre projet (où se trouve votre fichier `.xcodeproj`) en exécutant `pod init`. Ajoutez ensuite une ligne pour spécifier l'infrastructure {{site.data.keyword.visualrecognitionshort}} du SDK Watson Swift en tant que dépendance :

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

Pour une application de production, vous pouvez aussi indiquer qu'une [version particulière est requise](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") pour éviter les modifications inattendues des nouvelles versions du logiciel SDK Watson Swift.

Maintenant que votre fichier `Podfile` est en place, téléchargez les dépendances. Utilisez un terminal pour accéder au répertoire racine de votre projet, puis lancez CocoaPods :

```console
pod install
```
{: codeblock}

Cocoapods télécharge l'infrastructure {{site.data.keyword.visualrecognitionshort}} et la génère dans le dossier `Pods/` de votre projet.

Pour éviter les problèmes de génération de Pod, ouvrez le fichier qui se termine par `.xcworkspace` au lieu de `.xcodeproj` lorsque vous ouvrez le projet dans Xcode.
{: tip}

## Etape 3. Analyse des images dans votre application
{: #analyze-images-recognition}

Les exemples suivants vous aident à ajouter des fonctions {{site.data.keyword.visualrecognitionshort}} à votre application, généralement dans `ViewController.swift`. A l'aide de ces exemples, vous pouvez étendre les appels de Visual Recognition pour votre scénario d'utilisation.

1. Ajoutez une instruction d'importation pour Visual Recognition:
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. Transmettez la clé et la version d'API (vous pouvez utiliser la date d'aujourd'hui) afin d'initialiser le logiciel SDK :
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  Consultez la [documentation sur les paramètres de version](https://{DomainName}/apidocs/visual-recognition#versioning){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") ou utilisez la date à laquelle le service {site.data.keyword.conversationshort}} a été créé.
  {: tip}

3. Ajoutez le code suivant pour classifier une image:
  ```swift
  let url = "your-image-url"
  visualRecognition.classify(url: url) { response, error in
      if let error = error {
          print(error)
    }
      guard let classifiedImages = response?.result else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

Plusieurs méthodes de classification sont prises en charge par l'infrastructure Visual Recognition. Explorez la [documentation de Visual Recognition](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") du SDK Watson pour trouver celle qui correspond le mieux à votre application.
{: tip}

## Utilisation des kits de démarrage
{: #recognition_starterkits}

Les [kits de démarrage](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") constituent l'un des moyens les plus rapides d'utiliser les fonctionnalités d'{{site.data.keyword.cloud_notm}}. Vous pouvez utiliser le service {{site.data.keyword.visualrecognitionshort}} en sélectionnant le kit de démarrage **Visual Recognition for iOS with Watson**. Ce service évalue et classe vos images. Téléchargez des images nouvelles ou existantes à partir de votre appareil mobile. L'application Visual Recognition étiquettera et classera rapidement le contenu de l'image.

Pour démarrer :
1. Sélectionnez le kit de démarrage disponible [ici](https://{DomainName}/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").
2. Créez le projet avec les services par défaut.
3. Téléchargez le projet en cliquant sur **Télécharger le code**. Les données d'identification du service sont injectées dans le fichier `BMSCredentials.plist` des zones clé correspondantes.

## Etapes suivantes
{: #recognition_next notoc}

Félicitations ! Visual Recognition est désormais disponible dans votre application. Continuez sur votre lancée en essayant l'une des options suivantes :
* Consultez le [SDK Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") et explorez les autres services Watson pris en charge.
* Pour plus d'informations, voir [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").
