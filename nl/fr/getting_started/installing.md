---

copyright:
  years: 2018
lastupdated: "2018-06-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Installation de logiciels SDK dans les applis client
{: #installing}

Les logiciels SDK iOS d'{{site.data.keyword.cloud}} prennent en charge différents gestionnaires de dépendances connus, ce qui vous permet d'installer et d'utiliser facilement des services {{site.data.keyword.cloud_notm}} au sein de vos applications.

## Installation avec CocoaPods
{: #installing_with_cocoapods}

Pour installer un logiciel SDK à l'aide Cocoapods, ajoutez-le à votre `fichier Pod`. Si votre projet n'a pas encore de `fichier Pod`, utilisez la commande `pod init`.
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

Exécutez `pod install` et ouvrez le fichier `.xcworkspace` généré.

Pour plus d'informations, voir les [Guides Cocoapods](https://guides.cocoapods.org/using/index.html).

## Installation avec Carthage
{: #installing_with_carthage}

Pour installer un logiciel SDK avec Carthage, ajoutez la ligne suivante à votre `Cartfile` :
```
github "<github org name>/<github project name>"
```
{: codeblock}

Exécutez `carthage update` pour démarrer le processus de génération. Une fois la génération terminée, ajoutez l'infrastructure générée à votre projet. 

Pour plus d'informations, voir le [README Carthage](https://github.com/Carthage/Carthage#getting-started).

## Installation avec le gestionnaire de package Swift
{: #installing_with_swift_package_manager}

Pour installer un logiciel SDK à l'aide du gestionnaire de package Swift, ajoutez la ligne suivante à vos dépendances dans votre `Package.swift` :
```
.Package(url: "<SDK git url>")
```
{: codeblock}

Exécutez `swift build` pour démarrer le processus de génération. 

Pour plus d'informations, voir a [Présentation du gestionnaire de package Swift](https://swift.org/package-manager/).

## Installation manuelle 
{: #installing_manually}

Pour installer manuellement un logiciel SDK, téléchargez le logiciel SDK et ajoutez manuellement les fichiers source dans votre projet.
