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
{:note:.deprecated}

# Développement sans serveur
{: #serverless}

Que signifie développement sans serveur ? Le modèle de développement sans serveur fait référence au développement d'application dans lequel la logique côté serveur s'exécute dans des conteneurs sans état qui sont déclenchés par des événements, éphémères (le temps d'une simple exécution), et entièrement gérés par un tiers. Dans ce paradigme, également appelé FaaS (Functions as a Service), le développeur fournit une fonction sans état qui peut être déclenchée et exécutée sans la mise à disposition ou la gestion explicite d'un serveur.

Si l'on fait abstraction de l'infrastructure et des canevas nécessaires à un développement côté serveur, l'architecture sans serveur permet aux développeurs de se concentrer sur la génération de leur application et l'écriture de code pour une meilleure réactivité en cas de modification des données.

L'offre FaaS d'IBM, [{{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/), s'efforce de fournir une expérience de développement transparente, côté serveur, sans que des connaissances côté serveur spécialisées soient nécessaires. Grâce à la technologie sans serveur, vous pouvez rapidement développer des solutions de back end évolutives qui répondent pratiquement à n'importe quelle demande en charge de travail sans qu'il soit nécessaire de mettre à disposition à l'avance des ressources. Pour les applications qui présentent des modèles de charge imprévisibles ou des temps d'arrêt serveur élevés, {{site.data.keyword.openwhisk_short}} peut être une excellente solution de cloud avec des meilleures performances, et son système de paiement à l'utilisation permet de réduire les coûts.

## Modifications architecturales
{: #comparison}

Pour vous aider à comprendre les avantages architecturaux du passage à une offre FaaS, nous avons comparé une architecture classique et une architectures FaaS en utilisant une application iOS simple qui est reliée à une base de données.

Dans une architecture plus classique, l'application iOS se décharge des tâches réseau importantes ou traite les données à distance sur un serveur centralisé, lequel est lui-même connecté par ses propres services ou options de stockage. Avec un système classique, la lourde charge est placée sur un serveur particulier qui gère l'authentification, le traitement des tâches intensives afin de réduire la pression exercée sur le client, et permet une synchronisation au sein de sa base utilisateur.

Une architecture sans serveur peut modifier cette structure afin qu'elle ressemble davantage à l'image suivante.

![](./images/Architecture.png) Figure 1. Architecture sans serveur

Au lieu de gérer l'intégralité de la logique de traitement et d'authentification au sein d'un seul serveur, une architecture sans serveur optimise les fonctions hautement évolutives qui encapsulent la plupart de la logique côté serveur et décharge une partie de cette logique sur le client (ainsi que des services externes).

Dans le schéma présenté, vous pouvez constater les éléments suivants :

1. Le client s'authentifie auprès d'un fournisseur d'identité, par exemple App ID.
2. Des appels sont effectués à l'API de back end FaaS y compris le jeton d'accès.
3. Le back end est implémenté avec {{site.data.keyword.openwhisk_short}}. Les actions sans serveur, qui sont exposées en tant qu'actions Web, attendant que le jeton soit envoyé dans l'en-tête de requête et vérifient sa validité (signature et date d'expiration) afin de permettre l'accès à l'API.
4. Lorsque le client soumet des données, les commentaires en retour sont stockés dans une base de données {{site.data.keyword.cloudant_short_notm}}.
5. Le texte des commentaires en retour est traité avec {{site.data.keyword.toneanalyzershort}}.
6. En fonction du résultat de l'analyse, une notification est renvoyée au client par {{site.data.keyword.mobilepushshort}}.
7. Le client reçoit la notification.

Dans un modèle purement sans serveur, le client prend souvent des responsabilités supplémentaires en raison de l'impossibilité de stocker l'état de l'utilisateur. Par exemple, dans le cas le cas présent, l'autorisation est traitée par le client et le service du fournisseur d'identité {{site.data.keyword.appid_short_notm}}.

Même si les architectures sans serveur ne sont pas toujours idéales, elles peuvent présenter des avantages considérables avec une équipe adaptée et de bonnes conditions d'utilisation. Pour en apprendre davantage, consultez des exemples plus spécifiques dans quelques-une des [cas d'utilisation](#use_cases) les plus courants.

## Avantages du développement sans serveur
{: #benefits}

### Coût réduit

L'externalisation des coûts en termes de délais et de frais qui sont liés à l'administration de système permet de réduire le coût global qui est inhérent aux serveurs de back end classiques. De plus, {{site.data.keyword.openwhisk_short}} se différencie des technologies informatiques classiques dans la mesure où vous ne payez que pour la durée qui est nécessaire à votre code pour répondre aux demandes, arrondi au 100 ms les plus proches. Il est ainsi possible de réaliser d'importantes économies par comparaison à d'autres technologies, comme les machines virtuelles et les conteneurs, qui ne sont pas réellement utilisées à 100% , et prennent de la mémoire sur le système de votre fournisseur de cloud.

### Haute disponibilité et évolutivité

L'architectures sans serveur garantit tout naturellement une évolutivité instantanée et une disponibilité quasiment constante.

### Vitesse et développement simplifié

En éliminant la nécessité de recourir à l'administration de système, et grâce à des interfaces simples pour le déploiement, le paradigme sans serveur accélère le développement d'application. Les développeurs peuvent ainsi générer rapidement des applis à l'aide de séquences d'actions qui s'exécutent en réaction à un environnement axé sur les événements.

## Exemples de cas d'utilisation
{: #use_cases}

### Back end mobile 
![](./images/cloud-functions-rest-api-trigger.png)

Mes développeurs mobiles peuvent aisément accéder à la logique côté serveur et externaliser les tâches de calcul importantes sur une plateforme cloud évolutive. Vous pouvez implémenter des fonctions dans des langages tels que Swift et consommer facilement les fonctions côté serveur à l'aide du logiciel SDK iOS sans qu'une expérience côté serveur soit nécessaire.

### Traitement des données

![](./images/cloud-functions-cloudant-trigger.png)

Vous pouvez exécuter du code chaque fois que les données sont mises à jour dans votre magasin de données à l'aide de déclencheurs intégrés. Vous pouvez aussi facilement automatiser les processus comme la normalisation audio, la rotation d'image, l'affûtage, la réduction du bruit, la génération de miniatures ou le transcodage vidéo à travers un modèle de programmation côté serveur opérationnel.

### Traitement de données cognitives 

Vous pouvez analyser les données dès qu'elles sont disponibles. Vous laissez ainsi votre fonction tirer parti de la puissance des services cognitifs, comme IBM Watson, pour la détection d'objet ou de personnes dans des images ou des vidéos.

### Tâches planifiées

Exécutez vos fonctions régulièrement et définissez des plannings conformément à une syntaxe de type cron afin d'indiquer à quel moment les actions doivent être exécutées.

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
