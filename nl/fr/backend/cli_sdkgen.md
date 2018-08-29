---
copyright:
  years: 2017, 2018
lastupdated: "2018-08-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Intégration de services de back-end à votre appli avec un logiciel SDK généré
{: #sdk-cli}

Le plug-in de générateur de SDK {{site.data.keyword.IBM}} peut être installé dans l'[interface de ligne de commande {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli/reference/bluemix_cli/index.html){: new_window}.

Ce plug-in {{site.data.keyword.IBM_notm}} SDK Generator s'intègre aux services de back-end de votre appli avec un logiciel SDK généré. Lorsqu'une modification d'API REST se produit, vous pouvez regénérer le logiciel SDK, puis remplacer l'ancien par une mise à niveau SDK transparente. Vous pouvez aussi intégrer l'interface de ligne de commande dans un pipeline DevOps et vous assurer que le logiciel SDK est toujours cohérent avec la spécification d'API chaque fois que l'application est générée.

La définition d'API REST doit être valide et hébergée sur un noeud final de serveur opérationnel de votre système.

## Avant de commencer
{: #prereqs}

Vérifiez les points suivants :

* Vous disposez d'un compte [{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://bluemix.net){: new_window}
* Vous disposez d'une définition d'API valide conforme à la spécification [Open API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.openapis.org/){: new_window}

## Installation du plug-in de logiciel SDK
{: #installation}

1. [Installez l'interface CLI {{site.data.keyword.Bluemix}}](/docs/cli/reference/bluemix_cli/get_started.html).

2. [Installez le plug-in ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/cli/reference/bluemix_cli/index.html#install_plug-in){: new_window}.

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
* `EMPLACEMENT_DOC_OPENAPI` - URL ou chemin d'accès relatif au fichier de définition d'API REST brut JSON ou Yaml
* `GENERATED_SDK_NAME` (facultatif) - nom du SDK généré

### Options
{: #gen-options}

* `PLATEFORME` (requis)
   * `--ios` - génère un SDK Swift iOS
   * `--swift` - génère un SDK de serveur Swift 
   * `--js` - génère un SDK JavaScript
* `LOCATION` (requis) - spécifie le type pour `OPENAPI_DOC_LOCATION`
   * `-r` - adresse URL distante
   * `-f` - fichier
   * `-a` - application s'exécutant dans {{site.data.keyword.Bluemix_notm}}
   * `-l` - adresse URL de localhost
* `--output "YOUR_RELATIVE_PATH"` (facultatif) - place le logiciel SDK généré dans le répertoire qui est spécifié par `YOUR_RELATIVE_PATH` (Si un logiciel SDK existant est présent, il est remplacé.)
* `--unzip` (facultatif) - extrait le logiciel SDK généré (Si des artefacts de logiciel SDK existants sont présents, ils sont remplacés.)

### Utilisation
{: #gen-usage}

Pour générer un SDK depuis une application Cloud Foundry qui s'exécute dans {{site.data.keyword.Bluemix_notm}}, vous pouvez utiliser le nom de l'application en tant que paramètre dans l'interface de ligne de commande de l'application. La
commande suivante utilise le nom de l'application comme nom de SDK `SDK_Name`.

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

Pour générer un SDK depuis une URL dans un fichier de définition Open API ou un fichier Yaml ou JSON local, utilisez la commande suivante :

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
* `EMPLACEMENT_DOC_OPENAPI` - URL ou chemin d'accès relatif au fichier de définition d'API REST brut JSON ou Yaml

### Utilisation
{: #val-usage}

Pour valider une spécification d'API d'une application Cloud Foundry qui s'exécute dans {{site.data.keyword.Bluemix_notm}}, vous pouvez utiliser le nom de l'application en tant que paramètre dans l'interface de ligne de commande de l'application.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

Pour valider un logiciel SDK depuis l'URL dans un document de spécification d'API ou un fichier JSON ou Yaml local, utilisez la commande suivante :
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}
