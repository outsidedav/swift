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

# Creazione di un database protetto e altamente disponibile

Per avvalerti appieno di un database protetto e altamente disponibile, integri della logica supplementare nella tua applicazione. Utilizzando i frammenti di codice forniti, puoi creare e accedere a un database MongoDB. 

Attualmente, il linguaggio di programmazione supportato per l'utilizzo di {{site.data.keyword.ihsdbaas_full}}
è Swift 4.0 con MongoKitten SDK 4.0.0.

## Passo 1. Creazione di un cluster di database
{: #create_dbcluster}

1. Accedi alla schermata di configurazione del servizio {{site.data.keyword.ihsdbaas_full}} all'indirizzo
https://console.bluemix.net/catalog/services/hyper-protect-dbaas.

2. Fornisci le seguenti informazioni:

	<dl>
		<dt>Nome servizio:</dt>
		<dd>Un nome per il servizio database.</dd>

    <dt>Scegli una regione/ubicazione in cui eseguire la distribuzione:</dt>
    <dd>Seleziona il centro dati in cui si trova il database.</dd>

    <dt>Seleziona un gruppo di risorse:</dt>
    <dd>Se nessun gruppo di risorse è selezionabile, puoi crearne uno nel dashboard IBM Cloud.</dd>

		<dt>Nome cluster/serie di repliche:</dt>
		<dd>Un nome per il cluster di database.</dd>

		<dt>Nome amministratore database:</dt>
		<dd>Un ID utente per l'amministratore del database (DBA).</dd>

		<dt>Password amministratore database:</dt>
		<dd>Una password per l'ID utente DBA.</dd>

    <dt>Conferma password amministratore database:</dt>
    <dd>Conferma la password per l'ID utente DBA.</dd>

		<dt>Tipo di database:</dt>
		<dd>Seleziona il tipo di database. Attualmente è supportato solo MongoDB.</dd>

    <dt>Accordo di licenza:</dt>
    <dd>Leggi l'accordo di licenza e seleziona la casella per confermare che lo accetti.</dd>

    <dt>Prezzi:</dt>
		<dd>L'attuale soluzione fornisce solo una singola categoria di prezzi, che è quella gratuita. Nelle versioni successive, puoi selezionare la categoria di prezzi.</dd>
	</dl>

3. Fai clic su **Crea**.

  Viene visualizzato il dashboard {{site.data.keyword.cloud_notm}}. Potresti dover aggiornare il tuo browser per visualizzare il nuovo cluster, che è elencato nella sezione Servizi.</p></li>

4. Quando selezioni il servizio, vengono visualizzate le informazioni sul cluster.

5. Nella scheda Gestisci delle informazioni sul cluster, fai clic su **APRI**.

	Viene visualizzato il dashboard {{site.data.keyword.ihsdbaas_full}}.

6. Raccogli i nomi host e i numeri di porta delle tre istanze di database create che appartengono al tuo cluster di database. Ti servono i nomi host, i numeri di porta e le credenziali utente per i passi nella sezione [Connessione al database](#connect_db).

## Passo 2. Creazione di un progetto utilizzando un kit starter
{: #create_with_starter}

Ti serve un kit starter basato sul framework web Swift lato server Kitura.

![Dettagli della funzione](videos/StarterKit.gif){: gif}

Utilizza un progetto esistente che era stato creato da questo kit starter oppure crea un nuovo progetto.

1. Apri il dashboard {{site.data.keyword.cloud_notm}} App Service all'indirizzo https://console.bluemix.net/developer/appservice/dashboard.

2. Seleziona la scheda **Kit starter**.

3. Seleziona il kit starter **Swift Kitura** Backend for Frontend. Assicurati di non confonderlo con il kit starter Swift Kitura Basic App Web.
  Viene visualizzata la pagina Create new project.

4. Immetti i dettagli del progetto e fai clic su **Create Project**.
  Viene visualizzata la pagina del progetto.

5. Nella pagina del progetto, fai clic su **Download Code**.

6. Espandi il file compresso nella directory del tuo progetto.

## Passo 3. Connessione al database
{: #connect_db}

Per garantire un trasferimento dati protetto, scarica il file dell'autorità di certificazione (o CA, certificate authority) da
https://api.hypersecuredbaas.ibm.com/cert.pem, and copy it to your project directory.

1. Passa alla directory del tuo progetto dove si trovano i file di codice scaricati espansi.

2. Crea un file JSON denominato `cred.json` per memorizzare le credenziali di accesso al cluster di database.

3. Immetti i valori raccolti dai passi in [Creazione di un cluster di database](#create_dbcluster). I valori devono essere specificati in una singola riga.
  ```hljs
  {
  "uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
  <Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
   /admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
  }
  ```
  {: codeblock}

  Dove:
  <table>
  <tr>
    <th> Parametro </th>
    <th> Descrizione </th>
  </tr>
  <tr>
    <td> &lt;<em>admin_ID</em>&gt; </td>
    <td> È l'ID utente dell'amministratore del database come specificato in [Creazione di un cluster di database](#create_dbcluster).
  </td>
  </tr>
  <tr>
    <td> &lt;<em>admin_pwd</em>&gt; </td>
    <td> È l'ID utente della password dell'amministratore come specificato in [Creazione di un cluster di database](#create_dbcluster). </td>
  </tr>
  <tr>
    <td> &lt;<em>Hostname_i</em>&gt; </td>
    <td> È una replica del database <em>i</em> (<em>i</em>=1,2,3) come restituita in [Creazione di un cluster di database](create_dbcluster). </td>
  </tr>
  <tr>
    <td> &lt;<em>PortNumber_i</em>&gt; </td>
    <td> È il numero di porta <em>i</em> (<em>i</em>=1,2,3) come restituito in [Creazione di un cluster di database](#create_dbcluster). </td>
  </tr>
  <tr>
    <td> &lt;<em>CA_file</em>&gt; </td>
    <td> È il nome del file CA scaricato. Durante la distribuzione, viene copiato nella directory `/swift-project`.</td>
  </tr>
  </table>

4. Modifica il file `Package.swift` per aggiungere le dipendenze di pacchetto per l'utilizzo dell'SDK
MongoKitten.

  * Nella sezione dependencies, aggiungi la seguente riga:
   ```hljs
   .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
   ```
   {: codeblock}

  * Nella sezione targets, aggiungi la dipendenza "MongoKitten" alla seguente riga. **Nota:** i valori devono essere specificati in una singola riga.
   ```hljs
   .target(name: "Application", dependencies: [ "Kitura",
   "CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
   ```
   {: codeblock}

5. Modifica il file `Sources/Application/Application.swift` per inizializzare la connettività a MongoDB utilizzando MongoKitten.

  * Importa l'SDK MongoKitten:
    ```
	import MongoKitten
	```
	{: codeblock}

  * Aggiungi la classe `ApplicationServices`:
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

  * Nella classe pubblica `App`, aggiungi le seguenti righe per inizializzare la connessione al database:
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

## Passo 4. Verifica della connessione al database
{: #verify_database}

1. Verifica la tua connessione al database modificando il file `Sources/Application/Application.swift` per aggiungere un comando per verificare la connessione al database.
Ad esempio, aggiungi il seguente comando in `class ApplicationServices`:

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

Dopo che hai distribuito la tua applicazione nel [Passo 6](#use-step6), viene visualizzato il seguente messaggio, se la tua connessione al database viene stabilita correttamente:

```hljs
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## Passo 5. Integrazione del codice dell'applicazione
{: #embed_appcode}

Puoi ora aggiungere il tuo codice dell'applicazione al progetto. Per ulteriori informazioni sull'utilizzo dell'API MongoKitten, vedi http://beta.openkitten.org/tutorials/.

## Passo 6. Distribuzione della tua applicazione
{: #deploy_app}

Puoi eseguire l'applicazione in locale con gli strumenti di creazione necessari oppure in {{site.data.keyword.cloud_notm}} (Cloud Foundry o Kubernetes Cluster) tramite la {{site.data.keyword.dev_cli_notm}}.

Puoi eseguire la tua applicazione in locale sul tuo sistema host, in Cloud Foundry oppure in un cluster Kubernetes.

1. [Installa](/docs/cli/reference/bluemix_cli/get_started.html) la CLI {{site.data.keyword.cloud_notm}}

2. Installa il plug-in degli strumenti per gli sviluppatori utilizzando il comando `ibmcloud plugin install dev`.

3. Distribuisci l'applicazione a un [sistema locale](#deploy_local), [Cloud Foundry](#deploy_cf) o un [cluster Kubernetes](#deploy_cluster).

### Distribuzione in locale
{: #deploy_local}

1. Assicurati che Docker sia installato e in esecuzione sul tuo sistema host locale. Puoi scaricare Docker da https://www.docker.com/community-edition#/download.

2. Passa alla directory con i tuoi file del progetto.

3. Per distribuire l'applicazione sul tuo computer locale, immetti i comandi:
	```
	$ ibmcloud dev build
	...
	$ ibmcloud dev run
	```
	{: codeblock}

	Questo passo crea la tua applicazione e la esegue in locale all'interno di un contenitore Docker.

### Distribuzione a Cloud Foundry
{: #deploy_cf}

1. Passa alla directory con i tuoi file del progetto.

2. Accedi al tuo account IBM Cloud e imposta la regione su `us-south`, come mostrato qui:
  ```hljs
  $ ibmcloud login -a https://api.ng.bluemix.net
  $ ibmcloud target -o &lt;<em>your-organization</em>&gt; -s &lt;<em>your-space</em>&gt;
  ```
  {: codeblock}

  **Nota:** l'immissione del comando `ibmcloud login -a https://api.ng.bluemix.net` imposta automaticamente la regione su **us-south**.

3. Per distribuire l'applicazione a Cloud Foundry, immetti questo comando:
  ```
  $ ibmcloud dev deploy
  ```
  {: codeblock}

  Ricevi un link selezionabile mediante clic all'ubicazione dove è ospitata la tua applicazione.

### Distribuzione a un cluster Kubernetes
{: #deploy_cluster}

1. Crea un cluster Kubernetes all'indirizzo https://console.bluemix.net/containers-kubernetes/clusters.

2. Fai clic su **Crea cluster**. La scheda Accesso visualizza le informazioni su come accedere al cluster Kubernetes creato.

3. Per visualizzare le informazioni sul cluster Kubernetes, apri il dashboard dell'applicazione {{site.data.keyword.cloud_notm}}. Il dashboard visualizza un elenco dei tuoi servizi, quali i cluster creati, i cluster di database, le applicazioni cloud foundry e i servizi cloud foundry.

4. Passa alla directory con i tuoi file del progetto.

5. Esegui l'accesso al tuo account {{site.data.keyword.cloud_notm}} e imposta la regione su us-south, come mostrato qui:
  ```hljs
  $ ibmcloud login -a https://api.ng.bluemix.net
  $ ibmcloud target -o <your-organization> -s <your-space>
  ```
  {: codeblock}

  **Nota:** l'immissione del comando `ibmcloud login -a https://api.ng.bluemix.net` imposta automaticamente la regione su **us-south**.

6. Per distribuire la tua applicazione in Kubernetes, immetti questo comando:
  ```
  $ ibmcloud dev deploy -t container
  ```
  {: codeblock}

  Ti viene richiesto il nome del tuo cluster Kubernetes e del registro Docker. Una volta fornite queste informazioni, la tua applicazione viene distribuita al cluster Kubernetes.
