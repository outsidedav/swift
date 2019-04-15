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

# Características del programa de fondo móvil como servicio (MBaaS)
{: #mbaas}

La aplicación puede utilizar características externas mediante uno de estos dos enfoques:
* Uso directo de la aplicación a sus servicios, que normalmente están alojados como parte de una plataforma MBaaS (Mobile Backend as a Service).
* Uso a través de un componente BFF (Backend For Frontend) alojado, que puede proporcionar composición y control sobre los servicios y puede añadir lógica de fondo para la aplicación.

Por lo general, la adición de características externas directamente desde la aplicación móvil a través de un enfoque de estilo MBaaS es más directa. Hay que añadir menos elementos e infraestructura al primer conjunto de servicios. Sin embargo, este método carece de la máxima flexibilidad y ámbito que es posible a través del despliegue de un BFF.

Una de las ventajas de la plataforma {{site.data.keyword.cloud_notm}} es que no es necesario decidir por adelantado. Todos los servicios proporcionados se pueden utilizar directamente desde el dispositivo móvil utilizando SDK basados en Swift proporcionados. La plataforma {{site.data.keyword.cloud_notm}} también da soporte a la escritura de su lógica de fondo en Swift, ya sea a través de un modelo sin servidor con {{site.data.keyword.openwhisk_short}}, o a través de Swift del lado del servidor con infraestructuras como Kitura. Debido a esta funcionalidad, cuando desee añadir lógica de fondo a través de un modelo BFF, puede hacerlo en Swift, y puede optar por migrar el uso de los SDK de Swift desde la aplicación al BFF. Como resultado, puede pasar de forma incremental de un enfoque de estilo MBaaS a un enfoque de BFF con un mínimo esfuerzo adicional.
