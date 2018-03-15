---

copyright:
  years: 2018
lastupdated: "2018-03-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.conversationshort}}
{: #conversation}

The IBM Watson Conversation service makes it easy to integrate a chat bot into your apps. It enables you to build applications that understand natural-language input and respond to users with human-like conversation.

## How it works
{: ##how-it-works}

1. Users interact with the *interface* implemented in your app.
1. Your app sends user input to the Conversation service using the *Watson Swift SDK*.
1. The Watson Swift SDK connects to a *workspace*, which is a container for your dialog flow and training data.
1. The workspace interprets the user input and directs the flow of the conversation, sending a *response* to your app.
1. Your app displays the response for the user.

## Getting started
{: ##getting-started}

It's easy to get started with the Conversation service. The following steps will show you how to integrate a chat bot into your app.

### Before you begin
{: ###before-you-begin}

First, be sure you have the following prerequisites ready to go:
<ul>
  <li>iOS 8.0+</li>
  <li>Xcode 9.0+</li>
  <li>Swift 3.2+ or Swift 4.0+</li>
  <li>Carthage</li>
</ul>

We recommend using [Carthage](https://github.com/Carthage/Carthage) to manage dependencies and build the Watson Swift SDK for your application. If you haven't used Carthage before, you can install it with [Homebrew](http://brew.sh/):

```bash
$ brew update
$ brew install carthage
```

### Create and configure an instance of Conversation
{: ###create-and-configure-an-instance-of-conversation}

Provision an instance of the service to start building your chat bot.

1. In the {{site.data.keyword.cloud_notm}} catalog, select Conversation. The service configuration screen opens.
1. Give your service instance a name, or use the preset name.
1. Select an app from the Connect menu if you'd like to bind your instance to an app.
1. Select a pricing plan and click Create.
1. Click "Launch Tool" to open the Conversation tool.
1. Click "Edit Sample" to create a new workspace using the Cognitive Car sample.
1. Choose "Deploy" from the left-hand navigation menu (it's the third icon from the top).
1. Select the "Credentials" tab to view your service credentials and workspace id. We will use these values to connect to the service from your app.

If you'd like to learn more about the Conversation tool, click the information icon at the top-right corner.

### Download and build dependencies
{: ###download-and-build-dependencies}

Using your favorite text editor, create a new filed called `Cartfile` in the root directory of your project (where your `.xcodeproj` file is located). Then add a line to specify the Watson Swift SDK as a dependency:

```
github "watson-developer-cloud/swift-sdk"
```

For a production app, you may also want to specify a particular [version requirement](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) to avoid unexpected changes from new releases of the Watson Swift SDK.

With your `Cartfile` in place, let's download and build the dependencies. Use a terminal to navigate to the root directory of your project then run Carthage:

```bash
$ carthage update --platform iOS
```

Carthage will download the Watson Swift SDK and build its frameworks in the `Carthage/Build/iOS` folder of your project.

### Add frameworks to your app
{: ###add-frameworks-to-your-app}

Now that the Watson Swift SDK frameworks have been built by Carthage, we need to link the Conversation framework with your app.

1. Open your app in Xcode then select your project at the top of the Navigator to open its settings.
1. Select your app target then open the General tab.
1. Scroll down to "Linked Frameworks and Libraries" section and click the `+` icon.
1. In the window that appears, choose "Add Other..." and navigate to the `Carthage/Build/iOS` directory. Select `ConversationV1.framework` to link it with your app.

In addition to _linking_ the Conversation framework we also need to _copy_ it into the app to make it accessible at runtime. Xcode has several different ways to copy or embed a framework, but we'll use a Carthage script to avoid a particular [App Store submission bug](http://www.openradar.me/radar?id=6409498411401216).

1. With your app target's settings open in Xcode, navigate to the "Build Phases" tab.
1. Click the `+` icon then select "New Run Script Phase."
1. Add the following command to the run script phase: `/usr/local/bin/carthage copy-frameworks`
1. Add the Conversation framework to the "Input Files" list: `$(SRCROOT)/Carthage/Build/iOS/ConversationV1.framework`.

Now we're ready to start working with the Watson Swift SDK in your app!

### Add a chat bot to your app
{: ###add-a-chat-bot-to-your-app}

1. Open your `ViewController.swift` in Xcode.
1. Add an import statement for Conversation: `import ConversationV1`.
1. Create an empty function called `conversationExample` then call it from `viewDidLoad`.
1. Add the code below for the `conversationExample` function. Be sure to update your username, password, and workspace id!

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

When you run your app, you should see the following messages in the console:

```
Conversation ID: 56698937-538b-4e05-9aa3-c8d6c1f9f549
Response: Hi. It looks like a nice drive today. What would you like me to do?
Request: turn the radio on
Response: Sure thing! Which genre would you prefer? Jazz is my personal favorite..
```


## Quick start with starter kits
{: #conversation_starterkits}

Starter kits are one of the fastest way to leverage the capabilities of {{site.data.keyword.cloud_notm}}. You can add {{site.data.keyword.conversationshort}} to any server-side backend by using the starter kits.  The **Conversation for iOS with Watson** starter kit illustrates how to use Watson Conversation's deep learning capabilities to add a natural language interface to your application to automate interactions with your end users.

To add {{site.data.keyword.conversationshort}} to a starter kit:

1. Select the [Starter Kit](https://console.bluemix.net/developer/appledevelopment/starter-kits) with which you'd like to work.
2. Create the project with the default services.
3. Select **Add Resources > Watson > Conversation**.
4. Download the project by clicking **Download Code**. Service credentials can be found in the `config/local-dev.json` file.

## Next steps
{: #conversation_next}

Great job! You've added a level of conversation to your app. Keep the momentum going by trying one of the following options:

* View the [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* Learn more about and take advantage of all of the features that [IBM Watson {{site.data.keyword.conversationshort}}](https://www.ibm.com/watson/services/conversation/) has to offer!
* View the source code for the [Simple Chat sample app](https://github.com/watson-developer-cloud/simple-chat-swift) demonstrating the Watson Developer Cloud Swift SDK on GitHub.

