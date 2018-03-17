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

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

The IBM Watson Visual Recognition service enables your app to quickly and accurately tag, classify and train visual content using machine learning. The service can help to classify virtually any visual content, train your own custom model in minutes, and detect faces.

## How it works
{: ##how-it-works}

1. Your app chooses a selection of images to analyze.
1. Your app sends the image to the {{site.data.keyword.visualrecognitionshort}} service using the Watson Swift SDK.
1. The service analyzes the image using classification analysis to identify scenes, objects, faces, and more.
1. The service's analysis is returned to your app by the Watson Swift SDK.

## Before you begin
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

## Step 1. Create and configure an instance of Visual Recognition
{: ###create-and-configure-an-instance-of-visual-recognition}

Provision an instance of the {{site.data.keyword.visualrecognitionshort}} service:

1. In the {{site.data.keyword.cloud_notm}} catalog, select {{site.data.keyword.visualrecognitionshort}}. The service configuration screen opens.
1. Give your service instance a name, or use the preset name.
1. Select an app from the Connect menu if you'd like to bind your instance to an app.
1. Select a pricing plan and click Create.
1. Select the "Credentials" tab to view your service credentials. We will use these values to connect to the service from your app.

## Step 2. Download and build dependencies
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

## Step 3. Add frameworks to your app
{: ###add-frameworks-to-your-app}

Now that the Watson Swift SDK frameworks have been built by Carthage, we need to link the Visual Recognition framework with your app.

1. Open your app in Xcode then select your project at the top of the Navigator to open its settings.
1. Select your app target then open the General tab.
1. Scroll down to "Linked Frameworks and Libraries" section and click the `+` icon.
1. In the window that appears, choose "Add Other..." and navigate to the `Carthage/Build/iOS` directory. Select `VisualRecognitionV3.framework` to link it with your app.

In addition to _linking_ the Visual Recognition framework we also need to _copy_ it into the app to make it accessible at runtime. Xcode has several different ways to copy or embed a framework, but we'll use a Carthage script to avoid a particular [App Store submission bug](http://www.openradar.me/radar?id=6409498411401216).

1. With your app target's settings open in Xcode, navigate to the "Build Phases" tab.
1. Click the `+` icon then select "New Run Script Phase."
1. Add the following command to the run script phase: `/usr/local/bin/carthage copy-frameworks`
1. Add the Visual Recognition framework to the "Input Files" list: `$(SRCROOT)/Carthage/Build/iOS/VisualRecognitionV3.framework`.

Now we're ready to start working with the Watson Swift SDK in your app!

## Step 4. Analyze images in your app
{: ###analyze-images-in-your-app}

1. Open your `ViewController.swift` in Xcode.
1. Add an import statement for Visual Recognition: 
	```swift
	import VisualRecognitionV3
	```.
1. Pass the API key and version (you can use today's date) to initialize the SDK:
	```swift
    let visualRecognition = VisualRecognition(apiKey: <your-api-key>, version: <YYYY-MM-DD>)
    ```
    {: pre}
1. Add the code below to complete an example for facial recognition

	```swift
	let url = "your-image-url"
	let failure = { (error: Error) in print(error) }
	visualRecognition.classify(image: url, failure: failure) { classifiedImages in
    	print(classifiedImages)
	}
	```
	{: pre}

## Quick start with starter kits
{: #recognition_starterkits}

[Starter kits](https://console.bluemix.net/developer/appledevelopment/starter-kits) are one of the fastest way to leverage the capabilities of {{site.data.keyword.cloud_notm}}. You can make use of the {{site.data.keyword.visualrecognitionshort}} service by selecting the **Visual Recognition for iOS with Watson** starter kit.  This service evaluates and classify your images. Upload new or existing images from your mobile device and the Visual Recognition app will quickly and accurately tag and classify the image content.

To get started with this starter kit:

1. Select the starter kit found [here](https://console.bluemix.net/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson).
2. Create the project with the default services.
3. Download the project by clicking **Download Code**. Service credentials will be injected into the `BMSCredentials.plist` file in the corresponding key fields.


## Next steps
{: #recognition_next}

Great job! You've added a level of visual recognition to your app. Keep the momentum going by trying one of the following options:

* View the [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* Learn more about and take advantage of all of the features that [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/) has to offer!

