---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: chatbot swift, virtual assistant swift, assistant swift, watson swift starter, assistantv2 swift, watson sdk swift, add chatbot swift, add assistant swift

subcollection: swift

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
3. Le logiciel SDK {{site.data.keyword.watson}} Swift se connecte à un espace de travail, qui est un conteneur pour votre flux de dialogues et vos données d'apprentissage.
4. L'espace de travail interprète l'entrée utilisateur et dirige le flux de la conversation, en envoyant une réponse à votre application.
5. Votre application affiche la réponse à l'utilisateur.

## Avant de commencer
{: #prereqs-chatbot}

Assurez-vous que la configuration prérequise suivante est respectée :

* Une instance du service [{{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-getting-started#getting-started)
* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage ou Swift Package Manager

Vous pouvez installer le [logiciel SDK Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") avec [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), ou [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"). Si vous utilisez CocoaPods pour gérer les dépendances, vous obtenez uniquement les infrastructures dont vous avez besoin, par opposition au SDK Watson Swift complet. Si vous ne connaissez pas encore CocoaPods, vous pouvez l'installer facilement :

```console
sudo gem install cocoapods
```
{: codeblock}

## Etape 1. Création d'une instance de Watson Assistant
{: #instance-watson-chatbot}

Mettez à disposition une instance du service {{site.data.keyword.conversationshort}} :

1. Dans le catalogue {{site.data.keyword.cloud_notm}}, sélectionnez **{{site.data.keyword.conversationshort}}**. L'écran de configuration du service s'ouvre.
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Sélectionnez une application dans le menu **Se connecter** si vous voulez lier votre instance à une application.
4. Sélectionnez un plan de tarification, puis cliquez sur **Créer**.
5. Sélectionnez l'onglet **Données d'identification** pour afficher les données d'identification du service. Ces valeurs sont utilisées pour la connexion au service à partir de votre application.
6. Cliquez sur **Lancer l'outil** pour créer votre assistant d'agent conversationnel. Suivez les instructions de la page d'arrivée pour créer votre propre agent conversationnel ou accédez à l'onglet **Skills** et sélectionnez **Créer**. Sur la page **Add Dialog Skill**, cliquez sur l'onglet **Use sample skill** et sélectionnez **Customer Care Sample Skill** pour démarrer rapidement. Vous pouvez continuer d'affiner cette compétence ou en créer d'autres qui pourront être utilisées par votre application par la suite.
7. Cliquez sur le menu de votre nouvelle compétence et sélectionnez **View API Details**. Vous pouvez maintenant voir l'`ID d'espace de travail` dont vous avez besoin en plus des données d'identification du service.

## Etape 2. Téléchargement et génération des dépendances
{: #download-depend-chatbot}

L'exemple qui suit utilise AssistantV1. Pour en savoir plus sur l'infrastructure AssistantV2, consultez la [documentation relative à Watson SDK AssistantV2](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV2/index.html){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").

A l'aide de l'éditeur de texte de votre choix, créez un nouveau fichier `Podfile` dans le répertoire racine de votre projet (où se trouve votre fichier `.xcodeproj`) en exécutant `pod init`. Ajoutez ensuite une ligne pour spécifier l'infrastructure {{site.data.keyword.conversationshort}} du SDK Watson Swift en tant que dépendance :

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonAssistantV1'
```
{: codeblock}

Pour une application de production, vous pouvez aussi indiquer qu'une [version particulière est requise](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") pour éviter les modifications inattendues des nouvelles versions du logiciel SDK Watson Swift.

Maintenant que votre fichier `Podfile` est en place, téléchargez les dépendances. Utilisez un terminal pour accéder au répertoire racine de votre projet, puis lancez CocoaPods :

```console
pod install
```
{: codeblock}

Cocoapods télécharge l'infrastructure {{site.data.keyword.conversationshort}} et la génère dans le dossier `Pods/` de votre projet.

Pour éviter les problèmes de génération de Pod, ouvrez le fichier qui se termine par `.xcworkspace` au lieu de `.xcodeproj` lorsque vous ouvrez le projet dans Xcode.
{: tip}

## Etape 3. Ajout d'un assistant virtuel à votre application
{: #virtual-assist-chatbot}

Les exemples suivants vous aident à ajouter des fonctions {{site.data.keyword.conversationshort}} à votre application, généralement dans `ViewController.swift`. A l'aide de ces exemples, vous pouvez étendre les appels de l'assistant à votre scénario d'utilisation.

1. Ajoutez une instruction d'importation pour {{site.data.keyword.conversationshort}}, par exemple `import Assistant`.
  ```swift
  import Assistant
  ```
  {: codeblock}

2. Instanciez le service {{site.data.keyword.conversationshort}} :
  ```swift
  let assistant = Assistant(version: "yyyy-mm-dd", apikey: "your-api-key-here")

  // save context to state to continue the conversation later
  var context: Context?
  ```
  {: codeblock}

  **Astuce **: cet exemple sauvegarde le contexte à énoncer. Pour une meilleure compréhension de cet objet et pour savoir comment l'adapter à votre scénario d'utilisation, consultez la [documentation sur les variables de contexte](/docs/services/assistant?topic=assistant-dialog-runtime#context-variables). Consultez la [documentation sur les paramètres de version](https://cloud.ibm.com/apidocs/assistant#versioning){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") ou utilisez la date à laquelle le service {site.data.keyword.conversationshort}} a été créé.
  

3. Initialisez la conversation. Suivant la manière dont votre assistant est configuré, il peut fournir une réponse initiale à l'utilisateur :
  ```swift
  // Start a conversation
  assistant.message(workspaceID: "your-workspace-ID-here) { response, error in
      if let error = error {
         print(error)
    }

      guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Set the context to state
      self.context = message.context    
  }
  ```
  {: codeblock}

4. Envoyez un message et le contexte à l'assistant :
  ```swift
  print("Request: When are you open?")
  let input = InputData(text: "When are you open?")

  assistant.message(workspaceID: workspace, input: input, context: self.context) { response, error in
      if let error = error {
         print(error)
    }

      guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Update the context
      self.context = message.context
  }
  ```
  {: codeblock}

Lorsque vous exécutez votre application avec ces exemples avec l'assistant par défaut, les messages suivants s'affichent dans la console :
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hello, I’m a demo customer care virtual assistant to show you the basics.  I can help with directions to my store, hours of operation and booking an in-store appointment.
Request: When are you open?
Response: Our hours are Monday to Friday 10am to 8pm and Friday and Saturday 11am to 6pm.
```
{: screen}

5. Explorez la [documentation de l'Assistant](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV1/index.html){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") du logiciel SDK Watson pour développer les fonctionnalités de votre application.

## Utilisation des kits de démarrage
{: #starterkits-chatbot}

Les kits de démarrage constituent un moyen facile et rapide d'utiliser les fonctionnalités d'{{site.data.keyword.cloud_notm}}. Grâce à eux, vous pourrez ajouter {{site.data.keyword.conversationshort}} à un système de back end côté serveur. Le kit de démarrage Chatbot for iOS with Watson montre comment utiliser les fonctions d'apprentissage en profondeur de {{site.data.keyword.conversationshort}}, en ajoutant une interface en langage naturel à votre application, qui automatise les interactions avec vos utilisateurs.

1. Sélectionnez le [kit de démarrage](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") avec lequel vous voulez démarrer.
2. Créez l'application avec les services par défaut.
3. Cliquez sur **Ajout d'un service > Watson > {{site.data.keyword.conversationshort}}**.
4. Téléchargez le projet en cliquant sur **Télécharger le code**. Vous trouverez les données d'identification du service dans le fichier `config/local-dev.json`.

## Etapes suivantes
{: #next-chatbot notoc}

Félicitations ! Vous avez ajouté un assistant d'intelligence artificielle à votre application. Continuez sur votre lancée en essayant l'une des options suivantes :

* Consultez le [logiciel SDK {{site.data.keyword.watson}} Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") et explorez les autres services Watson pris en charge.
* Tirez parti des toutes les fonctions offertes par [{{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-index#index).
* Consultez le code source relatif à l'[exemple d'application de discussion simple](https://github.com/watson-developer-cloud/simple-chat-swift){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), qui démontre le logiciel SDK Swift {{site.data.keyword.watson}} sur GitHub.
