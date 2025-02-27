---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-19"

keywords: authentication swift, security swift, forgot password swift, social swift, identity provider swift, tentantid swift, cloud directory swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Aggiunta dell'autenticazione utente
{: #appid}

La sicurezza delle applicazioni è incredibilmente complicata. Per la maggior parte degli sviluppatori, è una delle attività più difficili della creazione di un'applicazione. Come fai a essere sicuro che stai proteggendo le informazioni dei tuoi utenti? Integrando {{site.data.keyword.appid_full}} nelle tue applicazioni, puoi proteggere le risorse e aggiungere l'autenticazione, anche quando non hai tanta esperienza nel campo della sicurezza.

Richiedendo agli utenti di eseguire l'accesso, puoi memorizzare i dati utente quali le preferenze dell'applicazione (o le informazioni dai profili social pubblici) e utilizzare quindi tali dati per personalizzare l'esperienza di ciascun utente nell'applicazione. {{site.data.keyword.appid_short_notm}} fornisce un framework di accesso per te ma puoi anche portare le tue schermate di accesso personalizzate da utilizzare con cloud directory.

Per tutti i modi in cui puoi utilizzare {{site.data.keyword.appid_short_notm}} e le informazioni sull'architettura, vedi [Informazioni su {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-about#about).

## Prima di cominciare
{: #prereqs-appid}

Assicurati innanzitutto di disporre dei seguenti prerequisiti pronti a essere utilizzati:
* CocoaPods (versione 1.1.0 o superiore)
* iOS (versione 9 o superiore)
* MacOS (versione 10.11.5 o superiore)
* Xcode (versione 9.0.1 o superiore)

## Passo 1. Creazione di un'istanza di {{site.data.keyword.appid_short_notm}}
{: #create-instance-appid}

Crea un'istanza del servizio {{site.data.keyword.appid_short_notm}}:

1. Nel [catalogo {{site.data.keyword.cloud_notm}}](https://{DomainName}/catalog){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"), seleziona {{site.data.keyword.appid_short_notm}}. Viene visualizzata la schermata di configurazione del servizio.
2. Dai un nome alla tua istanza del servizio oppure utilizza il nome preimpostato.
3. Seleziona il tuo piano dei prezzi e fai clic su **Crea**.

## Passo 2: Installazione dell'SDK Swift iOS
{: #install-sdk-appid}

Il servizio fornisce un SDK per semplificare la codifica della tua applicazione. L'SDK deve essere installato nel tuo codice dell'applicazione.

1. Apri la tua directory del progetto Xcode esistente nel file `Podfile`.

2. Nella destinazione dei tuoi progetti, aggiungi una dipendenza per il pod `IBMCloudAppID`. Assicurati che anche il comando `use_frameworks!` si trovi nella tua destinazione, come mostrato nel seguente esempio.
    ```swift
    target '<yourTarget>' do
      use_frameworks!
        pod 'IBMCloudAppID'
    end
    ```
    {: pre}

3. Scarica la dipendenza `IBMCloudAppID`.
    ```
    pod install --repo-update
    ```
    {: pre}

## Passo 3. Inizializzazione dell'SDK
{: #initialize-sdk-appid}

Dopo che hai inizializzato l'SDK nella tua applicazione, puoi iniziare a configurare le tue preferenze {{site.data.keyword.appid_short_notm}}.

1. Vai a **Project Settings > Capabilities > Keychain sharing** e abilita Keychain Sharing nel tuo progetto Xcode.

2. Vai a **Project Settings > Info >URL Types** e aggiungi il seguente valore sia alla casella di testo **URL Scheme** che a quella **Identifier**.
  ```
  $(PRODUCT_BUNDLE_IDENTIFIER)
  ```
  {: codeblock}

3. Aggiungi la seguente istruzione import al tuo file `AppDelegate.swift`.
  ```swift
  import IBMCloudAppID
  ```
  {: codeblock}

4. Passa i parametri `tenantID` e `AppID_region` per inizializzare l'SDK. Un'ubicazione comune, sebbene non obbligatoria, in cui posizionare il codice è nel metodo `application:didFinishLaunchingWithOptions` di `AppDelegate` nella tua applicazione.
  ```swift
  AppID.getInstance().initialize(getApplicationContext(), <tenantId>, <AppID_region>);
  ```
  {: codeblock}
  
  * `tenantID`: l'ID tenant è un identificativo univoco utilizzato per inizializzare la tua applicazione. Puoi trovare il valore nel dashboard {{site.data.keyword.appid_short_notm}} selezionando la scheda **Credenziali del servizio** e quindi facendo clic su **Visualizza credenziali**.
  * `AppID_region`: la regione {{site.data.keyword.appid_short_notm}} è la regione {{site.data.keyword.cloud_notm}} in cui stai lavorando con il servizio. Questa regione può essere trovata nel dashboard del servizio e può essere `AppID.REGION_US_SOUTH`, `AppID.REGION_SYDNEY` e `AppID.REGION_UK`.

5. Aggiungi il seguente codice al tuo file `AppDelegate`.
    ```swift
    func application(_ application: UIApplication, open url: URL, options :[UIApplicationOpenURLOptionsKey : Any]) -> Bool {
            return AppID.sharedInstance.application(application, open: url, options: options)
        }
    ```
    {: codeblock}

## Passo 4. Gestione dell'esperienza di accesso
{: #managing-signin-appid}

Un provider di identità fornisce le informazioni di autenticazione per i tuoi utenti in modo da consentirti di autorizzarli. Con {{site.data.keyword.appid_short_notm}}, puoi utilizzare i provider di identità social quali Facebook e Google+ oppure puoi gestire un registro utenti con cloud directory.

Puoi aggiornare le tue configurazioni in qualsiasi momento aggiornando il tuo codice dell'applicazione.
{: tip}

### Provider di identità social
{: #social-appid}

Con {{site.data.keyword.appid_short_notm}}, puoi utilizzare i provider di identità social quali Facebook e Google+ per proteggere le tue applicazioni.

Per configurare i provider di identità social:

1. Apri il dashboard {{site.data.keyword.appid_short_notm}} a **Identity Providers > Manage**.
2. Imposta i provider di identità che vuoi utilizzare su **On**. Puoi visualizzare qualsiasi combinazione di provider di identità ma, se vuoi portare delle schermate di collegamento personalizzate, devi abilitare solo Cloud Directory.
3. Aggiorna la [configurazione predefinita](/docs/services/appid?topic=appid-social#social) alle tue credenziali. {{site.data.keyword.appid_short_notm}} fornisce le credenziali IBM che puoi utilizzare per provare il servizio ma, prima di pubblicare la tua applicazione, devi aggiornare la configurazione.
4. Personalizza la schermata di accesso per visualizzare l'immagine e i colori da te scelti.
5. Per richiamare il widget di accesso con la tua applicazione, aggiungi il seguente comando al tuo codice.
    ```swift
    import IBMCloudAppID
    class delegate : AuthorizationDelegate {
        public func onAuthorizationSuccess(accessToken: AccessToken, identityToken: IdentityToken, response:Response?) {
            //User authenticated
        }

        public func onAuthorizationCanceled() {
            //Authentication cancelled by the user
        }

        public func onAuthorizationFailure(error: AuthorizationError) {
            //Exception occurred
        }
    }

    AppID.sharedInstance.loginWidget?.launch(delegate: delegate())
    ```
    {: codeblock}

### Cloud directory
{: #cloud-dir-appid}

Con {{site.data.keyword.appid_short_notm}}, puoi gestire il tuo registro utenti denominato Cloud Directory. Cloud Directory abilita gli utenti a registrarsi e ad accedere alle tue applicazioni mobili e web utilizzando la loro email e una password.

Per portare delle tue schermate dell'IU personalizzate, può essere abilitato solo cloud directory come provider di identità.
{: tip}

Per configurare cloud directory:

1. Apri il dashboard {{site.data.keyword.appid_short_notm}} alla scheda **Managing identity providers** e imposta Cloud Directory su **On**.
2. Configura le tue [impostazioni di directory e messaggi](/docs/services/appid?topic=appid-cloud-directory).
4. Scegli le combinazioni di schermate di collegamento che vuoi visualizzare e posiziona il codice per richiamare tali schermate nella tua applicazione.
    * Accesso
        ```swift
        class delegate : TokenResponseDelegate {
            public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
            /* User authenticated */
            }

            public func onAuthorizationFailure(error: AuthorizationError) {
            /* Exception occurred */
            }
        }

        AppID.sharedInstance.obtainTokensWithROP(username: username, password: password, delegate: delegate())
        ```
        {: codeblock}

    * Registrazione
        ```swift
        class delegate : AuthorizationDelegate {
          public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             if accessToken == nil && identityToken == nil {
              /* email verification is required */
              return
             }
           /* User authenticated */
          }

          public func onAuthorizationCanceled() {
              /* Sign up cancelled by the user */
          }

          public func onAuthorizationFailure(error: AuthorizationError) {
              /* Exception occurred */
          }
        }

        AppID.sharedInstance.loginWidget?.launchSignUp(delegate: delegate())
        ```
        {: codeblock}

    * Password dimenticata
        ```swift
        class delegate : AuthorizationDelegate {
           public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
              /* forgot password finished, in this case accessToken and identityToken will be null. */
           }

           public func onAuthorizationCanceled() {
               /* forgot password canceled by the user */
           }

           public func onAuthorizationFailure(error: AuthorizationError) {
               /* Exception occurred */
           }
        }

        AppID.sharedInstance.loginWidget?.launchForgotPassword(delegate: delegate())
        ```
        {: codeblock}

    * Dettagli dell'account
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

    * Modifica della password
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


## Passo 5. Esecuzione di test della tua applicazione
{: #testing-appid}

È tutto configurato correttamente? Puoi verificarlo eseguendo dei test.

1. Apri la tua applicazione con l'emulatore Xcode.
2. Utilizzando la GUI, segui il processo di accesso alla tua applicazione. Se hai configurato cloud directory, assicurati che tutte le tue schermate si presentino come previsto.
3. Aggiorna i provider di identità oppure la schermata del widget di accesso nel dashboard {{site.data.keyword.appid_short_notm}}.
4. Ripeti i passi 1 e 2 per appurare che le modifiche vengano implementate immediatamente. Non è richiesto alcun aggiornamento al tuo codice dell'applicazione.

Hai riscontrato dei problemi? Consulta [Risoluzione dei problemi di {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-troubleshooting).

## Passi successivi
{: #next-appid notoc}

Ottimo lavoro. Hai aggiunto un livello di sicurezza alla tua applicazione. Non fermarti ora e continua provando una delle seguenti opzioni:

* Scopri di più e avvaliti di tutte le funzioni che {{site.data.keyword.appid_short_notm}} ha da offrire, [controlla la documentazione](/docs/services/appid?topic=appid-getting-started#getting-started)!
* I kit starter sono uno dei modi più rapidi per utilizzare le funzioni di {{site.data.keyword.cloud_notm}}. Visualizza i kit starter disponibili nel [Dashboard dello sviluppatore mobile ](https://{DomainName}/developer/mobile/dashboard){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"). Scarica il codice. Esegui l'applicazione!
