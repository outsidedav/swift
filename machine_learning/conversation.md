---

copyright:
  years: 2018
lastupdated: "2018-03-16"

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Integrating a chatbot into your app
{: #conversation}

You can use the {{site.data.keyword.conversationshort}} service to build applications that understand natural-language input and respond to users with human-like conversation.

The following list outlines the flow of how the integration works:

  1. Users interact with the interface implemented in your app.
  2. Your app sends user input to the {{site.data.keyword.conversationshort}} by using the {{site.data.keyword.watson}} Swift SDK.
  3. The {{site.data.keyword.watson}} Swift SDK connects to a workspace, which is a container for your dialog flow and training data.
  4. The workspace interprets the user input and directs the flow of the conversation, sending a response to your app.
  5. Your app displays the response for the user.

## Before you begin
{: #before-you-begin}

Make sure you have the following prerequisites in place:

  * An instance of the [{{site.data.keyword.conversationshort}} service](/docs/services/conversation/getting-started.html)
  * iOS 8.0+
  * Xcode 9.0+
  * Swift 3.2+ or Swift 4.0+
  * Carthage

Use [Carthage](https://github.com/Carthage/Carthage){:new_window} to manage dependencies and build the {{site.data.keyword.watson}} Swift SDK for your application. If you haven't used Carthage before, you can install it with [Homebrew](http://brew.sh/){:new_window}:

```bash
$ brew update
$ brew install carthage
```

## Step 1. Downloading and building dependencies
{: #download-and-build-dependencies}

1. Using your favorite text editor, create a new filed named `Cartfile` in the root directory of your project (where your `.xcodeproj` file is located). 
2. Add a line to specify the Watson Swift SDK as a dependency:

  ```
  github "watson-developer-cloud/swift-sdk"
  ```

  For a production app, you may also want to specify a particular [version requirement](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement){:new_window} to avoid unexpected changes from new releases of the {{site.data.keyword.watson}} Swift SDK.
  {: tip}

3. Use a terminal to navigate to the root directory of your project and then run Carthage:

  ```bash
  $ carthage update --platform iOS
  ```

  The {{site.data.keyword.watson}} Swift SDK is then downloaded, and its framework is built in the `Carthage/Build/iOS` folder of your project.

## Step 2. Adding frameworks to your app
{: #add-frameworks-to-your-app}

Now that the {{site.data.keyword.watson}} Swift SDK frameworks have been built by Carthage, you need to link and copy the {{site.data.keyword.conversationshort}} framework with your app.

1. Open your app in Xcode and select your project at the top of the Navigator to open its settings.
2. Select your app target and open the **General** tab.
3. Scroll to the Linked Frameworks and Libraries section and click the `+` icon.
4. In the new window that is displayed, click **Add Other** and navigate to the `Carthage/Build/iOS` directory. 
5. Select `ConversationV1.framework` to link it with your app.

In addition to linking the {{site.data.keyword.conversationshort}} framework, you also need to copy it into the app to make it accessible at runtime. Use a Carthage script to avoid a particular [App Store submission bug](http://www.openradar.me/radar?id=6409498411401216){:new_window}.

1. With your app target's settings open in Xcode, navigate to the **Build Phases** tab.
2. Click the `+` icon and select **New Run Script Phase**.
3. Add the `/usr/local/bin/carthage copy-frameworks` command to the run script phase.
4. Add the {{site.data.keyword.conversationshort}} framework to the **Input Files** list: 

  ```
  $(SRCROOT)/Carthage/Build/iOS/ConversationV1.framework
  
  ```

## Step 3. Adding a chatbot to your app
{: #add-a-chat-bot-to-your-app}

1. Open your `ViewController.swift` in Xcode.
2. Add an import statement for {{site.data.keyword.conversationshort}}, for example `import ConversationV1`.
3. Create an empty function called `conversationExample` and then call it from `viewDidLoad`.
4. Add the following code for the `conversationExample` function. Be sure to update your user name, password, and workspace ID.

```swift
//
//  ViewController.swift
//  ConversationExample
//
//  Created by Glenn R. Fisher on 3/1/18.
//  Copyright © 2018 Glenn R. Fisher. All rights reserved.
//

import UIKit
import ConversationV1

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        conversationExample()
    }

    func conversationExample() {
        // conversation credentials
        let username = "your-username-here"
        let password = "your-password-here"
        let workspace = "your-workspace-id-here"

        // instantiate service
        let conversation = Conversation(username: username, password: password, version: "2018-03-01")

        // start a conversation
        conversation.message(workspaceID: workspace) { response in
            print("Conversation ID: \(response.context.conversationID)")
            print("Response: \(response.output.text.joined())")

            // continue conversation
            print("Request: turn the radio on")
            let input = InputData(text: "turn the radio on")
            let request = MessageRequest(input: input, context: response.context)
            conversation.message(workspaceID: workspace, request: request) { response in
                print("Response: \(response.output.text.joined())")
            }
        }
    }
}
```

When you run your app, the following messages are displayed in the console:

```
Conversation ID: 56698937-538b-4e05-9aa3-c8d6c1f9f549
Response: Hi. It looks like a nice drive today. What would you like me to do?
Request: turn the radio on
Response: Sure thing! Which genre would you prefer? Jazz is my personal favorite..
```
## Step 4. Using starter kits
{: #conversation_starterkits}

With starter kits, you can quickly and easily leverage the capabilities of {{site.data.keyword.cloud_notm}}. You can add {{site.data.keyword.conversationshort}} to any server-side back end by using the starter kits. The Chatbot for iOS with Watson starter kit illustrates how to use the deep learning capabilities of {{site.data.keyword.conversationshort}} to add a natural language interface to your application to automate interactions with your end users.

1. Select the [starter kit](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} with which you want to work.
2. Create the project with the default services.
3. Click **Add Resources > Watson > {{site.data.keyword.conversationshort}}**.
4. Download the project by clicking **Download Code**. You can find the service credentials in the `config/local-dev.json` file.

## Next steps
{: #conversation_next}

Great job! You added a level of conversation to your app. Keep the momentum going by trying one of the following options:

* Check out the [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.
* Take advantage of all of the features that [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) has to offer.
* View the source code for the [Simple Chat sample app](https://github.com/watson-developer-cloud/simple-chat-swift){:new_window}, demonstrating the {{site.data.keyword.watson}} Swift SDK on GitHub.
