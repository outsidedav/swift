---
copyright:
  years: 2017, 2018
lastupdated: "2018-11-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Intégration de services de back-end à votre application avec un logiciel SDK généré
{: #sdk-cli}

Le plug-in {{site.data.keyword.IBM}} SDK Generator peut être installé dans l'[interface de ligne de commande {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli/reference/bluemix_cli/index.html){: new_window}.

Ce plug-in {{site.data.keyword.IBM_notm}} SDK Generator s'intègre aux services de back-end de votre application avec un logiciel SDK généré. Lorsqu'un changement se produit au niveau d'une API REST, vous pouvez regénérer le kit de développement logiciel et remplacer l'ancien pour faciliter la mise à jour. Vous pouvez aussi intégrer l'interface de ligne de commande dans un pipeline DevOps et vous assurer que le SDK est toujours cohérent avec la spécification d'API chaque fois que l'application est générée.

La définition d'API REST doit être valide et hébergée sur un noeud final de serveur opérationnel de votre système.

## Avant de commencer
{: #prereqs}

Vérifiez les points suivants :

* Vous disposez d'un compte [{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://bluemix.net){: new_window}
* Vous disposez d'une définition d'API valide conforme à la spécification [Open API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.openapis.org/){: new_window}

## Installation du plug-in de logiciel SDK
{: #installation}

1. [Installez l'interface CLI {{site.data.keyword.Bluemix}}](/docs/cli/reference/bluemix_cli/get_started.html).

2. [Installez le plug-in ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli/reference/bluemix_cli/index.html#install_plug-in){: new_window}.

  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## Génération de logiciel SDK
{: #commands}

Générez un logiciel SDK en entrant : `ibmcloud sdk generate [arguments...] [command options]`

### Arguments
{: #gen-args}

* `NOM_APP` - nom de l'application Cloud Foundry dans votre espace actuel
* `OPENAPI_DOC_LOCATION` - URL ou chemin d'accès relatif au fichier de définition d'API REST brut JSON ou yaml
* `GENERATED_SDK_NAME` (facultatif) - nom du SDK généré

### Options
{: #gen-options}

* `PLATEFORME` (requis)
   * `--ios` - génère un logiciel SDK Swift iOS
   * `--swift` - génère un logiciel SDK Swift de serveur
   * `--js` - génère un logiciel SDK JavaScript
* `LOCATION` (requis) - spécifie le type pour `OPENAPI_DOC_LOCATION`
   * `-r` - adresse URL distante
   * `-f` - fichier
   * `-a` - application s'exécutant dans {{site.data.keyword.Bluemix_notm}}
   * `-l` - adresse URL de localhost
* `--output "YOUR_RELATIVE_PATH"` (facultatif) - place le logiciel SDK généré dans le répertoire spécifié par `YOUR_RELATIVE_PATH` (les logiciels SDK existants sont écrasés).
* `--unzip` (facultatif) - extrait le logiciel SDK généré (les logiciels SDK existants sont écrasés).

### Utilisation
{: #gen-usage}

Pour générer un SDK depuis une application Cloud Foundry qui s'exécute dans {{site.data.keyword.Bluemix_notm}}, vous pouvez utiliser le nom de l'application en tant que paramètre dans l'interface de ligne de commande de l'application. La
commande suivante utilise le nom de l'application comme nom de SDK `SDK_Name`.

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

Pour générer un SDK depuis une URL dans un fichier de définition Open API ou un fichier yaml ou JSON local, utilisez la commande suivante :

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}

## Validation des définitions d'API ouvertes
{: #validating}

Exécutez les commandes suivantes :
```
ibmcloud sdk validate [argument]
```
{: codeblock}

### Arguments
{: #val-args}

* `NOM_APP` - nom de l'application Cloud Foundry dans votre espace actuel
* `OPENAPI_DOC_LOCATION` - URL ou chemin d'accès relatif au fichier de définition d'API REST brut JSON ou yaml

### Utilisation
{: #val-usage}

Pour valider une spécification d'API d'une application Cloud Foundry qui s'exécute dans {{site.data.keyword.Bluemix_notm}}, vous pouvez utiliser le nom de l'application en tant que paramètre dans l'interface de ligne de commande de l'application.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

Pour valider un logiciel SDK depuis l'URL dans un document de spécification d'API ou un fichier JSON ou yaml local, utilisez la commande suivante :
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}
