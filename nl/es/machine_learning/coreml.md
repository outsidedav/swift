---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-31"

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Uso de Core ML con Watson
{: #swift-coreml}

Con [Core ML](https://developer.apple.com/documentation/coreml){:new_window}, puede integrar una amplia variedad de tipos de modelos de aprendizaje automático en la app. Además de dar soporte a un amplio aprendizaje profundo con más de 30 tipos de capas, también da soporte a modelos estándares, como conjuntos de árbol, SVM y modelos lineales generalizados. En lugar de enviar datos de forma remota para ser analizados, Core ML utiliza tecnologías de bajo nivel, como Metal y Accelerate, para aprovechar de forma transparente la CPU y GPU para ofrecer un rendimiento y una eficiencia máximos.

Puede añadir ML Core y Watson Visual Recognition a una aplicación Swift utilizando los pasos siguientes para entrenar y crear un modelo, descargar y crear dependencias y añadir la clasificación de imagen.
{: shortdesc}

## Antes de empezar
{: #prereqs-coreml}

Para utilizar Core ML y Watson Visual Recognition con Swift, necesita los siguientes componentes:

* iOS 11.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage o gestor de paquetes de Swift

Puede instalar el [SDK de Swift de Watson](https://github.com/watson-developer-cloud/swift-sdk)
mediante [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage) o el [Gestor de paquetes de
Swift](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager). Al usar CocoaPods(https://cocoapods.org/) para gestionar dependencias, solo obtiene las infraestructuras que
necesita, al contrario de lo que sucede con el SDK de Swift de Watson completo. Si no conoce CocoaPods, puede instalarlo fácilmente:

```console
sudo gem install cocoapods
```
{: codeblock}

## Paso 1. Entrenamiento de un modelo con {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}}
{: #train-module-coreml}

Si no existe un modelo actual, se utilizará el primer modelo descubierto de forma remota o cualquiera que exista localmente. Las siguientes instrucciones de gif y acompañamiento le muestran cómo enlazar el servicio a Watson Studio y entrenar su modelo.

![Guía paso a paso del modelo Core ML](images/CoreMLWalkthrough.gif)

### Configuración del servicio desde el panel de control de la app Core ML
{: #service-coreml}

1. Inicie la herramienta de reconocimiento visual desde el panel de control del kit de inicio seleccionando **Iniciar herramienta**.
2. Empiece a crear el modelo seleccionando **Crear modelo**.
2. Si un proyecto aún no está asociado con la instancia de Visual Recognition que ha creado, se crea un proyecto. De lo contrario, vaya a la sección **Crear modelo**.
3. Asigne un nombre al proyecto y pulse **Crear**.
  
  Si no se ha definido ningún almacenamiento, pulse Renovar.
  {: tip}

### Enlace de un servicio a un proyecto
{: #bind-service-coreml}

1. Después de crear el proyecto, se visualiza el panel de control del proyecto.
2. Vaya al separador de configuración, desplácese hacia abajo hasta **Servicios asociados**, seleccione **Añadir servicio** -> **Watson**.
3. En la página de servicios de Watson, seleccione **Reconocimiento visual**.
4. Seleccione el separador **Existente** en la página de información del servicio y, a continuación, seleccione la instancia de servicio desde el panel de control.
5. Ahora que el servicio está enlazado, puede empezar a crear el modelo seleccionando **Activos** en el panel de control de proyecto y, a continuación, pulsando **Añadir modelo de reconocimiento visual**.

### Creación de un modelo
{: #create-model-coreml}

1. Desde la herramienta de creación de modelos, puede actualizar el nombre del clasificador. Si desea utilizar un modelo específico, asegúrese de modificar el campo `defaultClassifierID` en el controlador de vista principal.

2. Desde la barra lateral, cargue los cursos de entrenamiento del modelo en `.zip` comprimidos. A continuación, seleccione cada conjunto de datos y añádalos al modelo desde el menú desplegable. Empiece a añadir más clases que utilicen sus propios conjuntos de imágenes para mejorar el clasificador.

![Adición de clases](images/add_classes.png)

3. Seleccione **Modelo de entrenamiento** y, a continuación, espere a que el modelo esté completamente entrenado.

Todo listo. Ahora, está preparado para descargar el modelo Core ML e integrarlo en la app mediante el [SDK de Swift de Watson Developer Cloud](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.

## Paso 2. Descarga y creación de dependencias
{: #install-depend-coreml}

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

Para una app de producción, puede que también desee especificar un [requisito de versión](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions) concreto para evitar cambios inesperados de los nuevos releases del SDK de Swift de Watson.

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

## Paso 3. Adición de la clasificación de imágenes a la app
{: #add-image-coreml}

Los ejemplos siguientes le ayudan a añadir prestaciones de {{site.data.keyword.visualrecognitionshort}}
Core ML a su aplicación, por lo general en `ViewController.swift`. Con los ejemplos siguientes,
puede ampliar las llamadas de modelo local para su caso de uso.

1. Añada una sentencia de importación para Visual Recognition:
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. Pase la clave de API y la versión para inicializar el SDK:
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  Compruebe la [documentación del parámetro de
versión](https://cloud.ibm.com/apidocs/visual-recognition#versioning) o utilice la fecha creada por el servicio {site.data.keyword.visualrecognitionshort}}.
  {: tip}

3. Añada el código siguiente para descargar o actualizar el modelo ML Core ML local con el clasificador de Watson:
  ```swift
  // Nombre del clasificador que se utilizará
  let classifierID = "your-classifier-ID-here"

  // Umbral de confianza mínimo para el reconocimiento de imágenes
        let threshold = 0.5

  // Actualizar o descargar su modelo
  visualRecognition.updateLocalModel(classifierID: classifierID) { _, error in
      if let error = error {
          print(error)
      } else {
          print("model successfully updated")
      }                
  }
  ```
  {: codeblock}

4. Añada el código siguiente para clasificar una imagen utilizando su modelo Core ML local:
  ```swift
  let image = UIImage(named: "your-image-filename")
  let classifierID = "your-classifier-ID-here"
  let threshold = 0.5

  // Classify your image using your model
  visualRecognition.classifyWithLocalModel(image: image, classifierIDs: [classifierID], threshold: threshold) { classifiedImages, error in
      if let error = error {
          print(error)
    }
      guard let classifiedImages = classifiedImages? else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

5. Explore el resto de las [Prestaciones de clasificación de Core ML](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html) admitidas por el SDK de Watson.

## Paso 4. Utilización de kits de inicio
{: #coreml_starterkits}

Con los kits de inicio, puede utilizar de forma rápida y sencilla las funciones de {{site.data.keyword.cloud_notm}} y Core ML. Puede añadir {{site.data.keyword.visualrecognitionshort}} a cualquier app iOS de cliente o de programa de fondo de lado del servidor utilizando los kits de inicio. Custom Vision Model for Core ML con el kit de inicio de {{site.data.keyword.watson}} ilustra cómo crear un modelo personalizado de {{site.data.keyword.visualrecognitionshort}} y crearlo como un modelo Core ML que puede ejecutar localmente en el dispositivo.

Para añadir {{site.data.keyword.visualrecognitionshort}} a un kit de inicio, siga los pasos siguientes:

1. Seleccione el [kit de inicio](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window} con el que desee trabajar.
2. Cree el proyecto con los servicios predeterminados.
3. Pulse **Añadir recursos > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Descargue el proyecto pulsando **Descargar código**. Para los proyectos de iOS, las credenciales se insertan en el archivo `BMSCredentials.plist` en los campos clave correspondientes. En el caso de proyectos Swift de lado del servidor, puede encontrar estas credenciales en el archivo `config/local-dev.json`.

## Pasos siguientes
{: #coreml_next}

Ahora puede analizar imágenes utilizando sus propios modelos Core ML generados de forma personalizada. Mantenga el ritmo probando una de las opciones siguientes:

* Revise el [SDK de Swift de {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){:new_window} y consulte el resto
de los servicios de Watson admitidos.
* Añada la lógica de nube. Empiece por [desarrollar apps sin servidor](/docs/swift/backend/functions.html).
* Aproveche todas las características que {{site.data.keyword.visualrecognitionshort}} ofrece. Consulte la [documentación](/docs/services/visual-recognition/index.html) para obtener más detalles.
