---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: mobile backend swift, mbaas swift, mbaas swift sdk, swift features, swift framework sdk

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# Fonctions back-end mobile sous forme de service
{: #mbaas}

Votre application peut utiliser des fonctionnalités externes grâce à l'une des deux approches suivantes :
* Utilisation directe des services depuis l'application. Ces services sont généralement hébergés au sein d'une plateforme MBaaS (Mobile Backend as a Service).
* Utilisation via un composant BFF (Backend For Frontend) hébergé, qui peut fournir la composition et le contrôle des services et ajouter de la logique de back-end pour l'application.

Généralement, l'ajout de fonctionnalités externes directement depuis votre application mobile via une approche de type MBaaS est plus simple. En effet, le nombre d'éléments et d'infrastructures à ajouter pour le premier ensemble de services est moindre. Cependant, cette méthode n'offre pas la souplesse et la portée optimales offertes par le déploiement d'un composant BFF.

L'un des avantages de la plateforme {{site.data.keyword.cloud_notm}}, c'est que vous n'avez pas à décider quoi que ce soit à l'avance. Tous les services fournis peuvent être utilisés directement depuis l'appareil mobile à l'aide des logiciels SDK basés sur Swift qui sont fournis. La plateforme {{site.data.keyword.cloud_notm}} prend également en charge l'écriture de sa logique de back-end dans Swift, via un modèle sans serveur avec {{site.data.keyword.openwhisk_short}} ou via des applications Swift côté serveur avec des infrastructures telles que Kitura. Avec cette fonctionnalité, lorsque vous voulez ajouter de la logique de back end via un modèle BFF, vous pouvez le faire dans Swift, puis vous pouvez choisir de migrer votre utilisation des logiciels SDK Swift depuis l'application vers le composant BFF. Par conséquent, vous pouvez passer progressivement d'une approche de type MBaaS à une approche BFF avec un minimum d'effort.
