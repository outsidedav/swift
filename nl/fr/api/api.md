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
{:tip: .tip}

# Ajout d'API aux applications iOS
{: #api_connect}

Vous pouvez utiliser API Connect pour gérer les API dans {{site.data.keyword.cloud}}, que celles-ci soient conservées à l'intérieur ou à l'extérieur d'{{site.data.keyword.cloud_notm}}. Apprenez à gérer vos API afin de pouvoir contrôler l'utilisation, accroître l'adoption et suivre les statistiques.

## Création d'une instance d'API Connect

Accédez au [catalogue](https://console.bluemix.net/catalog/) et créez une instance d'API Connect pour gérer vos API.

Utilisez `Menu->API` pour accéder à la console de gestion API Connect.

![API Connect](images/apiconnect.png)

Si vous définissez votre propre contrat d'API avant de commencer le développement back-end et front-end, utilisez les outils d'API Connect pour accélérer ce processus. Vous pouvez travailler avec votre équipe de développement numérique pour générer et définir un contrat d'API entre votre application iOS et votre logique de back-end Cette logique peut être distribuée à l'aide de [{{site.data.keyword.openwhisk}}](/docs/openwhisk/index.html) ou de l'[environnement d'exécution Swift](/docs/runtimes/swift/index.html) avec Kubernetes ou [Cloud Foundry](/docs/cloud-foundry/index.html).

Une fois votre API définie, vous pouvez définir des spécifications d'API ouvertes (Swagger) avec un certain nombre d'outils :

- [Editeur Swagger](http://editor.swagger.io/)
- [Concepteur d'API](https://www.ibm.com/support/knowledgecenter/en/SSFS6T/com.ibm.apic.toolkit.doc/task_apionprem_composing_apis.html)
- [LoopBack](https://loopback.io/)

## Définition de votre API gérée

Vous pouvez définir un proxy d'API qui gère la passerelle d'API entre votre application client et votre logique de back-end. Procédez comme suit pour créer un proxy à l'aide de votre spécification d'API ouverte (document Swagger) YAML ou JSON. 

1. Ouvrez la console `Menu -> APIs` et cliquez sur le proxy d'API.
2. Cliquez sur **Définition d'API > Importer YAML ou JSON**.
3. Sélectionnez le fichier YAML ou JSON que vous avez préalablement créé.
4. Sauvegardez et exposez.

Vous devez configurer le noeud final externe afin qu'il pointe sur l'URL qui relie à votre application de logique de back-end. 

## Création d'un back-end Swift

Vous pouvez créer votre application Swift de back-end sur la base de cette API. 

Dans la console de développement Apple, procédez comme suit :

1. Sélectionnez **Kits de démarrage**.
2. Cliquez sur **Créer une application**.
3. Sélectionnez **Swift** comme langage.

Sélectionnez le fichier YAML et JSON, puis cliquez sur **Créer**. L'application Swift de back-end est créée.

Vous pouvez ensuite **télécharger** le Code ou **Déployer sur le Cloud**, puis cloner votre référentiel GIT sur votre machine locale. Vous pouvez suivre les instructions dans le guide des connaissances pour ouvrir l'application côté serveur dans Xcode.

Dans le dossier **Source**, vous pouvez voir une route qui définit le fichier Swift qui a créé les points finaux REST qui mappent à l'API. 

L'exemple suivant utilise l'API Open `PetStore` :
```swift
import Kitura
import KituraContracts

func initializePet_Routes(app: App) {
    app.router.post("\(basePath)/pet") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.put("\(basePath)/pet") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.get("\(basePath)/pet/findByStatus") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.get("\(basePath)/pet/findByTags") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.get("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.post("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.delete("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next()
    }

    app.router.post("\(basePath)/pet/:petId/uploadImage") { request, response, next in
        response.send(json: [:])
        next()
    }
}
```
{: codeblock}

Une fois l'API définie à l'aide de {{site.data.keyword.openwhisk_short}} ou d'un environnement d'exécution Swift de pile complète, et une fois la définition API Connect créée, vous pouvez consommer l'API dans vos applications iOS.

## Consommation de l'API dans une application mobile iOS

Pour consommer l'API de back-end dans votre application iOS, créez un kit de démarrage mobile à l'aide de Apple Console. A partir de la vue du kit de démarrage, créez un kit de démarrage iOS de n'importe quel type.

Cliquez sur **Ajouter une ressource** et sélectionnez une API. 

![Boîte de dialogue d'API](../images/apidialog.png)

L'API est ajoutée à votre application iOS. Si vous *téléchargez* le code de l'application, vous pouvez voir un dossier qui figure dans les dossiers iOS Source et qui est nommé après l'API.

Pour une `mise à jour du pod` des logiciels SDK dépendants dans votre application iOS, suivez les instructions du Guide des connaissances. 

L'application iOS inclut un dossier avec la liaison SDK générée pour l'API. Ce dossier inclut les trois sous-dossiers suivants `Assets`,`Source` et `Docs`. 

![Dossier iOS](../images/sdkfolder.png)

Dans le dossier `Assets`, il existe un fichier qui gère l'URL dans votre API, qui est `localhost:3000` par défaut. Vous devez modifier la valeur qui fait référence à la route d'API. La définition d'API contient une section contenant le nom d'API et le chemin. Cliquez sur **Copier** à la fin du chemin pour copier l'URL. Vérifiez que l'option *Expose Managed API* est activée afin de permettre aux clients externes d'effectuer des appels d'API.

![Route d'API](../images/apiroute.png)  

Ouvrez le fichier `PLIST` et remplacez la valeur hôte par la valeur qui est copiée de la route d'API route qui permet au logiciel SDK d'appeler l'API dans {{site.data.keyword.cloud_notm}}.

## Documentation

Lorsque le logiciel SDK est inclus dans votre projet d'application iOS, un fichier *README.html* est disponible dans le dossier `Docs`. Ouvrez le dossier `Docs` dans un navigateur externe et lisez les instructions qui décrivent comment utiliser votre projet.

## Recréation du logiciel SDK après un changement d'API

Si l'API change ou si de nouvelles fonctions sont disponible et {{site.data.keyword.openwhisk}} est ajouté, vous pouvez re-créer le SDK client à l'aide de la commande `ibmcloud sdk`. Pour plus d'informations, des exemples et l'aide sur la syntaxe, consultez la documentation [SDK Generator](/docs/cli/sdk/index.html).

Pour activer la création d'un logiciel SDK, utilisez le fichier YAML ou JSON de la spécification d'API ouverte (Swagger). Vous pouvez extraire ce fichier à l'aide des utilitaires de gestion d'API dans l'{{site.data.keyword.cloud_notm}}. 

1. Accédez à `Menu -> API -> API gérées`.
2. Sélectionnez l'API que vous voulez extraire depuis la dernière spécification d'API ouverte. 
3. Ensuite, sélectionnez le menu **Explorateur**.

![Explorateur d'API](../images/downloadyaml.png)

4. Sélectionnez l'icône de téléchargement afin de télécharger le fichier yaml de l'API et de sauvegarder ce fichier dans votre répertoire de projet d'application iOS.

5. L'étape suivante consiste à exécuter la commande d'interface CLI `ibmcloud sdk`. 
    ```
    ibmcloud sdk generate --ios --unzip --output ./MyAppFunctions -f ./mobile-bff-functions-1.0.0.yaml SDKMyFunctions
    ```
    {: codeblock}

    Le logiciel SDK est recréé dans le répertoire de votre projet d'application iOS pour que vous puissiez continuer de travailler avec votre API.

## Référence

L'exemple de logiciel SDK suivant est créé pour {{site.data.keyword.openwhisk_short}} à partir du kit de démarrage. Vous pouvez voir chacune des actions et les fragments de code Swift que vous pouvez inclure dans votre application iOS.

### Méthodes d'API par défaut
 * [`getCreate`](#getCreate)
 * [`getDelete`](#getDelete)
 * [`getDeleteall`](#getDeleteall)
 * [`getRead`](#getRead)
 * [`getReadall`](#getReadall)
 * [`getUpdate`](#getUpdate)

### Utilisation de `getCreate`
{: #getCreate}

```swift
public static func getCreate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Paramètres de `getCreate`

- **completionHandler** (obligatoire)
    - Closure prend comme arguments `Response?` et `Error?`.

### Authentification avec `getCreate`

Aucune authentification requise

### Exemple avec `getCreate`
```swift
DefaultAPI.getCreate() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### Utilisation de `getDelete`
{: #getDelete}

```swift
public static func getDelete(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Paramètres de `getDelete`

- **completionHandler** (obligatoire)
    - Closure prend comme arguments `Response?` et `Error?`.

### Authentification avec `getDelete`

Aucune authentification requise

### Exemple avec `getDelete`
```swift
DefaultAPI.getDelete() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### Utilisation de `getDeleteall`
{: #getDeleteall}

```swift
public static func getDeleteall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Paramètres de `getDeleteall`

- **completionHandler** (obligatoire)
    - Closure prend comme arguments `Response?` et `Error?`.

### Authentification avec `getDeleteall`

Aucune authentification requise

### Exemple avec `getDeleteall`

```swift
DefaultAPI.getDeleteall() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### Utilisation de `getRead`
{: #getRead}

```swift
public static func getRead(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Paramètres de `getRead`

- **completionHandler** (obligatoire)
    - Closure prend comme arguments `Response?` et `Error?`.

### Authentification avec `getRead`

Aucune authentification requise

### Exemple avec `getRead`
```swift
DefaultAPI.getRead() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### Utilisation de `getReadall`
{: #getReadall}

```swift
public static func getReadall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Paramètres de `getReadall`

- **completionHandler** (obligatoire)
    - Closure prend comme arguments `Response?` et `Error?`.

### Authentification avec `getReadall`

Aucune authentification requise

### Exemple avec `getReadall`
```swift
DefaultAPI.getReadall() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

### Utilisation de `getUpdate`
{: #getUpdate}

```swift
public static func getUpdate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void) -> Void
```
{: codeblock}

#### Paramètres de `getUpdate`

- **completionHandler** (obligatoire)
    - Closure prend comme arguments `Response?` et `Error?`.

### Authentification avec `getUpdate`

Aucune authentification requise

### Exemple avec `getUpdate`
```swift
DefaultAPI.getUpdate() { (response, error) in
    guard error == nil else {
        print(error!)
        return
    }
    if let status = response?.statusCode {
        switch status {
        case 0:
            print("Default response")
        default:
            print("Response: \(response?.responseText)")
        }
    }
}
```
{: codeblock}

