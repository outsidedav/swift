---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip}

# Desarrollo sin servidor
{: #serverless-dev-swift}

¿Qué es sin servidor? El patrón de desarrollo sin servidor hace referencia al desarrollo de aplicaciones en el que la lógica del lado del servidor se ejecuta en contenedores sin estado. Los contenedores se desencadenan por sucesos, son efímeros (duran una sola ejecución) y los gestiona un tercero. Este paradigma, también conocido como FaaS (Functions as a Service), es donde el desarrollador proporciona una función sin estado que se puede desencadenar y ejecutar sin crear ni gestionar de forma explícita un servidor.

Mediante la abstracción de la infraestructura y los marcos necesarios para el desarrollo del lado del servidor, la arquitectura sin servidor permite a los desarrolladores centrarse en escribir código para que se ejecuten de forma reactiva para cambiar los datos.

La oferta de FaaS de IBM, [{{site.data.keyword.openwhisk}}](https://cloud.ibm.com/openwhisk/), se esfuerza por ofrecer una experiencia de desarrollo sencilla de servidor sin necesidad de tener conocimientos especializados del lado del servidor. Mediante la utilización de tecnología sin servidor, puede desarrollar rápidamente soluciones de fondo ampliables para satisfacer prácticamente cualquier demanda de carga de trabajo sin la necesidad de crear recursos antes de tiempo. Para las aplicaciones que tienen patrones de carga impredecibles o un tiempo de inactividad de servidor alto, {{site.data.keyword.openwhisk_short}} puede ser una excelente solución en la nube con un rendimiento mejorado, y su sistema de "pagar por lo que utilice" ayuda a reducir los costes.

## Cambios arquitectónicos
{: #comparison-serverless}

Para ayudarle a comprender los beneficios arquitectónicos de cambiar a FaaS, las arquitecturas tradicionales y FaaS se comparan utilizando una aplicación de iOS simple que se enlaza a una base de datos.

En una arquitectura más tradicional, la aplicación de iOS descarga tareas intensivas de red, o procesa datos de forma remota en un sistema centralizado, que se conecta utilizando sus propios servicios u opciones de almacenamiento. El trabajo pesado se coloca en un servidor especial que procesa varias tareas intensivas para minimizar el estrés en el cliente y proporcionar sincronización a través de su base de usuarios.

Una arquitectura sin servidor puede modificar esta estructura para que se parezca más a la siguiente imagen.

![](./images/Architecture.png) Figura 1. Arquitectura sin servidor

En lugar de manejar todo el proceso y la lógica de autenticación dentro de un solo servidor, una arquitectura sin servidor utiliza funciones que encapsulan gran parte de la lógica del lado del servidor, y descarga alguna lógica en el cliente (y servicios externos).

Viendo el esquema, puede ver los puntos siguientes:

1. El cliente se autentica en un proveedor de identidad como, por ejemplo, App ID.
2. Llama a la API de programa de fondo de FaaS, incluida la señal de acceso.
3. El programa de fondo se implementa con {{site.data.keyword.openwhisk_short}}. Las acciones sin servidor se exponen como acciones web y esperan que las señales se envíen en la cabecera de la solicitud para su verificación (firma y fecha de caducidad) a fin de proporcionar acceso a la API real.
4. Cuando el cliente envía datos, los comentarios se almacenan en un {{site.data.keyword.cloudant_short_notm}}.
5. El texto de comentarios se procesa con {{site.data.keyword.toneanalyzershort}}.
6. En función del resultado del análisis, la notificación se envía de nuevo al cliente mediante {{site.data.keyword.mobilepushshort}}.
7. El cliente recibe la notificación.

En un modelo puramente sin servidor, el cliente a menudo asume responsabilidades adicionales debido a la incapacidad de almacenar el estado del usuario. La autorización la maneja el cliente y el servicio proveedor de identidades de {{site.data.keyword.appid_short_notm}}.

Aunque las arquitecturas sin servidor no son siempre ideales, pueden proporcionar ventajas importantes con el equipo y las condiciones de uso adecuados. Consulte algunos ejemplos específicos para obtener información sobre unos pocos de los [casos de uso](#use_cases) más comunes.

## Prestaciones sin servidor
{: #benefits-serverless}

### Coste reducido
{: #reduced-cost-serverless}

Externalizar el tiempo y el coste monetario asociado con la administración de sistema reduce el coste global asociado con los servidores de fondo tradicionales. Además, {{site.data.keyword.openwhisk_short}} es diferente de las tecnologías de cálculo tradicionales porque solo paga por el tiempo que tarda el código en satisfacer las solicitudes, redondeado a los 100 ms más próximos. Es posible conseguir un ahorro de costes considerable en relación con otras tecnologías, como las VM y los contenedores, que no es probable que se utilicen al 100 % y consumen memoria en el sistema de su proveedor de nube.

### Alta disponibilidad y escalabilidad
{: #ha-serverless}

Las arquitecturas sin servidor proporcionan de forma inherente escalabilidad instantánea con una disponibilidad casi constante.

### Desarrollo rápido y simplificado
{: #speed-serverless}

Al eliminar la necesidad de administración del sistema y de proporcionar interfaces simples para el despliegue, el paradigma sin servidor acelera el desarrollo de aplicaciones. Los desarrolladores pueden crear rápidamente apps con secuencias de acción que se ejecutan en respuesta a un mundo controlado por sucesos.

## Casos de uso de ejemplo
{: #use-cases-serverless}

### Backend móvil
{: #mobile-backend-serverless}
![](./images/cloud-functions-rest-api-trigger.png)

Los desarrolladores de móvil pueden acceder fácilmente a la lógica en el lado del servidor y externalizar tareas de computación intensas a una plataforma en la nube. Puede implementar funciones en lenguajes como Swift y consumir funciones en el lado del servidor fácilmente utilizando el SDK de iOS sin necesidad de tener experiencia en el lado del servidor.

### Proceso de datos
{: #data-processing-serverless}

![](./images/cloud-functions-cloudant-trigger.png)

Puede ejecutar código cuando se actualicen los datos de su almacén de datos a través de desencadenantes integrados. También puede automatizar procesos fácilmente como la normalización de audio, rotación de imágenes, nitidez, reducción de ruido, generación de miniaturas o transcodificación de vídeo a través de un modelo de programación funcional del lado del servidor.

### Proceso de datos cognitivo
{: #cognitive-serverless}

Puede analizar los datos en cuanto estén disponibles. Su función puede aprovechar los potentes servicios cognitivos, como IBM Watson, para detectar objetos o personas en imágenes o vídeos.

### Tareas planificadas
{: #tasks-serverless}

Ejecute sus funciones periódicamente y defina planificaciones que siguen una sintaxis cronológica para especificar cuándo se deben ejecutar las acciones.

## Referencia de API
{: #apiref-serverless notoc}

<!-- * [REST API Documentation](./openwhisk_reference.html#openwhisk_ref_restapi)-->
* [API REST](https://cloud.ibm.com/apidocs)

## Enlaces relacionados
{: #related-serverless notoc}

* [Descubrir {{site.data.keyword.openwhisk_short}}](https://www.ibm.com/cloud/functions)
<!-- redirects to link above * [{{site.data.keyword.openwhisk_short}} on IBM developerWorks](https://developer.ibm.com/openwhisk/)-->
* [Sitio web del proyecto Apache OpenWhisk](http://openwhisk.org)
* [Más información sobre la arquitectura sin servidor](https://martinfowler.com/articles/serverless.html)
