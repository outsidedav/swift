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

# Hoch verfügbare und sichere Datenbank erstellen

Damit Sie eine hoch verfügbare und sichere Datenbank optimal nutzen
können, müssen Sie zusätzliche Logik in Ihre Anwendung integrieren. Mithilfe
der bereitgestellten Code-Snippets können Sie eine MongoDB-Datenbank erstellen
und auf diese Datenbank zugreifen. 

Gegenwärtig wird zur Verwendung von
{{site.data.keyword.ihsdbaas_full}} als Programmiersprache Swift 4.0
mit MongoKitten SDK 4.0.0 unterstützt.

## Schritt 1. Datenbankcluster erstellen
{: #create_dbcluster}

1. Greifen Sie auf die
Konfigurationsanzeige für den
{{site.data.keyword.ihsdbaas_full}}-Service zu, die Sie unter der
folgenden Adresse aufrufen:
https://console.bluemix.net/catalog/services/hyper-protect-dbaas.

2. Geben Sie die folgenden Informationen an:

	<dl>
		<dt>Servicename:</dt>
		<dd>Der Name für den Datenbankservice.</dd>

    <dt>Region/Standort für die Bereitstellung auswählen:</dt>
    <dd>Wählen Sie das Rechenzentrum aus, in dem sich die Datenbank befinden soll.</dd>

    <dt>Ressourcengruppe auswählen:</dt>
    <dd>Falls keine Ressourcengruppe auswählbar ist, können Sie im IBM Cloud-Dashboard eine Ressourcengruppe erstellen.</dd>

		<dt>Cluster-/Replikatgruppenname:</dt>
		<dd>Der Name für den Datenbankcluster.</dd>

		<dt>Name des Datenbankadministrators:</dt>
		<dd>Eine Benutzer-ID für den Datenbankadministrator (DBA).</dd>

		<dt>Kennwort des Datenbankadministrators:</dt>
		<dd>Das Kennwort für die Benutzer-ID des DBA.</dd>

    <dt>Kennwort des Datenbankadministrators bestätigen:</dt>
    <dd>Bestätigen Sie das Kennwort für die Benutzer-ID des DBA.</dd>

		<dt>Datenbanktyp:</dt>
		<dd>Wählen Sie den Datenbanktyp aus. Gegenwärtig wird ausschließlich MongoDB unterstützt.</dd>

    <dt>Lizenzvereinbarung:</dt>
    <dd>Lesen Sie die Lizenzvereinbarung und wählen Sie das Kontrollkästchen aus, um Ihre Zustimmung zu bestätigen.</dd>

    <dt>Preisstruktur:</dt>
		<dd>Die aktuelle Lösung bietet nur eine einzige - gebührenfreie - Kategorie in der Preisstruktur. In späteren Versionen können Sie die Preiskategorie auswählen.</dd>
	</dl>

3. Klicken Sie auf **Erstellen**.

  Das {{site.data.keyword.cloud_notm}}-Dashboard wird angezeigt. Möglicherweise müssen Sie Ihren Browser aktualisieren, damit der neue Cluster
angezeigt wird, der im Abschnitt "Services" aufgelistet ist.</p></li>

4. Wenn Sie den Service auswählen, werden die Clusterinformationen angezeigt.

5. Klicken Sie auf der Registerkarte "Verwalten" der
Clusterinformationen auf **Öffnen**.

	Das {{site.data.keyword.ihsdbaas_full}}-Dashboard wird
angezeigt.

6. Rufen Sie die Hostnamen und die Portnummern der drei erstellten Datenbankinstanzen ab, die zu Ihrem Datenbankcluster gehören. Sie benötigen die
Hostnamen, die Portnummern und die Benutzerberechtigungen für die Schritte im
Abschnitt [Verbindung zur Datenbank herstellen](#connect_db).

## Schritt 2. Projekt mithilfe eines Starter-Kits erstellen
{: #create_with_starter}

Sie benötigen ein Starter-Kit, das auf dem serverseitigen
Swift-Web-Framework "Kitura" basiert.

![Featuredetails](videos/StarterKit.gif){: gif}

Verwenden Sie ein vorhandenes Projekt, das aus diesem Starter-Kit erstellt wurde, oder erstellen Sie ein neues Projekt.

1. Öffnen Sie das {{site.data.keyword.cloud_notm}}-Dashboard für
App-Services unter der Adresse
"https://console.bluemix.net/developer/appservice/dashboard".

2. Wählen Sie die Registerkarte **Starter-Kits** aus.

3. Wählen Sie das BFF-Starter-Kit **Swift
Kitura** aus (BFF = Backend for
Frontend). Verwechseln Sie es nicht mit dem Basis-Starter-Kit von Swift Kitura
für Web-Apps.
  Die Seite "Neues Projekt erstellen" wird angezeigt.

4. Geben Sie die Projektdetails ein und klicken Sie auf **Projekt erstellen**.
  Die
Projektseite wird angezeigt.

5. Klicken Sie auf der Projektseite auf **Code herunterladen**.

6. Erweitern Sie die komprimierte Datei in Ihrem Projektverzeichnis. 

## Schritt 3. Verbindung zur Datenbank herstellen
{: #connect_db}

Um die Sicherheit der Datenübertragung zu gewährleisten, laden Sie
die Datei der Zertifizierungsstelle unter der folgenden Adresse herunter und
kopieren Sie sie in Ihr Projektverzeichnis:
https://api.hypersecuredbaas.ibm.com/cert.pem, and copy it to your project directory.

1. Wechseln Sie in Ihr Projektverzeichnis, das die dekomprimierten
Codedateien des Downloads enthält.

2. Erstellen Sie eine JSON-Datei namens `cred.json`, um
Ihre Zugriffsberechtigungsnachweise für den Datenbankcluster zu speichern.

3. Geben Sie die Werte ein, die Sie in den Schritten im Abschnitt
[Datenbankcluster erstellen](#create_dbcluster) gesammelt haben. Die Werte müssen in einer einzigen Zeile angegeben werden.
  ```hljs
  {
  "uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
  <Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
   /admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
  }
  ```
  {: codeblock}

  Hierbei gilt Folgendes:
  <table>
  <tr>
    <th> Parameter </th>
    <th> Beschreibung </th>
  </tr>
  <tr>
    <td> &lt;<em>admin_ID</em>&gt; </td>
    <td> Die Benutzer-ID des Datenbankadministrators, die Sie bei den Anweisungen im Abschnitt [Datenbankcluster erstellen](#create_dbcluster) angegeben haben.
  </td>
  </tr>
  <tr>
    <td> &lt;<em>admin_pwd</em>&gt; </td>
    <td> Das Kennwort für die Benutzer-ID des Datenbankadministrators, das Sie bei den Anweisungen im Abschnitt [Datenbankcluster erstellen](#create_dbcluster) angegeben haben. </td>
  </tr>
  <tr>
    <td> &lt;<em>Hostname_i</em>&gt; </td>
    <td> Ein Datenbankreplikat <em>i</em> (<em>i</em>=1,2,3), das im Abschnitt [Datenbankcluster erstellen](create_dbcluster) zurückgegeben wurde. </td>
  </tr>
  <tr>
    <td> &lt;<em>PortNumber_i</em>&gt; </td>
    <td> Eine Portnummer <em>i</em> (<em>i</em>=1,2,3), die im Abschnitt [Datenbankcluster erstellen](#create_dbcluster) zurückgegeben wurde. </td>
  </tr>
  <tr>
    <td> &lt;<em>CA_file</em>&gt; </td>
    <td> Der Dateiname der heruntergeladenen Datei der Zertifizierungsstelle. Während der Bereitstellung wird diese Datei in das Verzeichnis `/swift-project` kopiert.</td>
  </tr>
  </table>

4. Bearbeiten Sie die Datei `Package.swift` und fügen
Sie Paketabhängigkeiten für die Verwendung des
MongoKitten-SDK hinzu.

  * Fügen Sie im Abschnitt "dependencies" die folgende Zeile hinzu:
   ```hljs
   .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
   ```
   {: codeblock}

  * Fügen Sie im Abschnitt "targets" die Abhängigkeit "MongoKitten" zur
folgenden Zeile hinzu. **Hinweis:** Die Werte müssen in
einer einzigen Zeile angegeben werden.
   ```hljs
   .target(name: "Application", dependencies: [ "Kitura",
   "CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
   ```
   {: codeblock}

5. Bearbeiten Sie
die Datei `Sources/Application/Application.swift`, damit die
Konnektivität zu MongoDB mittels MongoKitten initialisiert wird.

  * Importieren Sie das MongoKitten-SDK:
    ```
	import MongoKitten
	```
	{: codeblock}

  * Fügen Sie die Klasse `ApplicationServices` hinzu:
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

  * Fügen Sie in der öffentlichen Klasse `App` die
folgenden Zeilen hinzu, um die Datenbankverbindung zu initialisieren:
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

## Schritt 4. Datenbankverbindung überprüfen
{: #verify_database}

1. Überprüfen Sie Ihre Datenbankverbindung, indem Sie die Datei
`Sources/Application/Application.swift` bearbeiten und einen
Befehl zum Testen der Datenbankverbindung hinzufügen.
Fügen Sie beispielsweise
den folgenden Befehl in der
Klasse `ApplicationServices` hinzu:

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

Nachdem Sie Ihre Anwendung in [Schritt
6](#use-step6) bereitgestellt haben, wird die folgende Nachricht ausgegeben, falls
die Verbindung zur Datenbank erfolgreich hergestellt werden konnte:

```hljs
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## Schritt 5. Anwendungscode integrieren
{: #embed_appcode}

Jetzt können Sie Ihren eigenen Anwendungscode zum Projekt hinzufügen. Weitere
Informationen zum Arbeiten mit der MongoKitten-API finden Sie unter der Adresse "http://beta.openkitten.org/tutorials/".

## Schritt 6. Anwendung bereitstellen
{: #deploy_app}

Sie können die Anwendung mit den erforderlichen Build-Tools lokal oder in
{{site.data.keyword.cloud_notm}} (Cloud Foundry oder
Kubernetes-Cluster) mit dem {{site.data.keyword.dev_cli_notm}}
ausführen.

Sie können Ihre Anwendung lokal auf Ihrem Hostsystem, in Cloud Foundry
oder in einem Kubernetes-Cluster ausführen.

1. [Installieren](/docs/cli/reference/bluemix_cli/get_started.html)
Sie die {{site.data.keyword.cloud_notm}}-CLI.

2. Installieren Sie das Plug-in für Entwicklertools unter Verwendung des
Befehls `ibmcloud plugin install dev`.

3. Stellen Sie die Anwendung für ein [lokales
System](#deploy_local), für [Cloud Foundry](#deploy_cf) oder für einen
[Kubernetes-Cluster](#deploy_cluster) bereit.

### Anwendung lokal bereitstellen
{: #deploy_local}

1. Stellen Sie sicher, dass Docker auf Ihrem lokalen Hostsystem
installiert ist und ausgeführt wird. Sie können Docker unter der Adresse
"https://www.docker.com/community-edition#/download" herunterladen.

2. Wechseln Sie in das Verzeichnis, das Ihre Projektdateien enthält.

3. Geben Sie die folgenden Befehle ein, um die Anwendung auf Ihrem
lokalen Computer bereitzustellen:
	```
	$ ibmcloud dev build
	...
	$ ibmcloud dev run
	```
	{: codeblock}

	Mit diesem Schritt wird Ihre Anwendung erstellt und lokal in einem
Docker-Container ausgeführt.

### Anwendung in Cloud Foundry bereitstellen
{: #deploy_cf}

1. Wechseln Sie in das Verzeichnis, das Ihre Projektdateien enthält.

2. Melden Sie sich bei Ihrem IBM Cloud-Konto an und legen Sie die Region
wie nachfolgend gezeigt mit `us-south` fest:
  ```hljs
  $ ibmcloud login -a https://api.ng.bluemix.net
  $ ibmcloud target -o &lt;<em>your-organization</em>&gt; -s &lt;<em>your-space</em>&gt;
  ```
  {: codeblock}

  **Hinweis:** Bei Ausgabe des Befehls
`ibmcloud login -a https://api.ng.bluemix.net` wird
automatisch **us-south** als Region festgelegt.

3. Geben Sie den folgenden Befehl ein, um die Anwendung in Cloud Foundry
bereitzustellen:
  ```
  $ ibmcloud dev deploy
  ```
  {: codeblock}

  Sie empfangen daraufhin einen Link, auf den Sie klicken können und der
Sie zu der Position führt, an der Ihre Anwendung gehostet wird.

### Anwendung in einem Kubernetes-Cluster bereitstellen
{: #deploy_cluster}

1. Erstellen Sie einen Kubernetes-Cluster unter der Adresse "https://console.bluemix.net/containers-kubernetes/clusters".

2. Klicken Sie auf **Cluster erstellen**. Auf der
Registerkarte "Zugriff" werden Informationen dazu angezeigt, wie Sie auf den
erstellten Kubernetes-Cluster zugreifen können.

3. Wenn Sie Informationen zum Kubernetes-Cluster anzeigen möchten,
öffnen Sie das App-Dashboard von {{site.data.keyword.cloud_notm}}. Im
Dashboard wird eine Liste Ihrer Services angezeigt, z. B. erstellte Cluster,
Datenbankcluster, Cloud Foundry-Apps und Cloud Foundry-Services.

4. Wechseln Sie in das Verzeichnis, das Ihre Projektdateien enthält.

5. Melden Sie sich bei Ihrem {{site.data.keyword.cloud_notm}}-Konto an und legen Sie die Region
wie nachfolgend gezeigt mit "us-south" fest:
  ```hljs
  $ ibmcloud login -a https://api.ng.bluemix.net
  $ ibmcloud target -o <your-organization> -s <your-space>
  ```
  {: codeblock}

  **Hinweis:** Bei Ausgabe des Befehls
`ibmcloud login -a https://api.ng.bluemix.net` wird
automatisch **us-south** als Region festgelegt.

6. Geben Sie den folgenden Befehl ein, um Ihre Anwendung in Kubernetes
bereitzustellen:
  ```
  $ ibmcloud dev deploy -t container
  ```
  {: codeblock}

  Sie werden aufgefordert, den Namen Ihres Kubernetes-Clusters und die
Docker-Registry anzugeben. Nachdem Sie die Informationen bereitgestellt haben,
wird Ihre Anwendung im Kubernetes-Cluster bereitgestellt.
