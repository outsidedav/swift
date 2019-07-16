---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: chatbot swift, virtual assistant swift, assistant swift, watson swift starter, assistantv2 swift, watson sdk swift, add chatbot swift, add assistant swift

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Adición de un chatbot
{: #assistant}

Puede utilizar el servicio {{site.data.keyword.conversationshort}} para crear aplicaciones que comprendan la entrada de lenguaje natural y respondan a los usuarios con conversación de tipo humano.

En la lista siguiente se describe el flujo de la forma en que funciona la integración:

1. Los usuarios interactúan con la interfaz que se implementa en la app.
2. La app envía la entrada de usuario a {{site.data.keyword.conversationshort}} utilizando el SDK de Swift de {{site.data.keyword.watson}}.
3. El SDK de Swift de {{site.data.keyword.watson}} se conecta a un espacio de trabajo, que es un contenedor para el flujo de diálogo y los datos de entrenamiento.
4. El espacio de trabajo interpreta la entrada de usuario y dirige el flujo de la conversación, enviando una respuesta a la app.
5. La app muestra la respuesta para el usuario.

## Antes de empezar
{: #prereqs-chatbot}

Asegúrese de cumplir los siguientes requisitos previos:

* Una instancia del [servicio {{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-getting-started#getting-started)
* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage o gestor de paquetes de Swift

Puede instalar el [SDK de Swift de Watson](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") utilizando
[CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") o el
[Gestor de paquetes de Swift](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"). Al usar CocoaPods para gestionar dependencias, solo obtiene las infraestructuras que necesita, al contrario de lo que sucede con el SDK de Swift de Watson completo. Si no conoce CocoaPods, puede instalarlo fácilmente:

```console
sudo gem install cocoapods
```
{: codeblock}

## Paso 1. Creación de una instancia de Watson Assistant
{: #instance-watson-chatbot}

Suministre una instancia del servicio {{site.data.keyword.conversationshort}}:

1. En el catálogo de {{site.data.keyword.cloud_notm}}, seleccione **{{site.data.keyword.conversationshort}}**. Se abre la pantalla de configuración del servicio.
2. Dé un nombre a la instancia de servicio, o utilice el nombre preestablecido.
3. Seleccione una app en el menú **Conectar** si desea enlazar la instancia a una app.
4. Seleccione un plan de precios y pulse **Crear**.
5. Seleccione el separador **Credenciales** para ver las credenciales de servicio. Estos valores se utilizan para conectar con el servicio desde la app.
6. Pulse **Herramienta de lanzamiento** para construir su asistente chatbot. Siga las instrucciones de la página de destino para construir su propio chatbot, o acceda al separador **Capacidades** (Skill) y seleccione **Crear nueva**. En la página **Agregar capacidad de diálogo**, seleccione el separador **Usar habilidad de muestra** y seleccione la **Customer Care Sample Skill** para empezar rápidamente. Puede seguir ajustando esta capacidad o crear otras que podrá usar posteriormente en su app.
7. Pulse en el menú de su nueva capacidad y seleccione **Ver detalles de API**. Ahora puede ver el `ID del espacio de trabajo` que necesita, además de las credenciales de servicio.

## Paso 2. Descarga y creación de dependencias
{: #download-depend-chatbot}

El ejemplo siguiente utiliza AssistantV1. Para obtener más información sobre la infraestructura AssistantV2, consulte la [Documentación de Watson SDK AssistantV2](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV2/index.html){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

Mediante su editor de texto preferido, cree un `Podfile` nuevo en el directorio raíz de su proyecto (donde se encuentra su archivo `.xcodeproj`), ejecutando `pod init`. A continuación, añada una línea para especificar la infraestructura {{site.data.keyword.conversationshort}} del SDK de Swift de Watson como dependencia:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonAssistantV1'
```
{: codeblock}

Para una app de producción, puede que también desee especificar un [requisito de versión](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") concreto para evitar cambios inesperados de los nuevos releases del SDK de Swift de Watson.

Con el archivo `Podfile` en su lugar, ahora puede descargar las dependencias. Utilice un terminal para acceder al directorio raíz de su proyecto y ejecute CocoaPods:

```console
pod install
```
{: codeblock}

Cocoapods descarga la infraestructura {{site.data.keyword.conversationshort}} y la compila en la carpeta `Pods/` de su proyecto.

Para evitar errores de construcción de Pod, cuando abra el proyecto en Xcode, abra el archivo que termina en `.xcworkspace` en lugar de `.xcodeproj`.
{: tip}

## Paso 3. Añadir un asistente virtual a su app
{: #virtual-assist-chatbot}

Los ejemplos siguientes le ayudan a añadir prestaciones de {{site.data.keyword.conversationshort}} a su aplicación, por lo general en `ViewController.swift`. Con los ejemplos siguientes, puede ampliar las llamadas del Asistente para su caso de uso.

1. Añada una sentencia de importación para {{site.data.keyword.conversationshort}}, por ejemplo `import Assistant`.
  ```swift
  import Assistant
  ```
  {: codeblock}

2. Crear una instancia del servicio {{site.data.keyword.conversationshort}}:
  ```swift
  let assistant = Assistant(version: "yyyy-mm-dd", apikey: "your-api-key-here")

  // guardar contexto en estado para continuar la conversación más adelante
  var context: Context?
  ```
  {: codeblock}

  **Sugerencia**: este ejemplo guarda el contexto en el estado. Para entender mejor este objeto y saber cómo adaptarlo a su caso de uso, consulte la [documentación de variable de contexto](/docs/services/assistant?topic=assistant-dialog-runtime#context-variables). Compruebe la [documentación del parámetro de
versión](https://{DomainName}/apidocs/assistant#versioning){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")
o utilice la fecha creada por el servicio {site.data.keyword.conversationshort}}.

3. Inicializar la conversación. En función de cómo esté configurado el asistente, puede proporcionar una respuesta inicial al usuario:
  ```swift
  // Iniciar una conversación
  assistant.message(workspaceID: "your-workspace-ID-here) { response, error in
      if let error = error {
         print(error)
    }

      guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Establecer el contexto en el estado
      self.context = message.context
  }
  ```
  {: codeblock}

4. Enviar un mensaje y el contexto al asistente:
  ```swift
  print("Request: When are you open?")
  let input = InputData(text: "When are you open?")

  assistant.message(workspaceID: workspace, input: input, context: self.context) { response, error in
      if let error = error {
         print(error)
    }

      guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Actualizar el contexto
      self.context = message.context
  }
  ```
  {: codeblock}

Cuando ejecuta la app con estos ejemplos con el asistente predeterminado, en la consola se muestran los mensajes siguientes:
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hello, I’m a demo customer care virtual assistant to show you the basics.  I can help with directions to my store, hours of operation and booking an in-store appointment.
Request: When are you open?
Response: Our hours are Monday to Friday 10am to 8pm and Friday and Saturday 11am to 6pm.
```
{: screen}

5. Explore la [documentación de Assistant](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV1/index.html){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")
del SDK de Watson para crear la funcionalidad de su aplicación.

## Utilización de los kits de inicio
{: #starterkits-chatbot}

Con los kits de inicio, puede utilizar de forma rápida y sencilla las funciones de {{site.data.keyword.cloud_notm}}. Puede añadir {{site.data.keyword.conversationshort}} a cualquier programa de fondo de lado del servidor utilizando los kits de inicio. El Chatbot for iOS con Watson Starter Kit ilustra cómo utilizar las funciones de aprendizaje profundo de {{site.data.keyword.conversationshort}} añadiendo una interfaz de lenguaje natural a la aplicación que automatiza las interacciones con los usuarios.

1. Seleccione el [kit de inicio](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") con el que desee trabajar.
2. Cree la app con los servicios predeterminados.
3. Pulse **Añadir servicio > Watson > {{site.data.keyword.conversationshort}}**.
4. Descargue el proyecto pulsando **Descargar código**. Puede encontrar las credenciales de servicio en el archivo `config/local-dev.json`.

## Pasos siguientes
{: #next-chatbot notoc}

¡Buen trabajo! Ha añadido un asistente de IA a la app. Mantenga el ritmo probando una de las opciones siguientes:

* Revise el [SDK de Swift de {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") y consulte el resto de los servicios de Watson admitidos.
* Aproveche todas las características que [{{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-index#index) ofrece.
* Visualice el código fuente de la [app de ejemplo Simple Chat](https://github.com/watson-developer-cloud/simple-chat-swift){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"),
que muestra el SDK de Swift de {{site.data.keyword.watson}} en GitHub.
