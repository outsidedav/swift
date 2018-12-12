---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:gif: data-image-type='gif'}

# Gestion des versions des fonctions d'application
{: #mobile_applaunch}

{{site.data.keyword.engage_full}} permet aux développeurs de générer des applications conviviales en contrôlant les accès et en déployant des fonctions d'applications fournissant des métriques mesurables. Le service permet aux développeurs de retirer le couplage qui existe aujourd'hui entre le déploiement de fonctions d'application et les mises à jour d'applications en production. Vous pouvez désormais publier des fonctions sans les exposer en production afin de mettre graduellement en production de nouvelles versions d'une application de manière contrôlée. Avec le service {{site.data.keyword.engage_short}}, les propriétaires d'application ont le contrôle complet sur le déploiement de fonctions d'un segment ciblé.

Le service {{site.data.keyword.engage_short}} définit une fonction, crée un public sur la base de plateformes d'appareil (y compris les attributs de public personnalisés), et définit finalement une interaction qui orchestre l'heure et l'emplacement de la fonction. Une fois les logiciels SDK utilisés, ainsi que la fonction et les attributs de métrique qui sont incorporés dans l'application, le service commence à mesurer les expériences du public. Vous pouvez maintenant utiliser votre application en fonction de ces informations pour créer des engagements clients personnalisés à travers les différentes catégories d'utilisateurs de votre application.

![Aperçu de l'engagement cognitif](images/process_app_launch.png) Figure 1. Vue d'ensemble du cycle de vie du service {{site.data.keyword.engage_short}} 

Les fonctions du service {{site.data.keyword.engage_short}} sont les suivantes :

 - **Accélération du déploiement des fonctions**

    Accélération de distribution des fonctions pour votre application par une mise en production contrôlée afin de minimiser les risques. Mise en production des fonctions dans un sous-ensemble de segments de public et décisions de déploiements ou d'annulations plus larges sur la base de commentaires en retour en temps réel. Déploiements de fonctions distincts à partir de cycles de mise en production réguliers.

 - **Public de segment**

    Des segments utilisateur peuvent être définis à partir d'attributs démographiques, contextuels et comportementaux. Vous pouvez également déployer des fonctionnalités sur un certain pourcentage de l'ensemble de la base d'utilisateurs. Des indicateurs clés de performance peuvent être définis pour chaque fonction et code côté client pour mesurer les résultats.

 - **Adaptation de l'application en fonction du contexte**

    Il est possible de personnaliser le comportement d'une application, l'interface utilisateur et les notifications pour des segments de public spécifiques. Par exemple, l'arrière-plan d'une app peut être modifié en fonction de l'emplacement de l'utilisateur. Cette personnalisation utilisateur accroît l'engagement des utilisateurs avec l'application.

 - **Fonctions de test A / B**

    Gagnez en confiance par l'expérimentation. Deux variantes des fonctions d'application peuvent être comparées en déployant chacune d'elles en même temps. Il est ainsi possible de prendre des décisions sur la base de données factuelles.

 - **Accroissement de l'interaction avec les clients**

    Encouragez l'interaction avec les clients. Des notifications peuvent être ciblées pour tous les utilisateurs d'une application ou pour un ensemble spécifique d'utilisateurs et d'appareils. Il est aussi possible de définir un planning de messages. L'interaction utilisateur joue un rôle essentiel dans les relations client.


## Avant de commencer :

Tout d'abord, assurez-vous que vous respectez la configuration prérequise suivante :

 - iOS 10+
 - Xcode 9
 - Swift 3.2 - 4
 - CocoaPods ou Carthage

## Etape 1. Création d'une instance de {{site.data.keyword.engage_short}}
{: ##app_launch_create}

1. Dans le catalogue {{site.data.keyword.cloud_notm}}, cliquez sur **Mobile** > **App Launch**. L'écran de configuration du service s'ouvre.
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Cliquez sur **Créer**.
4. Dans le panneau de navigation, cliquez sur **Connexions** pour sélectionner une application et la lier à votre service. Vous pouvez lier l'instance de service à votre application ultérieurement si vous ne la liez pas pendant l'étape de création.

## Etape 2. Initialisation de votre application
{: #step2}

Le service fournit des logiciels SDK spécifiques à la plateforme afin de simplifier le développement d'application. Les logiciels SDK {{site.data.keyword.cloud_notm}} Mobile Services Swift peuvent être installés avec CocoaPods ou Carthage. 

1. Cliquez sur **Paramètres**.
2. Installez le logiciel [SDK](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-applaunch). Pour plus d'informations, consultez le fichier `README` qui contient les étapes d'installation et des concepts techniques. 
3. Copiez les clés de configuration pour initialiser votre application. Utilisez la valeur confidentielle de l'application, l'identificateur global unique de l'application et la valeur confidentielle du client pour configurer votre application et créer des engagements.

## Etape 3. Création d'une fonction
{: #step3}

Le service {{site.data.keyword.engage_short}} crée et teste les réponses aux fonctions.

![Détails d'une fonction](images/feature_creation_animated.gif){: gif}

Pour créer une fonction, procédez comme suit : 
1. Dans le panneau de navigation, cliquez sur **Fonctions** > **Créer Nouvelles fonctions**.
2. Entrez un nom de fonction et une description dans le formulaire de création d'une nouvelle fonction et de métriques. Vous pouvez également définir les propriétés de la fonction et ajouter des métriques afin de mesurer l'impact de votre engagement. Cliquez sur **Edition en bloc** pour ajouter plusieurs propriétés en éditant le fichier JSON.
3. Cliquez sur **Créer**. La nouvelle fonction apparaît maintenant dans le panneau Fonctions.
4. Activez la fonction une fois que celle-ci a été développée.
5. Pour activer une fonction à utiliser comme engagement, cliquez sur la fonction que vous avez créée.
6. Dans la fenêtre des détails de fonction, choisissez de mettre à jour l'état de votre fonction sur **Prêt**.
7. Cliquez sur **Mettre à jour l'état**.
8. Mettez à jour votre application afin d'inclure les attributs nouvellement créés et les codes de fonction dans votre application iOS.
9. La fonction est désormais prête pour utilisation.

La fenêtre des détails de la fonction peut exporter la fonction en tant que fichier JSON, lequel peut être utilisé dans l'application client pour charger les valeurs par défaut.

## Etape 4. Création d'un public
{: #step4}

![Création d'un public](images/create_audience_animated.gif){: gif}

Pour créer un public, procédez comme suit :

### Création d'un **attribut de public**:

1. Cliquez sur **Public** > **Créer un attribut**.
2. Indiquez les valeurs suivantes :
  * **Nom** : Entrez un nom approprié pour l'attribut.
  * **Description** : Entrez une brève description de l'attribut.
  * **Type** : Choisissez le type d'attribut.
  * **Valeurs admises** : Entrez les valeurs d'attribut que vous voulez utiliser.

  Vous pouvez choisir de créer plus d'un attribut d'audience, comme indiqué dans l'image suivante, en fonction de vos besoins.

### Création d'un **public**:

1. Cliquez sur **Créer un public**.
2. Entrez un nom et une description appropriés dans la fenêtre Nouveau public.
3. Sélectionnez un attribut, puis cliquez sur **Ajouter**.
4. Sélectionnez les options requises dans les attributs répertoriés.
5. Cliquez sur **Sauvegarder**.

Vous pouvez maintenant créer un engagement.

## Etape 5. Création d'un engagement

Un engagement est une instanciation d'une fonction avec des propriétés initialisées et qui se connecte à l'un des publics prédéfinis. Vous pouvez créer un engagement à l'aide de **Feature Control** ou de **In-App Messaging**.

### Activation de la fonction de contrôle de fonction

Grâce à cet engagement, le propriétaire d'une application peut contrôler la visibilité d'une fonction en l'activant ou en la désactivant lors de l'exécution. Une fonction peut être activée ou désactivée pour tous les utilisateurs d'une application ou pour un ensemble spécifique d'utilisateurs et d'appareils.

Les déploiements de fonction peuvent être planifiés et coordonnées en définissant une heure et date de début ou de fin. Vous pouvez également choisir un jour spécifique auquel une fonction définie doit être activée ou désactivée.

![gif animé](images/create_engagement.gif){: gif}

Procédez comme suit pour créer un engagement à l'aide de la fonction Feature Control :

1. Vous pouvez créer un engagement à l'aide de l'une des méthodes suivantes :
  - Cliquez sur **Engagements** dans le panneau de navigation.
  - Sélectionnez **Create Engagements** sur la nouvelle fonction que vous avez créée.
  - Dans le panneau de navigation, cliquez sur **Overview** > **Create New Engagement**.

  La fenêtre New Engagement s'ouvre.

2. Entrez un nom et une description pour le nouvel engagement. Assurez-vous de fournir un nom d'engagement unique, pas un nom déjà répertorié dans la liste Engagements.
  - Sélectionnez le type d'engagement**** **Feature Control**.
  - Pour effectuer une expérience contrôlée avec plusieurs variantes de la fonction, sélectionnez **A/B testing** sous  **Select Experimentation Type**. Cliquez sur **Next**.

3. Sélectionnez la fonction que vous avez créée. Vous pouvez également choisir d'ajouter et de définir des variantes que vous souhaiterez peut-être expérimenter. Cliquez sur **Next**.

4. Sélectionnez un public. Cliquez sur **Next**.

5. Définissez un déclencheur en choisissant l'heure et la date ou l'heure de **début** ou l'heure et une date ou une heure de **fin**. Cliquez sur **Sauvegarder**.

  La nouvel engagement apparaît maintenant dans la fenêtre des détails d'engagement.

Vous pouvez maintenant mesurer les [performances](/docs/services/app-launch/app_measure_performance.html#applaunch_type) de votre engagement.

### Activation de la fonction de messagerie dans les applications

Avec cet engagement, le propriétaire d'une application peut envoyer des notifications aux utilisateurs de l'application alors qu'ils utilisent activement l'application.

Des messages peuvent être ciblés pour tous les utilisateurs d'une application ou pour un ensemble spécifique d'utilisateurs et d'appareils. Le public visé reçoit une notification pour chaque message que vous soumettez au service.

Les messages dans l'application peuvent être planifiés par la définition d'une date et d'heure de début ou de fin. Vous pouvez également les planifier en fonction d'un événement. Ces messages sont plus personnalisés car ils reposent sur des données d'analyse relatives aux choix, aux interactions, à l'appareil, aux journaux d'application, etc, de l'utilisateur.

Voir les exemples de messages suivants dans les applications :

- Envoyer des messages personnalisés.
- Envoyer des messages aux utilisateurs qui ont désactivé les notifications push.
- Demander des commentaires ou engager les utilisateurs dans une conversation.
- Envoyer des messages pertinents qui correspondent à ce que l'utilisateur recherche.
- Interagir avec des clients actifs et fidèles.
- Informer les utilisateurs des mises à jour d'applications (lancement d'une nouvelle fonction).

![gif animé](images/in-app-engagement_animated.gif){: gif}

Procédez comme suit pour créer un engagement qui utilise l'option de messagerie :

1. Vous pouvez créer un engagement à l'aide de l'une des méthodes suivantes :
  - Cliquez sur **Engagements** dans le panneau de navigation.
  - Sélectionnez **Create Engagement** sur la nouvelle fonction que vous avez créée.
  - Dans le panneau de navigation, cliquez sur **Overview** > **Create New Engagement**.

  La fenêtre New Engagement s'ouvre.

2. Entrez un nom et une description pour le nouvel engagement. Assurez-vous de fournir un nom d'engagement unique, pas un nom déjà répertorié dans la liste Engagements.
  - **Sélectionnez le type d'engagement** **In-App Messaging**.
  - Pour procéder à une expérimentation contrôlée avec plusieurs variantes de la fonction de messagerie, sélectionnez **A/B testing** dans **Select Experimentation Type**. Cliquez sur **Next**.

3. Entrez les propriétés du message et cliquez sur **Next**.

4. **Sélectionnez un public** et le pourcentage du public que vous souhaitez atteindre. Cliquez sur **Next**.

5. Définissez un déclencheur en sélectionnant une **date et une heure de début/fin**.

6. Sélectionnez un **événement** et cliquez sur **Next**.

7. Mappez les éléments aux métriques que vous voulez mesurer. Sélectionnez l'élément et entrez les détails des métriques. Cliquez sur **Save**.

  Le nouvel engagement apparaît maintenant dans la fenêtre Engagement Details.

Vous pouvez maintenant mesurer les [performances](/docs/services/app-launch/app_measure_performance.html#applaunch_type) de votre engagement.

## Onglet Quick Links
{: #links notoc}

Consultez les liens suivants pour évaluer et comprendre les fonctions de {{site.data.keyword.engage_short}} :

 - Essayez le [service App Launch](https://console.bluemix.net/catalog/services/app-launch).
 - [Blogues et Vidéos](/docs/services/app-launch/relatedlinks.html#blogs-and-videos)
 - Pour plus d'informations, voir le [tutoriel d'initiation à App Launch](/docs/services/app-launch/index.html#gettingstartedtemplate).
