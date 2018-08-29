---

copyright:
  years: 2018
lastupdated: "2018-08-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Ajout de services de back-end à votre appli avec un logiciel SDK généré
{: #sdk-cli}

Le plug-in de générateur de SDK {{site.data.keyword.IBM}} peut être installé dans l'interface CLI de [{{site.data.keyword.cloud_notm}}](/docs/cli/reference/bluemix_cli/get_started.html).

Avec le plug-in du générateur de SDK {{site.data.keyword.IBM_notm}}, vous pouvez aisément intégrer vos services de back-end à votre appli à l'aide d'un logiciel généré. Lorsqu'une modification d'API REST se produit, vous pouvez regénérer le logiciel SDK, puis remplacer l'ancien par une mise à niveau SDK transparente. Vous pouvez aussi intégrer l'interface de ligne de commande dans un pipeline DevOps et vous assurer que le logiciel SDK est toujours cohérent avec la spécification d'API chaque fois que l'application est générée.

La définition d'API REST doit être valide et hébergée sur un noeud final de serveur opérationnel de votre système. 

## Avant de commencer :
{: #prereqs}

Assurez-vous que vous respectez la configuration prérequise suivante :

* Vous disposez d'un compte [{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://bluemix.net){: new_window}.
* Vous disposez d'une définition d'API valide conforme à la spécification [Open API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.openapis.org/){: new_window}.

## Installation du plug-in de logiciel SDK
{: #installation}

1. [Installez l'interface CLI {{site.data.keyword.cloud_notm}}](/docs/cli/reference/bluemix_cli/get_started.html).

2. [Installez le plug-in du logiciel SDK](/docs/cli/sdk/index.html).
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## Génération de logiciel SDK
{: #commands}

Pour générer un logiciel SDK, saisissez :
```
ibmcloud sdk generate [arguments...] [command options]
```
{: codeblock}

### Arguments
{: #gen-args}

* **APP_NAME** - nom de l'application Cloud Foundry dans votre espace actuel.
* **OPENAPI_DOC_LOCATION** - URL ou chemin d'accès relatif au fichier de définition d'API REST brut JSON ou Yaml.
* **GENERATED_SDK_NAME** (facultatif) - nom du logiciel SDK généré.

### Options
{: #gen-options}

**Platform** (plateforme) :
  * `--ios` - Générer un logiciel SDK iOS Swift.
  * `--swift` - Générer un logiciel SDK de serveur Swift.
  * `--js` - Générer un logiciel SDK JavaScript.

**Location** (obligatoire) :

Indique le type de l'argument `OPENAPI_DOC_LOCATION`.

  * `-r` - URL distante.
  * `-f` - Nom de fichier.
  * `-a` - Appli qui s'exécute sur {{site.data.keyword.coud_notm}}
  * `-l` - URL d'emplacement.

**Facultatif** :
  * `--output "YOUR_RELATIVE_PATH"` - Place le logiciel SDK généré dans le répertoire qui est spécifié par `YOUR_RELATIVE_PATH` (les logiciels SDK existants sont remplacés le cas échéant).
  * `--unzip` - Extrait le logiciel SDK généré (remplace les données si des artefacts SDK existants sont présents).

### Utilisation
{: #gen-usage}

Pour générer un SDK depuis une application Cloud Foundry qui s'exécute dans {{site.data.keyword.cloud_notm}}, vous pouvez utiliser le nom de l'application en tant que paramètre dans l'interface de ligne de commande de l'application. La
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

Exécutez la commande suivante : `ibmcloud sdk validate [argument]`

### Arguments
{: #val-args}

* `NOM_APP` - nom de l'application Cloud Foundry dans votre espace actuel
* `EMPLACEMENT_DOC_OPENAPI` - URL ou chemin d'accès relatif au fichier de définition d'API REST brut JSON ou Yaml

### Utilisation
{: #val-usage}

Pour valider une spécification d'API d'une application Cloud Foundry qui s'exécute dans {{site.data.keyword.cloud_notm}}, vous pouvez utiliser le nom de l'application en tant que paramètre dans l'interface de ligne de commande de l'application.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

Pour valider un logiciel SDK depuis l'URL dans un document de spécification d'API ou un fichier JSON ou Yaml local, utilisez la commande suivante :
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

