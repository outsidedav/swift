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

# Adding machine learning to an app
{: #overview}

With [Core ML](https://developer.apple.com/documentation/coreml){:new_window}, you can integrate a broad variety of machine learning model types into your app. In addition to supporting extensive deep learning with over 30 layer types, it also supports standard models such as tree ensembles, SVMs, and generalized linear models. Instead of sending data remotely to be analyzed, Core ML utilizes low-level technologies, such as Metal and Accelerate, to seamlessly take advantage of the CPU and GPU to provide maximum performance and efficiency.

## Before you begin
{: #before-you-begin}

To use Core ML and Watson Visualization, you need the following components:

  * iOS 11.0+
  * Xcode 9.0+
  * Swift 4.0+
  * Carthage

To install Carthage, use the following `brew` commands:
```
$ brew update
$ brew install carthage
```
{: codeblock}

## Step 1. Training a model with {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}}
{: #training-your-model}

If a current model does not exist, the first model found remotely or any one that exists locally is used. The following gif and accompanying instructions show you how to link your service to the Watson Studio, and train your model.

![Core ML Model Walkthrough](images/CoreMLWalkthrough.gif)

### Setting up the service from your Core ML App dashboard

1. Launch the Visual Recognition Tool from your Starter Kit's dashboard by selecting **Launch Tool**.
2. Begin creating your model by selecting **Create Model**.
2. If a project is not yet associated with the Visual Recognition instance that you created, a project is created. Otherwise, skip to the **Create Model** section.
3. Name your project, and click **Create**.
  **Tip**: If no storage is defined, click refresh.

### Binding a Service to a Project

1. After you create your project, the project dashboard is displayed.
2. Go to the settings tab, scroll down to **Associated Services**, select **Add Service** -> **Watson**.
3. From the Watson services page, select **Visual Recognition**.
4. Select the **Existing** tab on the service info page and then select your service instance from the dashboard.
5. Now that your service is bound, you can begin creating your model by selecting **Assets** from your Project dashboard, and then clicking **Add Visual Recognition Model**.

### Creating a Model

1. From the model creation tool, modify the classifier name if desired. By default, the application uses the first available unless specified. If you would like to use a specific model, make sure to modify the `defaultClassifierID` field in main View Controller.

2. From the sidebar, upload the model training courses in .zip files. Then, select each dataset and add them to your model from the drop-down menu. Feel free to add more classes that use your own image sets to enhance the classifier!

![Adding Classes](images/add_classes.png)

3. Select **Train Model**, and then wait for the model to be fully trained.

You're all set! Now, you're ready to download your Core ML model and integrate it into your app by using the [{{site.data.keyword.watson}} Developer Cloud Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window}.

## Step 2. Downloading and building dependencies
{: #installing-dependencies}

1. Using your favorite text editor, create a new filed named **Cartfile** in the root directory of your project where your `.xcodeproj` file is located.

2. Add a line to specify the {{site.data.keyword.watson}} Developer Cloud Swift SDK as a dependency, for example:

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: pre}

  For a production app, you might also want to specify a particular version requirement to avoid unexpected changes from new releases of the SDK.
  {: tip}

3. Use a terminal to navigate to the root directory of your project and run Carthage:

  ```
  $ carthage update --platform iOS
  ```
  {: pre}

  The {{site.data.keyword.watson}} Developer Cloud Swift SDK is then downloaded, and its framework is built in the `Carthage/Build/iOS` folder of your project.

## Step 3. Adding image classification to your app
{: #adding-image-classification}

```Swift
//
//  ViewController.swift
//  CoreMLImageClassificationExample
//
//  Created by Aaron Liberatore on 3/1/18.
//  Copyright © 2018 Aaron Liberatore. All rights reserved.
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
        // Configuration

        // Your Watson Visual Recognition service api key
        let apiKey = "your-apiKey-here"

        // Name of the classifier to use
        let classifierID = "your-classifier-here"

        // Minimum confidence threshold for image recognition
        let threshold = 0.5

        // Initialize Service
        let visualRecognition = VisualRecognition(apiKey: apiKey, version: "03-01-2018")

        // Update or download your model
        visualRecognition.updateLocalModel(classifierID: classifierID,
                                           failure: failureHandler) {

            // Classify your image using your model                                         
            visualRecognition.classifyWithLocalModel(image: image,
                                                     classifierIDs: [classifierID],
                                                     threshold: threshold,
                                                     failure: self.failureHandler) { classifiedImages in

                 print(classifiedImages)
             }            
        }
    }
}
```
{: codeblock}

## Step 4. Using starter kits
{: #coreml_starterkits}

With starter kits, you can quickly and easily leverage the capabilities of {{site.data.keyword.cloud_notm}} and Core ML. You can add {{site.data.keyword.visualrecognitionshort}} to any client iOS app or server-side back end by using the starter kits. The Custom Vision Model for Core ML with {{site.data.keyword.watson}} Starter Kit illustrates how to create a custom {{site.data.keyword.visualrecognitionshort}} model, and instantiate it as a Core ML model that you can run locally on your device.

To add {{site.data.keyword.visualrecognitionshort}} to a starter kit, complete the following steps:

1. Select the [starter kit](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} with which you want to work.
2. Create the project with the default services.
3. Click **Add Resources > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Download the project by clicking **Download Code**. For iOS projects, the credentials are inserted in the `BMSCredentials.plist` file in the corresponding key fields. For server-side Swift projects, you can find these credentials in the `config/local-dev.json` file.

## Next steps
{: #coreml_next}

Now you can analyze images by using custom generated Core ML models. Keep the momentum by trying one of the following options:

* Add cloud logic. Start with [developing serverless apps](/docs/swift/backend/functions.html).
* Take advantage of all the features that {{site.data.keyword.visualrecognitionshort}} has to offer. See the [documentation](/docs/services/visual-recognition/index.html) for more details.
