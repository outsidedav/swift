---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: swift visual recognition, train swift, cocoapods swift, swift sdk install, starter kit watson swift, image swift classify, machine learning swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

El servicio de {{site.data.keyword.visualrecognitionfull}} permite que la app etiquete, clasifique y entrene contenido visual de forma rápida y precisa utilizando el aprendizaje automático. El servicio puede ayudar a clasificar virtualmente cualquier contenido visual, a entrenar su propio modelo personalizado en minutos y a detectar caras.

## Cómo funciona
{: #how-it-works-recognition}

1. La app elige una selección de imágenes para analizarse.
2. La app envía la imagen al servicio {{site.data.keyword.visualrecognitionshort}} utilizando el SDK de Swift de Watson.
3. El servicio analiza la imagen utilizando el análisis de clasificación para identificar escenas, objetos, caras, etc.
4. El análisis del servicio se devuelve a la app mediante el SDK de Swift de Watson.

## Antes de empezar
{: #prereqs-recognition}

Asegúrese de cumplir los siguientes requisitos previos:

* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage o gestor de paquetes de Swift

Puede instalar el [SDK de Swift de Watson](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") utilizando
[CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") o el
[Gestor de paquetes de Swift](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"). Al usar CocoaPods(https://cocoapods.org/) para gestionar dependencias, solo obtiene las infraestructuras que
necesita, al contrario de lo que sucede con el SDK de Swift de Watson completo. Si no conoce CocoaPods, puede instalarlo fácilmente:
```console
sudo gem install cocoapods
```
{: codeblock}

## Paso 1. Creación de una instancia de Visual Recognition
{: #create-instance-recognition}

Suministre una instancia del servicio {{site.data.keyword.visualrecognitionshort}}:

1. En el catálogo de {{site.data.keyword.cloud_notm}}, seleccione **{{site.data.keyword.visualrecognitionshort}}**. Se abre la pantalla de configuración del servicio.
2. Dé un nombre a la instancia de servicio, o utilice el nombre preestablecido.
3. Seleccione una app en el menú **Conectar** si desea enlazar la instancia a una app.
4. Seleccione un plan de precios y pulse **Crear**.
5. Seleccione el separador **Credenciales** para ver las credenciales de servicio. Estos valores se utilizan para conectar con el servicio desde la app.

## Paso 2. Descarga y creación de dependencias
{: #download-depend-recognition}

Mediante su editor de texto preferido, cree un `Podfile` nuevo en el directorio raíz
de su proyecto (donde se encuentra su archivo `.xcodeproj`), ejecutando
`pod init`. A continuación, añada una línea para especificar la infraestructura
{{site.data.keyword.visualrecognitionshort}} del SDK de Swift de Watson como dependencia:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

Para una app de producción, puede que también desee especificar un [requisito de versión](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") concreto para evitar cambios inesperados de los nuevos releases del SDK de Swift de Watson.

Con el archivo `Podfile` en su lugar, ahora puede descargar las dependencias. Utilice un terminal para acceder al directorio raíz de su proyecto y ejecute CocoaPods:

```console
pod install
```
{: codeblock}

Cocoapods descarga la infraestructura {{site.data.keyword.visualrecognitionshort}} y la compila
en la carpeta `Pods/` de su proyecto.

Para evitar errores de construcción de Pod, cuando abra el proyecto en Xcode,
abra el archivo que termina en `.xcworkspace` en lugar de `.xcodeproj`.
{: tip}

## Paso 3. Análisis de imágenes en su app
{: #analyze-images-recognition}

Los ejemplos siguientes le ayudan a añadir prestaciones de {{site.data.keyword.visualrecognitionshort}}
a su aplicación, por lo general en `ViewController.swift`. Con los ejemplos siguientes,
puede ampliar las llamadas de Reconocimiento visual (Visual Recognition) para su caso de uso.

1. Añada una sentencia de importación para Visual Recognition:
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. Pase la clave de API y la versión (puede utilizar la fecha de hoy) para inicializar el SDK:
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  Puede revisar la [documentación del parámetro de
versión](https://cloud.ibm.com/apidocs/visual-recognition#versioning){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") o utilice la fecha creada por el servicio {site.data.keyword.conversationshort}}.
  {: tip}

3. Añada el código siguiente para clasificar una imagen:
  ```swift
  let url = "your-image-url"
  visualRecognition.classify(url: url) { response, error in
      if let error = error {
          print(error)
    }
      guard let classifiedImages = response?.result else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

Hay varios métodos de clasificación admitidos por la infraestructura de Reconocimiento visual. Explore la
[documentación de Visual Recognition](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") del SDK de Watson para ver el que mejor se ajusta a su aplicación.
{: tip}

## Utilización de los kits de inicio
{: #recognition_starterkits}

Los [Kits de inicio](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") son una de las formas
más rápidas de utilizar las prestaciones de {{site.data.keyword.cloud_notm}}. Puede utilizar el servicio de {{site.data.keyword.visualrecognitionshort}} seleccionando el kit de inicio de **Visual Recognition for iOS with Watson**. Este servicio evalúa y clasifica las imágenes. Cargue imágenes nuevas o existentes desde su dispositivo móvil, y la app de Visual Recognition etiqueta y clasifica rápidamente el contenido de la imagen.

Para empezar:
1. Seleccione el kit de inicio que se encuentra [aquí](https://cloud.ibm.com/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").
2. Cree el proyecto con los servicios predeterminados.
3. Descargue el proyecto pulsando **Descargar código**. Las credenciales de servicio se inyectan en el archivo `BMSCredentials.plist` en los campos clave correspondientes.

## Pasos siguientes
{: #recognition_next notoc}

¡Buen trabajo! Visual Recognition ahora está disponible para la app. Mantenga el ritmo probando una de las opciones siguientes:
* Revise el [SDK de Swift de Watson](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") y consulte el resto de los servicios de Watson admitidos.
* Para obtener más información, consulte [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").
