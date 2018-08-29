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

# Composants MBaaS et back-end
{: #mbaas}

Votre application peut utiliser des fonctionnalités externes au moyen de l'une des deux approches suivantes :
* Utilisation directe des services depuis l'application, lesquels services sont généralement hébergés au sein d'une plateforme MBaaS (Mobile Backend as a Service).
* Utilisation via un composant BFF (Backend For Frontend) hébergé, qui peut fournir la composition et le contrôle des services et ajouter de la logique de back end pour l'application.

Généralement, l'ajout de fonctionnalités externes directement depuis votre application mobile via une approche de type MBaaS est plus simple. Très peu de composants et d'infrastructures doivent être ajoutés au premier ensemble de services. Toutefois, cette méthode n'offre pas la souplesse ultime et la portée possible via le déploiement d'un composant BFF.

L'un des avantages de la plateforme {{site.data.keyword.cloud_notm}}, c'est que vous n'avez pas à décider d'avance. Tous les services fournis peuvent être utilisés directement depuis l'appareil mobile à l'aide des logiciels SDK basés sur Swift qui sont fournis. La plateforme {{site.data.keyword.cloud_notm}} prend en également en charge l'écriture de sa logique de back end en Swift, via un modèle sans serveur avec {{site.data.keyword.openwhisk_short}} ou via des Swift côté serveur avec des infrastructures telles que Kitura. Avec cette fonctionnalité, lorsque vous voulez ajouter de la logique de back end via un modèle BFF, vous pouvez le faire dans Swift, puis vous pouvez choisir de migrer votre utilisation des logiciels SDK Swift de l'application vers le composant BFF. Par conséquent, vous pouvez passer de manière incrémentielle d'une approche de type MBaaS à une approche de type BFF à peu de frais en raison d'une prise en charge de bout en bout pour Swift.
