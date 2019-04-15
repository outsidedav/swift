---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: getting started swift, custom app, create app swift, stater kit swift, apple app swift, swift dependency, ios development

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Guía de aprendizaje de iniciación
{: #getting_started_swift}

{{site.data.keyword.cloud}} ofrece soluciones y servicios para ayudar a los desarrolladores de Swift a crear aplicaciones que integren la seguridad, la IA y el valor que sus clientes exigen. Con una amplia cartera de ofertas y SDK, puede utilizar estos servicios y llevar aplicaciones innovadoras al mercado rápidamente. En esta guía de programación de Swift se muestra cómo añadir servicios a una aplicación Swift nueva o existente, ya sea un cliente de iOS o un Swift del lado del servidor.
{: shortdesc}

En la siguiente guía de aprendizaje se muestra cómo crear fácilmente una app móvil Swift con {{site.data.keyword.mobileanalytics_full}} utilizando un kit de inicio vacío desde la [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo"). Desde la consola, añada el servicio {{site.data.keyword.mobileanalytics_short}}, descargue el código, ejecute la app de iOS localmente en Xcode, configure y supervise la app.

## Paso 1. Requisitos para los desarrolladores
{: #dev-requirements-swift}

Para empezar a utilizar el desarrollo de iOS en {{site.data.keyword.cloud_notm}}, asegúrese de que cumple los requisitos siguientes.

### Sistema operativo
{: #swift-os-requirements}

La práctica recomendada para desarrollar apps de Swift consiste en utilizar el hardware soportado más reciente de MacOS, y probarlas con los releases más recientes de iOS. Regístrese para una cuenta de [Desarrollador de Apple](https://developer.apple.com/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") para habilitar las pruebas en un dispositivo físico.

### iOS y Xcode
{: #ios_and_xcode}

- Instale [Xcode 8+](https://developer.apple.com/xcode/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") (o superior).
- Realice el despliegue en [dispositivos iOS 8](https://support.apple.com/downloads/ios){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") (o superior).
- Revise las [Directrices de envío de la App Store](https://developer.apple.com/app-store/guidelines/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") antes de enviar apps a Apple.

### SDK y gestión de dependencias
{: #swift-sdk-management}

Las herramientas siguientes garantizan que puede instalar los SDK nativos para que funcionen con los distintos {{site.data.keyword.cloud_notm}} Services. Puede utilizar CocoaPods o Carthage para la gestión de dependencias.

* **Uso de CocoaPods** - Instale [CocoaPods](https://cocoapods.org/) para el soporte de los SDK de {{site.data.keyword.cloud_notm}}:
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}

* **Utilización de Carthage**: algunos SDK también están disponibles a través de los gestores de dependencias
[Carthage](https://github.com/Carthage/Carthage){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") o [Gestor de paquetes de Swift](https://swift.org/package-manager/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo"). Para utilizar Carthage para la gestión de dependencias, realice los pasos siguientes:

  Instale [Homebrew](https://brew.sh/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") como ayuda para la instalación de Carthage:
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

  Instale Carthage:
  ```
  brew install carthage
  ```
  {: codeblock}

## Paso 2. Creación de una app Swift de iOS personalizada
{: #create-ios-app-swift}

1. Inicie sesión en la [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo").
2. Pulse **Crear app**.
3. En la página [Iniciador vacío](https://cloud.ibm.com/developer/appledevelopment/create-app){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo"), puede utilizar la configuración predeterminada, o actualizar los campos según sea necesario. Asegúrese de que **iOS Swift** sea el lenguaje seleccionado. Pulse **Crear**.

## Paso 3. Adición del servicio {{site.data.keyword.cloudant_short_notm}}
{: #resources-swift}

Ahora puede añadir servicios a su aplicación Swift. Para esta guía de aprendizaje, añada el servicio {{site.data.keyword.cloudant_short_notm}} a su app Swift, que crea una base de datos de documentos `JSON` distribuida y completamente gestionada. Cloudant potencia su app aportando escalabilidad, alta disponibilidad y durabilidad en una infraestructura ligera que mantiene sus datos a salvo y sincronizados.

1. En la página **Detalles de app**, pulse **Añadir servicio**.
2. Seleccione **Datos** y pulse **Siguiente**.
3. Seleccione **Cloudant** y pulse **Siguiente**.
4. Pulse **Crear**.
5. Una vez creado el servicio, pulse en él para iniciarlo. En esta página nueva, seleccione **Lanzar el panel de control de Cloudant** para empezar a crear una base de datos y documentos JSON.  También se puede hacer mediante programación.

## Paso 4. Descarga del código y configuración de los SDK de cliente
{: #run-locally-swift}

Para descargar el código, pulse **Descargar código** en `Apps` > `Su app`. El código descargado incluye el [SDK de SwiftCloudant](https://github.com/cloudant/swift-cloudant){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo"), así como un código de inicialización básico. En CocoaPods y el gestor de paquetes de Swift están disponibles los SDK de cliente. Esta solución utiliza CocoaPods.

1. Extraiga el código descargado. A continuación, utilizando un terminal, vaya a la carpeta extraída.
  ```
  cd <Name of Project>
  ```

2. La carpeta incluye un podfile con las dependencias necesarias. Ejecute el mandato siguiente para instalar las dependencias (SDK de cliente):
  ```
  pod install
  ```
  {: codeblock}

## Paso 5. Configurar la app para que utilice su base de datos nueva
{: #config-db-swift}

1. Abra el nombre de archivo que termina en `.xcworkspace` con Xcode, y acceda a `ViewController.swift`. `SwiftCloudant` es el SDK central de Cloudant. `CouchDBClient` es una clase de `SwiftCloudant` y se inicializa en `ViewController.swift`.

  Abrir siempre el nuevo espacio de trabajo Xcode, en lugar del archivo de proyecto Xcode original:
`MyApp.xcworkspace`.
  {: tip}

2. El código de inicialización de la base de datos ya está incluido, tal como se muestra en el siguiente fragmento de código:
  ```swift
  /* Leer las credenciales Cloudant e inicializar la conexión de base de datos */
  if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"), let dictionary = NSDictionary(contentsOfFile: contents) {
            let url = URL(string: dictionary["cloudantUrl"] as! String)
            let client = CouchDBClient(url:url!,
                                       username:dictionary["cloudantUsername"] as? String,
                                       password:dictionary["cloudantPassword"] as? String)
        }
  ```
  {: codeblock}

  Las credenciales de servicio forman parte del archivo `BMSCredentials.plist`.
  {: note}

## Paso 6. Construir sus operaciones de base de datos
{: #build_ops-swift}

Ahora que dispone de una conexión a base de datos en funcionamiento y el SDK configurado, puede empezar a construir las operaciones básica de [crear, leer, actualizar y destruir](/docs/swift/data?topic=swift-cloudant#cloudant) para su caso de uso concreto.

## Pasos siguientes
{: #next-swift}

### Adición de más servicios
{: #moreresources-swift}

Puede añadir más servicios a la app de iOS directamente desde la consola web, como por ejemplo los siguientes servicios utilizados habitualmente:

* [Adición del servicio de notificaciones push](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate#gettingstartedtemplate)
* [Adición de autenticación de usuario con App ID](/docs/services/appid?topic=appid-getting-started#getting-started)

### Utilización de herramientas de desarrollador de {{site.data.keyword.cloud_notm}}
{: #devtools-swift}

También puede aprender a desarrollar apps de Swift utilizando las [Herramientas de desarrollador de {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli), que ofrecen un enfoque de línea de mandatos para crear, desarrollar y desplegar aplicaciones web, móviles y de microservicios completas.
