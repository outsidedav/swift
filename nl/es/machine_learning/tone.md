---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: watson swift, tone anaylzer swift, cocoapods swift, swift sdk install, starter kit watson

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

El servicio {{site.data.keyword.ibmwatson}} {{site.data.keyword.toneanalyzershort}} permite que la app pueda entender las emociones y los tonos del texto. Puede utilizar el servicio para comprender mejor las conversaciones de su usuario o ayudar a los usuarios a comprender cómo se percibe su comunicación por escrito.

## Cómo funciona
{: #how-it-works-tone}

1. La app elige una selección de texto que se va a analizar (por ejemplo, un mensaje de texto o un canal de Twitter).
2. La app envía el texto al servicio {{site.data.keyword.toneanalyzershort}} utilizando el SDK de Swift de Watson.
3. El servicio analiza el texto utilizando el análisis lingüístico para identificar las emociones y los tonos.
4. El análisis del servicio se devuelve a la app mediante el
[SDK de Swift de Watson](https://github.com/watson-developer-cloud/swift-sdk).

## Antes de empezar
{: #prereqs-tone}

Asegúrese de cumplir los siguientes requisitos previos:

* OS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage o gestor de paquetes de Swift

Puede instalar el [SDK de Swift de Watson](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") utilizando
[CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") o el
[Gestor de paquetes de Swift](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"). Al usar CocoaPods para gestionar dependencias, solo obtiene las infraestructuras que necesita, al contrario de lo que sucede con el SDK de Swift de Watson completo. Si no conoce CocoaPods, puede instalarlo fácilmente:

```bash
$ sudo gem install cocoapods
```
{: codeblock}

## Paso 1. Creación de una instancia de Tone Analyzer
{: #create-instance-tone}

Suministre una instancia del servicio {{site.data.keyword.toneanalyzershort}}:

1. En el catálogo de {{site.data.keyword.cloud_notm}}, seleccione {{site.data.keyword.toneanalyzershort}}. Se abre la pantalla de configuración del servicio.
2. Dé un nombre a la instancia de servicio, o utilice el nombre preestablecido.
3. Seleccione una app en el menú Conectar si desea enlazar la instancia a una app.
4. Seleccione un plan de precios y pulse **Crear**.
5. Seleccione el separador **Credenciales** para ver las credenciales de servicio. Estos valores se utilizan para conectar con el servicio desde la app.

## Paso 2. Descarga y creación de dependencias
{: #download-depend-tone}

Mediante su editor de texto preferido, cree un `Podfile` nuevo en el directorio raíz
de su proyecto (donde se encuentra su archivo `.xcodeproj`), ejecutando
`pod init`. A continuación, añada una línea para especificar la infraestructura
{{site.data.keyword.conversationshort}} del SDK de Swift de Watson como dependencia:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonToneAnalyzerV3'
```
{: codeblock}

Para una app de producción, puede que también desee especificar un [requisito de versión](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") concreto para evitar cambios inesperados de los nuevos releases del SDK de Swift de Watson.

Con el archivo `Podfile` en su lugar, ahora puede descargar las dependencias. Utilice un terminal para acceder al directorio raíz de su proyecto y ejecute CocoaPods:

```console
pod install
```
{: codeblock}

Cocoapods descarga la infraestructura {{site.data.keyword.toneanalyzershort}} y la compila
en la carpeta `Pods/` de su proyecto.

Para evitar errores de construcción de Pod, cuando abra el proyecto en Xcode,
abra el archivo que termina en `.xcworkspace` en lugar de `.xcodeproj`.
{: tip}

## Paso 3. Análisis de texto en su app
{: #analyze-text-tone}

Los ejemplos siguientes le ayudan a añadir prestaciones de {{site.data.keyword.toneanalyzershort}}
a su aplicación, por lo general en `ViewController.swift`. Con los ejemplos siguientes,
puede ampliar las llamadas de Tone Analyzer para su caso de uso.

1. Añada una sentencia de importación para Tone Analyzer:
  ```swift
  import ToneAnalyzer
  ```
  {: codeblock}

2. Crear una instancia del servicio Tone Analyzer:
  ```swift
  let toneAnalyzer = ToneAnalyzer(version: "yyyy-mm-dd", apiKey: "your-api-key-here")
  ```
  {: codeblock}

  Compruebe la [documentación del parámetro de
versión](https://{DomainName}/apidocs/tone-analyzer#versioning){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")
o utilice la fecha creada por el servicio {site.data.keyword.conversationshort}}.
  {: tip}

3. Proporcione texto para el análisis y proceso de los resultados:
  ```swift
  // Texto a analizar
  let review = """
      I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
  """

  // Analizar texto
  let input = ToneInput(text: review)
  toneAnalyzer.tone(toneInput: .toneInput(input)) { response, error in
      if let error = error {
          print(error)
          return
      }

      guard let tones = response?.result?.documentTone.tones else {
          print("No se ha podido analizar la entrada de tono")
          return
      }

      for tone in tones {
          print("\(tone.toneName): \(tone.score)")
              }
  }
  ```
  {: codeblock}

  Cuando ejecute el código, verá el análisis siguiente en la consola:
  ```
  Sadness (Tristeza): 0.575803
Tentative (Indecisión): 0.867377
  ```
  {: screen}

4. Explore la [documentación de Tone Analyzer](https://watson-developer-cloud.github.io/swift-sdk/services/ToneAnalyzerV3/index.html){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")
del SDK de Watson para crear la funcionalidad de su aplicación.

## Utilización de los kits de inicio
{: #tone_starterkits}

Los [Kits de inicio](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") son una de las formas
más rápidas de utilizar las prestaciones de {{site.data.keyword.cloud_notm}}. Puede utilizar el servicio {{site.data.keyword.toneanalyzershort}} seleccionando el kit de inicio de **Tone Analyzer for iOS with Watson**. Este servicio utiliza funciones de aprendizaje profundo para evaluar los pasajes de texto. La aplicación Tone Analyzer identifica el tono del orador (feliz, triste, seguro, etc.), ya que se relaciona con varias categorías.

Para empezar con este kit de inicio:

1. Seleccione el kit de inicio que se encuentra [aquí](https://{DomainName}/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").
2. Cree el proyecto con los servicios predeterminados.
3. Descargue el proyecto pulsando **Descargar código**. Las credenciales de servicio se inyectan en el archivo `BMSCredentials.plist` en los campos clave correspondientes.

## Pasos siguientes
{: #tone_next notoc}

¡Buen trabajo! {{site.data.keyword.toneanalyzershort}} se ha añadido a su app. Mantenga el ritmo probando una de las opciones siguientes:

* Consulte el [SDK de Swift de Watson en GitHub](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") y explore el resto de los servicios de Watson con soporte.
* Para obtener más información, consulte [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").
