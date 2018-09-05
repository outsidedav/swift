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
{: #before-you-begin}

Asegúrese de cumplir los siguientes requisitos previos:

  * Una instancia del [servicio de {{site.data.keyword.conversationshort}}](/docs/services/conversation/getting-started.html)
  * iOS 8.0+
  * Xcode 9.0+
  * Swift 3.2+ o Swift 4.0+
  * Carthage

Utilice [Carthage](https://github.com/Carthage/Carthage){:new_window} para gestionar dependencias y crear el SDK de Swift de {{site.data.keyword.watson}} para la aplicación. Si es nuevo en Carthage, puede instalarlo con [Homebrew](http://brew.sh/){:new_window}:

```bash
$ brew update
$ brew install carthage
```

## Descarga y creación de dependencias
{: #download-and-build-dependencies}

1. Mediante su editor de texto favorito, cree un archivo nuevo denominado `Cartfile` en el directorio raíz del proyecto (donde se encuentra el archivo `.xcodeproj`).

2. Añada una línea para especificar el SDK de Swift de Watson como una dependencia:
  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

  Para una app de producción, puede especificar un [requisito de versión](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement){:new_window} para evitar cambios inesperados de los nuevos releases del SDK de Swift de {{site.data.keyword.watson}}.
  {: tip}

3. Utilice un terminal para navegar hasta el directorio raíz del proyecto, y a continuación ejecute Carthage:
  ```bash
  carthage update --platform iOS
  ```
  {: codeblock}

  El SDK de Swift de {{site.data.keyword.watson}} se descargará y su infraestructura se crea en la carpeta `Carthage/Build/iOS` del proyecto.

## Adición de infraestructuras a la app
{: #add-frameworks-to-your-app}

Ahora que se ha creado la infraestructura del SDK de Swift de {{site.data.keyword.watson}}, debe enlazar y copiar la infraestructura de {{site.data.keyword.conversationshort}} en la app.

1. Abra la app en Xcode y seleccione el proyecto en el Navegador para abrir sus valores.
2. Seleccione el destino de la app y abra el separador **General**.
3. Desplácese a la sección Infraestructura y bibliotecas enlazados y pulse el icono `+`.
4. En la ventana nueva que se visualiza, pulse **Añadir otros** y vaya al directorio `Carthage/Build/iOS`.
5. Seleccione `AssistantV1.framework` para enlazarlo con la app.

Además de enlazar la infraestructura de {{site.data.keyword.conversationshort}}, también debe copiarla en la app para que sea accesible en el tiempo de ejecución. A continuación, se utiliza un script de Carthage para evitar un [defecto de envío de App Store](http://www.openradar.me/radar?id=6409498411401216){:new_window} determinado.

1. Con los valores del destino de app abiertos en Xcode, vaya al separador **Fases de compilación**.
2. Pulse el icono `+` y seleccione **Nueva fase de ejecución del script**.
3. Añada el mandato `/usr/local/bin/carthage copy-frameworks` a la fase de ejecución del script.
4. Añada la infraestructura {{site.data.keyword.conversationshort}} a la lista **Archivos de entrada**:
  ```
  $(SRCROOT)/Carthage/Build/iOS/AssistantV1.framework
  ```
  {: codeblock}

## Adición de un asistente virtual a la app
{: #add-a-virtual-assistant-to-your-app}

1. Abra `ViewController.swift` en Xcode.
2. Añada una sentencia de importación para {{site.data.keyword.conversationshort}}, por ejemplo `import AssistantV1`.
3. Cree una función vacía denominada `assistantExample` y, a continuación, llámela desde `viewDidLoad`.
4. Añada el código siguiente para la función `assistantExample`. Asegúrese de actualizar el nombre de usuario, la contraseña y el ID de espacio de trabajo.

```swift
//
//  ViewController.swift
//  WatsonAssistantExample
//
//  Created by Glenn R. Fisher on 3/1/18.
//  Copyright © 2018 Glenn R. Fisher. Todos los derechos reservados.
//

import UIKit
import AssistantV1

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        assistantExample()
    }

    func assistantExample() {
        // Credenciales del asistente
        let username = "your-username-here"
        let password = "your-password-here"
        let workspace = "your-workspace-id-here"

        // Crear instancia de servicio
        let assistant = Assistant(username: username, password: password, version: "2018-03-01")

        // Iniciar una conversación
        assistant.message(workspaceID: workspace) { response in
            print("Conversation ID: \(response.context.conversationID!)")
            print("Response: \(response.output.text.joined())")

            // Continuar asistente
            print("Request: turn the radio on")
            let input = InputData(text: "turn the radio on")
            let request = MessageRequest(input: input, context: response.context)
            assistant.message(workspaceID: workspace, request: request) { response in
                print("Response: \(response.output.text.joined())")
            }
        }
    }
}
```
{: codeblock}

Cuando ejecuta la app, se muestran los mensajes siguientes en la consola:
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hi. It looks like a nice drive today. What would you like me to do?
Request: turn the radio on
Response: Sure thing! Which genre would you prefer? Jazz is my personal favorite..
```
{: screen}

## Utilización de los kits de iniciación
{: #conversation_starterkits}

Con los kits de iniciación, puede aprovechar de forma rápida y sencilla las funciones de {{site.data.keyword.cloud_notm}}. Puede añadir {{site.data.keyword.conversationshort}} a cualquier programa de fondo de lado del servidor utilizando los kits de iniciación. El Chatbot for iOS con Watson Starter Kit ilustra cómo utilizar las funciones de aprendizaje profundo de {{site.data.keyword.conversationshort}} añadiendo una interfaz de lenguaje natural a la aplicación que automatiza las interacciones con los usuarios finales.

1. Seleccione el [kit de iniciación](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} con el que desee trabajar.
2. Cree el proyecto con los servicios predeterminados.
3. Pulse **Añadir recursos > Watson > {{site.data.keyword.conversationshort}}**.
4. Descargue el proyecto pulsando **Descargar código**. Puede encontrar las credenciales de servicio en el archivo `config/local-dev.json`.

## Pasos siguientes
{: #assistant_next}

¡Buen trabajo! Ha añadido un asistente de IA a la app. Mantenga el ritmo probando una de las opciones siguientes:

* Consulte el [SDK de Swift de {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.
* Aproveche todas las características que [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) tiene que ofrecer.
* Visualice el código fuente de la [app de ejemplo Simple Chat](https://github.com/watson-developer-cloud/simple-chat-swift){:new_window}, que muestra el SDK de Swift de {{site.data.keyword.watson}} en GitHub.
