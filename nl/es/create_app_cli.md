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
{:tip: .tip}

# Creación de una app de Swift de lado del servidor con la CLI
{: #swift_cli}

{{site.data.keyword.cloud}} ofrece soluciones y servicios para potenciar a los desarrolladores y aplicaciones de Swift con la seguridad, la IA y el valor que sus clientes exigían. Con una amplia cartera de ofertas y SDK, puede aprovechar estos servicios y llevar sus aplicaciones de última línea al mercado rápidamente.
{: shortdesc}

La siguiente guía está pensada para ayudar a crear, ejecutar localmente y desplegar una app de Swift de lado del servidor. Obtenga información sobre cómo utilizar {{site.data.keyword.dev_cli_long}} para ejecutar estas acciones con mandatos estándares.

Puede utilizar el {{site.data.keyword.dev_cli_short}} para gestionar las aplicaciones del lado del servidor con más de doce mandatos. Obtenga más información sobre los mandatos `ibmcloud dev` en la [CLI de IBM Cloud Developer Tools](/docs/cli/idt/commands.html).

## Paso 1. Requisitos para los desarrolladores

Para empezar a utilizar {{site.data.keyword.cloud_notm}}, asegúrese de que cumple los requisitos siguientes.

### Sistema operativo

Es recomendable desarrollar apps de Swift utilizando el hardware soportado más reciente de MacOS, y probarlas con los releases más recientes de iOS. Regístrese para una cuenta de [Desarrollador de Apple](https://developer.apple.com/) para habilitar las pruebas en un dispositivo físico.

### iOS y Xcode
{: #ios_and_xcode}

- [Instale Xcode 8+ (o superior)](https://developer.apple.com/xcode/)
- [Despliegue en dispositivos iOS 8 (o superior)](https://support.apple.com/downloads/ios)
- Antes de enviar a Apple, revise las [Directrices de envío de la App Store](https://developer.apple.com/app-store/guidelines/)

### SDK y gestión de dependencias

Las herramientas siguientes garantizan que puede instalar los SDK nativos para que funcionen con los distintos {{site.data.keyword.cloud_notm}} Services.

- [Instale CocoaPods para los SDK de IBM Cloud](https://cocoapods.org/)
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Instale Homebrew para ayudar a instalar Carthage](https://brew.sh/)
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Instale Carthage para los SDK de Watson](https://github.com/Carthage/Carthage)
  ```
  brew install carthage
  ```
  {: codeblock}

## Paso 2. Instalación de herramientas para el desarrollo local

{{site.data.keyword.cloud}} proporciona herramientas de CLI locales que le ayudan a trabajar con varios aspectos de la {{site.data.keyword.cloud_notm}}. Para obtener más información, consulte [Información de {{site.data.keyword.dev_cli_long}}](../cli/index.html). Se recomienda instalarlos para probar un programa de fondo de Swift en una imagen de Docker local antes del despliegue en la nube.

* Para MacOS y Linux, ejecute el mandato siguiente:
  ```
  curl -sL http://ibm.biz/idt-installer | bash
  ```
  {: codeblock}

* Para Windows 10, ejecute el mandato siguiente como administrador:
  ```
  Set-ExecutionPolicy Unrestricted; iex(New-Object Net.WebClient).DownloadString('http://ibm.biz/idt-win-installer')
  ```
  {: codeblock}

  Pulse con el botón derecho del ratón el icono de Windows PowerShell y seleccione **Ejecutar como administrador**.
  {: tip}

## Paso 3. Creación de una app Swift

1. Ejecute el mandato `ibmcloud dev create` desde la CLI de {{site.data.keyword.dev_cli_short}} para generar un iniciador preconfigurado. 
  ```
  ibmcloud dev create
  ```
  {: codeblock}

  Asegúrese de iniciar sesión con una cuenta de {{site.data.keyword.cloud_notm}} para crear un proyecto. Los nuevos usuarios pueden [registrar ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/registration/?cm_sp=dw-bluemix-_-swift-_-devcenter) para obtener una cuenta gratuita. Utilice el mandato `ibmcloud login` para iniciar sesión en la línea de mandatos.
  {: tip}

2. Cuando se le solicite, elija las opciones 1, luego 6 y, por último, 2, tal como se muestra en el ejemplo siguiente:
  ```
  ? Select a resource type:                  
  1. Backend Service / Web App
  2. Mobile Client
  Enter a number> 1

  ? Select a language:
  1. Java - MicroProfile / Java EE
  2. Java - Spring
  3. Node
  4. Python - Django
  5. Python - Flask
  6. Swift
  Enter a number> 6

  ? Select a Starter Kit:
  1. Backend for Frontend - Swift Kitura - A starter for building 
  backend-for-frontend APIs in Swift, using the Kitura framework.
  2. Web App - Create Project
  3. Web App - Swift Kitura Basic - A starter that provides a basic web serving 
  application in Swift, using the Kitura framework.
  Enter a number> 2
  ```
  {: screen}

3. Proporcione un nombre para la aplicación:
  ```
  ? Enter a name for your application> swift_project
  ```
  {: screen}

4. Elija habilitar el soporte de OpenAPI 2.0:
  ```
  ? Enable your application based on an OpenAPI 2.0 Specification document? [y/n]> y
  ```
  {: screen}

  Si el soporte de OpenAPI 2.0 está habilitado, debe proporcionar una vía de acceso al documento de OpenAPI 2.0 como un URL:
  ```
  ? Path to OpenAPI 2.0 document as a url (both yaml and json formats supported)> http://hostname.domain.com/path/to/file.json

  Generating application ...
  ```
  {: screen}

## Paso 4. Creación, ejecución y despliegue de la aplicación

Ahora puede crear, ejecutar y desplegar la aplicación utilizando el {{site.data.keyword.dev_cli_short}}.

1. **Crear**

  Ahora puede crear la aplicación, que es un requisito previo para ejecutar la aplicación. Utilice el mandato siguiente en la raíz del directorio de la aplicación para crear la app:
  ```
  ibmcloud dev build
  ```
  {:codeblock}

2. **Ejecutar**

  Después de una creación correcta, puede ejecutar la aplicación en un contenedor local con el mandato siguiente:
  ```
  ibmcloud dev run
  ```
  {:codeblock}

  Se visualiza un host y un puerto locales para ver la página de destino de la aplicación si el mandato se ejecuta correctamente.

3. **Desplegar**

  Despliegue la aplicación en el {{site.data.keyword.cloud_notm}} con el mandato `deploy`:
  ```
  ibmcloud dev deploy
  ```
  {:codeblock}

## Pasos siguientes

Aprenda a utilizar la {{site.data.keyword.cloud_notm}} Developer Console for Apple que permite a los desarrolladores crear apps a partir de varios kits de iniciación, suministrar y conectar servicios clave optimizados para {{site.data.keyword.cloud_notm}} y descargar rápidamente código de trabajo (o configurar para la entrega continua). Los usuarios pueden crear, ver, configurar y gestionar la app, así como descargar el código de la misma. Al utilizar la Developer Console for Apple, puede evaluar rápidamente y probar los servicios de {{site.data.keyword.cloud_notm}} con una app nueva.

¿Listo para lanzarse? Visite la [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits) ahora para empezar.
{: tip}

Para obtener más información, consulte [Desarrollo de apps de Swift con kits de iniciación](/docs/swift/starter_kit/starter_kits.html).
