---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Benutzerauthentifizierung hinzufügen
{: #appid}

Anwendungssicherheit ist ein sehr kompliziertes Thema. Für die meisten
Entwickler stellt sie eine der schwierigsten Aufgaben beim Erstellen einer App
dar. Es muss gewährleistet sein, dass die Daten Ihrer Benutzer geschützt sind. Durch
die Integration von {{site.data.keyword.appid_full}} in Ihre Apps
können Sie auch ohne große Erfahrungen in Bezug auf die Sicherheit Ressourcen
schützen und eine Authentifizierung hinzufügen.

Wenn sich Benutzer anmelden müssen, können Sie Benutzerdaten wie
App-Vorgaben (oder auch Informationen aus öffentlichen Social-Media-Profilen)
speichern und dann unter Nutzung dieser Daten die Attraktivität Ihrer App für
die einzelnen Benutzer anpassen. {{site.data.keyword.appid_short_notm}}
stellt Ihnen ein Anmeldeframework bereit; Sie können jedoch auch Ihre eigenen
markenspezifischen Anmeldeanzeigen zur Verwendung beim Cloudverzeichnis einbinden.

Informationen zu allen Möglichkeiten für die Nutzung von
{{site.data.keyword.appid_short_notm}} sowie Angaben über die
Architektur finden Sie unter [Informationen zu {{site.data.keyword.appid_short_notm}}](/docs/services/appid/about.html).

## Vorbereitende Schritte
{: #prereqs-appid}

Stellen Sie zunächst sicher, dass die folgenden Voraussetzungen gegeben
sind:
* CocoaPods (Version 1.1.0 oder höher)
* iOS (Version 9 oder höher)
* MacOS (Version 10.11.5 oder höher)
* Xcode (Version 9.0.1 oder höher)

## Schritt 1. Instanz von {{site.data.keyword.appid_short_notm}}
erstellen
{: #create-instance-appid}

Stellen Sie eine Instanz des {{site.data.keyword.appid_short_notm}}-Service bereit:

1. Wählen Sie im
[{{site.data.keyword.cloud_notm}}-Katalog](https://cloud.ibm.com/catalog/)
den Eintrag für {{site.data.keyword.appid_short_notm}} aus. Die Anzeige für die Servicekonfiguration wird geöffnet.
2. Vergeben Sie für die Serviceinstanz einen Namen oder verwenden Sie den voreingestellten Namen.
3. Wählen Sie Ihren Preistarif aus und
klicken Sie auf **Erstellen**.

## Schritt 2. Swift-SDK für iOS installieren
{: #install-sdk-appid}

Der Service bietet ein SDK, das die Codierung Ihrer App
vereinfacht. Das SDK
muss in Ihrem App-Code installiert sein.

1. Öffnen Sie das vorhandene XCode-Projektverzeichnis für die
`Poddatei`.

2. Fügen Sie unter dem Ziel Ihres Projekts (Abschnitt "target") eine
Abhängigkeit für den Pod
`BluemixAppID` hinzu. Stellen Sie sicher, dass der Befehl
`use_frameworks!` ebenfalls wie im
folgenden Beispiel gezeigt unter Ihrem Ziel angegeben ist.
    ```swift
    target '<yourTarget>' do
      use_frameworks!
        pod 'BluemixAppID'
    end
    ```
    {: pre}

3. Laden Sie die Abhängigkeit `BluemixAppID` herunter.
    ```
    pod install --repo-update
    ```
    {: pre}

## Schritt 3. SDK initialisieren
{: #initialize-sdk-appid}

Nachdem Sie das SDK in Ihrer App initialisiert haben, können Sie mit der
Konfiguration der {{site.data.keyword.appid_short_notm}}-Einstellungen beginnen.

1. Navigieren Sie zu **Projekteinstellungen > Funktionen >
Gemeinsame Nutzung der Schlüsselkette** und aktivieren Sie die
gemeinsame Nutzung der Schlüsselkette in Ihrem XCode-Projekt.

2. Navigieren Sie zu **Projekteinstellungen >
Info > URL-Typen**
und fügen Sie den folgenden Wert zu beiden Textfeldern
**URL-Schema** und **ID** hinzu.
  ```
  $(PRODUCT_BUNDLE_IDENTIFIER)
  ```
  {: codeblock}

3. Fügen Sie den folgenden Import zu
Ihrer Datei `AppDelegate.swift` hinzu.
  ```swift
  import BluemixAppID
  ```
  {: codeblock}

4. Übergeben Sie die Parameter `tenant ID` und `region`, um die SDK zu initialisieren. Eine gängige, aber nicht verbindliche Position für den Code ist
die Methode `application:didFinishLaunchingWithOptions` von `AppDelegate` in Ihrer App.
  ```swift
  AppID.sharedInstance.initialize(tenantId: <tenantId>, bluemixRegion: <AppID_region>)
  ```
  {: codeblock}
  
  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt=""/> Wissenswertes zu den Befehlskomponenten </th>
    </thead>
    <tbody>
      <tr>
        <td><em>tenantID</em></td>
        <td>Die Tenant-ID ist eine eindeutige Kennung, die zum Initialisieren
Ihrer App verwendet wird. Der Wert ist im
{{site.data.keyword.appid_short_notm}}-Dashboard zu finden. Klicken Sie
auf der Registerkarte
<b>Serviceberechtigungsnachweise</b> auf <b>Berechtigungsnachweise anzeigen</b>.</td>
      </tr>
      <tr>
        <td><em>AppID_region</em></td>
        <td>Die {{site.data.keyword.appid_short_notm}}-Region ist die {{site.data.keyword.cloud_notm}}-Region, in der Sie mit dem Service arbeiten. Diese Region ist im
Service-Dashboard zu finden und kann <em>AppID.REGION_US_SOUTH</em>,
<em>AppID.REGION_SYDNEY</em> oder <em>AppID.REGION_UK</em> lauten.</td>
      </tr>
    </tbody>
  </table>

5. Fügen Sie den folgenden Code zu Ihrer AppDelegate-Datei hinzu.
    ```swift
    func application(_ application: UIApplication, open url: URL, options :[UIApplicationOpenURLOptionsKey : Any]) -> Bool {
            return AppID.sharedInstance.application(application, open: url, options: options)
        }
    ```
    {: codeblock}

## Schritt 4. Anmeldefunktionalität verwalten
{: #managing-signin-appid}

Ein Identitätsprovider stellt die Authentifizierungsinformationen für
Ihre Benutzer bereit, damit Sie sie autorisieren können. Bei
{{site.data.keyword.appid_short_notm}} können Sie
Social-Media-Identitätsprovider wie Facebook und Google+ verwenden oder eine
Benutzerregistry mit Cloudverzeichnis verwalten.

Ihre Konfigurationen können Sie jederzeit ohne eine Aktualisierung Ihres
App-Codes aktualisieren.
{: tip}

### Social-Media-Identitätsprovider
{: #social-appid}

Bei {{site.data.keyword.appid_short_notm}} können Sie
Social-Media-Identitätsprovider wie Facebook und Google+ verwenden, um Ihre
Apps zu schützen.

So konfigurieren Sie Social-Media-Identitätsprovider:

1. Öffnen Sie im {{site.data.keyword.appid_short_notm}}-Dashboard
die Anzeige
**Identitätsprovider > Verwalten**.
2. Legen Sie für die Identitätsprovider, die Sie verwenden wollen, die
Einstellung **Ein** fest. Sie können eine beliebige
Kombination von Identitätsprovidern verwenden. Wenn Sie jedoch angepasste
Anmeldebildschirme verwenden möchten, müssen Sie lediglich die Option
"Cloudverzeichnis" aktivieren.
3. Aktualisieren Sie die
[Standardkonfiguration](/docs/services/appid/identity-providers.html)
mit Ihren eigenen Berechtigungsnachweisen. {{site.data.keyword.appid_short_notm}}
stellt IBM Berechtigungsnachweise bereit, mit deren Hilfe Sie den Service
ausprobieren können. Bevor Sie Ihre App veröffentlichen, müssen Sie jedoch die
Konfiguration aktualisieren.
4. Passen Sie die Anmeldeanzeige so an, dass das Bild und die Farben Ihrer Wahl angezeigt werden.
5. Fügen Sie den folgenden Befehl zu Ihrem Code hinzu, um das
Anmeldewidget mit Ihrer App aufzurufen.
    ```swift
    import BluemixAppID
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


### Cloudverzeichnis
{: #cloud-dir-appid}

Bei {{site.data.keyword.appid_short_notm}} können Sie eine eigene
Benutzerregistry verwalten, die als "Cloudverzeichnis" bezeichnet wird. Das
Cloudverzeichnis ermöglicht es den Benutzern, sich bei Ihren mobilen und
Web-Apps unter Verwendung ihrer E-Mail-Adresse und eines Kennworts anzumelden.

Wenn Sie eigene markenspezifische Anmeldeanzeigen verwenden wollen, kann nur das
Cloudverzeichnis als Identitätsprovider aktiviert werden.
{: tip}

So konfigurieren Sie das Cloudverzeichnis:

1. Öffnen Sie im {{site.data.keyword.appid_short_notm}}-Dashboard
die Registerkarte **Identitätsprovider verwalten** und legen
Sie für das Cloudverzeichnis die Einstellung **Ein** fest.
2. Konfigurieren Sie Ihre
[Verzeichnis- und
Nachrichteneinstellungen](/docs/services/appid/cloud-drectory.html).
4. Wählen Sie die Kombinationen der Anmeldeanzeigen aus, die angezeigt
werden sollen, und geben Sie den Code für den Aufruf dieser Anzeigen in Ihrer
Anwendung an.
    * Anmeldung
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

    * Anmeldung
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

    * Vergessenes Kennwort
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

    * Kontodetails
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

    * Kennwortänderung
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


## Schritt 5. App testen
{: #testing-appid}

Ist alles richtig konfiguriert? Jetzt können Sie testen, ob Sie alles richtig konfiguriert haben.

1. Öffnen Sie Ihre App mit dem Xcode-Emulator.
2. Durchlaufen Sie den Prozess für die Anmeldung bei Ihrer Anwendung
mithilfe der GUI. Falls Sie das Cloudverzeichnis konfiguriert haben, stellen
Sie fest, ob alle Anzeigen wie von Ihnen beabsichtigt aufgerufen werden.
3. Aktualisieren Sie die Identitätsprovider oder die Anzeige für das
Anmeldewidget im {{site.data.keyword.appid_short_notm}}-Dashboard.
4. Wiederholen Sie die Schritte 1 und 2, um die sofort implementierten
Änderungen anzuzeigen. Aktualisierungen des App-Codes sind nicht erforderlich.

Falls Probleme auftreten, finden Sie im Abschnitt [Fehlerbehebung bei {{site.data.keyword.appid_short_notm}}](/docs/services/appid/ts_index.html) weitere Informationen.

## Nächste Schritte
{: #next-appid}

Hervorragend! Sie haben Ihre App mit einer Sicherheitsstufe ausgestattet. Nutzen Sie diesen Schwung und probieren
Sie gleich eine der folgenden Optionen aus:

* Lesen Sie weitere Informationen zu
{{site.data.keyword.appid_short_notm}} in der
[Dokumentation](/docs/services/appid/index.html). Dort
erfahren Sie auch, wie Sie alle gebotenen Funktionen nutzen können.
* Starter-Kits bieten eine der schnellsten Möglichkeiten zur Nutzung des Leistungsspektrums von {{site.data.keyword.cloud_notm}}. Im [Dashboard für Entwickler für Mobilgeräte](https://cloud.ibm.com/developer/mobile/dashboard) werden die verfügbaren Starter-Kits angezeigt. Sie können den Code herunterladen und die App ausführen.
