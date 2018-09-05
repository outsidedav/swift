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

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

El servicio de {{site.data.keyword.visualrecognitionfull}} permite que la app etiquete, clasifique y entrene contenido visual de forma rápida y precisa utilizando el aprendizaje automático. El servicio puede ayudar a clasificar virtualmente cualquier contenido visual, a entrenar su propio modelo personalizado en minutos y a detectar caras.

## Cómo funciona
{: ##how-it-works}

1. La app elige una selección de imágenes para analizarse.
2. La app envía la imagen al servicio {{site.data.keyword.visualrecognitionshort}} utilizando el SDK de Swift de Watson.
3. El servicio analiza la imagen utilizando el análisis de clasificación para identificar escenas, objetos, caras, etc.
4. El análisis del servicio se devuelve a la app mediante el SDK de Swift de Watson.

## Antes de empezar
{: ###before-you-begin}

Primero, asegúrese de cumplir los siguientes requisitos previos:
<ul>
  <li>iOS 8.0+</li>
  <li>Xcode 9.0+</li>
  <li>Swift 3.2+ o Swift 4.0+</li>
  <li>Carthage</li>
</ul>

Se recomienda utilizar [Carthage](https://github.com/Carthage/Carthage) para gestionar dependencias y crear el SDK de Swift de Watson para la aplicación. Si es nuevo en Carthage, puede instalarlo con [Homebrew](http://brew.sh/):

```bash
$ brew update
$ brew install carthage
```

## Paso 1. Creación de una instancia de Visual Recognition
{: ###create-and-configure-an-instance-of-visual-recognition}

Suministre una instancia del servicio {{site.data.keyword.visualrecognitionshort}}:

1. En el catálogo de {{site.data.keyword.cloud_notm}}, seleccione **{{site.data.keyword.visualrecognitionshort}}**. Se abre la pantalla de configuración del servicio.
2. Dé un nombre a la instancia de servicio, o utilice el nombre preestablecido.
3. Seleccione una app en el menú **Conectar** si desea enlazar la instancia a una app.
4. Seleccione un plan de precios y pulse **Crear**.
5. Seleccione el separador **Credenciales** para ver las credenciales de servicio. Estos valores se utilizan para conectar con el servicio desde la app.

## Paso 2. Descarga y creación de dependencias
{: ###download-and-build-dependencies}

Utilizando su editor de texto favorito, cree un archivo denominado `Cartfile` en el directorio raíz del proyecto (donde se encuentra el archivo `.xcodeproj`). A continuación, añada una línea para especificar el SDK de Swift de Watson como una dependencia:
```
github "watson-developer-cloud/swift-sdk"
```
{: pre}

Para una app de producción, puede especificar un [requisito de versión](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) concreto para evitar cambios inesperados de los nuevos releases del SDK de Swift de Watson.

Con el `Cartfile` en su lugar, ahora puede descargar y compilar las dependencias. Utilice un terminal para navegar hasta el directorio raíz del proyecto y, a continuación, ejecute Carthage:

```bash
carthage update --platform iOS
```
{: pre}

Carthage descarga el SDK de Swift de Watson y crea sus infraestructuras en la carpeta `Carthage/Build/iOS` del proyecto.

## Paso 3. Adición de infraestructuras a la app
{: ###add-frameworks-to-your-app}

### Enlazar los pasos de Visual Recognition:

Ahora que Carthage crea las infraestructuras del SDK de Swift de Watson, debe enlazar la infraestructura de Visual Recognition con la app.

1. Abra la app en Xcode, y seleccione el proyecto para abrir sus valores.
2. Seleccione el destino de la app y, a continuación, abra el separador **General**.
3. Desplácese hacia abajo a la sección "Infraestructura y bibliotecas enlazados" y pulse el icono `+`.
4. En la ventana que aparece, seleccione **Añadir otros...** y vaya al directorio `Carthage/Build/iOS`. Seleccione **VisualRecognitionV3.framework** para enlazarlo con la app.

### Copiar los pasos de Visual Recognition:

Además de _enlazar_ la infraestructura de Visual Recognition, también debe _copiarla_ en la app para que sea accesible en el tiempo de ejecución. Xcode tiene varias formas distintas de copiar o incorporar una infraestructura, pero puede utilizar un script de Carthage para evitar un [defecto de envío de App Store](http://www.openradar.me/radar?id=6409498411401216) concreto.

1. Con los valores del destino de app abiertos en Xcode, vaya al separador **Fases de compilación**.
2. Pulse el icono `+` y, a continuación, seleccione **Nueva fase de ejecución del script**.
3. Añada el mandato siguiente a la fase de ejecución del script: `/usr/local/bin/carthage copy-frameworks`.
4. Añada la infraestructura de Visual Recognition a la lista **Archivos de entrada**: `$(SRCROOT)/Carthage/Build/iOS/VisualRecognitionV3.framework`.

Ahora está listo para empezar a trabajar con el SDK de Swift de Watson en su app.

## Paso 4. Análisis de imágenes en la app
{: #analyze-images-in-your-app}

1. Abra el archivo `ViewController.swift` en Xcode.

1. Añada una sentencia de importación para Visual Recognition:
    ```swift
    import VisualRecognitionV3
    ```
    {: pre}

1. Pase la clave de API y la versión (puede utilizar la fecha de hoy) para inicializar el SDK:
    ```swift
    let visualRecognition = VisualRecognition(apiKey: "your-api-key", version: "yyyy-mm-dd")
    ```
    {: pre}

1. Añada el código siguiente para clasificar una imagen:
    ```swift
    let url = "your-image-url"
    let failure = { (error: Error) in print(error) }
    visualRecognition.classify(url: url, failure: failure) { classifiedImages in
        print(classifiedImages)
    }
    ```
    {: pre}

## Utilización de los kits de iniciación
{: #recognition_starterkits}

Los [kits de iniciación](https://console.bluemix.net/developer/appledevelopment/starter-kits) son una de las formas más rápidas de aprovechar las prestaciones de {{site.data.keyword.cloud_notm}}. Puede utilizar el servicio de {{site.data.keyword.visualrecognitionshort}} seleccionando el kit de iniciación de **Visual Recognition for iOS with Watson**. Este servicio evalúa y clasifica las imágenes. Cargue imágenes nuevas o existentes desde su dispositivo móvil, y la app de Visual Recognition etiqueta y clasifica rápidamente el contenido de la imagen.

Para empezar:
1. Seleccione el kit de iniciación que se encuentra [aquí](https://console.bluemix.net/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson).
2. Cree el proyecto con los servicios predeterminados.
3. Descargue el proyecto pulsando **Descargar código**. Las credenciales de servicio se inyectan en el archivo `BMSCredentials.plist` en los campos clave correspondientes.

## Pasos siguientes
{: #recognition_next}

¡Buen trabajo! Visual Recognition ahora está disponible para la app. Mantenga el ritmo probando una de las opciones siguientes:
* Vea el [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* Para obtener más información, consulte [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/).

