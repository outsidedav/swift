---

copyright:
  years: 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip} 

# Building with advanced security
{: #security}

Swift developers can easily leverage advanced security services that {{site.data.keyword.cloud}} offers to protect their keys and data at rest, in use, or in transit at the industry highest security level.
{:shortdesc}

As an easy approach, you can directly use the recently launched experimental {{site.data.keyword.cloud_notm}} HyperSecure Platform Services for all advanced security services in {{site.data.keyword.cloud}}.  For more information, see [Getting Started with {{site.data.keyword.cloud_notm}} HyperSecure Platform Services](/docs/services/hypersecure-platform/index.html){:new_window}.


## **{{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}**  

{{site.data.keyword.hscrypto}} is an experimental service that offers cryptography on your keys and data with the industry's highest security level. It brings the security and integrity of System z to the cloud. The same state-of-the-art cryptographic technology that banks and financial services rely on is now offered to cloud users via {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.hscrypto}} offers network addressable hardware security modules (HSMs) to provide safe and secure cryptography via PKCS#11 application programming interfaces (APIs). You can access to the highest level attainable of security for cryptographic hardware, that is, FIPS 140-2 Level 4. {{site.data.keyword.hscrypto}} also leverages the IBM Advanced Crypto Service Provider (ACSP) solution that enables remote access to the IBM’s cryptographic coprocessors.

{{site.data.keyword.hscrypto}} is also the key store for the {{site.data.keyword.keymanagementserviceshort}} service.

For more information about {{site.data.keyword.hscrypto}}, see [Getting started with {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}](/docs/services/hs-crypto/index.html){:new_window}.


## **{{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}}**

{{site.data.keyword.keymanagementserviceshort}} helps you provision encrypted keys for apps across {{site.data.keyword.cloud_notm}} services. As you manage the lifecycle of your keys, you can benefit from knowing that your keys are secured by cloud-based hardware security modules (HSMs) that protect against the theft of information. Together with {{site.data.keyword.hscrypto}}, your keys are secured at the highest security level of FIPS 140-2 Level 4 certificate.

For more information about {{site.data.keyword.keymanagementserviceshort}}, see [Getting started with {{site.data.keyword.keymanagementserviceshort}}](/docs/services/keymgmt/index.html){:new_window}.


## **{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS**
{: #hypersecure-dbaas}

{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS is an experimental {{site.data.keyword.cloud_notm}} service that provides secure databases on demand. It offers a flexible and scalable platform that allows you to quickly and easily provision and manage your MongoDB database.

With this service, you can create database clusters in the {{site.data.keyword.cloud_notm}}. Each database cluster that you create comprises one primary database instance and two secondary database instances serving as replicas to the primary. The {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS logic enables you to create and access MongoDB databases in your database clusters.


### Data security and privacy

{{site.data.keyword.IBM}} hosts your databases in a highly available and secure environment:
 * Data is encrypted at rest and in flight.
 * The system hardware, the system configuration, and the database setup ensure high availability.
 * The underlying technologies prevent {{site.data.keyword.IBM_notm}} or a third party from being able to access your data.


### Adding {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS logic to your application

To use a MongoDB database in your application, see
[Getting started with using a database](../hypersecure_dbaas/use-steps.html).  


### Learn more about {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS

To learn more about {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS, see [Getting Started with IBM Cloud Hyper Protect DBaaS](/docs/services/hypersecure-dbaas/index.html).


## **{{site.data.keyword.cloud_notm}} {{site.data.keyword.hscontainers}}**  

{{site.data.keyword.hscontainers}} delivers powerful tools by combining Docker and Kubernetes technologies, an intuitive user experience, and built-in security and isolation to automate the deployment, operation, scaling, and monitoring of containerized apps in a cluster of compute hosts.

Note that {{site.data.keyword.hscontainers}} is now available to only sponsor users. If you expect dedicated security support, register as sponsor users with the [IBM Z Client Feedback Program](https://www-01.ibm.com/marketing/iwm/iwmdocs/web/cc/earlyprograms/zcustomer.shtml) to deploy your application to the {{site.data.keyword.hscontainers}} cluster.
{: tip}

Before you become a sponsor user, you can use {{site.data.keyword.containershort_notm}} to secure your application. For more information, see [Getting started with {{site.data.keyword.containershort_notm}}](/docs/containers/container_index.html#container_index).
