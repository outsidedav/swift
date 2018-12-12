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

# Développement en local

En développant en local, vous pouvez facilement créer, exécuter et tester des applications Swift. Vous utilisez {{site.data.keyword.dev_cli_short}} pour exécuter ces actions à l'aide de commandes standard. 

Vous pouvez utiliser {{site.data.keyword.dev_cli_short}} pour gérer vos applications côté serveur avec plus d'une douzaine de commandes. Pour en savoir plus sur les différentes commandes, consultez [IBM Cloud Developer Tools CLI `ibmcloud dev` commands](/docs/cli/idt/commands.html).

## Avant de commencer

Pour développer en local, vous devez installer {{site.data.keyword.dev_cli_notm}}. Exécutez la commande suivante pour exécuter le script d'installation :
```
curl -sL https://ibm.biz/idt-installer | bash
```
{: codeblock}

Voir [Configuration de l'interface de ligne de commande IBM Cloud Developer Tools](/docs/cli/idt/setting_up_idt.html) pour en savoir plus sur la configuration et l'utilisation d'{{site.data.keyword.dev_cli_notm}}.

## Extraction des données d'identification de service

Après avoir cloné votre application dans Git, vous devez extraire les données d'identification des services liés à votre application, car ils ne sont pas stockés dans le référentiel Git de votre application. L'extraction de données d'identification permet l'utilisation de services liés. Vous pouvez télécharger facilement les données d'identification en exécutant la commande suivante à la racine du répertoire de l'application :
```
ibmcloud dev get-credentials
```
{: codeblock}

## Génération, exécution et déploiement de votre application 

1. **Génération** - Vous pouvez désormais générer votre application, laquelle est prérequis à l'exécution de celle-ci.
  Utilisez la commande suivante à la racine du répertoire de l'application pour générer votre application :
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **Exécution** - Après une génération réussie, vous pouvez exécuter votre application dans un conteneur local à l'aide de la commande suivante :
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  Un hôte et un port locaux pour l'affichage de la page d'arrivée de votre application s'affiche si la commande aboutit.

3. **Déploiement** - Déployez votre application dans {{site.data.keyword.Bluemix_notm}} à l'aide de la commande `deploy` :
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}
