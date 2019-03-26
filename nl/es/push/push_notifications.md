---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Envío de {{site.data.keyword.mobilepushshort}}
{: #push_notifications}

Mejore la app Swift utilizando el servicio {{site.data.keyword.mobilepushshort}} en {{site.data.keyword.cloud}} para enviar notificaciones en tiempo real a dispositivos móviles y aplicaciones web.

 - Las notificaciones se pueden entregar a todos los usuarios de la aplicación, o a un conjunto seleccionado de usuarios o dispositivos.
 - Da soporte tanto a notificaciones interactivas como silenciosas.
 - Los clientes pueden optar por suscribirse a etiquetas o temas específicos para la notificación.
 - Permite al propietario de la app analizar el número de dispositivos registrados para recibir notificaciones y el número de notificaciones enviadas.

Puede utilizar el servicio {{site.data.keyword.mobilepushshort}} como parte del contenedor modelo de iniciador de servicios MobileFirst o bien como [servicios dedicados](/docs/dedicated/index.html) de {{site.data.keyword.cloud_notm}}. También puede utilizar un SDK (kit de desarrollo de software) y [API REST ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](https://mobile.{DomainName}/imfpush/){: new_window} para desarrollar más las aplicaciones de cliente.

![Visión general](images/push_notification_lifecycle.jpg) Figura 1. Visión general del ciclo de vida del servicio {{site.data.keyword.mobilepushshort}}

## Antes de empezar
{: #prereqs-push}

Primero, asegúrese de cumplir los siguientes requisitos previos:

 - iOS 8.0+
 - Xcode 7.3, 8.0
 - Swift 2.3 - 4.0
 - CocoaPods o Carthage

## Paso 1. Creación de una instancia de {{site.data.keyword.mobilepushshort}}
{: #push_create}

1. En el catálogo de {{site.data.keyword.cloud_notm}}, pulse **Móvil** > **{{site.data.keyword.mobilepushshort}}**. Se abre la pantalla de configuración del servicio.
2. Dé un nombre a la instancia de servicio, o utilice el nombre preestablecido.
3. Pulse **Crear**.
4. En el panel de navegación, pulse **Conexiones** para seleccionar una app y enlazarla al servicio. Puede enlazar la instancia de servicio a su app más adelante si la deja desenlazada durante la creación.


## Paso 2. Obtener las credenciales del proveedor de notificaciones
{: #get_creds-push}

Para configurar el servicio de notificaciones push, debe obtener las credenciales necesarias del servicio de notificaciones push de Apple (APNs). Siga los pasos que se indican a continuación para [obtener y configurar las credenciales de APNs ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/mobilepush/push_step_1.html#push_step_1_ios){: new_window}.


## Paso 3. Configurar una instancia de servicio
{: #config-resource-push}

Para utilizar el servicio {{site.data.keyword.mobilepushshort}} para enviar notificaciones, cargue el almacén de claves `.p12` que ha creado, que contiene la clave privada y los certificados SSL necesarios para crear y publicar la aplicación. También se puede utilizar la API REST para subir un certificado de APNs.

Después de que el archivo `.cer` se encuentre en el acceso de cadena de claves, expórtelo al sistema para crear un certificado `.p12`.

Para obtener más información sobre cómo utilizar APNs, consulte en la [Biblioteca de desarrolladores de iOS la guía de programación de notificaciones push y locales ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1){: new_window}.

Para configurar APNs en la consola de servicios de notificación push, siga estos pasos:

1. Seleccione **Configurar** en la consola de servicios de {{site.data.keyword.mobilepushshort}}.
2. Elija la opción **Móvil** para actualizar la información del formulario **Credenciales de push de APNs**.
3. Seleccione una de las opciones siguientes:
	- Para la opción **Móvil**
		1. Seleccione **Recinto de seguridad** (desarrollo) o **Producción** (distribución) según convenga y, a continuación, cargue el certificado `p.12` que ha creado.
		  ![Establecer la consola de {{site.data.keyword.mobilepushshort}}](images/wizard.jpg)

		2. En el campo **Contraseña**, especifique la contraseña asociada con el archivo de certificado `.p12` y, a continuación, pulse **Guardar**.
	- Para la opción **Web**
		- En la sección Push de Safari, actualice el formulario con la información necesaria.
		- **Nombre de sitio web**: El nombre de sitio web proporcionado en el centro de notificaciones.
		- **ID Push del sitio web**: Actualice con la serie de dominio invertido para el ID Push del sitio web. Por ejemplo, web.com.acmebanks.www.
		- **URL del sitio web**: Proporcione el URL del sitio web que está suscrito a notificaciones push. Por ejemplo, https://www.acmebanks.com.
		- **Dominios permitidos**: (parámetro opcional) Una lista de sitios web que solicitan permiso del usuario. Asegúrese de que las URL sean valores separados por coma. Los valores del URL del sitio web se utilizan si no se proporciona la información.
		- **Serie de formato de URL**: El URL a resolver cuando se pulse la notificación. Por ejemplo, ["https://www.acmebanks.com"]. Asegúrese de que el URL utilice el esquema http o https.
		-**Certificado push web de Safari**: cargue el certificado `.p12` y proporcione la contraseña.
4. Pulse **Guardar**.	
	![Consola de {{site.data.keyword.mobilepushshort}}](images/push_configure_safari.jpg)

## Paso 4. Configurar el SDK del cliente de servicio
{: #service-client-push}

Para habilitar las aplicaciones de iOS para que reciban notificaciones push en sus dispositivos, debe configurar el SDK de iOS para el servicio {{site.data.keyword.mobilepushshort}}.

Los SDK de Swift de {{site.data.keyword.cloud_notm}} Mobile Services se pueden instalar con Cocoapods o Carthage. Para obtener más información, consulte [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application).

## Paso 5. Envío de una notificación
{: #send-notify-push}

Una vez que haya desarrollado su aplicación, puede enviar notificaciones push básicas.

Para enviar notificaciones push básicas, siga estos pasos:

1. Seleccione **Enviar notificaciones** y para redactar un mensaje seleccione la opción **Enviar a**. Las opciones admitidas son **Dispositivo por etiqueta**, **ID de dispositivo**, **ID de usuario**, **Dispositivos iOS**, **Notificaciones web** y **Todos los dispositivos**.
**Nota**: Cuando seleccione la opción **Todos los dispositivos**, todos los dispositivos que están suscritos a {{site.data.keyword.mobilepushshort}} reciben notificaciones.

	![Pantalla Notificaciones](images/tag_notification.jpg)

2. En el campo **Mensaje**, redacte el mensaje. Elija configurar los valores opcionales según sea necesario.
3. Pulse **Enviar**.
3. Verifique que los dispositivos o el navegador hayan recibido la notificación.

La captura de pantalla siguiente muestra un recuadro de alerta que maneja una notificación push en el primer plano en el dispositivo.
	![Notificación push en primer plano en Android](images/Android_Screenshot.jpg)

La captura de pantalla siguiente muestra una notificación push en segundo plano.
	![Notificación push en segundo plano en Android](images/background.png)

### Valores opcionales
{: #optional-push}

Puede personalizar los valores de {{site.data.keyword.mobilepushshort}} para el envío de notificaciones a dispositivos iOS. Se admiten las siguientes opciones de personalización.

- **Identificador**: Indica el número que se muestra en el identificador de la aplicación. El valor predeterminado es cero (0), que no muestra ningún identificador.
- **Sonido**: indica un fragmento de sonido que se reproducirá al recibir una notificación. Da soporte a la opción predeterminada o al nombre de un recurso de sonido incorporado en la app.
- **Carga útil adicional**: permite especificar valores personalizados de carga útil para las notificaciones.

También puede decidir habilitar las [notificaciones interactivas](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#interactive-notifications) y las [notificaciones de Rich Media](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enabling-rich-media-notifications).

## Paso 6. Supervisión de las notificaciones entregadas
{: #monitor-push}

El servicio de {{site.data.keyword.mobilepushshort}} proporciona un programa de utilidad de supervisión para ayudarle a comprobar el estado de los mensajes enviados. Para configurar el programa de utilidad de supervisión, consulte [Habilitar supervisión para aplicaciones de iOS](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enable-monitoring).

## Pasos siguientes
{: #next-push}

 - Para obtener más información sobre el servicio y aprovechar todas las características, consulte nuestra [documentación](/docs/services/mobilepush/c_overview_push.html#overview-push).

 - Para ver una visión general de los servicios para móviles y {{site.data.keyword.cloud_notm}}, consulte [Iniciación a las apps para móvil en {{site.data.keyword.cloud_notm}}](/docs/services/mobile/index.html).

 - Los kits de inicio son una de las formas más rápidas de utilizar las características de {{site.data.keyword.cloud_notm}}. Vea nuestros kits de inicio disponibles en el [panel de control de desarrollador de Mobile](https://cloud.ibm.com/developer/mobile/dashboard). Descargue el código. Ejecute la app.

 - Puede utilizar la [IU de Swagger](https://mobile.ng.bluemix.net/imfpush/) para consultar de forma rápida la documentación de la API REST.
