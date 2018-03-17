---

copyright:
  years: 2018
lastupdated: "2018-03-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}


# Getting started with using a database

To take full advantage of a highly available and secure database, you embed
additional logic into your application. The embedded code snippets enable the
creation and the access to a MongoDB database.

Currently, the supported programming language for using IBM Cloud Hyper Protect DBaaS
is Swift 4.0 with MongoKitten SDK 4.0.0.

Follow these steps to create, test, build, and run your application:
<ol>
<li>[Create a database cluster](use-step1.html)<br>
Use the IBM Cloud Hyper Protect DBaaS service to create a database cluster.<br><br>
Do not confuse a database cluster with a Kubernetes
cluster. A *database cluster* comprises one primary database instance and two secondary database 
instances serving as replicas to the primary. In step 3, you will obtain additional 
logic, which enables you to create and access MongoDB databases in your database cluster. 
For more information, refer to [IBM Cloud Hyper Protect DBaaS](../security/advanced_security.html#hypersecure-dbaas). 
<br><br></li>
<li>[Create a project using Starter Kit](use-step2.html)<br>
Generate a run-time environment based on the server-side Swift web
framework Kitura. If you have already an existing project created from the
Swift Kitura Starter Kit, you can omit this step.<br><br>
</li>
<li>[Connect to the database](use-step3.html)<br>
Connect a Swift application to the database instance using the MongoKitten SDK.
<br><br></li>
<li>[Verify the database connection](use-step4.html)<br>
Test your database connection.
<br><br></li>
<li>[Embed your application code](use-step5.html)<br>
Now you can add your own application code.
<br><br></li>
<li>[Deploy the application](use-step6.html)<br>
Deploy your application to your local computer, to the Cloud Foundry, or to a
Kubernetes Cluster.
<br><br></li>
</ol>


## Learn more about Hyper Protect DBaaS

To learn more about IBM Cloud Hyper Protect DBaaS, see the information at this URL: 
[Getting Started with IBM Cloud Hyper Protect DBaaS](https://console.bluemix.net/docs/services/hypersecure-dbaas/index.html){:new_window}.
