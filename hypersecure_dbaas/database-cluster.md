---

copyright:
  years: 2018
lastupdated: "2018-03-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:gif: .gif}


# Highly available and secure database

To take full advantage of a highly available and secure database, you embed
additional logic into your application. The embedded code snippets enable the
creation of and access to a MongoDB database.

Currently, the supported programming language for using {{site.data.keyword.ihsdbaas_full}}
is Swift 4.0 with MongoKitten SDK 4.0.0.

## Step 1: Creating a database cluster
{: #use-step1}

1. Access the {{site.data.keyword.ihsdbaas_full}} service configuration screen at
https://console.bluemix.net/catalog/services/hypersecure-dbaas.
2. Enter the required values.

	<dl>
		<dt>Service name:</dt>
		<dd>A name for the database service.</dd>

    <dt>Choose a region/location to deploy in:</dt>
    <dd>Select the data center where the database will be located.</dd>

    <dt>Select a resource group:</dt>
    <dd>If no resource group is selectable, you can create one on the IBM Cloud dashboard.</dd>

		<dt>Cluster/Replicaset Name:</dt>
		<dd>A name for the database cluster.</dd>

		<dt>Database Admin Name:</dt>
		<dd>A user ID for the database administrator (DBA).</dd>

		<dt>Database Admin Password:</dt>
		<dd>A password for the DBA user ID.</dd>

    <dt>Confirm Database Admin Password:</dt>
    <dd>Confirm the password for the DBA user ID.</dd>

		<dt>Database Type:</dt>
		<dd>Select the database type. Currently, only MongoDB is supported.</dd>

    <dt>License Agreement:</dt>
    <dd>After reading the license agreement, check the box to confirm your agreement.</dd>

    <dt>Pricing:</dt>
		<dd>The current solution provides only one pricing category, which is free
		of charge. In later versions, you will be able to select the pricing category.</dd>
	</dl>

3. Click **Create**.

  The {{site.data.keyword.cloud_notm}} dashboard is displayed. You might have to refresh your browser to see the new cluster, which is listed in the Services section.</p></li>

4. When you select the service, the cluster information is displayed.
5. In the Manage tab of the cluster information, click **OPEN**.

	The {{site.data.keyword.ihsdbaas_full}} dashboard is displayed.
6. Obtain the host names and the port numbers of the three created database
instances belonging to your database cluster.

You will need the host names, the port numbers, and the user credentials in [Step 3](#use-step3).


## Step 2: Creating a project by using a starter kit
{: #use-step2}

You need a starter kit that is based on the server-side Swift web framework Kitura.

![Feature Details](videos/StarterKit.gif){: gif}

Use an existing project that was created from this starter kit, or create a new project.

1. Open the {{site.data.keyword.cloud_notm}} App Service dashboard at https://console.bluemix.net/developer/appservice/dashboard.
2. Select the **Starter Kits** tab.
3. Select the `Swift Kitura` Backend for Frontend starter kit. Be sure not to confuse it with the Swift Kitura Basic Web-App Starter Kit.
  The Create new project page is displayed.
4. Enter the project details and click **Create Project**.
  The project page is displayed.
5. On the projects page, click **Download Code**.
6. Expand the downloaded zip file to your project directory.


## Step 3: Connecting to the database
{: #use-step3}

To ensure secure data transfer, download the Certificate Authority (CA) file from
https://api.hypersecuredbaas.ibm.com/cert.pem and copy it to your project directory.

1. Change to your project directory, which contains the expanded download code
files.
2. Create a JSON file named `cred.json` to store your
access credentials to the database cluster.
3. Enter the values you obtained as a result of [Step 1](#use-step1). The values must be specified in a single line.

	```hljs
	{
	"uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
	<Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
	/admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
	}
	```
	{: codeblock}

	Where:

	<table>
	  <tr>
	    <th> Parameter </th>
	    <th> Description </th>
	  </tr>
	  <tr>
	    <td> &lt;<em>admin_ID</em>&gt; </td>
	    <td> Is the user ID of the database administrator as specified in [Step 1](use-step1.html).
	  </td>
	  </tr>
	  <tr>
	    <td> &lt;<em>admin_pwd</em>&gt; </td>
	    <td> Is the user ID of the administrator password as specified in [Step 1](use-step1.html). </td>
	  </tr>
	  <tr>
	    <td> &lt;<em>Hostname_i</em>&gt; </td>
	    <td> Is a database replica <em>i</em> (<em>i</em>=1,2,3) as returned in [Step 1](use-step1.html). </td>
	  </tr>
	  <tr>
	    <td> &lt;<em>PortNumber_i</em>&gt; </td>
	    <td> Is a port number <em>i</em> (<em>i</em>=1,2,3) as returned in [Step 1](use-step1.html). </td>
	  </tr>
	  <tr>
	    <td> &lt;<em>CA_file</em>&gt; </td>
	    <td> Is the file name of the downloaded CA file. During deployment, it will be copied to the directory `/swift-project`.</td>
	  </tr>
	</table>

4. Edit the `Package.swift` file to add package dependencies for the use of the
MongoKitten SDK.

	a. In the dependencies section, add the following line:
			```hljs
			 .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
			```
			{: codeblock}
	b. In the targets section, add the dependency "MongoKitten" to the following line. **Note:** Please be aware that the values must be specified in a single line.
			```hljs
			 .target(name: "Application", dependencies: [ "Kitura",
                        				"CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
			```
			{: codeblock}
5. Edit the `Sources/Application/Application.swift` file to initialize a connectivity
to MongoDB using MongoKitten.

	a. Import the MongoKitten SDK:
		```
		import MongoKitten
		```
		{: codeblock}

	b. Add the class `ApplicationServices`:

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

	c. In the public class `App`, add the following lines to initialize the database connection:

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


## Step 4: Verifying the database connection
{: use-step4}

1. Verify your database connection by editing the file `Sources/Application/Application.swift` to add a command to test the database connection.
For example, add the following command in the `class ApplicationServices`:

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

After you deploy your application in [Step 6](#use-step6), the following message is displayed, if
your connection to the database succeeds:

```hljs
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## Step 5: Embedding your application code
{: #use-step5}

You might want to add your own application code to the project at this point. You
can find more information about working with the MongoKitten API at
http://beta.openkitten.org/tutorials/.


## Step 6: Deploying your application
{: #use-step6}

You can run the application locally on your host system if you install the necessary
build tools, or in the {{site.data.keyword.cloud_notm}} (Cloud Foundry or Kubernetes Cluster) through the {{site.data.keyword.dev_cli_notm}}.

You can run your application locally on your host system, in Cloud
Foundry, or in a Kubernetes Cluster.

1. [Install](/docs/cli/reference/bluemix_cli/get_started.html) the {{site.data.keyword.cloud_notm}} CLI
2. Install the developer tools plug-in by using the command `bx plugin install dev`.
3.  Deploy the application.

	* Deploy the application locally
		1. Ensure that Docker is installed and running on your local host system. You can download Docker from https://www.docker.com/community-edition#/download.
		2. Switch to the directory containing your project files.
		3. To deploy the application on your local computer, enter the commands:
	    ```
	    $ bx dev build
		...
		$ bx dev run
	    ```
	    {: codeblock}

	    This step will build your application and run it locally on your computer inside a Docker container.


	* Deploying the application to the Cloud Foundry
		1. Switch to the directory containing your project files.
		2. Log in to your IBM Cloud account, and set the region to `us-south`, as shown here:

			```hljs
		    $ bx login -a https://api.ng.bluemix.net
			...
			$ bx target -o &lt;<em>your-organization</em>&gt; -s &lt;<em>your-space</em>&gt;
			```
			{: codeblock}

    		**Note:** Issuing the command `bx login -a https://api.ng.bluemix.net` automatically sets the region to **us-south**.    
		3. To deploy the application to the Cloud Foundry, enter this command:
			 ```
		    $ bx dev deploy
		    ```
		    {: codeblock}

			You will receive a clickable link to the location where your application is hosted.


	* Deploying the application to a Kubernetes Cluster
		1. Create a Kubernetes cluster at https://console.bluemix.net/containers-kubernetes/clusters.
		2. Click **Create Cluster**. The Access tab displays information on how to access the created Kubernetes cluster.
		3. To display information about the Kubernetes cluster, open the {{site.data.keyword.cloud_notm}} app dashboard. The dashboard displays a list of your services, such as created clusters,
database clusters, cloud foundry apps, cloud foundry services.

4. Switch to the directory containing your project files.
5. Log in to your {{site.data.keyword.cloud_notm}} account, and set the region to us-south, as shown here:
	```hljs
    $ bx login -a https://api.ng.bluemix.net
	 ...
	 $ bx target -o <your-organization> -s <your-space>
	 ```
	 {: codeblock}

    **Note:** Issuing the command `bx login -a https://api.ng.bluemix.net` automatically sets the region to **us-south**.  
6. To deploy your application in Kubernetes, by entering this command:
	```
    $ bx dev deploy -t container
    ```
    {: codeblock}

	You will be prompted for the name of your Kubernetes cluster and the Docker registry.

After entering these values, your application is deployed to the Kubernetes cluster.
