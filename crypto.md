---

copyright:
  years: 2018
lastupdated: "2018-3-11"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}
{: #crypto}

{site.data.keyword.cloud}} {{site.data.keyword.hscrypto}} brings System z cryptography to cloud. The same state-of-the-art cryptographic technology that banks and financial services rely on is now offered to cloud users via IBM Cloud.
{:shortdesc}

{{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} will prove very useful for iOS developers. It protects your keys, data at rest, in use, and in transit at the industry highest security level, that is, FIPS 140-2 Level 4. Besides, {{site.data.keyword.hscrypto}} binds the {{site.data.keyword.keymanagementservicelong_notm}} service and protect your keys in a hyper secure environment on System z.


## Provisioning and service
{: ##crypto_create}

The {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} is an {{site.data.keyword.cloud_notm}} experimental service. To provision it, create an instance of {{site.data.keyword.hscrypto}} from [{{site.data.keyword.cloud_notm}} Experimental Services](https://console.bluemix.net/catalog/labs/){:new_window}. Navigate to the Security catalog and click {{site.data.keyword.hscrypto}}. In the {{site.data.keyword.hscrypto}} catalog page, select **zCrypto Free Plan** and click **Create**.


## Configuring the service
{: ##crypto_config}

You need to install and configure the Advanced Cryptography Service Provider (ACSP) client in your environment.
1. Download the [`acsp-pkcs11-client_1.5-3.5_amd64.deb` ![External link icon](image/external_link.svg "External link icon")](http://nginx.bluemixsecurity.com/acsp-pkcs11-client_1.5-3.5_amd64.deb){:new_window} file.  If the downloading doesn't start automatically, right-click the link and save it.
2. Run the `acsp-pkcs11-client_1.5-3.5_amd64.deb` file to install the ACSP client.
3. In your {{site.data.keyword.hscrypto}} service instance, select **Manage** from the left navigator.
4. On the **Manage** screen, click the **Download Config** button to download the client credentials file.
3. Copy the client credentials file, for example, the `acsp_client_credentials.uue` file, to the `/opt/ibm/acsp-pkcs11-client/config` directory in your local environment.
4. In the `/opt/ibm/acsp-pkcs11-client/config` directory, decode the client credentials file with the following command.
       `base64 -D -i acsp_client_credentials.uue -o acsp_client_credentials.tgz`
5. Extract the client credentials file with the following command:
       `tar zxf acsp_client_credentials.tgz`
6. Rename the client configuration file with the following command:
       `mv acsp.properties.client acsp.properties`
7. Change group id of these files with the following command:
       `chown root.pkcs11 *`

Now your ACSP client is operational and your {{site.data.keyword.hscrypto}} is ready to use! An easy way to adopt {{site.data.keyword.cloud_notm}} HyperSecure Crypto Services is to start with the {{site.data.keyword.cloud_notm}} {{site.data.keyword.hsplatform}}.  For more information about {{site.data.keyword.hsplatform}}, see [Getting Started with {{site.data.keyword.cloud_notm}} {{site.data.keyword.hsplatform}}](/docs/services/hypersecure-platform/index.html){:new_window}.
{: #crypto_starterkit}


For more information about {{site.data.keyword.cloud_notm}} HyperSecure Crypto Services, see [Getting Started with {{site.data.keyword.cloud_notm}} HyperSecure Crypto Services](/docs/services/hs-crypto/index.html){:new_window}.
{: #push_learn}
