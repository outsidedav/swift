---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: swift-cfenv, service bindings swift, environment swift, swift configuration, cloudenvironment swift, VCAP_SERVICES swift, swift credentials

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuration de l'environnement Swift
{: #configuration}

Le développement pour le cloud comporte deux pratiques étroitement liées qui se chevauchent dans la manière dont vous gérez les données de configuration. La première consiste à devoir générer des artefacts non modifiables afin de minimiser la probabilité que des erreurs soient introduites lorsque votre application passe du développement en production. La seconde indique que vous devez atteindre la parité entre vos environnements de développement et de production afin d'éviter les problèmes créés par le code spécifique à l'environnement. 

La gestion de la configuration de service et des données d'identification (liaisons de service) varie entre les plateformes. Cloud Foundry stocke des détails de liaison de service dans un objet JSON converti sous forme de chaîne qui est transmis à l'application en tant que variable d'environnement `VCAP_SERVICES`. Kubernetes stocke les liaisons de service sous forme d'attributs à plat ou JSON représentés sous forme de chaîne dans `ConfigMaps` ou `Secrets`, lesquels peuvent être transmis à l'application conteneurisée en tant que variables d'environnement ou montés en tant que volume temporaire. Le développement local a sa propre configuration, car les tests locaux sont souvent un dérivé simplifié de tout ce qui s'exécute dans le cloud. Il peut être difficile d'alterner entre ces différentes variations de manière portable sans disposer de chemins de code spécifiques à un environnement.

Vous pouvez suivre de simples instructions qui vous aideront à écrire des applications portables, et les utilitaires que vous pouvez utiliser pour encapsuler la recherche de liaisons de service (ou autre configuration) dans des emplacements spécifiques à un environnement. Qu'il s'agisse d'ajouter une prise en charge de cloud à des applications existantes ou de créer des applications avec des kits de démarrage, l'objectif est de permettre une portabilité pour que les applications Swift puissent être utilisées sur un grand nombre de plateformes de déploiement.

## Ajout d'{{site.data.keyword.cloud_notm}} à des applications Swift existantes
{: #addcloud-env}

Le processus qui permet d'extraire les valeurs de l'environnement peut différer d'un environnement cloud à l'autre. La bibliothèque [CloudEnvironment](https://github.com/IBM-Swift/CloudEnvironment.git){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") isole la configuration de l'environnement et les informations d'identification de divers fournisseurs de cloud afin que votre application Swift puisse accéder de manière cohérente aux informations en exécutant localement ou dans des instances Cloud Foundry, Cloud Foundry Enterprise Environment, Kubernetes, {{site.data.keyword.openwhisk}} ou virtuelles. Le détournement des données d'identification est effectué par la bibliothèque `CloudEnvironment`, laquelle utilise en interne [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") pour la configuration Cloud Foundry et [Configuration](https://github.com/IBM-Swift/Configuration){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") comme gestionnaire de configuration.

Avec `CloudEnvironment`, vous pouvez extraire les détails de faible niveau du code source de votre application en définissant une clé de recherche que votre application Swift peut utiliser pour la recherche de sa valeur correspondante.

La bibliothèque `CloudEnvironment` fournit une clé de recherche cohérente qui peut être utilisée dans votre code source. La bibliothèque effectue ensuite des recherches dans un tableau de modèles de recherche pour trouver un objet JSON contenant les attributs de configuration ou les données d'identification du service. 

### Ajout du package CloudEnvironment à votre application Swift
{: #add-cloudenv}

Pour utiliser le package `CloudEnvironment` dans votre application Swift, indiquez-le dans la section **dependencies:** de votre fichier `Package.swift` :
```swift
.package(url: "https://github.com/IBM-Swift/CloudEnvironment.git", from: "8.0.0"),
```
{: codeblock}

Ensuite, ajoutez le code d'instrumentation suivant à votre application :
```swift
import CloudEnvironment

let cloudEnv = CloudEnv()
```
{: codeblock}

### Accès aux données d'identification
{: #access-credentials}

Maintenant que la bibliothèque `CloudEnvironment` est initialisée, vous pouvez accéder à vos données d'identification comme illustré dans les exemples suivants :
```swift
let cloudantCredentials = cloudEnv.getCloudantCredentials(name: "cloudant-credentials")
// cloudantCredentials.username, cloudantCredentials.password, cloudantCredentials.url, etc.
let objStorageCredentials = cloudEnv.getObjectStorageCredentials(name: "object-storage-credentials")
// objStorageCredentials.username, objStorageCredentials.password, objStorageCredentials.projectID, etc.

let service1Credentials = cloudEnv.getDictionary("service1-credentials")
let service1CredentialsStr = cloudEnv.getString("service1-credentials")

// Get a default PORT and URL
let port = cloudEnv.port
let url = cloudEnv.url
```
{: codeblock}

Cet exemple permet d'accéder aux jeux de données d'identification pour les services, qui peuvent maintenant être utilisés pour initialiser les connexions à ces [services pris en charge ou à un dictionnaire générique](https://github.com/IBM-Swift/CloudEnvironment#supported-services){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"). Consultez la configuration spécifique à [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") pour Cloud Foundry, ainsi que les [détails de configuration](https://github.com/IBM-Swift/Configuration){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") sur le chargement des données de configuration.

## Présentation des données d'identification de service
{: #service_creds}

La bibliothèque `CloudEnvironment` utilise un fichier nommé `mappings.json`, qui se trouve dans le répertoire `config`, pour communiquer l'emplacement de stockage des données d'identification pour chaque service. Le fichier `mappings.json` prend en charge la recherche de valeurs au moyen des trois modèles de recherche suivants :
- **`cloudfoundry`** - Type de pattern utilisé pour la recherche d'une valeur dans la variable d'environnement des services de Foundry (`VCAP_SERVICES`). Pour Cloud Foundry Enterprise Edition, voir ce [tutoriel d'initiation](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started) pour de plus amples informations.
- **`env`** - Type de pattern utilisé pour la recherche d'une valeur qui est mappée à une variable d'environnement, comme dans Kubernetes ou Functions.
- **`file`** - Type de pattern utilisé pour la recherche d'une valeur dans un fichier JSON. Le chemin doit être relatif au dossier racine de votre application Swift.

Une recherche est effectuée dans le tableau des valeurs associées à une clé de recherche dans leur ordre d'apparition.

L'exemple suivant illustre un fichier `mappings.json` :
```javascript
{
    "cloudant-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-cloudant-db",
                "env:my_awesome_cloudant_db_credentials",
                "file:localdev/my-awesome-cloudant-db-credentials.json"
            ]
        }
    },
    "object-storage-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-object-storage",
                "env:my_awesome_object_storage_credentials",
                "file:localdev/my-awesome-object-storage-credentials.json"
            ]
        }
    }
}
```
{: codeblock}

Dans cet exemple, `cloudant-credentials` et `object-storage-credentials` sont les clés de recherche utilisées par votre application Swift pour rechercher les données d'identification correspondantes. Conformément au tableau des modèles de recherche, le gestionnaire de configuration recherche d'abord `my-awesome-cloudant-db` dans `VCAP_SERVICES`, puis il recherche `my_awesome_cloudant_db_credentials` en tant que variable d'environnement. Si aucune définition n'existe, il recherche le contenu de `my-awesome-object-storage-credentials.json` dans le dossier `localdev`. 

Lorsque l'application s'exécute en local, elle peut utiliser des données d'identification stockées dans un fichier, par exemple `localdev/my-awesome-object-storage-credentials.json` comme illustré dans l'exemple précédent. Les informations de connexion pour les services auxquels vous voulez accéder en local, comme le nom d'utilisateur, le mot de passe et le nom d'hôte, sont tous stockés dans ce fichier. 

Pour des raisons de sécurité, les fichiers de données d'identification ne doivent pas se trouver dans des référentiels. Dans l'exemple précédent, un dossier `localdev` est utilisé pour stocker les données d'identification locales, vous devez donc ajouter ce dossier dans le fichier `.gitignore` afin d'éviter toute validation accidentelle. Si vous utilisez une région propriétaire de fichier de kit de démarrage, ce dossier est créé pour vous, et il se trouve dans le fichier `.gitignore`.

Pour plus d'informations sur le fichier `mappings.json`, consultez la section [Présentation des données d'identification de service](#service_creds).

## Utilisation du gestionnaire de configuration Swift depuis les applications du kit de démarrage
{: #configmanager-swift}

Les applications Swift qui sont créées avec les [kits de démarrage](https://cloud.ibm.com/developer/appledevelopment/starter-kits/){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") sont fournies automatiquement avec les données d'identification et la configuration nécessaire à une exécution en local, ainsi que dans de nombreux environnements de déploiement cloud, tels que Cloud Foundry, Kubernetes, VSI et Functions. La création de base du gestionnaire de configuration se trouve dans `Sources/Application/Application.swift`. Lorsque vous créez un application du kit de démarrage basée sur Swift avec des services, un dossier `config` et un fichier `mappings.json` sont créés pour vous. Si vous téléchargez l'application, le dossier `config` contient un fichier `localdev-config.json` qui comporte toutes les données d'identification de vos services, et est présent dans le fichier `.gitignore`.

## Etapes suivantes
{: #next-configß notoc}

Consultez les trois bibliothèques pour permettre le masquage de vos applications dans leurs environnements :

* [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")
* [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")
* [Configuration](https://github.com/IBM-Swift/Configuration){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")
