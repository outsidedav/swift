---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Adición de aprendizaje automático a una app
{: #overview}

Con [Core ML](https://developer.apple.com/documentation/coreml){:new_window}, puede integrar una amplia variedad de tipos de modelos de aprendizaje automático en la app. Además de dar soporte a un amplio aprendizaje profundo con más de 30 tipos de capas, también da soporte a modelos estándares, como conjuntos de árbol, SVM y modelos lineales generalizados. En lugar de enviar datos de forma remota para ser analizados, Core ML utiliza tecnologías de bajo nivel, como Metal y Accelerate, para aprovechar de forma transparente la CPU y GPU para ofrecer un rendimiento y una eficiencia máximos.

## Antes de empezar
{: #before-you-begin}

Para utilizar Core ML y Watson Visualization, necesita los siguientes componentes:

  * iOS 11.0+
  * Xcode 9.0+
  * Swift 4.0+
  * Carthage

Para instalar Carthage, utilice los siguientes mandatos `brew`:
```
$ brew update
$ brew install carthage
```
{: codeblock}

## Paso 1. Entrenamiento de un modelo con {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}}
{: #training-your-model}

Si no existe un modelo actual, se utilizará el primer modelo que se encuentre de forma remota o cualquiera que exista localmente. Las siguientes instrucciones de gif y acompañamiento le muestran cómo enlazar el servicio a Watson Studio y entrenar el modelo.

![Guía paso a paso del modelo Core ML](images/CoreMLWalkthrough.gif)

### Configuración del servicio desde el panel de control de la app Core ML

1. Inicie la herramienta de reconocimiento visual desde el panel de control del kit de iniciación seleccionando **Iniciar herramienta**.
2. Empiece a crear el modelo seleccionando **Crear modelo**.
2. Si un proyecto aún no está asociado con la instancia de Visual Recognition que ha creado, se crea un proyecto. De lo contrario, vaya a la sección **Crear modelo**.
3. Asigne un nombre al proyecto y pulse **Crear**.
  **Sugerencia**: Si no se ha definido ningún almacenamiento, pulse Renovar.

### Enlace de un servicio a un proyecto

1. Después de crear el proyecto, se visualiza el panel de control del proyecto.
2. Vaya al separador de configuración, desplácese hacia abajo hasta **Servicios asociados**, seleccione **Añadir servicio** -> **Watson**.
3. En la página de servicios de Watson, seleccione **Reconocimiento visual**.
4. Seleccione el separador **Existente** en la página de información del servicio y, a continuación, seleccione la instancia de servicio desde el panel de control.
5. Ahora que el servicio está enlazado, puede empezar a crear el modelo seleccionando **Activos** en el panel de control de proyecto y, a continuación, pulsando **Añadir modelo de reconocimiento visual**.

### Creación de un modelo

1. Desde la herramienta de creación del modelo, modifique el nombre del clasificador si lo desea. De forma predeterminada, la aplicación utiliza el primero disponible a menos que se especifique. Si desea utilizar un modelo específico, asegúrese de modificar el campo `defaultClassifierID` en el controlador de vista principal.

2. Desde la barra lateral, cargue los cursos de entrenamiento del modelo en archivos .zip. A continuación, seleccione cada conjunto de datos y añádalos al modelo desde el menú desplegable. Empiece a añadir más clases que utilicen sus propios conjuntos de imágenes para mejorar el clasificador.

![Adición de clases](images/add_classes.png)

3. Seleccione **Modelo de entrenamiento** y, a continuación, espere a que el modelo esté completamente entrenado.

Todo listo. Ahora, está preparado para descargar el modelo Core ML e integrarlo en la app mediante el [SDK de Swift de {{site.data.keyword.watson}} Developer Cloud](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.

## Paso 2. Descarga y creación de dependencias
{: #installing-dependencies}

1. Mediante su editor de texto favorito, cree un archivo nuevo denominado **Cartfile** en el directorio raíz del proyecto donde se encuentra el archivo `.xcodeproj`.

2. Añada una línea para especificar el SDK de Swift de {{site.data.keyword.watson}} Developer Cloud como una dependencia, por ejemplo:

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: pre}

  Para una app de producción, es posible que también desee especificar un requisito de versión concreto para evitar cambios inesperados de los nuevos releases del SDK.
  {: tip}

3. Utilice un terminal para navegar hasta el directorio raíz del proyecto y ejecutar Carthage:

  ```
  $ carthage update --platform iOS
  ```
  {: pre}

  El SDK de Swift de {{site.data.keyword.watson}} Developer Cloud se descargará y su infraestructura se crea en la carpeta `Carthage/Build/iOS` del proyecto.

## Paso 3. Adición de la clasificación de imágenes a la app
{: #adding-image-classification}

```Swift
//
//  ViewController.swift
//  CoreMLImageClassificationExample
//
//  Created by Aaron Liberatore on 3/1/18.
//  Copyright © 2018 Aaron Liberatore. Todos los derechos reservados.
//

import UIKit
import VisualRecognitionV3

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        initializeCoreMLExample()
    }

    func failureHandler(_ error: Error) {
        print(error)
    }

    func initializeCoreMLExample() {
        // Configuración

        // Clave de API del servicio Watson Visual Recognition
        let apiKey = "your-apiKey-here"

        // Nombre del clasificador que se utilizará
        let classifierID = "your-classifier-here"

        // Umbral de confianza mínimo para el reconocimiento de imágenes
        let threshold = 0.5

        // Inicializar servicio
        let visualRecognition = VisualRecognition(apiKey: apiKey, version: "03-01-2018")

        // Actualizar o descargar su modelo
        visualRecognition.updateLocalModel(classifierID: classifierID,
                                           failure: failureHandler) {

            // Clasificar su imagen utilizando su modelo
            visualRecognition.classifyWithLocalModel(image: image, classifierIDs: [classifierID],
                                                     threshold: threshold,
                                                     failure: self.failureHandler) { classifiedImages in

                 print(classifiedImages)
             }            
        }
    }
}
```
{: codeblock}

## Paso 4. Utilización de kits de iniciación
{: #coreml_starterkits}

Con los kits de iniciación, puede aprovechar de forma rápida y sencilla las funciones de {{site.data.keyword.cloud_notm}} y Core ML. Puede añadir {{site.data.keyword.visualrecognitionshort}} a cualquier app iOS de cliente o de programa de fondo de lado del servidor utilizando los kits de iniciación. Custom Vision Model for Core ML con el kit de iniciación de {{site.data.keyword.watson}} ilustra cómo crear un modelo personalizado de {{site.data.keyword.visualrecognitionshort}} y crearlo como un modelo Core ML que puede ejecutar localmente en el dispositivo.

Para añadir {{site.data.keyword.visualrecognitionshort}} a un kit de iniciación, siga los pasos siguientes:

1. Seleccione el [kit de iniciación](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} con el que desee trabajar.
2. Cree el proyecto con los servicios predeterminados.
3. Pulse **Añadir recursos > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Descargue el proyecto pulsando **Descargar código**. Para los proyectos de iOS, las credenciales se insertan en el archivo `BMSCredentials.plist` en los campos clave correspondientes. En el caso de proyectos Swift de lado del servidor, puede encontrar estas credenciales en el archivo `config/local-dev.json`.

## Pasos siguientes
{: #coreml_next}

Ahora puede analizar imágenes utilizando modelos de Core ML generados de forma personalizada. Mantenga el ritmo probando una de las opciones siguientes:

* Añada la lógica de nube. Empiece por [desarrollar apps sin servidor](/docs/swift/backend/functions.html).
* Aproveche todas las características que {{site.data.keyword.visualrecognitionshort}} tiene que ofrecer. Consulte la [documentación](/docs/services/visual-recognition/index.html) para obtener más detalles.
