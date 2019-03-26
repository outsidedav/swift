---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:gif: .gif}
{:tip: .tip}

# Hoch verfügbare und sichere Datenbank erstellen
{: #create-database-cluster}

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
https://cloud.ibm.com/catalog/services/hyper-protect-dbaas.

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
{: #create_starter}

Sie benötigen ein Starter-Kit, das auf dem serverseitigen
Swift-Web-Framework "Kitura" basiert.

![Featuredetails](videos/StarterKit.gif){: gif}

Verwenden Sie ein vorhandenes Projekt, das aus diesem Starter-Kit erstellt wurde, oder erstellen Sie ein neues Projekt.

1. Öffnen Sie das {{site.data.keyword.cloud_notm}}-Dashboard für App-Services unter der Adresse https://cloud.ibm.com/developer/appservice/dashboard.

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
    <td> Das Kennwort für die Benutzer-ID des Administrators, das Sie bei den Anweisungen im Abschnitt [Datenbankcluster erstellen](#create_dbcluster) angegeben haben. </td>
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
   ```swift
   .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
   ```
   {: codeblock}

  * Fügen Sie im Abschnitt "targets" die Abhängigkeit "MongoKitten" zur
folgenden Zeile hinzu. **Hinweis:** Die Werte müssen in
einer einzigen Zeile angegeben werden.
   ```swift
   .target(name: "Application", dependencies: [ "Kitura",
   "CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
   ```
   {: codeblock}

5. Bearbeiten Sie
die Datei `Sources/Application/Application.swift`, damit die
Konnektivität zu MongoDB mittels MongoKitten initialisiert wird.

  * Importieren Sie das MongoKitten-SDK:
    ```swift
	  import MongoKitten
	  ```
	  {: codeblock}

  * Fügen Sie die Klasse `ApplicationServices` hinzu:
    ```swift
	  cclass ApplicationServices {
	  /* Service references */
	  public let mongoDBService: MongoKitten.Database
	  public let myCredFile = "/swift-project/cred.json"

    public init() throws {
        /* Read credentials from json file cred.json */
        struct ResponseData: Decodable {
            var uri: String
	        }
	        let data = try? Data(contentsOf: URL(fileURLWithPath: myCredFile))
	        let decoder = JSONDecoder()
	        let jsonData = try decoder.decode(ResponseData.self, from: data!)

        /* Run service initializers */
        let server = try Server(jsonData.uri)
        mongoDBService = MongoKitten.Database(named: "admin", atServer: 		server)
    }
	}
	```
	{: codeblock}

  * Fügen Sie in der öffentlichen Klasse `App` die
folgenden Zeilen hinzu, um die Datenbankverbindung zu initialisieren:
    ```swift
	  public class App {
	  ...
	  let services: ApplicationServices

	  public init() throws {
	  /* Services */
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

	```swift
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

```
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
{: #deploy-dbcluster}

Sie können die Anwendung mit den erforderlichen Build-Tools [lokal](/docs/swift/create_app_cli.html#swift-install-tools) ausführen oder in {{site.data.keyword.cloud_notm}} bereitstellen.

Klicken Sie auf **In Cloud bereitstellen**, um eine Bereitstellungstoolchain im Dashboard zu erstellen. Richten Sie Ihre Bereitstellungsmethode gemäß den Anweisungen für die Methode ein, die Sie auswählen:
  * **In [Kubernetes](/docs/apps/deploying/containers.html#containers) bereitstellen**. Mit dieser Option wird ein Cluster mit Hosts erstellt, die als Workerknoten bezeichnet werden, um hoch verfügbare Anwendungscontainer bereitzustellen und zu verwalten. Sie können einen Cluster erstellen oder die Bereitstellung in einem vorhandenen Cluster vornehmen.
  * **In Cloud Foundry bereitstellen**. Mit dieser Option wird die cloudnative App bereitgestellt, ohne dass Sie die zugrunde liegende Infrastruktur verwalten müssen. Wenn Ihr Konto über Zugriff auf {{site.data.keyword.cfee_full_notm}} verfügt, können Sie als Bereitstellertyp entweder **[Public Cloud](/docs/cloud-foundry-public/about-cf.html#about-cf)** oder **[Enterprise Environment](/docs/cloud-foundry-public/cfee.html#cfee)** auswählen, das zum Erstellen und Verwalten isolierter Umgebungen für das exklusive Hosting von Cloud Foundry-Anwendungen für Ihr Unternehmen verwendet werden kann.
  * **In einem [virtuellen Server](/docs/apps/vsi-deploy.html#vsi-deploy)** bereitstellen. Mit dieser Option wird eine Instanz eines virtuellen Servers eingerichtet, ein Image mit Ihrer App geladen, eine DevOps-Toolchain erstellt und der erste Bereitstellungszyklus initiiert.
