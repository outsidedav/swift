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

# Ajout d'un agent conversationnel
{: #assistant}

Vous pouvez utiliser le service {{site.data.keyword.conversationshort}} pour générer des applications qui comprennent la saisie en langage naturel et répondent aux utilisateurs avec des conversations de type humain.

La liste suivante présente le flux des travaux d'intégration :

  1. Les utilisateurs interagissent avec l'interface implémentée dans votre application.
  2. Votre application envoie une entrée utilisateur à {{site.data.keyword.conversationshort}} à l'aide du logiciel SDK Swift {{site.data.keyword.watson}}.
  3. Le logiciel SDK Swift {{site.data.keyword.watson}} se connecte à un espace de travail, qui est un conteneur pour votre flux de dialogues et vos données d'apprentissage.
  4. L'espace de travail interprète l'entrée utilisateur et dirige le flux des conversation, en envoyant une réponse à votre application.
  5. Votre application affiche la réponse pour l'utilisateur.

## Avant de commencer
{: #before-you-begin}

Assurez-vous que vous respectez la configuration prérequise suivante :

  * Une instance du [service {{site.data.keyword.conversationshort}}](/docs/services/conversation/getting-started.html)
  * iOS 8.0+
  * Xcode 9.0+
  * Swift 3.2+ ou Swift 4.0+
  * Carthage

Utilisez [Carthage](https://github.com/Carthage/Carthage){:new_window} pour gérer les dépendances, et gérez le logiciel SDK Swift {{site.data.keyword.watson}} pour votre application. Si vous débutez avec Carthage, vous pouvez l'installer avec [Homebrew](http://brew.sh/){:new_window} :

```bash
$ brew update
$ brew install carthage
```

## Téléchargement et génération des dépendances
{: #download-and-build-dependencies}

1. A l'aide de votre éditeur de texte préféré, créez un fichier nommé `Cartfile` dans le répertoire racine de votre projet (où se trouve votre fichier `.xcodeproj`).

2. Ajoutez une ligne pour indiquer le logiciel SDK Swift Watson comme dépendance :
  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

  Pour une application de production, vous pouvez indiquer une [version requise](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement){:new_window} afin d'éviter des modifications inattendues des nouvelles versions du logiciel SDK Swift {{site.data.keyword.watson}}.
  {: tip}

3. Utilisez un terminal pour accéder au répertoire racine de votre projet, puis lancez Carthage :
  ```bash
  carthage update --platform iOS
  ```
  {: codeblock}

  Le logiciel SDK Swift {{site.data.keyword.watson}} est ensuite téléchargé, et son infrastructure est générée dans le dossier `Carthage/Build/iOS` de votre projet.

## Ajout d'infrastructures à votre application
{: #add-frameworks-to-your-app}

A présent que l'infrastructure du logiciel SDK Swift {{site.data.keyword.watson}} est générée, vous devez lier et copier l'infrastructure {{site.data.keyword.conversationshort}} dans votre application.

1. Ouvrez votre application dans Xcode et sélectionnez votre projet dans le navigateur afin d'afficher ses paramètres.
2. Sélectionnez la cible de votre application et ouvrez l'onglet **Général** tab.
3. Faites défiler jusqu'à la section Linked Frameworks and Libraries et cliquez sur l'icône `+`.
4. Dans la nouvelle fenêtre qui s'affiche, cliquez sur **Add Other** et accédez au répertoire `Carthage/Build/iOS`.
5. Sélectionnez `AssistantV1.framework` afin de le lier à votre application.

En plus de lier l'infrastructure {{site.data.keyword.conversationshort}}, vous devez aussi la copier dans l'application afin de la rendre accessible au moment de l'exécution. Un script Carthage est ensuite utilisé afin d'éviter un [bogue de soumission App Store](http://www.openradar.me/radar?id=6409498411401216){:new_window} particulier.

1. Une fois les paramètres de la cible de votre application affichés dans Xcode, accédez à l'onglet **Build Phases**.
2. Cliquez sur l'icône `+` et sélectionnez **New Run Script Phase**.
3. Ajoutez la commande `/usr/local/bin/carthage copy-frameworks` à la phase d'exécution du script.
4. Ajoutez l'infrastructure {{site.data.keyword.conversationshort}} à la liste **Input Files** :
  ```
  $(SRCROOT)/Carthage/Build/iOS/AssistantV1.framework
  ```
  {: codeblock}

## Ajout d'un assistant virtuel à votre application
{: #add-a-virtual-assistant-to-your-app}

1. Ouvrez `ViewController.swift` dans Xcode.
2. Ajoutez une instruction d'importation pour {{site.data.keyword.conversationshort}}, par exemple `import AssistantV1`.
3. Créez une fonction vide appelée `assistantExample`, puis appelez-la depuis `viewDidLoad`.
4. Ajoutez le code suivant pour la fonction `assistantExample`. Veillez à mettre à jour votre nom d'utilisateur, votre mot de passe et l'ID d'espace de travail.

```swift
//
//  ViewController.swift
//  WatsonAssistantExample
//
//  Created by Glenn R. Fisher on 3/1/18.
//  Copyright © 2018 Glenn R. Fisher. All rights reserved.
//

import UIKit
import AssistantV1

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        assistantExample()
    }

    func assistantExample() {
        // Assistant credentials
        let username = "your-username-here"
        let password = "your-password-here"
        let workspace = "your-workspace-id-here"

        // instantiate service
        let assistant = Assistant(username: username, password: password, version: "2018-03-01")

        // start a conversation
        assistant.message(workspaceID: workspace) { response in
            print("Conversation ID: \(response.context.conversationID!)")
            print("Response: \(response.output.text.joined())")

            // continue assistant
            print("Request: turn the radio on")
            let input = InputData(text: "turn the radio on")
            let request = MessageRequest(input: input, context: response.context)
            assistant.message(workspaceID: workspace, request: request) { response in
                print("Response: \(response.output.text.joined())")
            }
        }
    }
}
```
{: codeblock}

Lorsque vous exécutez l'application, les messages suivants s'affichent sur la console :
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hi. It looks like a nice drive today. What would you like me to do?
Request: turn the radio on
Response: Sure thing! Which genre would you prefer? Jazz is my personal favorite..
```
{: screen}

## Utilisation des kits de démarrage
{: #conversation_starterkits}

Grâce aux kits de démarrage, vous pouvez rapidement et facilement tirer profit des fonctionnalités de {{site.data.keyword.cloud_notm}}. Vous pouvez ajouter {{site.data.keyword.conversationshort}} à un système de back end côté serveur avec les kits de démarrage. Le kit de démarrage Chatbot for iOS with Watson illustre l'utilisation des fonctions d'apprentissage en profondeur de {{site.data.keyword.conversationshort}} par l'ajout d'une interface en langage naturel à votre application qui automatise les interactions avec vos utilisateurs finaux.

1. Sélectionnez le [kit de démarrage](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} avec lequel vous voulez démarrer.
2. Créez le projet avec les services par défaut.
3. Cliquez sur **Ajouter des ressources > Watson > {{site.data.keyword.conversationshort}}**.
4. Téléchargez le projet en cliquant sur **Télécharger le code**. Vous trouverez les données d'identification du service dans le fichier `config/local-dev.json`.

## Etapes suivantes
{: #assistant_next}

Félicitations ! Vous avez ajouté un assistant d'intelligence artificielle à votre application. Poursuivez sur votre lancée en essayant l'une des options suivantes :

* Consultez la section relative au [logiciel SDK Swift{{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.
* Tirez parti de toutes les fonctionnalités offertes par [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html).
* Consultez le code source relatif à l'[exemple d'application de conversion simple](https://github.com/watson-developer-cloud/simple-chat-swift){:new_window}, qui démontre le logiciel SDK Swift {{site.data.keyword.watson}} sur GitHub.
