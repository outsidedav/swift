---

copyright:
  years: 2018
lastupdated: "2018-05-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}

# Step 6: Deploying your application

You can run the application locally on your host system if you install the necessary
build tools, or in the {{site.data.keyword.cloud}} (Cloud Foundry or Kubernetes Cluster) through the {{site.data.keyword.dev_cli_long}} CLI.

## Procedure

<ol>
	<li> Install the IBM Cloud CLI by following the directions at this URL:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started
		<p>Be sure to install the plug-in using the command `bx plugin install dev`.</p></li>
	<li> You can run your application locally on your host system, in the IBM Cloud
Foundry, or in a Kubernetes Cluster:
		<ol>
			<li> To run your application on your local host system, see [Deploying the application locally](#deploy-locally) </li>
			<li> To run your application in the IBM Cloud Foundry, see [Deploying the application to the Cloud Foundry](#deploy-foundry) </li>
			<li> To run your application in a Kubernetes cluster, see [Deploying the application to a Kubernetes Cluster](#deploy-kube) </li>
		</ol>
	</li>	
</ol>

## Deploying the application locally
{: #deploy-locally}

### Before you begin

Make sure that Docker is installed and running on your local host system.
You can download Docker from https://www.docker.com/community-edition#/download.

### Procedure

1. Switch to the directory that contains your project files.

2. To deploy the application on your local computer, enter the following commands:
    ```
    $ bx dev build
	...
	$ bx dev run
    ```
    {: pre}

	This step builds your application, and run it locally inside a Docker container.

## Deploying the application to the Cloud Foundry
{: #deploy-foundry}

### Procedure

1. Switch to the directory that contains your project files.

2. Log in to your {{site.data.keyword.cloud_notm}} account, and set the region to `us-south`, as shown here:
	<pre><code class="hljs">
    $ bx login -a https://api.ng.bluemix.net
	...
	$ bx target -o &lt;<em>your-organization</em>&gt; -s &lt;<em>your-space</em>&gt;
    </code></pre>
    **Note:** Issuing the command **bx login -a https://api.ng.bluemix.net** automatically sets the region to **us-south**.

3. To deploy the application to the Cloud Foundry, enter this command:
	```
    $ bx dev deploy
    ```
    {: pre}

	You receive a clickable link to the location where your application is hosted.

## Deploying the application to a Kubernetes Cluster
{: #deploy-kube}

### Before you begin

To deploy your application in Kubernetes, you need a Kubernetes cluster. 

For information about creating a Kubernetes cluster, refer to 
https://console.bluemix.net/docs/containers/container_index.html#clusters.

1. You create a Kubernetes cluster at https://console.bluemix.net/containers-kubernetes/clusters.
2. Click **Create Cluster**.<br>
The Access tab displays information on how to access the created Kubernetes cluster.
3. To display information about the Kubernetes cluster, open the IBM Cloud dashboard at
https://console.bluemix.net/dashboard/apps.<br>
The dashboard displays a list of your services, such as created clusters,
database clusters, cloud foundry apps, cloud foundry services.
	
### Procedure

1. Switch to the directory containing your project files.

2. Log in to your IBM Cloud account, and set the region to us-south, as shown here:
	<pre><code class="hljs">
    $ bx login -a https://api.ng.bluemix.net
	...
	$ bx target -o &lt;<em>your-organization</em>&gt; -s &lt;<em>your-space</em>&gt;
    </code></pre>
    **Note:** Issuing the command **bx login -a https://api.ng.bluemix.net** automatically sets the region to **us-south**.

3. To deploy your application in Kubernetes, enter this command:
	```
    $ bx dev deploy -t container
    ```
    {: pre}

	You are prompted for:
	1. The name of your Kubernetes cluster
	2. The Docker registry
	
Once the information is provided, your application is deployed to the Kubernetes cluster.
