---

copyright:
  years: 2018
lastupdated: "2018-08-29"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Creación de apps de Swift con kits de iniciación
{: #intro}

La consola del desarrollador de {{site.data.keyword.cloud_notm}} permite a los desarrolladores crear apps a partir de varios kits de iniciación, a suministrar y conectar servicios clave optimizados para {{site.data.keyword.cloud_notm}} y a descargar rápidamente código de trabajo o a realizar la configuración para un suministro continuo. Los usuarios pueden crear, ver, configurar y gestionar la app, así como descargar el código de la misma. El uso de kits de iniciación le ayuda a evaluar y probar con rapidez los servicios de {{site.data.keyword.cloud_notm}} con una app completamente nueva.

¿Listo para lanzarse? Visite la [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits) ahora para empezar.
{: tip}

## ¿Qué es un kit de iniciación?
{: #starter_kits}

Con la {{site.data.keyword.cloud_notm}} Developer Experience, puede elegir entre varios kits de iniciación. Los kits de iniciación instruyen a {{site.data.keyword.cloud_notm}} a ensamblar una app de producción de esqueleto, en el lenguaje de su elección, listo para el despliegue en la nube. Cada kit de iniciación incluye un lenguaje, una infraestructura y un patrón para un caso de uso específico en el mundo real que permite reutilizar el código en lugar de reinventarlo.

Los kits de iniciación están listos para ser utilizados en producción y se centran en demostrar una implementación de un patrón clave utilizando un tiempo de ejecución (por ejemplo, Swift). En algunos casos, los kits de iniciación ofrecen una experiencia de usuario simple para resaltar la integración del servicio. En otros casos, los kits de iniciación representan una implementación personalizable de un caso de uso sofisticado.

Los kits de iniciación contienen instrucciones que permiten a {{site.data.keyword.cloud_notm}} generar automáticamente apps estructurales con código portátil, y especificar recursos para que se suministren automáticamente al crear una app desde el kit de iniciación.

## Utilización de la {{site.data.keyword.cloud_notm}} Developer Console for Apple
{: #journey}

{{site.data.keyword.cloud_notm}} Developer Console for Apple le proporciona una vía de acceso sencilla para crear una app de inicio de Swift para el caso de uso específico. Vamos a ver los pasos que puede realizar.

### Pantalla de visión general
{: #overview_screen}

La pantalla Visión general le proporciona contenido que se adapta a un conjunto de casos de uso como Watson, Weather, etc. Desde la pantalla de visión general, puede ver documentación, acceder a los recursos educativos, examinar los servicios, ver kits de iniciación destacados o enlazar a una colección más grande de kits de iniciación. Pulse `Kits de iniciación` en el área de navegación de la izquierda para entrar en la vista Kits de iniciación.

![Pantalla Visión general de {{site.data.keyword.cloud_notm}} Developer Console for Apple](images/overview_screen.png "Pantalla Visión general") <br> *Pantalla Visión general de {{site.data.keyword.cloud_notm}} Developer Console for Apple*

### Vista de kits de iniciación
{: #starter_kits_view}

La vista Kits de iniciación muestra la colección de kits de iniciación específicos de un área de caso de uso. Puede pulsar varios enlaces en una tarjeta del kit de iniciación para ver las demostraciones y más información. Seleccione un kit de iniciación para ir a la vista Crear app.

![Vista Kits de iniciación de {{site.data.keyword.cloud_notm}} Developer Console for Apple](images/starter_kits_screen.png "Vista Kits de iniciación") <br> *Vista Kits de iniciación de {{site.data.keyword.cloud_notm}} Developer Console for Apple*

### Crear vista de app
{: #create_new_app_view}

Desde la vista **Crear app**, puede dar un nombre a la app, así como proporcionar información de despliegue y de direccionamiento. A la derecha, también puede ver los servicios que se suministran automáticamente cuando se crea la app, junto con los planes de precios y los términos para cada app. Pulse `Crear` para ir a la vista Detalles de la app. Si no ha iniciado sesión en {{site.data.keyword.cloud_notm}}, tiene que hacerlo ahora.

![Vista Crear nueva app de {{site.data.keyword.cloud_notm}} Developer Console for Apple](images/create_new_project_screen.png "Vista Crear nueva app") <br> *Vista Crear nueva app de {{site.data.keyword.cloud_notm}} Developer Console for Apple*

## Vista Detalles de la app
{: #app_details_view}

La vista Detalles de la app muestra una lista de servicios que están configurados para la app. Para cada elemento de la lista, puede ver el nombre de servicio, los enlaces a otra información y un botón **acciones** con tres puntos alineados verticalmente. Las opciones del botón **acciones** son eliminar los servicios de una app, abrir el panel de control para el servicio y suprimir el servicio. Cuando se elimina una instancia de servicio se elimina la asociación a esa app, y no suprime la instancia de servicio. Además, las credenciales de servicio se consolidan en esta vista, de modo que no tiene que visitar cada vista de instancia de servicio individual para obtenerlas.

![Vista Detalles de app de {{site.data.keyword.cloud_notm}} Developer Console for Apple](images/project_details_screen.png "Vista Detalles de app") <br> *Vista Detalles de app de {{site.data.keyword.cloud_notm}} Developer Console for Apple*

Al utilizar la vista Detalles de app, puede añadir servicios nuevos o existentes a la app que no formaban parte del Kit de iniciación original. Pulse el enlace **Añadir recurso** en el recuadro de lista de servicios para añadir servicios. Los servicios disponibles dependen del tipo de app y de los servicios que están disponibles en una región, por lo que no todos los servicios están disponibles para asociarse con todas las apps.

![ Diálogo Añadir recurso de {{site.data.keyword.cloud_notm}} Developer Console for Apple](images/add_resource_screen.png "Diálogo Añadir recurso") <br> *Diálogo Añadir recurso de {{site.data.keyword.cloud_notm}} Developer Console for Apple*

### Descarga del código

* En la vista Detalles de la app, puede acceder a su código seleccionando **Descargar código** para generar y descargar el código de la app.

### Vista Lista de apps
{: #app_list_view}

Puede obtener una lista de todas las apps creadas desde la vista Lista de apps. Puede cambiar el nombre o suprimir las apps desde aquí. Pulse en una fila de nombre de app para volver a la vista Detalles de la app.

![Vista Lista de apps de {{site.data.keyword.cloud_notm}} Developer Console for Apple](images/project_list_screen.png "Vista Lista de apps") <br> *Vista Lista de apps de {{site.data.keyword.cloud_notm}} Developer Console for Apple*

Para obtener más información, visite los [{{site.data.keyword.cloud_notm}} Developer Console for Apple Learning Resources](https://console.bluemix.net/developer/appledevelopment/learning-resources).
{: tip}
