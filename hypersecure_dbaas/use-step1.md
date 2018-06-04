---

copyright:
  years: 2018
lastupdated: "2018-05-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}

# Step 1: Creating a database cluster

You can create a database cluster by accessing the {{site.data.keyword.cloud}} Hyper Protect DBaaS service configuration screen.

## Procedure

<ol>
<li>Access the {{site.data.keyword.cloud}} Hyper Protect DBaaS service configuration screen at 
https://console.bluemix.net/catalog/services/hypersecure-dbaas.</li>
<li>Enter the required values.
	<dl>
		<dt>Service name:</dt>
		<dd>A name for the database service.</dd>

    <dt>Choose a region/location to deploy in:</dt>
    <dd>Select the data center where the database is located.</dd>

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
    <dd>Read the license agreement, and check the box to confirm your agreement.</dd>

    <dt>Pricing:</dt>
		<dd>The current solution provides only one pricing category, which is free
		of charge. In later versions, you can select the pricing category.</dd>
	</dl>
</li>
<li>Click **Create**.

<p>The IBM Cloud dashboard is displayed. You might have to refresh your browser to see the new cluster, 
which is listed in the Services section.</p></li>

<li>When you select the service, the cluster information is displayed.</li>
<li>In the Manage tab of the cluster information, click **OPEN**.
	<p>The IBM Cloud Hyper Protect DBaaS dashboard is displayed.</p></li>
<li>Obtain the host names and the port numbers of the three created database instances belonging to your database cluster.
	<p> You will need the host names, the port numbers, and the user credentials in [Step 3](use-step3.html).
</p></li>
</ol>

## What to do next

Proceed with [Step 2](use-step2.html).
