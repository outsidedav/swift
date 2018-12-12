---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:gif: .gif}
{:tip: .tip}

# Création d'une base de données hautement disponible et sécurisée

Pour tirer pleinement parti d'une base de données hautement disponible et sécurisée, vous imbriquez une logique supplémentaire dans votre application. A l'aide des fragments de code fournis, vous pouvez créer une base de données MongoDB et y accéder. 

Actuellement, le langage de programmation pris en charge pour l'utilisation de {{site.data.keyword.ihsdbaas_full}}
est Swift 4.0 avec MongoKitten SDK 4.0.0.

## Etape 1. Création d'un cluster de base de données
{: #create_dbcluster}

1. Accédez à l'écran de configuration de service {{site.data.keyword.ihsdbaas_full}} à l'adresse
https://console.bluemix.net/catalog/services/hyper-protect-dbaas.

2. Fournissez les informations suivantes :

	<dl>
		<dt>Nom du service :</dt>
		<dd>Nom pour le service de base de données.</dd>

    <dt>Sélectionnez une région/un emplacement où effectuer le déploiement :</dt>
    <dd>Sélectionnez le centre de données dans lequel se trouve la base de données.</dd>

    <dt>Sélectionnez un groupe de ressources :</dt>
    <dd>Si aucun groupe de ressources ne peut être sélectionné, vous pouvez en créer un dans le tableau de bord IBM Cloud.</dd>

		<dt>Nom de cluster / jeu de répliques :</dt>
		<dd>Nom pour le cluster de base de données.</dd>

		<dt>Nom de l'administrateur de base de données :</dt>
		<dd>ID utilisateur de l'administrateur de base de données (DBA).</dd>

		<dt>Mot de passe de l'administrateur de base de données :</dt>
		<dd>Mot de passe pour l'ID utilisateur DBA.</dd>

    <dt>Confirmation du mot de passe de l'administrateur de base de données :</dt>
    <dd>Confirmation du mot de passe pour l'ID utilisateur de l'administrateur de base de données.</dd>

		<dt>Type de base de données :</dt>
		<dd>Sélectionnez le type de base de données. Actuellement, seul le type MongoDB est pris en charge.</dd>

    <dt>Contrat de licence :</dt>
    <dd>Lisez le contrat de licence et cochez la case pour confirmer votre accord.</dd>

    <dt>Tarification :</dt>
		<dd>La solution actuelle ne fournit qu'une seule catégorie de tarification, qui est gratuite. Dans les versions ultérieures, vous pourrez sélectionner la catégorie de tarification.</dd>
	</dl>

3. Cliquez sur **Créer**.

  Le tableau de bord de {{site.data.keyword.cloud_notm}} s'affiche. Vous devrez peut-être actualiser votre navigateur pour voir le nouveau cluster, lequel est répertorié dans la section Services.</p></li>

4. Lorsque vous sélectionnez le service, les informations de cluster s'affichent.

5. Dans l'onglet Gérer des informations de cluster, cliquez sur **Ouvrir**.

	Le tableau de bord de {{site.data.keyword.ihsdbaas_full}} s'affiche.

6. Obtenez le nom d'hôte et les numéros de port des trois instances de base de données créées qui appartiennent à votre cluster de base de données. Vous aurez besoin des noms d'hôtes, des numéros de port et des données d'identification de l'utilisateur pour les étapes de la section [Connexion à la base de données](#connect_db).

## Etape 2. Création d'un projet à l'aide d'un kit de démarrage
{: #create_with_starter}

Vous avez besoin d'un kit qui repose sur l'infrastructure Web Swift côté serveur Kitura.

![Détails d'une fonction](videos/StarterKit.gif){: gif}

Utilisez un projet existant qui a été créé à partir de ce kit de démarrage, ou créez-en un nouveau.

1. Ouvrez le tableau de bord du service d'application {{site.data.keyword.cloud_notm}} à l'adresse https://console.bluemix.net/developer/appservice/dashboard.

2. Sélectionnez l'onglet **Kits de démarrage**.

3. Sélectionnez le kit de démarrage BFF (Backend for Frontend) **Swift Kitura**. Assurez-vous de ne pas le confondre avec le kit de démarrage de l'application Web de base Swift Kitura.
  La page Créer un projet s'affiche.

4. Entrer les détails du projet et cliquez sur **Créer un projet**.
  La page du projet s'affiche.

5. Sur la page des projets, cliquez sur **Télécharger le code**.

6. Développez le fichier compressé dans le répertoire du projet.

## Etape 3. Connexion à la base de données
{: #connect_db}

Pour garantir un transfert sécurisé des données, téléchargez le fichier de l'autorité de certification à l'adresse
https://api.hypersecuredbaas.ibm.com/cert.pem, et copiez-le dans le répertoire de votre projet.

1. Accédez au répertoire de votre projet où se trouvent les fichiers de code téléchargés développés.

2. Créez un fichier JSON nommé `cred.json` pour stocker vos données d'identification d'accès au cluster de base de données.

3. Entrez les valeurs collectées dans les étapes de [Création d'un cluster de base de données](#create_dbcluster). Les valeurs doivent être spécifiées sur une seule ligne.
  ```hljs
  {
  "uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
  <Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
   /admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
  }
  ```
  {: codeblock}

  Où :
  <table>
  <tr>
    <th> Paramètre </th>
    <th> Description </th>
  </tr>
  <tr>
    <td> &lt;<em>admin_ID</em>&gt; </td>
    <td> ID utilisateur de l'administrateur de base de données comme indiqué dans [Création d'un cluster de base de données](#create_dbcluster).
  </td>
  </tr>
  <tr>
    <td> &lt;<em>admin_pwd</em>&gt; </td>
    <td> ID utilisateur du mot de passe de l'administrateur de base de données comme indiqué dans [Création d'un cluster de base de données](#create_dbcluster). </td>
  </tr>
  <tr>
    <td> &lt;<em>Hostname_i</em>&gt; </td>
    <td> Réplique de base de données <em>i</em> (<em>i</em>=1,2,3) telle que retournée dans [Création d'un cluster de base de données](create_dbcluster). </td>
  </tr>
  <tr>
    <td> &lt;<em>PortNumber_i</em>&gt; </td>
    <td> Numéro de port <em>i</em> (<em>i</em>=1,2,3) tel que retourné dans [Création d'un cluster de base de données](#create_dbcluster). </td>
  </tr>
  <tr>
    <td> &lt;<em>CA_file</em>&gt; </td>
    <td> Nom de fichier du fichier de l'autorité de certification téléchargé. Lors du déploiement, il est copié dans le répertoire `/rapide-projet`.</td>
  </tr>
  </table>

4. Editez le fichier `Package.swift` afin d'ajouter les dépendances de package pour l'utilisation
du MongoKitten.

  * Dans la section dependencies, ajoutez la ligne suivante :
   ```hljs
   .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
   ```
   {: codeblock}

  * Dans la section targets, ajoutez la dépendance "MongoKitten" sur la ligne suivante. **Remarque :** La valeur doit être indiquée sur une seule ligne.
   ```hljs
   .target(name: "Application", dependencies: [ "Kitura",
   "CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
   ```
   {: codeblock}

5. Editez le fichier `Sources/Application/Application.swift` pour initialiser la connectivité à MongoDB à l'aide de MongoKitten.

  * Importez le logiciel SDK MongoKitten :
    ```
	import MongoKitten
	```
	{: codeblock}

  * Ajoutez la classe `ApplicationServices` :
    ```hljs
	cclass ApplicationServices {
	// Service references
	    public let mongoDBService: MongoKitten.Database
	    public let myCredFile = "/swift-project/cred.json"

    public init() throws {
        // Read credentials from json file cred.json
	        struct ResponseData: Decodable {
            var uri: String
	        }
	        let data = try? Data(contentsOf: URL(fileURLWithPath: myCredFile))
	        let decoder = JSONDecoder()
	        let jsonData = try decoder.decode(ResponseData.self, from: data!)

        // Run service initializers
	        let server = try Server(jsonData.uri)
	        mongoDBService = MongoKitten.Database(named: "admin", atServer: 		server)
	    }
	}
	```
	{: codeblock}

  * Dans public class `App`, ajoutez les lignes suivantes afin d'initialiser la connexion de base de données :
    ```hljs
	public class App {
	...
	let services: ApplicationServices

	public init() throws {
	   // Services
	    services = try ApplicationServices()
	 }
	...
    ```
    {: codeblock}

## Etape 4. Vérification de la connexion de base de données
{: #verify_database}

1. Vérifiez votre connexion de base de données en éditant le fichier `Sources/Application/Application.swift` afin d'ajouter une commande pour tester ka connexion de base de données, connexion à la base de données.
Par exemple, ajoutez la commande suivante dans `class ApplicationServices` :

	```hljs
		class ApplicationServices {
		    ...
		    public init() throws {
		        ...
		        let server = try Server(jsonData.uri)
		        mongoDBService = MongoKitten.Database(named: "admin", atServer: server)
		        if server.isConnected {
		            print("Connected to mongodb: " + String(describing: mongoDBService))
		        }
		        ...
		    }
		}
	```
	{: codeblock}

Après avoir déployé votre application dans l'[étape 6](#use-step6), le message suivant s'affiche, si votre connexion à la base de données aboutit :

```hljs
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## Etape 5. Imbrication de votre code d'application 
{: #embed_appcode}

Vous pouvez maintenant ajouter votre propre code d'application au projet. Pour plus d'informations sur l'utilisation de l'API MongoKitten, consultez l'adresse http://beta.openkitten.org/tutorials/.

## Etape 6. Déploiement de votre application 
{: #deploy_app}

Vous pouvez exécuter l'application en local avec les outils de génération nécessaires, ou dans {{site.data.keyword.cloud_notm}} (Cloud Foundry or Kubernetes Cluster) à l'aide de {{site.data.keyword.dev_cli_notm}}.

Vous pouvez exécuter votre application en local sur votre système hôte, dans Cloud Foundry, ou dans un cluster Kubernetes.

1. [Installez l'interface de ligne de commande ](/docs/cli/reference/bluemix_cli/get_started.html) {{site.data.keyword.cloud_notm}}

2. Installez le plug-in Developer Tools à l'aide de la commande `ibmcloud plugin install dev`.

3. Déployez l'application sur un [système local](#deploy_local), dans un cluster [Cloud Foundry](#deploy_cf) ou [Kubernetes](#deploy_cluster).

### Déploiement en local
{: #deploy_local}

1. Assurez-vous que Docker est installé et opérationnel sur votre système hôte local. Vous pouvez télécharger Docker depuis l'adresse https://www.docker.com/community-edition#/download.

2. Accédez au répertoire contenant vos fichiers de projet.

3. Pour déployer l'application sur votre ordinateur local, exécutez les commandes suivantes :
	```
	$ ibmcloud dev build
	...
	$ ibmcloud dev run
	```
	{: codeblock}

	Cette étape génère votre application et l'exécute en local dans un conteneur Docker.

### Déploiement dans Cloud Foundry
{: #deploy_cf}

1. Accédez au répertoire contenant vos fichiers de projet.

2. Connectez-vous à votre compte IBM Cloud, puis définissez la région sur `us-south`, comme illustré ici :
  ```hljs
  $ ibmcloud login -a https://api.ng.bluemix.net
  $ ibmcloud target -o &lt;<em>your-organization</em>&gt; -s &lt;<em>your-space</em>&gt;
  ```
  {: codeblock}

  **Remarque :** L'exécution de la commande `ibmcloud login -a https://api.ng.bluemix.net` définit automatiquement la région sur **us-south**.

3. Pour déployer l'application dans Cloud Foundry, entrez la commande suivante :
  ```
  $ ibmcloud dev deploy
  ```
  {: codeblock}

  Vous recevez un lien cliquable à l'emplacement dans lequel votre application est hébergée.

### Déploiement dans un cluster Kubernetes
{: #deploy_cluster}

1. Créez un cluster Kubernetes à l'adresse https://console.bluemix.net/containers-kubernetes/clusters.

2. Cliquez sur **Créer un cluster**. L'onglet d'accès affiche des informations sur la manière d'accéder au cluster Kubernetes créé.

3. Pour afficher des informations sur le cluster Kubernetes, ouvrez le tableau de bord {{site.data.keyword.cloud_notm}}. Le tableau de bord affiche une liste de vos services, tels que les clusters, les clusters de base de données, les applications Cloud Foundry, et les services Cloud Foundry créés.

4. Accédez au répertoire contenant vos fichiers de projet.

5. Connectez-vous à votre compte {{site.data.keyword.cloud_notm}}, puis définissez la région us-south, comme illustré ici :
  ```hljs
  $ ibmcloud login -a https://api.ng.bluemix.net
  $ ibmcloud target -o <your-organization> -s <your-space>
  ```
  {: codeblock}

  **Remarque :** L'exécution de la commande `ibmcloud login -a https://api.ng.bluemix.net` définit automatiquement la région sur **us-south**.

6. Pour déployer votre application dans Kubernetes, entrez la commande suivante :
  ```
  $ ibmcloud dev deploy -t container
  ```
  {: codeblock}

  Vous êtes invité à entrer le nom de votre cluster Kubernetes et le registre Docker. Une fois les informations fournies, votre application est déployée dans le cluster Kubernetes.
