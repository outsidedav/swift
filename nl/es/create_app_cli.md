---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-19"

keywords: server-side swift, swift cli, swift dependency, swift commands app, create app swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Creación de una app de Swift de lado del servidor con la CLI
{: #swift_cli}

{{site.data.keyword.cloud}} ofrece soluciones y servicios que proporcionan a los desarrolladores de Swift y a las aplicaciones la seguridad, la IA y el valor que sus clientes exigen. Con una amplia cartera de ofertas y SDK, puede utilizar estos servicios y llevar aplicaciones innovadoras al mercado rápidamente.
{: shortdesc}

La siguiente guía está pensada para ayudar a crear, ejecutar localmente y desplegar una app de Swift de lado del servidor. Obtenga información sobre cómo utilizar {{site.data.keyword.dev_cli_long}} para ejecutar estas acciones con mandatos estándares.

Puede utilizar el {{site.data.keyword.dev_cli_short}} para gestionar las aplicaciones del lado del servidor con más de doce mandatos. Obtenga más información sobre los mandatos `ibmcloud dev` en la [CLI de {{site.data.keyword.dev_cli_notm}}](/docs/cli/idt?topic=cloud-cli-idt-cli).

## Paso 1. Requisitos para los desarrolladores
{: #prereqs-swift-cli}

Para empezar a utilizar {{site.data.keyword.cloud_notm}}, asegúrese de que cumple los requisitos siguientes.

### Sistema operativo
{: #swift-cli-os-reqs}

Desarrolle apps de Swift con prácticas recomendadas utilizando el hardware soportado más reciente de MacOS, y pruébelas con los releases más recientes de iOS. Regístrese para una cuenta de [Desarrollador de Apple](https://developer.apple.com/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") para habilitar las pruebas en un dispositivo físico.

### iOS y Xcode
{: #swift-cli-ios_xcode}

- [Instale Xcode 8+ (o superior)](https://developer.apple.com/xcode/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")
- [Despliegue en dispositivos iOS 8 (o superior)](https://support.apple.com/downloads/ios){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")
- Antes de enviar a Apple, revise las [Directrices de envío de la App Store](https://developer.apple.com/app-store/resources/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")

### SDK y gestión de dependencias
{: #swift-cli-sdk-dependency}

Las herramientas siguientes garantizan que puede instalar los SDK nativos para que funcionen con los distintos {{site.data.keyword.cloud_notm}} Services.

- [Instale CocoaPods para los SDK de {{site.data.keyword.cloud_notm}}](https://cocoapods.org/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Instale Homebrew para ayudar a instalar Carthage](https://brew.sh/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Instale Carthage para los SDK de Watson](https://github.com/Carthage/Carthage){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")
  ```
  brew install carthage
  ```
  {: codeblock}

## Paso 2. Instalación de herramientas para el desarrollo local
{: #swift-cli-install-tools}

{{site.data.keyword.cloud}} proporciona herramientas de CLI locales que le ayudan a trabajar con varios aspectos de la {{site.data.keyword.cloud_notm}}. Para obtener más información, consulte [Información de {{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-getting-started). Puede utilizar las herramientas para probar un programa de fondo de Swift en una imagen de Docker local antes del despliegue en la nube.

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
{: #create-swift-app-cli}

1. Ejecute el mandato `ibmcloud dev create` desde la CLI de {{site.data.keyword.dev_cli_short}} para generar un iniciador preconfigurado. 
  ```
  ibmcloud dev create
  ```
  {: codeblock}

  Asegúrese de iniciar sesión con una cuenta de {{site.data.keyword.cloud_notm}} para crear un proyecto. Los nuevos usuarios pueden [registrar](https://{DomainName}/registration){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") para obtener una cuenta gratuita. Utilice el mandato `ibmcloud login` para iniciar sesión en la línea de mandatos.
  {: tip}

2. Cuando se le solicite, seleccione las opciones 1, luego 6 y, por último, 2, tal como se muestra en el ejemplo siguiente:
  ```
  ? Select a service type:
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

  Generating application ...
  ```
  {: screen}

## Paso 4. Creación, ejecución y despliegue de la aplicación
{: #swift-cli-deploy}

Ahora puede crear, ejecutar y desplegar la aplicación utilizando el {{site.data.keyword.dev_cli_short}}.

### Creación de la app
{: #swift-build-cli}

Ahora puede crear la aplicación, que es un requisito previo para ejecutar la aplicación. Utilice el mandato siguiente en la raíz del directorio de la aplicación para crear la app:
```
ibmcloud dev build
```
{: codeblock}

### Ejecución de la app
{: #swift-run-cli}

Después de una creación correcta, puede ejecutar la aplicación en un contenedor local con el mandato siguiente:
```
ibmcloud dev run
```
{: codeblock}

Se visualiza un host y un puerto locales para ver la página de destino de la aplicación si el mandato se ejecuta correctamente.

### Despliegue de la app
{: #swift-deploy-cli}

Despliegue la aplicación en el {{site.data.keyword.cloud_notm}} con el mandato `deploy`:
```
ibmcloud dev deploy
```
{: codeblock}

## Pasos siguientes
{: #swift-cli-next notoc}

Aprenda a utilizar la {{site.data.keyword.cloud_notm}} Developer Console for Apple que permite a los desarrolladores crear apps a partir de varios kits de inicio, crear y conectar servicios clave optimizados para {{site.data.keyword.cloud_notm}} y descargar rápidamente código funcional o configurarlo para la entrega continua. Los usuarios pueden crear, ver, configurar y gestionar la app, así como descargar el código de la misma. Al utilizar la Developer Console for Apple, puede evaluar rápidamente y probar los servicios de {{site.data.keyword.cloud_notm}} con una app nueva.

¿Listo para lanzarse? Visite la [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") ahora para empezar.
{: tip}

Para obtener más información, consulte [Desarrollo de apps de Swift con kits de inicio](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro).
