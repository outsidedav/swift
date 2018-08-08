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

The {{site.data.keyword.visualrecognitionfull}} service enables your app to quickly and accurately tag, classify, and train visual content by using machine learning. The service can help to classify virtually any visual content, train your own custom model in minutes, and detect faces.

## How it works
{: ##how-it-works}

1. Your app chooses a selection of images to analyze.
2. Your app sends the image to the {{site.data.keyword.visualrecognitionshort}} service by using the Watson Swift SDK.
3. The service analyzes the image by using classification analysis to identify scenes, objects, faces, and more.
4. The service's analysis is returned to your app by the Watson Swift SDK.

## Before you begin
{: ###before-you-begin}

First, be sure that you have the following prerequisites ready to go:
<ul>
  <li>iOS 8.0+</li>
  <li>Xcode 9.0+</li>
  <li>Swift 3.2+ or Swift 4.0+</li>
  <li>Carthage</li>
</ul>

It is recommended to use [Carthage](https://github.com/Carthage/Carthage) to manage dependencies, and build the Watson Swift SDK for your application. If you are new to Carthage, you can install it with [Homebrew](http://brew.sh/):

```bash
$ brew update
$ brew install carthage
```

## Step 1. Creating an instance of Visual Recognition
{: ###create-and-configure-an-instance-of-visual-recognition}

Provision an instance of the {{site.data.keyword.visualrecognitionshort}} service:

1. In the {{site.data.keyword.cloud_notm}} catalog, select **{{site.data.keyword.visualrecognitionshort}}**. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Select an app from the **Connect** menu if you'd like to bind your instance to an app.
4. Select a pricing plan, and click **Create**.
5. Select the **Credentials** tab to view your service credentials. These values are used to connect to the service from your app.

## Step 2. Downloading and building dependencies
{: ###download-and-build-dependencies}

Using your favorite text editor, create a file that is called `Cartfile` in the root directory of your project (where your `.xcodeproj` file is located). Then, add a line to specify the Watson Swift SDK as a dependency:
```
github "watson-developer-cloud/swift-sdk"
```
{: pre}

For a production app, you can specify a particular [version requirement](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) to avoid unexpected changes from new releases of the Watson Swift SDK.

With the `Cartfile` in place, now you can download and build the dependencies. Use a terminal to navigate to the root directory of your project, then run Carthage:

```bash
carthage update --platform iOS
```
{: pre}

Carthage downloads the Watson Swift SDK, and build its frameworks in the `Carthage/Build/iOS` folder of your project.

## Step 3. Adding frameworks to your app
{: ###add-frameworks-to-your-app}

### Link Visual Recognition steps:

Now that the Watson Swift SDK frameworks are built by Carthage, you must link the Visual Recognition framework with your app.

1. Open your app in Xcode, and select your project to open its settings.
2. Select your app target then open the **General** tab.
3. Scroll down to the "Linked Frameworks and Libraries" section, and click the `+` icon.
4. In the window that appears, choose **Add Other...**, and navigate to the `Carthage/Build/iOS` directory. Select **VisualRecognitionV3.framework** to link it with your app.

### Copy Visual Recognition steps:

In addition to _linking_ the Visual Recognition framework, you must also _copy_ it into the app to make it accessible at run time. Xcode has several different ways to copy or embed a framework, but you can use a Carthage script to avoid a particular [App Store submission bug](http://www.openradar.me/radar?id=6409498411401216).

1. With your app target's settings open in Xcode, navigate to the **Build Phases** tab.
2. Click the `+` icon then select **New Run Script Phase**.
3. Add the following command to the run script phase: `/usr/local/bin/carthage copy-frameworks`.
4. Add the Visual Recognition framework to the **Input Files** list: `$(SRCROOT)/Carthage/Build/iOS/VisualRecognitionV3.framework`.

Now you are ready to start working with the Watson Swift SDK in your app!

## Step 4. Analyzing images in your app
{: #analyze-images-in-your-app}

1. Open the `ViewController.swift` file in Xcode.

1. Add an import statement for Visual Recognition:
    ```swift
    import VisualRecognitionV3
    ```
    {: pre}

1. Pass the API key and version (you can use today's date) to initialize the SDK:
    ```swift
    let visualRecognition = VisualRecognition(apiKey: "your-api-key", version: "yyyy-mm-dd")
    ```
    {: pre}

1. Add the following code to classify an image:
    ```swift
    let url = "your-image-url"
    let failure = { (error: Error) in print(error) }
    visualRecognition.classify(url: url, failure: failure) { classifiedImages in
        print(classifiedImages)
    }
    ```
    {: pre}

## Using starter kits
{: #recognition_starterkits}

[Starter kits](https://console.bluemix.net/developer/appledevelopment/starter-kits) are one of the fastest ways to leverage the capabilities of {{site.data.keyword.cloud_notm}}. You can use the {{site.data.keyword.visualrecognitionshort}} service by selecting the **Visual Recognition for iOS with Watson** starter kit. This service evaluates and classifies your images. Upload new or existing images from your mobile device, and the Visual Recognition app quickly tags and classifies the image content.

To get started:
1. Select the starter kit found [here](https://console.bluemix.net/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson).
2. Create the project with the default services.
3. Download the project by clicking **Download Code**. Service credentials are injected into the `BMSCredentials.plist` file in the corresponding key fields.

## Next steps
{: #recognition_next}

Great job! Visual Recognition is now available to your app. Keep the momentum by trying one of the following options:
* View the [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* For more information, see [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/).

