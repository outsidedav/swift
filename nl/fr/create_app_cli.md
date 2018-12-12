---

copyright:
  years: 2018
lastupdated: "2018-11-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Création d'une application Swift côté serveur avec l'interface de ligne de commande
{: #swift_cli}

{{site.data.keyword.cloud}} propose des solutions et des services qui permettent aux développeurs et aux applications Swift de bénéficier de fonctions de sécurité, d'intelligence artificielle et de la valeur exigée par vos clients. Avec une gamme étendue d'offres et de logiciels SDK, vous pouvez utiliser ces services et mettre rapidement sur le marché vos applications de pointe.
{: shortdesc}

Le guide suivant est destiné à vous assister dans la génération, l'exécution en local et le déploiement d'une application Swift côté serveur. Découvrez comment utiliser {{site.data.keyword.dev_cli_long}} pour exécuter ces actions avec des commandes standard.

Vous pouvez utiliser {{site.data.keyword.dev_cli_short}} pour gérer vos applications côté serveur avec plus d'une douzaine de commandes. Découvrez les commandes `ibmcloud dev` de l'[interface de ligne de commande IBM Cloud Developer Tools](/docs/cli/idt/commands.html).

## Etape 1. Exigences pour les développeurs

Pour démarrer avec {{site.data.keyword.cloud_notm}}, assurez-vous de respecter les exigences suivantes :

### Système d'exploitation

Développez des applications Swift dans les meilleurs conditions possibles en utilisant le dernier matériel MacOS pris en charge et les testant avec les dernières versions iOS. Inscrivez-vous pour un compte [Apple Developer](https://developer.apple.com/) afin de pouvoir effectuer des tests sur une unité physique.

### iOS et Xcode
{: #ios_and_xcode}

- [Installez Xcode 8+ (ou ultérieur)](https://developer.apple.com/xcode/)
- [Déployez sur des appareils iOS 8 (ou ultérieur)](https://support.apple.com/downloads/ios)
- Avant de soumettre une application dans Apple, passez en revue les [Règles de soumission App Store](https://developer.apple.com/app-store/guidelines/)

### Logiciels SDK et gestion des dépendances

Les outils suivants garantissent que vous pouvez installer les logiciels SDK natifs pour qu'ils fonctionnent avec les différents services {{site.data.keyword.cloud_notm}}.

- [Installation de CocoaPods pour les logiciels SDK IBM Cloud](https://cocoapods.org/)
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Installation de Homebrew pour permettre l'installation de Carthage](https://brew.sh/)
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Installation de Carthage pour les logiciels SDK de Watson](https://github.com/Carthage/Carthage)
  ```
  brew install carthage
  ```
  {: codeblock}

## Etape 2. Installation d'outils pour le développement local

{{site.data.keyword.cloud}} fournit des outils d'interface de ligne de commande en local qui vous permettent de traiter les différents aspects d'{{site.data.keyword.cloud_notm}}. Pour plus d'informations, consultez les [Informations sur {{site.data.keyword.dev_cli_long}}](../cli/index.html). Vous pouvez utiliser les outils pour tester un système de back-end Swift dans une image Docker locale avant le déploiement en cloud. 

* Pour MacOS et Linux, exécutez la commande suivante :
  ```
  curl -sL http://ibm.biz/idt-installer | bash
  ```
  {: codeblock}

* Pour Windows 10, exécutez la commande suivante en tant qu'administrateur :
  ```
  Set-ExecutionPolicy Unrestricted; iex(New-Object Net.WebClient).DownloadString('http://ibm.biz/idt-win-installer')
  ```
  {: codeblock}

  Cliquez avec le bouton droit de la souris sur l'icône Windows PowerShell et sélectionnez **Exécuter en tant qu'administrateur**.
  {: tip}

## Etape 3. Création d'une application Swift

1. Exécutez la commande `ibmcloud dev create` depuis l'interface de ligne de commande {{site.data.keyword.dev_cli_short}} afin de générer un module de démarrage préconfiguré. 
  ```
  ibmcloud dev create
  ```
  {: codeblock}

  Assurez-vous de vous connecter avec compte {{site.data.keyword.cloud_notm}} pour créer un projet. Les nouveaux utilisateurs peuvent [s'enregistrer ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/registration/?cm_sp=dw-bluemix-_-swift-_-devcenter) pour un compte gratuit. Utilisez la commande `ibmcloud login` pour vous connecter en ligne de commande.
  {: tip}

2. Lorsque vous y êtes invité, sélectionnez l'option 1, puis 6, et enfin 2, comme illustré dans l'exemple suivant :
  ```
  ? Sélectionnez un type de ressource :
  1. Service de back-end / Application Web
  2. Client mobile
  Entrez un nombre> 1

  ? Sélectionnez un langage :
  1. Java - MicroProfile / Java EE
  2. Java - Spring
  3. Node
  4. Python - Django
  5. Python - Flask
  6. Swift
  Entrez un nombre> 6

  ? Sélectionnez un kit de démarrage :
  1. BFF (Backend for Frontend) - Swift Kitura - Module de démarrage pour la génération
  d'API BFF dans Swift, à l'aide de l'infrastructure Kitura.
  2. Application Web - Créer un projet
  3. Application Web - Swift Kitura Basic - Module de démarrage qui fournit une application de serveur
  Web de base dans Swift, à l'aide de l'infrastructure Kitura
  Entrez un nombre> 2
  ```
  {: screen}

3. Indiquez un nom pour votre application.
  ```
  ? Entrez un nom pour votre application > swift_project
  ```
  {: screen}

4. Choisissez d'activer la prise en charge OpenAPI 2.0 :
  ```
  ? Activer votre application sur la base d'un document de spécification OpenAPI 2.0 ? [o/n]> o
  ```
  {: screen}

  Si la prise en charge OpenAPI 2.0 est activée, vous devez fournir un chemin d'accès au document OpenAPI 2.0 sous la forme d'une URL :
  ```
  ? Chemin d'accès au document OpenAPI 2.0 en tant qu'URL (format yaml et json pris en charge)> http://hostname.domain.com/path/to/file.json

  Génération de l'application en cours ...
  ```
  {: screen}

## Etape 4. Génération, exécution et déploiement de votre application 

Vous pouvez maintenant générer, exécuter et déployer votre application à l'aide de {{site.data.keyword.dev_cli_short}}.

1. **Génération**

  Vous pouvez désormais générer votre application, laquelle est prérequis à l'exécution de celle-ci. Utilisez la commande suivante à la racine du répertoire de l'application pour générer votre application :
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **Exécution**

  Après une génération réussie, vous pouvez exécuter votre application dans un conteneur local à l'aide de la commande suivante :
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  Un hôte et un port locaux pour l'affichage de la page d'arrivée de votre application s'affiche si la commande aboutit.

3. **Procédez au déploiement.**

  Déployez votre application dans {{site.data.keyword.cloud_notm}} à l'aide de la commande `deploy` :
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}

## Etapes suivantes

Découvrez comment utiliser la console de développement pour Apple {{site.data.keyword.cloud_notm}} afin de permettre aux développeurs de créer des applications à partir des différents kits de démarrage, créer et connecter les principaux services optimisés {{site.data.keyword.cloud_notm}}, puis télécharger rapidement le code opérationnel ou définir une distribution continue. Les utilisateurs peuvent créer, consulter, configurer et gérer votre application, mais également télécharger son code. Grâce à la console de développement pour Apple, vous pouvez rapidement évaluer et tester les services {{site.data.keyword.cloud_notm}} avec une toute nouvelle application.

Prêt à vous lancer ? Visitez dès maintenant le site [Console de développement pour Apple {{site.data.keyword.cloud_notm}}](https://console.bluemix.net/developer/appledevelopment/starter-kits) pour démarrer.
{: tip}

Pour plus d'informations, voir [Développement d'applications Swift avec des kits de démarrage](/docs/swift/starter_kit/starter_kits.html).
