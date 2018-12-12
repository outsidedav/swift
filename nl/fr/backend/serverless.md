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
{:note: .deprecated}
{:tip: .tip}

# Développement sans serveur
{: #serverless}

Que signifie développement sans serveur ? Le modèle de développement sans serveur fait référence au développement d'applications où la logique côté serveur est exécutée dans des conteneurs sans état. Les conteneurs sont déclenchés par des événements, ils sont éphémères (ils durent le temps d'une exécution) et sont entièrement gérés par un tiers. Dans ce paradigme, également connu sous le nom de Functions as a Service (FaaS), le développeur fournit une fonction sans état qui peut être déclenchée et exécutée sans créer ou gérer explicitement un serveur.

En supprimant l'infrastructure et les frameworks nécessaires au développement côté serveur, l'architecture sans serveur permet aux développeurs de se concentrer sur l'écriture de code pour une meilleure réactivité en cas de modification des données. 

L'offre FaaS d'IBM, [{{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/), est conçue pour fournir une expérience simple côté serveur, sans connaissance spécialisée en développement côté serveur. Grâce à la technologie sans serveur, vous pouvez rapidement développer des solutions de back-end extensibles pour répondre à pratiquement n'importe quelle demande de charge de travail sans avoir besoin de créer des ressources à l'avance. Pour les applications qui ont des modèles de charge imprévisibles ou des temps d'arrêt de serveur élevés, {{site.data.keyword.openwhisk_short}} peut être une excellente solution de cloud offrant des performances améliorées. En outre, son système de "paiement à l'utilisation" permet de réduire les coûts.

## Modifications architecturales
{: #comparison}

Pour vous aider à comprendre les avantages architecturaux du passage à une offre FaaS, nous avons comparé une architecture classique et une architecture FaaS à l'aide d'une application iOS simple reliée à une base de données.

Dans une architecture plus traditionnelle, l'application iOS déleste le réseau des tâches intensives, ou traite les données à distance sur un serveur centralisé, auquel elle est elle-même connectée par ses propres services ou options de stockage. Les charges lourdes sont traitées sur un serveur unique qui traite de nombreuses tâches intensives afin de minimiser le stress qui pèse sur le client et d'assurer une synchronisation dans l'ensemble de sa base d'utilisateurs.

Une architecture sans serveur peut modifier cette structure afin qu'elle ressemble davantage à l'image suivante.

![](./images/Architecture.png) Figure 1. Architecture sans serveur

Plutôt que de gérer toute la logique de traitement et d'authentification à l'intérieur d'un seul serveur, une architecture sans serveur utilise des fonctions qui encapsulent une grande partie de la logique côté serveur, et décharge une partie de la logique vers le client (et les services externes).

Dans le schéma présenté, vous pouvez constater les éléments suivants :

1. Le client s'authentifie auprès d'un fournisseur d'identité, par exemple App ID.
2. Des appels sont effectués à l'API de back end FaaS y compris le jeton d'accès.
3. Le back end est implémenté avec {{site.data.keyword.openwhisk_short}}. Les actions sans serveur sont exposées en tant qu'actions web, et s'attendent à ce que des jetons soient envoyés dans l'en-tête de la requête pour vérification (signature et date d'expiration) afin de donner accès à l'API en question.
4. Lorsque le client soumet des données, les commentaires en retour sont stockés dans une base de données {{site.data.keyword.cloudant_short_notm}}.
5. Le texte des commentaires en retour est traité avec {{site.data.keyword.toneanalyzershort}}.
6. En fonction du résultat de l'analyse, une notification est renvoyée au client par {{site.data.keyword.mobilepushshort}}.
7. Le client reçoit la notification.

Dans un modèle purement sans serveur, le client prend souvent des responsabilités supplémentaires en raison de l'impossibilité de stocker l'état de l'utilisateur. L'autorisation est traitée par le client et le service du fournisseur d'identité {{site.data.keyword.appid_short_notm}}.

Bien que les architectures sans serveur ne soient pas toujours idéales, elles peuvent apporter de nombreux avantages dans des conditions d'équipe et d'utilisation appropriées. Pour en apprendre davantage, consultez des exemples plus spécifiques dans quelques-une des [cas d'utilisation](#use_cases) les plus courants.

## Avantages du développement sans serveur
{: #benefits}

### Coût réduit

L'externalisation des coûts en termes de délais et de frais qui sont liés à l'administration de système permet de réduire le coût global qui est inhérent aux serveurs de back end classiques. De plus, {{site.data.keyword.openwhisk_short}} se différencie des technologies informatiques classiques dans la mesure où vous ne payez que pour la durée qui est nécessaire à votre code pour répondre aux demandes, arrondi au 100 ms les plus proches. Des économies considérables sont possibles lorsque vous comparez avec d'autres technologies comme les machines virtuelles et les conteneurs, qui ne sont pas réellement utilisés à 100 %, et qui occupent de la mémoire sur le système de votre fournisseur de cloud.

### Haute disponibilité et évolutivité

L'architectures sans serveur garantit tout naturellement une évolutivité instantanée et une disponibilité quasiment constante.

### Vitesse et développement simplifié

En éliminant la nécessité de recourir à l'administration de système, et grâce à des interfaces simples pour le déploiement, le paradigme sans serveur accélère le développement d'application. Les développeurs peuvent ainsi générer rapidement des applications à l'aide de séquences d'actions qui s'exécutent en réaction à un environnement axé sur les événements.

## Exemples de cas d'utilisation
{: #use_cases}

### Back end mobile
![](./images/cloud-functions-rest-api-trigger.png)

Les développeurs mobiles peuvent facilement accéder à la logique côté serveur et externaliser des tâches de calcul intensives sur une plate-forme cloud. Vous pouvez implémenter des fonctions dans des langages tels que Swift et consommer facilement les fonctions côté serveur à l'aide du logiciel SDK iOS sans qu'une expérience côté serveur soit nécessaire.

### Traitement des données

![](./images/cloud-functions-cloudant-trigger.png)

Vous pouvez exécuter du code chaque fois que des données sont mises à jour dans votre magasin de données grâce à des déclencheurs intégrés. Vous pouvez aussi facilement automatiser les processus comme la normalisation audio, la rotation d'image, l'affûtage, la réduction du bruit, la génération de miniatures ou le transcodage vidéo à travers un modèle de programmation côté serveur opérationnel.

### Traitement de données cognitives

Vous pouvez analyser les données dès qu'elles sont disponibles. Votre fonction peut tirer parti de services cognitifs puissants, comme IBM Watson, pour détecter des objets ou des personnes dans des images ou des vidéos.

### Tâches planifiées

Exécutez vos fonctions périodiquement et définissez des programmes qui suivent une syntaxe de type cron pour spécifier quand les actions doivent être exécutées.

## Référence d'API
{: #openwhisk_start_api notoc}

<!-- * [REST API Documentation](./openwhisk_reference.html#openwhisk_ref_restapi)-->
* [API REST](https://console.{DomainName}/apidocs/98)

## Liens connexes
{: #general notoc}

* [Découvrez {{site.data.keyword.openwhisk_short}}](http://www.ibm.com/cloud-computing/bluemix/openwhisk/)
<!-- redirects to link above * [{{site.data.keyword.openwhisk_short}} on IBM developerWorks](https://developer.ibm.com/openwhisk/)-->
* [Site Web du projet Apache OpenWhisk](http://openwhisk.org)
* [Infos supplémentaire sur le développement sans serveur](https://martinfowler.com/articles/serverless.html)
