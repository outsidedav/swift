---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: install sdk swift, sdk client swift, carthage, cocoapods, swift package manager, ios sdk

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Installation de logiciels SDK dans les applications client
{: #install-sdks}

Les logiciels SDK iOS d'{{site.data.keyword.cloud}} prennent en charge différents gestionnaires de dépendances connus, ce qui vous permet d'installer et d'utiliser facilement des services {{site.data.keyword.cloud_notm}} au sein de vos applications.

## Installation avec CocoaPods
{: #install_cocoapods}

Pour installer un logiciel SDK à l'aide CocoaPods, ajoutez-le à votre `fichier Pod`. Si votre projet n'a pas encore de `fichier Pod`, utilisez la commande `pod init`.
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

Exécutez `pod install` et ouvrez le fichier `.xcworkspace` généré.

Pour plus d'informations, consultez les [guides CocoaPods](https://guides.cocoapods.org/using/index.html){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").

## Installation avec Carthage
{: #install_carthage}

Pour installer un logiciel SDK avec Carthage, ajoutez la ligne suivante à votre `Cartfile` :
```
github "<github org name>/<github project name>"
```
{: codeblock}

Exécutez `carthage update` pour démarrer le processus de génération. Une fois la génération terminée, ajoutez l'infrastructure générée à votre projet. 

Pour plus d'informations, voir la documentation sur l'[initiation à Carthage](https://github.com/Carthage/Carthage#getting-started){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").

## Installation avec le gestionnaire de package Swift
{: #install_swift_package}

Pour installer un logiciel SDK à l'aide du gestionnaire de package Swift, ajoutez la ligne suivante à vos dépendances dans votre `Package.swift` :
```swift
.Package(url: "<SDK git url>")
```
{: codeblock}

Exécutez `swift build` pour démarrer le processus de génération.

Pour plus d'informations, voir la [présentation du gestionnaire de package Swift](https://swift.org/package-manager/){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").

## Installation manuelle
{: #install_manually}

Pour installer manuellement un logiciel SDK, téléchargez le logiciel SDK et ajoutez manuellement les fichiers source dans votre projet.
