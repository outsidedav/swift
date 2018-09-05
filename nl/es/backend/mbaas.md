---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}

# MBaaS frente a componentes de fondo
{: #mbaas}

La aplicación puede utilizar prestaciones externas mediante uno de dos enfoques:
* Uso directo de la aplicación a sus servicios, que normalmente están alojados como parte de una plataforma MBaaS (Mobile Backend as a Service).
* Uso a través de un componente BFF (Backend For Frontend) alojado, que puede proporcionar composición y control sobre los servicios y puede añadir lógica de fondo para la aplicación.

Por lo general, la adición de capacidades externas directamente desde la aplicación móvil a través de un enfoque de estilo MBaaS es más directa. Menos componentes e infraestructura van a añadir el primer conjunto de servicios. Sin embargo, este método carece de la máxima flexibilidad y ámbito que es posible a través del despliegue de un BFF.

Una de las ventajas de la plataforma {{site.data.keyword.cloud_notm}} es que no es necesario decidir por adelantado. Todos los servicios proporcionados se pueden utilizar directamente desde el dispositivo móvil utilizando SDK basados en Swift proporcionados. La plataforma {{site.data.keyword.cloud_notm}} también da soporte a la escritura de su lógica de fondo en Swift, ya sea a través de un modelo sin servidor con {{site.data.keyword.openwhisk_short}}, o a través de Swift del lado del servidor con infraestructuras como Kitura. Debido a esta funcionalidad, cuando desee añadir lógica de fondo a través de un modelo BFF, puede hacerlo en Swift, y puede optar por migrar el uso de los SDK de Swift desde la aplicación al BFF. Como resultado, puede pasar de forma incremental de un enfoque de estilo MBaaS a un enfoque de BFF con una sobrecarga mínima debido al soporte de extremo a extremo para Swift.
