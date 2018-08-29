---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Ajout de l'apprentissage automatique à une appli
{: #overview}

Avec [Core ML](https://developer.apple.com/documentation/coreml){:new_window}, vous pouvez intégrer une large variété de types de modèle d'apprentissage automatique dans votre appli. Outre la prise en charge de l'apprentissage en profondeur étendu avec plus de 30 types de couches, cette fonction prend également en charge des modèles standard comme les ensembles d'arborescence, les SVM et les modèles linéaires généralisés. Au lieu d'envoyer des données à distance pour analyse, Core ML utilise des technologies de bas niveau, comme Metal and Accelerate, pour de façon à tirer parti de l'unité centrale et du Processeur graphique (GPU) pour optimiser les performances et l'efficacité.

## Avant de commencer
{: #before-you-begin}

Pour utiliser Core ML et Watson Visualization, vous avez besoin des composants suivants :

  * iOS 11.0+
  * Xcode 9.0+
  * Swift 4.0+
  * Carthage

Pour installer Carthage, utilisez les commandes `brew` suivantes :
```
$ brew update
$ brew install carthage
```
{: codeblock}

## Etape 1. Entraînement d'un modèle avec {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}}
{: #training-your-model}

S'il n'existe aucun modèle actuel, le premier modèle trouvé à distance ou l'un de ceux existant en local est utilisé. Le gif suivant et les instructions qui l'accompagnent vous expliquent comment lier votre service à Watson Studio, et comment entraîner votre modèle.

![Déroulement du modèle Core ML](images/CoreMLWalkthrough.gif)

### Configuration du service à partir du tableau de votre appli Core ML

1. Lancez l'outil de reconnaissance visuelle (Visual Recognition Tool) depuis le tableau de bord de votre kit de démarrage en sélectionnant **Lancer l'outil**.
2. Commencez à créer votre modèle en sélectionnant **Créer un modèle**.
2. Si un projet n'est pas encore associée à l'instance de reconnaissance visuelle que vous avez créée, un projet est créé. Sinon, passez à la section **Créer un modèle**.
3. Nommez votre projet, puis cliquez sur **Créer**.
  **Conseil **: Si aucun stockage n'est défini, cliquez sur Actualiser.

### Liaison d'un service à un projet

1. Une fois que vous avez créé votre projet, le tableau de bord de projet s'affiche.
2. Accédez à l'onglet des paramètres, faites défiler jusqu'à **Services associés**, sélectionnez **Ajouter service** -> **Watson**.
3. Depuis la page des services de Watson, sélectionnez **Visual Recognition**.
4. Sélectionnez l'onglet **Existant** sur la page d'infos de service, puis sélectionnez votre instance de service depuis le tableau de bord.
5. A présent que votre service est lié, vous pouvez commencer à créer votre modèle en sélectionnant **Ressources** depuis votre tableau de bord de projet, puis en cliquant sur **Add Visual Recognition Model**.

### Création d'un modèle

1. Dans l'outil de création de modèle, modifiez le nom du discriminant si vous le souhaitez. Par défaut, l'application utilise le premier disponible sauf indication contraire. Si vous souhaitez utiliser un modèle spécifique, veillez à modifier la zone `defaultClassifierID` dans la vue de contrôleur principale. 

2. Depuis la barre latérale, importez les cours d'entraînement du modèle dans des fichiers .zip. Ensuite, sélectionnez chaque jeu de données et ajoutez-les à votre modèle à partir du menu déroulant. N'hésitez pas à ajouter plusieurs classes qui utilisent vos propres jeux d'images pour améliorer le discriminant!

![Ajout de classes](images/add_classes.png)

3. Sélectionnez **Train Model**, puis patientez jusqu'à ce que le modèle soit entièrement entraîné. 

Vous voilà prêt(e). Vous pouvez maintenant télécharger votre modèle Core ML et l'intégrer dans votre appli à l'aide du [logiciel SDK Swift cloud de développement {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.

## Etape 2. Téléchargement et génération de dépendances
{: #installing-dependencies}

1. A l'aide de votre éditeur de texte préféré, créez un fichier nommé **Cartfile** dans le répertoire racine de votre projet (où se trouve votre fichier `.xcodeproj`).

2. Ajoutez une ligne pour indiquer le logiciel SDK Swift cloud de développement {{site.data.keyword.watson}} comme dépendance, par exemple :

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: pre}

  Pour une appli de production, vous pouvez aussi indiquer si vous le souhaitez, qu'une version particulière est requise pour éviter les modifications inattendues des nouvelles versions du logiciel SDK.
  {: tip}

3. Utilisez un terminal pour accéder au répertoire racine de votre projet, puis lancez Carthage :

  ```
  $ carthage update --platform iOS
  ```
  {: pre}

  Le logiciel SDK Swift cloud de développement {{site.data.keyword.watson}} est ensuite téléchargé, et son infrastructure est générée dans le dossier `Carthage/Build/iOS` de votre projet.

## Etape 3. Ajout d'une classification d'image à votre appli
{: #adding-image-classification}

```Swift
//
//  ViewController.swift
//  CoreMLImageClassificationExample
//
//  Created by Aaron Liberatore on 3/1/18.
//  Copyright © 2018 Aaron Liberatore. All rights reserved.
//

import UIKit
import VisualRecognitionV3

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        initializeCoreMLExample()
    }

    func failureHandler(_ error: Error) {
        print(error)
    }

    func initializeCoreMLExample() {
        // Configuration

        // Your Watson Visual Recognition service api key
        let apiKey = "your-apiKey-here"

        // Name of the classifier to use
        let classifierID = "your-classifier-here"

        // Minimum confidence threshold for image recognition
        let threshold = 0.5

        // Initialize Service
        let visualRecognition = VisualRecognition(apiKey: apiKey, version: "03-01-2018")

        // Update or download your model
        visualRecognition.updateLocalModel(classifierID: classifierID,
                                           failure: failureHandler) {

            // Classify your image using your model                                         
            visualRecognition.classifyWithLocalModel(image: image,
                                                     classifierIDs: [classifierID],
                                                     threshold: threshold,
                                                     failure: self.failureHandler) { classifiedImages in

                 print(classifiedImages)
             }            
        }
    }
}
```
{: codeblock}

## Etape 4. Utilisation de kits de démarrage 
{: #coreml_starterkits}

Grâce aux kits de démarrage, vous pouvez rapidement et facilement tirer profit des fonctionnalités de {{site.data.keyword.cloud_notm}} et de Core ML. Vous pouvez ajouter {{site.data.keyword.visualrecognitionshort}} à une appli iOS client ou un système de back end côté serveur avec les kits de démarrage. Le modèle Custom Vision Model pour Core ML fourni avec le kit de démarrage {{site.data.keyword.watson}} illustre comment créer un modèle {{site.data.keyword.visualrecognitionshort}} personnalisé, et comment l'instancier en tant que modèle Core ML que vous pouvez exécuter en local sur votre appareil.

Pour ajouter {{site.data.keyword.visualrecognitionshort}} à un kit de démarrage, procédez comme suit :

1. Sélectionnez le [kit de démarrage](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} avec lequel vous voulez démarrer.
2. Créez le projet avec les services par défaut.
3. Cliquez sur **Ajouter des ressources > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Téléchargez le projet en cliquant sur **Télécharger le code**. Pour les projets iOS, les données d'identification sont insérées dans le fichier `BMSCredentials.plist` des zones clés correspondantes. Pour les projets Swift côté serveur, vous trouverez ces données d'identification dans le fichier `config/local-dev.json`.

## Etapes suivantes
{: #coreml_next}

Vous pouvez maintenant analyser les images personnalisées générées à l'aide des modèles Core ML générées personnalisés. Poursuivez sur votre lancée en essayant l'une des options suivantes :

* Ajoutez un logique de cloud. Commencez par le [développement d'applis sans serveur](/docs/swift/backend/functions.html).
* Tirez parti de toutes les fonctionnalités offertes par {{site.data.keyword.visualrecognitionshort}}. Consultez la [documentation](/docs/services/visual-recognition/index.html) pour plus de détails.
