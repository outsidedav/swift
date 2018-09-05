---

copyright:
  years: 2018
lastupdated: "2018-08-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip} 

# Creación con seguridad avanzada
{: #security}

Los desarrolladores de Swift pueden aprovechar fácilmente los servicios de seguridad avanzada que {{site.data.keyword.cloud}} ofrece para proteger sus claves y datos en reposo, en uso o en tránsito en el nivel de seguridad más alto del sector.
{:shortdesc}

Como un enfoque sencillo, puede utilizar directamente los {{site.data.keyword.cloud_notm}} HyperSecure Platform Services para todos los servicios de seguridad avanzada en {{site.data.keyword.cloud}}. Para obtener más información, consulte [Iniciación a los {{site.data.keyword.cloud_notm}} HyperSecure Platform Services](/docs/services/hypersecure-platform/index.html){:new_window}.

## Utilización de {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}

{{site.data.keyword.hscrypto}} es un servicio experimental que ofrece criptografía en sus claves y datos con el nivel de seguridad más alto del sector. Lleva la seguridad y la integridad de System z a la nube. La misma tecnología criptográfica de última generación de la que dependen los bancos y los servicios financieros ahora se ofrece a los usuarios de la nube a través de {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.hscrypto}} ofrece módulos de seguridad de hardware direccionables de red (HSM) para proporcionar criptografía segura a través de las interfaces de programación de aplicaciones (API) PKCS#11. Puede acceder al nivel más alto alcanzable de seguridad para el hardware de cifrado, es decir, FIPS 140-2 Nivel 4. {{site.data.keyword.hscrypto}} también aprovecha la solución de IBM Advanced Crypto Service Provider (ACSP) que permite el acceso remoto a los coprocesadores criptográficos de IBM.

{{site.data.keyword.hscrypto}} es también el almacén de claves para el servicio {{site.data.keyword.keymanagementserviceshort}}.

Para obtener más información sobre {{site.data.keyword.hscrypto}}, consulte [Iniciación a {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}](/docs/services/hs-crypto/index.html){:new_window}.

## Utilización de {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}}

{{site.data.keyword.keymanagementserviceshort}} le ayuda a suministrar claves cifradas para apps en servicios de {{site.data.keyword.cloud_notm}}. A medida que gestiona el ciclo de vida de las claves, podrá
beneficiarse de saber que las claves están protegidas por módulos de seguridad de hardware basados en nube (HSM)
que protegen contra el robo de información. Junto con {{site.data.keyword.hscrypto}}, las claves están protegidas en el nivel de seguridad más alto del certificado FIPS 140-2 Nivel 4.

Para obtener más información sobre {{site.data.keyword.keymanagementserviceshort}}, consulte [Iniciación a {{site.data.keyword.keymanagementserviceshort}}](/docs/services/keymgmt/index.html){:new_window}.

## Utilización de {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS
{: #hypersecure-dbaas}

{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS es un servicio experimental de {{site.data.keyword.cloud_notm}} que proporciona bases de datos seguras a petición. Ofrece una plataforma flexible y escalable para suministrar y gestionar de forma rápida y sencilla la base de datos MongoDB.

Con este servicio, puede crear clústeres de base de datos en el {{site.data.keyword.cloud_notm}}. Cada clúster de base de datos que cree comprende una instancia de base de datos primaria y dos instancias de base de datos secundarias que sirven como réplicas en el primario. La lógica de {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS crea y accede a las bases de datos de MongoDB en los clústeres de base de datos.

### Protección de datos y privacidad

{{site.data.keyword.IBM_notm}} aloja las bases de datos en un entorno de alta disponibilidad y seguridad:
 * Los datos se cifran en reposo y en curso.
 * El hardware del sistema, la configuración del sistema y la configuración de base de datos garantizan la alta disponibilidad.
 * Las tecnologías subyacentes impiden a {{site.data.keyword.IBM_notm}} o a un tercero poder acceder a sus datos.

### Adición de la lógica de {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS a la aplicación

Para utilizar una base de datos MongoDB en la aplicación, consulte
[Iniciación a la utilización de una base de datos](../hypersecure_dbaas/database-cluster.html).  

### Más información sobre {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS

Para obtener más información acerca de {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS, consulte [Iniciación a IBM Cloud Hyper Protect DBaaS](/docs/services/hyper-protect-dbaas/index.html).

## Utilización de {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscontainers}}

{{site.data.keyword.hscontainers}} combina Docker y
Kubernetes para ofrecer herramientas potentes, una interfaz intuitiva para el usuario y funciones integradas de seguridad e identificación para automatizar el despliegue, operación, escalado y supervisión de apps contenerizadas sobre un clúster de hosts de cálculo.

Tenga en cuenta que {{site.data.keyword.hscontainers}} solo está disponible para los usuarios patrocinados. Si espera un soporte de seguridad dedicado, regístrese como un usuario patrocinado con el [IBM Z Client Feedback Program](https://www-01.ibm.com/marketing/iwm/iwmdocs/web/cc/earlyprograms/zcustomer.shtml) para desplegar la aplicación en el clúster de {{site.data.keyword.hscontainers}}.
{: tip}

Antes de que se convierta en un usuario patrocinado, puede utilizar {{site.data.keyword.containershort_notm}} para proteger la aplicación. Para obtener más información, consulte [Iniciación a {{site.data.keyword.containershort_notm}}](/docs/containers/container_index.html#container_index).
