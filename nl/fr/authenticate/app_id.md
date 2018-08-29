---

copyright:
  years: 2018
lastupdated: "2018-08-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Ajout d'une authentification d'utilisateur

La sécurité des applications peut s'avérer être un sujet incroyablement compliqué. Pour la plupart des développeurs, il s'agit de l'une des composantes les plus difficiles du processus de création d'application. Comment pouvez-vous être sûr que vous protégez les informations de vos utilisateurs ? En intégrant {{site.data.keyword.appid_full}} dans vos applis, vous pouvez sécuriser les ressources et ajouter une authentification, et ce même si vous ne possédez pas beaucoup d'expérience en matière de sécurité.

En exigeant que les utilisateurs se connectent, vous pouvez stocker des données utilisateur comme les préférences d'appli (ou des informations de profils sociaux publics), puis vous pouvez optimiser ces données afin de personnaliser chaque expérience utilisateur au sein de l'appli. {{site.data.keyword.appid_short_notm}} fournit une infrastructure de connexion que vous pouvez utiliser, mais vous pouvez également implémenter les écrans de connexion de votre entreprise à utiliser avec le répertoire cloud.

Pour en savoir plus sur les différentes utilisations du service {{site.data.keyword.appid_short_notm}} et les informations d'architecture, voir [About {{site.data.keyword.appid_short_notm}}](/docs/services/appid/about.html).

## Avant de commencer

Tout d'abord, assurez-vous que vous respectez la configuration prérequise suivante :
 * CocoaPods (version 1.1.0 ou ultérieure) 
 * iOS (version 9 ou ultérieure) 
 * MacOS (version 10.11.5 ou ultérieure) 
 * Xcode (version 9.0.1 ou ultérieure)

## Etape 1. Création d'une instance de {{site.data.keyword.appid_short_notm}}

Mettez à disposition une instance du service :

1. Dans le cataloguer [{{site.data.keyword.cloud_notm}}](https://console.bluemix.net/catalog/), sélectionnez {{site.data.keyword.appid_short_notm}}. L'écran de configuration du service s'ouvre.
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Sélectionnez votre plan de tarification, puis cliquez sur **Créer**.

## Etape 2. Installation du logiciel SDK iOS Swift

Le service fournit un logiciel SDK pour simplifier le codage de votre appli. Ce logiciel SDK doit être installé dans votre code d'appli.

1. Ouvrez votre répertoire projet Xcode existant sur le `Fichier pod`.

2. Sous votre cible projets, ajoutez une dépendance pour le pod `BluemixAppID`. Assurez-vous que la commande `use_frameworks!` figure également sous votre cible, comme illustré dans l'exemple suivant.
    ```swift
    target '<yourTarget>' do
      use_frameworks!
        pod 'BluemixAppID'
    end
    ```
    {: pre}

3. Téléchargez la dépendance `BluemixAppID`.
    ```
    pod install --repo-update
    ```
    {: pre}

## Etape 3. Initialisation du logiciel SDK

Une fois le logiciel SDK initialisé dans votre appli, vous pouvez commencer à configurer vos préférences {{site.data.keyword.appid_short_notm}}.

1. Sélectionnez **Paramètres du projet > Fonctions > Partage de chaîne de certificats** et activez le partage de chaîne de certificats dans votre projet Xcode.

2. Sélectionnez **Paramètres du projet > Info >Types d'URL** et ajoutez la valeur suivante dans les zones de texte **Schéma d'URL** et **Identificateur**.
    ```
    $(PRODUCT_BUNDLE_IDENTIFIER)
    ```
    {: pre}

3. Ajoutez l'importation ci-dessous dans votre fichier `AppDelegate.swift`.
    ```swift
    import BluemixAppID
    ```
    {:pre}

4. Transmettez les paramètres ID titulaire et les paramètres de région afin d'initialiser le logiciel SDK. Une pratique courante, mais non obligatoire, consiste à placer le code dans la méthode `application:didFinishLaunchingWithOptions` de AppDelegate dans votre appli.
    ```swift
    AppID.sharedInstance.initialize(tenantId: <tenantId>, bluemixRegion: <AppID_region>)
    ```
    {: pre}
  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt=""/> Signification des composants de la commande </th>
    </thead>
    <tbody>
      <tr>
        <td><em>tenantID</em></td>
        <td>L'ID titulaire est un identificateur unique qui est utilisé pour initialiser votre application. Vous pouvez trouver la valeur dans le tableau de bord {{site.data.keyword.appid_short_notm}}. Sous l'onglet <b>Données d'identification pour le service</b>, cliquez sur <b>Afficher Identifiants</b>.</td>
      </tr>
      <tr>
        <td><em>AppID_region</em></td>
        <td>La région {{site.data.keyword.appid_short_notm}} est la région IBM dans laquelle vous gérez le service. Cette région est visible dans le tableau de bord des services et il peut s'agir de <em>AppID.REGION_US_SOUTH</em>,<em>AppID.REGION_SYDNEY</em> ou <em>AppID.REGION_UK</em>.</td>
      </tr>
    </tbody>
  </table>

5. Ajoutez le code suivant à votre fichier AppDelegate.
    ```swift
    func application(_ application: UIApplication, open url: URL, options :[UIApplicationOpenURLOptionsKey : Any]) -> Bool {
            return AppID.sharedInstance.application(application, open: url, options: options)
        }
    ```
    {: pre}

## Step 4. Gestion de l'expérience de connexion
{: #managing-signin-appid}

Un fournisseur d'identité fournit les informations d'authentification pour vos utilisateurs afin que vous puissiez les autoriser. Avec {{site.data.keyword.appid_short_notm}}, vous pouvez utiliser les fournisseurs d'identité de réseaux sociaux tels que Facebook et Google+ ou vous pouvez gérer un registre d'utilisateurs avec un répertoire cloud 

Vous pouvez mettre à jour vos configurations à tout moment sans mettre à jour le code de votre appli.
{: tip}


### Fournisseurs d'identité de réseaux sociaux

Avec {{site.data.keyword.appid_short_notm}}, vous pouvez utiliser des fournisseurs d'identité de réseaux sociaux tels que Facebook et Google+ pour sécuriser vos applis.

Pour configurer des fournisseurs d'identité de réseaux sociaux :

1. Ouvrez le tableau de bord {{site.data.keyword.appid_short_notm}} sur **Fournisseurs d'identité > Gérer**.
2. Définissez les fournisseurs d'identité que vous voulez utiliser sur **Actif**. Vous pouvez utiliser une combinaison quelconque de fournisseurs d'identité, mais si vous souhaitez implémenter des écrans de connexion personnalisés, vous devez activer le Répertoire cloud uniquement.
3. Mettez à jour la [configuration par défaut](/docs/services/appid/identity-providers.html) avec vos propres données d'identification. {{site.data.keyword.appid_short_notm}} fournit des données d'identification IBM que vous pouvez utiliser pour tester le service, mais avant de publier votre appli, vous devez mettre à jour la configuration.
4. Personnalisez l'écran de connexion préconfiguré afin d'afficher l'image et les couleurs de votre choix.
5. Pour appeler le widget de connexion avec votre appli, ajoutez la commande suivante à votre code.
    ```swift
    import BluemixAppID
    class delegate : AuthorizationDelegate {
        public func onAuthorizationSuccess(accessToken: AccessToken, identityToken: IdentityToken, response:Response?) {
            //User authenticated
        }

        public func onAuthorizationCanceled() {
            //Authentication canceled by the user
        }

        public func onAuthorizationFailure(error: AuthorizationError) {
            //Exception occurred
        }
    }

    AppID.sharedInstance.loginWidget?.launch(delegate: delegate())
    ```
    {: codeblock}


### Répertoire cloud

Avec {{site.data.keyword.appid_short_notm}}, vous pouvez gérer votre propre registre d'utilisateurs appelé répertoire cloud. Le répertoire cloud permet aux utilisateurs de s'inscrire et de se connecter à vos applis mobile et Web à l'aide de leur e-mail et d'un mot de passe.

Pour l'implémentation des écrans d'interface utilisateur de votre marque, seul le répertoire cloud peut être activé en tant que fournisseur d'identité.
{: tip}

Pour configurer le répertoire cloud :

1. Accédez dans le tableau de bord {{site.data.keyword.appid_short_notm}} à l'onglet de **gestion des fournisseurs d'identité** et définissez le répertoire cloud sur **Actif**.
2. Configurez votre répertoire [ et les paramètres de message](/docs/services/appid/cloud-drectory.html).
4. Choisissez plusieurs combinaisons d'écrans de connexion et placez le code permettant de les appeler dans votre application.
    * Sign in
        ```swift
        class delegate : TokenResponseDelegate {
            public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
            //User authenticated
            }

            public func onAuthorizationFailure(error: AuthorizationError) {
            //Exception occurred
            }
        }

        AppID.sharedInstance.obtainTokensWithROP(username: username, password: password, delegate: delegate())
        ```
        {: codeblock}
    * Sign up
        ```swift
        class delegate : AuthorizationDelegate {
          public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             if accessToken == nil && identityToken == nil {
              //email verification is required
              return
             }
           //User authenticated
          }

          public func onAuthorizationCanceled() {
              //Sign up canceled by the user
          }

          public func onAuthorizationFailure(error: AuthorizationError) {
              //Exception occurred
          }
        }

        AppID.sharedInstance.loginWidget?.launchSignUp(delegate: delegate())
        ```
        {: codeblock}

    * Forgot password
        ```swift
        class delegate : AuthorizationDelegate {
           public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
              //forgot password finished, in this case accessToken and identityToken will be null.
           }

           public func onAuthorizationCanceled() {
               //forgot password canceled by the user
           }

           public func onAuthorizationFailure(error: AuthorizationError) {
               //Exception occurred
           }
        }

        AppID.sharedInstance.loginWidget?.launchForgotPassword(delegate: delegate())
        ```
        {: codeblock}

    * Account details
        ```swift

         class delegate : AuthorizationDelegate {
             public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             }

             public func onAuthorizationCanceled() {
             }

             public func onAuthorizationFailure(error: AuthorizationError) {
             }
         }

         AppID.sharedInstance.loginWidget?.launchChangeDetails(delegate: delegate())
        ```
        {: codeblock}

    * Change password
        ```swift
         class delegate : AuthorizationDelegate {
             public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             }

             public func onAuthorizationCanceled() {
             }

             public func onAuthorizationFailure(error: AuthorizationError) {
             }
          }

          AppID.sharedInstance.loginWidget?.launchChangePassword(delegate: delegate())
        ```
        {: codeblock}


## Step 5. Test de votre appli
{: #appid_testing}

Est-ce que tout est correctement configuré ? Vous pouvez le tester !

1. Ouvrez votre application avec l'émulateur Xcode.
2. Depuis l'interface graphique utilisateur, suivez le processus de connexion à votre application. Si vous avez configuré un répertoire cloud, vérifiez que tous vos écrans s'affichent comme prévu. 
3. Mettez à jour les fournisseurs d'identité ou l'écran du widget de connexion dans le tableau de bord {{site.data.keyword.appid_short_notm}}.
4. Répétez les étapes 1 et 2 pour vérifier que les modifications sont immédiatement appliquées. Aucune mise à jour du code de votre appli n'est nécessaire.

Vous rencontrez des problèmes ? Consultez la section [troubleshooting {{site.data.keyword.appid_short_notm}}](/docs/services/appid/ts_index.html).

## Etapes suivantes
{: #appid_next}

Félicitations !  Vous avez ajouté un niveau de sécurité à votre appli. Poursuivez sur votre lancée en essayant l'une des options suivantes :

* Découvrez toutes les fonctions offertes par {{site.data.keyword.appid_short_notm}} et profitez-en, [consultez les docs](/docs/services/appid/index.html)!
* Les Kits de démarrage sont des moyens rapides d'optimiser les fonctionnalités d'IBM Cloud. La liste des kits de démarrage disponibles est accessible sur le [tableau de bord du développeur d'applications mobiles](https://console.bluemix.net/developer/mobile/dashboard). Téléchargez le code. Exécutez l'appli.

