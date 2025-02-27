---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: swift sdk plug-in, sdk generator swift, generated sdk swift, devops pipeline swift, open api swift, sdkgen swift, ibmcloud sdk swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Ajout de services de back-end à votre application avec un logiciel SDK généré
{: #sdk-cli}

Le plug-in {{site.data.keyword.IBM}} SDK Generator peut être installé dans l'[interface de ligne de commande {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

Avec le plug-in {{site.data.keyword.IBM_notm}} SDK Generator, vous pouvez intégrer vos services de back-end à votre application à l'aide d'un logiciel généré. Lorsqu'un changement se produit au niveau d'une API REST, vous pouvez re-générer le logiciel SDK et remplacer l'ancien pour faciliter la mise à jour. Vous pouvez ensuite ajouter l'interface de ligne de commande dans un pipeline DevOps et vous assurer que le logiciel SDK est toujours cohérent avec la spécification d'API chaque fois que l'application est générée.

La définition d'API REST doit être valide et hébergée sur un noeud final de serveur opérationnel de votre système.

## Avant de commencer
{: #prereqs-sdk-cli}

Assurez-vous que la configuration prérequise suivante est respectée :

* Vous disposez d'un compte [{{site.data.keyword.cloud_notm}}](http://cloud.ibm.com){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").
* Vous disposez d'une définition d'API valide conforme à la spécification [Open API ](https://www.openapis.org/){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").

## Installation du plug-in de logiciel SDK
{: #install-sdk-cli}

1. [Installez l'interface de ligne de commande {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

2. Installez le plug-in du logiciel SDK :
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

Vous pouvez installer les outils [{{site.data.keyword.dev_cli_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#install_plug-in), qui incluent l'interface de ligne de commande {{site.data.keyword.cloud_notm}} de base, ainsi que le plug-in `sdk-gen`, entre autres outils locaux utiles.
{: tip}

## Génération de logiciel SDK
{: #commands-sdk-cli}

Pour générer un logiciel SDK, saisissez :
```
ibmcloud sdk generate [arguments...] [command options]
```
{: codeblock}

### Arguments
{: #gen-args-sdk-cli}

* **APP_NAME** - nom de l'application Cloud Foundry dans votre espace actuel.
* **OPENAPI_DOC_LOCATION** - URL ou chemin d'accès relatif au fichier de définition d'API REST brut JSON ou yaml.
* **GENERATED_SDK_NAME** (facultatif) - nom du logiciel SDK généré.

### Options
{: #gen-options-sdk-cli}

**Platform** (plateforme) :
  * `--ios` - Générer un logiciel SDK Swift iOS.
  * `--swift` - Générer un logiciel SDK de serveur Swift.
  * `--js` - Générer un logiciel SDK JavaScript.

**Location** (obligatoire) :

Indique le type de l'argument `OPENAPI_DOC_LOCATION`.

  * `-r` - URL distante.
  * `-f` - Nom de fichier.
  * `-a` - Application qui s'exécute sur {{site.data.keyword.cloud_notm}}
  * `-l` - URL d'emplacement.

**Facultatif** :
  * `--output "YOUR_RELATIVE_PATH"` - Place le logiciel SDK généré dans le répertoire qui est spécifié par `YOUR_RELATIVE_PATH` (les logiciels SDK existants sont remplacés le cas échéant).
  * `--unzip` - Extrait le logiciel SDK généré (écrase les données existantes si des artefacts SDK sont présents).

### Utilisation
{: #gen-usage-sdk-cli}

Pour générer un SDK depuis une application Cloud Foundry qui s'exécute dans {{site.data.keyword.cloud_notm}}, vous pouvez utiliser le nom de l'application en tant que paramètre dans l'interface de ligne de commande de l'application. La
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
{: #validating-sdk-cli}

Exécutez la commande suivante : `ibmcloud sdk validate [argument]`

### Arguments
{: #val-args-sdk-cli}

* `APP_NAME` - nom de l'application Cloud Foundry dans votre espace actuel.
* `OPENAPI_DOC_LOCATION` - URL ou chemin d'accès relatif au fichier de définition d'API REST brut JSON ou yaml.

### Utilisation
{: #val-usage-sdk-cli}

Pour valider une spécification d'API d'une application Cloud Foundry qui s'exécute dans {{site.data.keyword.cloud_notm}}, vous pouvez utiliser le nom de l'application en tant que paramètre dans l'interface de ligne de commande de l'application.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

Pour valider un logiciel SDK depuis l'URL dans un document de spécification d'API ou un fichier JSON ou yaml local, utilisez la commande suivante :
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

