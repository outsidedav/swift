---

copyright:
  years: 2018
lastupdated: "2018-02-06"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}

# MBaaS versus back-end components
{: #mbaas}

Your application can use additional external capabilites through one of two approaches:
* Direct usage from the application to its services, which are typically hosted as part of a Mobile Backend as a Service (MBaaS) platform.
* Usage through a hosted Backend For Frontend (BFF) component, which can provide additional composition and control over the services and can add backend logic for the application.

Typically, adding external capabilities directly from your mobile application through an MBaaS style approach is more straight forward. Less components and infrastructure are required in order to add the first set of services; however, this lacks the ultimate flexibility and scope that is possible through deploying a BFF.

One of the advantages of the {{site.data.keyword.cloud_notm} platform is that you don't need to make that decision up front. All of the provided services can be utilized directly from the mobile device using provided Swift based SDKs. The {{site.data.keyword.cloud_notm} platform also provides support for writing its back-end logic in Swift, either through a serverless model with {{site.data.keyword.openwhisk}} or through server-side Swift with frameworks such as Kitura. Because of this, when you want to start to add additional back-end logic through a BFF model, you can do so in Swift, and can choose to migrate your usage of the Swift SDKs from the application to the BFF. As a result, you can incrementally move from a MBaaS style approach to a BFF approach with minimal overhead because of the end-to-end support for Swift.
