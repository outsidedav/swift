---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: coreml swift, core ml swift, watson swift core, create model swift, image classification swift, version parameter swift, swift coreml watson

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation de Core ML avec Watson
{: #swift-coreml}

Avec [Core ML](https://developer.apple.com/documentation/coreml){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), vous pouvez intégrer de nombreux types de modèle d'apprentissage automatique à votre application. Outre la prise en charge de l'apprentissage en profondeur étendu avec plus de 30 types de couches, cette fonction prend également en charge des modèles standard comme les ensembles d'arborescence, les SVM et les modèles linéaires généralisés. Au lieu d'envoyer les données à distance pour les analyser, Core ML utilise des technologies de bas niveau, telles que Metal et Accelerate, pour tirer parti de l'unité centrale et du GPU de manière transparente et fournir des performances et une efficacité maximales.

Les étapes décrites ci-dessous vous indiquent comment ajouter Core ML et Watson Visual Recognition à une application Swift pour entraîner et créer un modèle, télécharger et créer des dépendances, et ajouter une classification d'image.
{: shortdesc}

## Avant de commencer
{: #prereqs-coreml}

Pour utiliser Core ML et Watson Visual Recognition avec Swift, vous avez besoin des composants suivants :

* iOS 11.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage ou Swift Package Manager

Vous pouvez installer le [logiciel SDK Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") avec [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), ou [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"). Si vous utilisez CocoaPods pour gérer les dépendances, vous obtenez uniquement les infrastructures dont vous avez besoin, par opposition au SDK Watson Swift complet. Si vous ne connaissez pas encore CocoaPods, vous pouvez l'installer facilement :

```console
sudo gem install cocoapods
```
{: codeblock}

## Etape 1. Entraînement d'un modèle avec {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}}
{: #train-module-coreml}

S'il n'existe aucun modèle en cours, le premier modèle détecté à distance ou tout modèle existant localement est utilisé. Le gif suivant et les instructions qui l'accompagnent vous expliquent comment lier votre service à Watson Studio, et comment entraîner votre modèle.

![Présentation du modèle Core ML](images/CoreMLWalkthrough.gif)

### Configuration du service à partir du tableau de votre application Core ML
{: #service-coreml}

1. Démarrez l'outil Visual Recognition depuis le tableau de bord de votre kit de démarrage en sélectionnant **Lancer l'outil**.
2. Commencez à créer votre modèle en sélectionnant **Créer un modèle**.
2. Si aucun projet n'est encore associé à l'instance de reconnaissance visuelle que vous avez créée, un projet est créé. Sinon, passez à la section **Créer un modèle**.
3. Nommez votre projet, puis cliquez sur **Créer**.
  
  Si aucun stockage n'est défini, cliquez sur Actualiser.
  {: tip}

### Liaison d'un service à un projet
{: #bind-service-coreml}

1. Une fois que vous avez créé votre projet, le tableau de bord du projet s'affiche.
2. Accédez à l'onglet des paramètres, faites défiler jusqu'à **Services associés**, sélectionnez **Ajouter service** -> **Watson**.
3. Depuis la page des services de Watson, sélectionnez **Visual Recognition**.
4. Sélectionnez l'onglet **Existant** sur la page d'information du service, puis sélectionnez votre instance de service sur le tableau de bord.
5. A présent que votre service est lié, vous pouvez commencer à créer votre modèle en sélectionnant **Ressources** depuis le tableau de bord de votre projet, puis en cliquant sur **Add Visual Recognition Model**.

### Création d'un modèle
{: #create-model-coreml}

1. Dans l'outil de création de modèle, vous pouvez mettre à jour le nom du classifieur. Si vous souhaitez utiliser un modèle spécifique, veillez à modifier la zone `defaultClassifierID` dans la vue principale du contrôleur.

2. Depuis la barre latérale, envoyez par téléchargement les cours de formation au format `.zip`. Sélectionnez ensuite chaque jeu de données et ajoutez-les à votre modèle à l'aide du menu déroulant. N'hésitez pas à ajouter plusieurs classes qui utilisent vos propres jeux d'images pour améliorer le classifieur.

![Ajout de classes](images/add_classes.png)

3. Sélectionnez **Train Model**, puis patientez jusqu'à ce que le modèle soit entièrement entraîné.

Tout est prêt. Vous pouvez maintenant télécharger votre modèle Core ML et l'intégrer dans votre application à l'aide du [logiciel SDK Watson Developer Cloud Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").

## Etape 2. Téléchargement et génération des dépendances
{: #install-depend-coreml}

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

## Etape 3. Ajout d'une classification d'image à votre application
{: #add-image-coreml}

Les exemples suivants vous aident à ajouter les fonctions Core ML de {{site.data.keyword.visualrecognitionshort}} à votre application, généralement dans `ViewController.swift`. A l'aide de ces exemples, vous pouvez étendre les appels de modèles locaux à votre scénario d'utilisation.

1. Ajoutez une instruction d'importation pour Visual Recognition:
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. Transmettez la clé d'API et la version pour initialiser le logiciel SDK :
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  Consultez la [documentation sur les paramètres de version](https://cloud.ibm.com/apidocs/visual-recognition#versioning){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") ou utilisez la date à laquelle le service {site.data.keyword.visualrecognitionshort}} a été créé.
  {: tip}

3. Ajoutez le code suivant pour télécharger et mettre à jour le modèle Core ML local avec votre classifieur Watson :
  ```swift
  // Name of the classifier to use
  let classifierID = "your-classifier-ID-here"

  // Minimum confidence threshold for image recognition
        let threshold = 0.5

  // Update or download your model
  visualRecognition.updateLocalModel(classifierID: classifierID) { _, error in
      if let error = error {
          print(error)
      } else {
          print("model successfully updated")
      }                
  }
  ```
  {: codeblock}

4. Ajoutez le code suivant pour classifier une image à l'aide de votre modèle Core ML local :
  ```swift
  let image = UIImage(named: "your-image-filename")
  let classifierID = "your-classifier-ID-here"
  let threshold = 0.5

  // Classify your image using your model                                         
  visualRecognition.classifyWithLocalModel(image: image, classifierIDs: [classifierID], threshold: threshold) { classifiedImages, error in
      if let error = error {
          print(error)
    }
      guard let classifiedImages = classifiedImages? else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

5. Explorez les autres [fonctionnalités de classification de Core ML](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") prises en charge par le logiciel SDK Watson.

## Etape 4. Utilisation des kits de démarrage
{: #coreml_starterkits}

Les kits de démarrage constituent un moyen facile et rapide d'utiliser les fonctionnalités d'{{site.data.keyword.cloud_notm}} et de Core ML. Grâce à eux, vous pouvez ajouter {{site.data.keyword.visualrecognitionshort}} à n'importe quelle application client iOS ou n'importe quel système de back-end côté serveur. Le modèle Custom Vision Model pour Core ML fourni avec le kit de démarrage {{site.data.keyword.watson}} illustre comment créer un modèle {{site.data.keyword.visualrecognitionshort}} personnalisé, et comment l'instancier en tant que modèle Core ML que vous pouvez exécuter en local sur votre appareil.

Pour ajouter {{site.data.keyword.visualrecognitionshort}} à un kit de démarrage, procédez comme suit :

1. Sélectionnez le [kit de démarrage](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") avec lequel vous voulez démarrer.
2. Créez l'application avec les services par défaut.
3. Cliquez sur **Ajout d'un service > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Téléchargez le projet en cliquant sur **Télécharger le code**. Pour les projets iOS, les données d'identification sont insérées dans le fichier `BMSCredentials.plist` des zones clés correspondantes. Pour les projets Swift côté serveur, vous trouverez ces données d'identification dans le fichier `config/local-dev.json`.

## Etapes suivantes
{: #coreml_next}

Vous pouvez maintenant analyser des images avec vos propres modèles Core ML personnalisés. Continuez sur votre lancée en essayant l'une des options suivantes :

* Consultez le [logiciel SDK {{site.data.keyword.watson}} Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") et explorez les autres services Watson pris en charge.
* Ajoutez un logique de cloud. Commencez par le [développement d'applications sans serveur](/docs/swift/backend?topic=swift-serverless-dev-swift#serverless-dev-swift).
* Tirez parti de toutes les fonctions offertes par {{site.data.keyword.visualrecognitionshort}}. Consultez la [documentation](/docs/services/visual-recognition?topic=visual-recognition-index#index) pour plus de détails.
