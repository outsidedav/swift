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

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

El servicio IBM Watson Tone Analyzer permite a la app comprender las emociones y los tonos en el texto. Puede utilizar el servicio para comprender mejor las conversaciones de su usuario o ayudar a los usuarios a comprender cómo se percibe su comunicación por escrito.

## Cómo funciona
{: ##how-it-works}

1. La app elige una selección de texto que se va a analizar (por ejemplo, un mensaje de texto o un canal de Twitter).
2. La app envía el texto al servicio {{site.data.keyword.toneanalyzershort}} utilizando el SDK de Swift de Watson.
3. El servicio analiza el texto utilizando el análisis lingüístico para identificar las emociones y los tonos.
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
{: codeblock}

## Paso 1. Creación de una instancia de Tone Analyzer
{: #create-and-configure-an-instance-of-tone-analyzer}

Suministre una instancia del servicio {{site.data.keyword.toneanalyzershort}}:

1. En el catálogo de {{site.data.keyword.cloud_notm}}, seleccione {{site.data.keyword.toneanalyzershort}}. Se abre la pantalla de configuración del servicio.
2. Dé un nombre a la instancia de servicio, o utilice el nombre preestablecido.
3. Seleccione una app en el menú Conectar si desea enlazar la instancia a una app.
4. Seleccione un plan de precios y pulse **Crear**.
5. Seleccione el separador **Credenciales** para ver las credenciales de servicio. Estos valores se utilizan para conectar con el servicio desde la app.

## Paso 2. Descarga y creación de dependencias
{: ###download-and-build-dependencies}

Utilizando su editor de texto favorito, cree un nuevo archivo denominado `Cartfile` en el directorio raíz del proyecto (donde se encuentra el archivo `.xcodeproj`). A continuación, añada una línea para especificar el SDK de Swift de Watson como una dependencia:

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

Para una app de producción, puede que también desee especificar un [requisito de versión](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) concreto para evitar cambios inesperados de los nuevos releases del SDK de Swift de Watson.

Con el `Cartfile` en su lugar, ahora puede descargar y compilar las dependencias. Utilice un terminal para navegar hasta el directorio raíz del proyecto y, a continuación, ejecute Carthage:
  
  ```bash
  $ carthage update --platform iOS
  ```
  {: codeblock}

Carthage descarga el SDK de Swift de Watson y crea sus infraestructuras en la carpeta `Carthage/Build/iOS` del proyecto.

## Paso 3. Adición de infraestructuras a la app
{: ###add-frameworks-to-your-app}

### Enlazar pasos de Tone Analyzer

Ahora que Carthage crea las infraestructuras del SDK de Swift de Watson, debe enlazar la infraestructura de Tone Analyzer con la app.

1. Abra la app en Xcode y seleccione el proyecto para abrir sus valores.
2. Seleccione el destino de la app y, a continuación, abra el **separador General**.
3. Desplácese hacia abajo a la sección "Infraestructura y bibliotecas enlazados" y pulse el icono `+`.
4. En la ventana que aparece, seleccione **Añadir otros...** y vaya al directorio `Carthage/Build/iOS`. Seleccione **ToneAnalyzerV3.framework** para enlazarlo con la app.

### Copiar pasos de Tone Analyzer

Además de _enlazar_ la infraestructura de Tone Analyzer, también debe _copiarla_ en la app para que sea accesible en tiempo de ejecución. Xcode tiene varias formas distintas de copiar o incorporar una infraestructura, pero puede utilizar un script de Carthage para evitar un [defecto de envío de App Store](http://www.openradar.me/radar?id=6409498411401216) concreto.

1. Con los valores del destino de app abiertos en Xcode, vaya al separador **Fases de compilación**.
2. Pulse el icono `+` y, a continuación, seleccione **Nueva fase de script de ejecución**.
3. Añada el mandato siguiente a la fase de ejecución del script: `/usr/local/bin/carthage copy-frameworks`.
4. Añada la infraestructura de Tone Analyzer a la lista "Archivos de entrada": `$(SRCROOT)/Carthage/Build/iOS/ToneAnalyzerV3.framework`.

Ahora está listo para empezar a trabajar con el SDK de Swift de Watson en su app.

## Paso 4. Análisis de texto en la app
{: #analyze-text-in-your-app}

1. Abra el archivo `ViewController.swift` en Xcode.
2. Añada una sentencia de importación para Tone Analyzer: 
    ```swift
    import ToneAnalyzerV3
    ```
    {: codeblock}

3. Cree una función vacía denominada `toneAnalyzerExample` y, a continuación, llámela desde `viewDidLoad`.
4. Añada el código siguiente a la función `toneAnalyzerExample`. Asegúrese de actualizar el nombre de usuario y la contraseña del servicio.
  ```swift
  import UIKit
  import ToneAnalyzerV3

  class ViewController: UIViewController {

      override func viewDidLoad() {
          super.viewDidLoad()
          toneAnalyzerExample()
      }

      func toneAnalyzerExample() {

          // crear instancia de servicio
          let toneAnalyzer = ToneAnalyzer(
              username: "your-username-here",
              password: "your-password-here",
              version: "yyyy-mm-dd"
          )

          // texto a analizar
          let review = """
              I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
          """

          // analizar texto
          let toneInput = ToneInput(text: review)
          let failure = { (error: Error) in print(error) }
          toneAnalyzer.tone(toneInput: toneInput, contentType: "application/json", failure: failure) { analysis in
              for tone in analysis.documentTone.tones! {
                  print("\(tone.toneName): \(tone.score)")
              }
          }
      }
  }
  ```
  {: codeblock}

Cuando ejecuta la app, ve el siguiente análisis en la consola:
```
Sadness (Tristeza): 0.575803
Tentative (Indecisión): 0.867377
```
{: screen}

## Utilización de los kits de iniciación
{: #tone_starterkits}

Los [kits de iniciación](https://console.bluemix.net/developer/appledevelopment/starter-kits) son una de las formas más rápidas de aprovechar las prestaciones de {{site.data.keyword.cloud_notm}}. Puede utilizar el servicio {{site.data.keyword.toneanalyzershort}} seleccionando el kit de iniciación de **Tone Analyzer for iOS with Watson**. Este servicio utiliza funciones de aprendizaje profundo para evaluar los pasajes de texto. La aplicación Tone Analyzer identifica el tono del orador (feliz, triste, seguro, etc.), ya que se relaciona con varias categorías.

Para empezar con este kit de iniciación:

1. Seleccione el kit de iniciación que se encuentra [aquí](https://console.bluemix.net/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson).
2. Cree el proyecto con los servicios predeterminados.
3. Descargue el proyecto pulsando **Descargar código**. Las credenciales de servicio se inyectan en el archivo `BMSCredentials.plist` en los campos clave correspondientes.

## Pasos siguientes
{: #tone_next}

¡Buen trabajo! El {{site.data.keyword.toneanalyzershort}} ahora se añade a la app. Mantenga el ritmo probando una de las opciones siguientes:

* Vea el [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* Para obtener más información, consulte [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/).

