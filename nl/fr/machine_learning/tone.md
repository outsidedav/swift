---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-24"

keywords: watson swift, tone anaylzer swift, cocoapods swift, swift sdk install, starter kit watson

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

Le service {{site.data.keyword.ibmwatson}} {{site.data.keyword.toneanalyzershort}} permet à votre application de comprendre les émotions et le ton d'un texte. Vous pouvez utiliser ce service pour mieux comprendre les conversations de votre utilisateur ou pour aider les utilisateurs à comprendre comment est perçue leur communication écrite.

## Fonctionnement
{: #how-it-works-tone}

1. Votre application choisit une sélection de texte à analyser (par exemple, un message texte ou un flux Twitter).
2. Votre application envoie le texte au service {{site.data.keyword.toneanalyzershort}} à l'aide du logiciel SDK Watson Swift.
3. Le service analyse le texte en utilisant l'analyse linguistique pour identifier les émotions et le tons.
4. L'analyse du service est renvoyée à votre application via le [logiciel SDK Watson Swift](https://github.com/watson-developer-cloud/swift-sdk).

## Avant de commencer
{: #prereqs-tone}

Assurez-vous que la configuration prérequise suivante est respectée :

* OS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage ou Swift Package Manager

Vous pouvez installer le [logiciel SDK Watson Swift](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") avec [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), ou [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"). Si vous utilisez CocoaPods pour gérer les dépendances, vous obtenez uniquement les infrastructures dont vous avez besoin, par opposition au SDK Watson Swift complet. Si vous ne connaissez pas encore CocoaPods, vous pouvez l'installer facilement :

```bash
$ sudo gem install cocoapods
```
{: codeblock}

## Etape 1. Création d'une instance de Tone Analyzer
{: #create-instance-tone}

Mettez à disposition une instance du service {{site.data.keyword.toneanalyzershort}} :

1. Dans le catalogue {{site.data.keyword.cloud_notm}}, sélectionnez {{site.data.keyword.toneanalyzershort}}. L'écran de configuration du service s'ouvre.
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Sélectionnez une application dans le menu Se connecter si vous voulez lier votre instance à une application.
4. Sélectionnez un plan de tarification, puis cliquez sur **Créer**.
5. Sélectionnez l'onglet **Données d'identification** pour afficher les données d'identification du service. Ces valeurs sont utilisées pour la connexion au service à partir de votre application.

## Etape 2. Téléchargement et génération de dépendances
{: #download-depend-tone}

A l'aide de votre éditeur de texte préféré, créez un nouveau fichier `Podfile` dans le répertoire racine de votre projet (où se trouve votre fichier `.xcodeproj`) en exécutant `pod init`. Ajoutez ensuite une ligne pour spécifier l'infrastructure {{site.data.keyword.conversationshort}} du SDK Watson Swift en tant que dépendance :

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonToneAnalyzerV3'
```
{: codeblock}

Pour une application de production, vous pouvez aussi indiquer qu'une [version particulière est requise](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") pour éviter les modifications inattendues des nouvelles versions du logiciel SDK Watson Swift.

Maintenant que votre fichier `Podfile` est en place, téléchargez les dépendances. Utilisez un terminal pour accéder au répertoire racine de votre projet, puis lancez CocoaPods :

```console
pod install
```
{: codeblock}

Cocoapods télécharge l'infrastructure {{site.data.keyword.toneanalyzershort}} et la génère dans le dossier `Pods/` de votre projet.

Pour éviter les problèmes de génération de Pod, ouvrez le fichier qui se termine par `.xcworkspace` au lieu de `.xcodeproj` lorsque vous ouvrez le projet dans Xcode.
{: tip}

## Etape 3. Analyse des textes dans votre application
{: #analyze-text-tone}

Les exemples suivants vous aident à ajouter des fonctions {{site.data.keyword.toneanalyzershort}} à votre application, généralement dans `ViewController.swift`. A l'aide de ces exemples, vous pouvez étendre les appels de Tone Analyzer à votre scénario d'utilisation.

1. Ajoutez une instruction d'importation pour Tone Analyzer :
  ```swift
  import ToneAnalyzer
  ```
  {: codeblock}

2. Créez une instance du service Tone Analyzer :
  ```swift
  let toneAnalyzer = ToneAnalyzer(version: "yyyy-mm-dd", apiKey: "your-api-key-here")
  ```
  {: codeblock}

  Consultez la [documentation sur les paramètres de version](https://cloud.ibm.com/apidocs/tone-analyzer#versioning){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") ou utilisez la date à laquelle le service {site.data.keyword.conversationshort}} a été créé.
  {: tip}

3. Fournissez un texte à analyser et traitez les résultats :
  ```swift
  // Text to analyze
  let review = """
      I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
  """

  // Analyze text
  let input = ToneInput(text: review)
  toneAnalyzer.tone(toneInput: .toneInput(input)) { response, error in
      if let error = error {
          print(error)
          return
      }

      guard let tones = response?.result?.documentTone.tones else {
          print("Failed to analyze the tone input")
          return
      }

      for tone in tones {
          print("\(tone.toneName): \(tone.score)")
              }
  }
  ```
  {: codeblock}

  Lorsque vous exécutez le code, vous voyez l'analyse suivante dans la console :
  ```
  Sadness: 0.575803
Tentative: 0.867377
  ```
  {: screen}

4. Explorez la [documentation sur Tone Analyzer](https://watson-developer-cloud.github.io/swift-sdk/services/ToneAnalyzerV3/index.html){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") du logiciel SDK Watson pour développer les fonctionnalités de votre application.

## Utilisation des kits de démarrage
{: #tone_starterkits}

Les [kits de démarrage](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") constituent l'un des moyens les plus rapides d'utiliser les fonctionnalités d'{{site.data.keyword.cloud_notm}}. Vous pouvez utiliser le service {{site.data.keyword.toneanalyzershort}} en sélectionnant le kit de démarrage **Tone Analyzer for iOS with Watson**. Ce service utilise les fonctions d'apprentissage en profondeur pour évaluer des passages de texte. L'application Tone Analyzer identifie le ton de l'orateur (joyeux, triste, sûr, etc) et le relie à un certain nombre de catégories.

Pour commencer à utiliser ce kit de démarrage :

1. Sélectionnez le kit de démarrage disponible [ici](https://cloud.ibm.com/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").
2. Créez le projet avec les services par défaut.
3. Téléchargez le projet en cliquant sur **Télécharger le code**. Les données d'identification du service sont injectées dans le fichier `BMSCredentials.plist` des zones clé correspondantes.

## Etapes suivantes
{: #tone_next notoc}

Félicitations ! {{site.data.keyword.toneanalyzershort}} est maintenant ajouté à votre application. Continuez sur votre lancée en essayant l'une des options suivantes :

* Affichez le [logiciel Watson Swift SDK sur GitHub](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") et explorez les autres services Watson pris en charge.
* Pour plus d'informations, voir [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").
