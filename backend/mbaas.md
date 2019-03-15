---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: mobile backend swift, mbaas swift, mbaas swift sdk, swift features, swift framework sdk

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# Mobile backend as a service features
{: #mbaas}

Your application can use external features through 1 of two approaches:
* Direct usage from the application to its services, which are typically hosted as part of a Mobile Backend as a Service (MBaaS) platform.
* Usage through a hosted Backend For Frontend (BFF) component, which can provide composition and control over the services and can add backend logic for the application.

Typically, adding external features directly from your mobile application through an MBaaS style approach is more straight forward. Fewer items and infrastructure are to add the first set of services. However, this method lacks the ultimate flexibility, and scope that is possible through deploying a BFF.

One of the advantages of the {{site.data.keyword.cloud_notm}} platform is that you don't need to decide up front. All of the provided services can be used directly from the mobile device by using provided Swift-based SDKs. The {{site.data.keyword.cloud_notm}} platform also supports writing its back-end logic in Swift, either through a serverless model with {{site.data.keyword.openwhisk_short}}, or through server-side Swift with frameworks such as Kitura. Because of this functionality, when you want to add back-end logic through a BFF model, you can do so in Swift, and can choose to migrate your usage of the Swift SDKs from the application to the BFF. As a result, you can incrementally move from a MBaaS style approach, to a BFF approach with minimal extra effort.
