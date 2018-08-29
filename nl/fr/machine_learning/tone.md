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

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

Le service IBM Watson Tone Analyzer permet à votre appli de comprendre les émotions et les tonalités dans un texte. Vous pouvez utiliser ce service pour mieux comprendre les conversations de votre utilisateur ou pour aider les utilisateurs à comprendre comment est perçue leur communication écrite.

## Fonctionnement
{: ##how-it-works}

1. Votre application choisit une sélection de texte à analyser (par exemple, un message texte ou un flux Twitter).
2. Votre appli envoie le texte au service {{site.data.keyword.toneanalyzershort}} à l'aide du logiciel SDK Swift Watson.
3. Le service analyse le texte en utilisant l'analyse linguistique pour identifier les émotions et les tonalités.
4. L'analyse du service est renvoyée à votre appli par le logiciel SDK Swift Watson.

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
{: codeblock}

## Etape 1. Création d'une instance de Tone Analyzer
{: #create-and-configure-an-instance-of-tone-analyzer}

Mettez à disposition une instance du service {{site.data.keyword.toneanalyzershort}} :

1. Dans le catalogue {{site.data.keyword.cloud_notm}}, sélectionnez {{site.data.keyword.toneanalyzershort}}. L'écran de configuration du service s'ouvre.
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Sélectionnez une appli dans la menu Se connecter si vous voulez lier votre instance à une appli.
4. Sélectionnez un plan de tarification, puis cliquez sur **Créer**.
5. Sélectionnez l'onglet **Données d'identification** pour afficher les données d'identification du service. Ces valeurs sont utilisées pour la connexion au service à partir de votre application.

## Etape 2. Téléchargement et génération de dépendances
{: ###download-and-build-dependencies}

A l'aide de votre éditeur de texte préféré, créez un fichier nommé `Cartfile` dans le répertoire racine de votre projet (où se trouve votre fichier `.xcodeproj`).Ensuite, ajoutez une ligne pour indiquer le logiciel SDK Swift Watson en tant que dépendance :

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

Pour une appli de production, vous pouvez aussi si vous le souhaitez indiquer si vous le souhaitez, qu'une [version particulière est requise](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) pour éviter les modifications inattendues des nouvelles versions du logiciel SDK Swift Watson.

Une fois `Cartfile` installé, vous pouvez maintenant télécharger et générer les dépendances. Utilisez un terminal pour accéder au répertoire racine de votre projet, puis lancez Carthage :
  
  ```bash
  $ carthage update --platform iOS
  ```
  {: codeblock}

Carthage télécharge le logiciel SDK Swift Watson, puis il génère ses infrastructures dans le dossier `Carthage/Build/iOS` de votre projet.

## Etape 3. Ajout d'infrastructures à votre appli
{: ###add-frameworks-to-your-app}

### Etapes de liaison à Tone Analyzer

A présent que les infrastructures du logiciel SDK Swift Watson sont générés par Carthage, vous devez lier l'infrastructure Tone Analyzer à votre appli.

1. Ouvrez votre appli dans Xcode et sélectionnez votre projet dans le navigateur afin d'afficher ses paramètres.
2. Sélectionnez la cible de votre appli et ouvrez l'onglet **Général**.
3. Faites défiler jusqu'à la section "Linked Frameworks and Libraries" et cliquez sur l'icône `+`.
4. Dans la nouvelle fenêtre qui s'affiche, cliquez sur **Add Other** et accédez au répertoire `Carthage/Build/iOS`. Sélectionnez **ToneAnalyzerV3.framework** afin de le lier à votre appli.

### Etapes de copie de Tone Analyzer

En plus de _lier_ l'infrastructure Tone Analyzer, vous devez aussi la _copier_ dans l'appli afin de la rendre accessible lors de l'exécution. Xcode propose différentes méthodes pour la copie ou l'imbrication d'une infrastructure, mais vous pouvez utiliser un script Carthage afin d'éviter un [bogue de soumission App Store](http://www.openradar.me/radar?id=6409498411401216) particulier.

1. Une fois les paramètres de la cible de votre appl affichés dans Xcode, accédez à l'onglet **Build Phases**.
2. Cliquez sur l'icône `+` et sélectionnez **New Run Script Phase**.
3. Ajoutez la commande suivante pour exécuter le script phase : `/usr/local/bin/carthage copy-frameworks`.
4. Ajoutez l'infrastructure Tone Analyzer à la liste "Fichiers d'entrée" : `$(SRCROOT)/Carthage/Build/iOS/ToneAnalyzerV3.framework`.

Vous êtes maintenant prêt à commencer à utiliser le logiciel SDK Swift Watson dans votre appli !

## Etape 4. Analyse de texte dans votre appli
{: #analyze-text-in-your-app}

1. Ouvrez le fichier `ViewController.swift` dans Xcode.
2. Ajoutez une instruction d'importation pour Tone Analyzer :
    ```swift
    import ToneAnalyzerV3
    ```
    {: codeblock}

3. Créez une fonction vide appelée `toneAnalyzerExample`, puis appelez-la depuis `viewDidLoad`.
4. Ajoutez le code suivant à la fonction `toneAnalyzerExample`. Veillez à mettre à jour le nom d'utilisateur et le mot de passe de votre service.
  ```swift
  import UIKit
  import ToneAnalyzerV3

  class ViewController: UIViewController {

      override func viewDidLoad() {
          super.viewDidLoad()
          toneAnalyzerExample()
      }

      func toneAnalyzerExample() {

          // instantiate service
          let toneAnalyzer = ToneAnalyzer(
              username: "your-username-here",
              password: "your-password-here",
              version: "yyyy-mm-dd"
          )

          // text to analyze
          let review = """
              I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
          """

          // analyze text
          let toneInput = ToneInput(text: review)
          let failure = { (error: Error) in print(error) }
          toneAnalyzer.tone(toneInput: toneInput, contentType: "application/json", failure: failure) { analysis in
              for tone in analysis.documentTone.tones! {
                  print("\(tone.toneName): \(tone.score)")
              }
          }
      }
  }
  ```
  {: codeblock}

Lorsque vous exécutez votre application, vous voyez l'analyse suivante dans la console :
```
Sadness: 0.575803
Tentative: 0.867377
```
{: screen}

## Utilisation des kits de démarrage
{: #tone_starterkits}

Les [kits de démarrage](https://console.bluemix.net/developer/appledevelopment/starter-kits) sont l'un des moyens les plus rapides d'optimiser les fonctionnalités d'{{site.data.keyword.cloud_notm}}. Vous pouvez utiliser le service {{site.data.keyword.toneanalyzershort}} en sélectionnant le kit de démarrage **Tone Analyzer for iOS with Watson**. Ce service utilise les fonctions d'apprentissage en profondeur pour évaluer des passages de texte. L'application Tone Analyzer identifie le ton de l'orateur (heureux, triste, confiant, etc) et le relie à un certain nombre de catégories.

Pour démarrer avec ce kit de démarrage :

1. Sélectionnez le kit de démarrage qui se trouve [ici](https://console.bluemix.net/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson).
2. Créez le projet avec les services par défaut.
3. Téléchargez le projet en cliquant sur **Télécharger le code**. Les données d'identification du service sont injectées dans le fichier `BMSCredentials.plist` des zones clé correspondantes.

## Etapes suivantes
{: #tone_next}

Félicitations !  {{site.data.keyword.toneanalyzershort}} est maintenant ajouté à votre appli. Poursuivez sur votre lancée en essayant l'une des options suivantes :

* Consultez le [logiciel SDK Swift Watson sur GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* Pour plus d'informations, voir [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/).

