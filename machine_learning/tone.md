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

The IBM Watson Tone Analyzer service enables your app to understand emotions and tones in text. You can use the service to better understand your user's conversations or help users understand how their written communication is perceived.

## How it works
{: ##how-it-works}

1. Your app chooses a selection of text to analyze (for example, a text message or Twitter feed).
2. Your app sends the text to the {{site.data.keyword.toneanalyzershort}} service by using the Watson Swift SDK.
3. The service analyzes the text by using linguistic analysis to identify emotions and tones.
4. The service's analysis is returned to your app through the Watson Swift SDK.

## Before you begin
{: ###before-you-begin}

First, be sure that you have the following prerequisites ready to go:
<ul>
  <li>iOS 8.0+</li>
  <li>Xcode 9.0+</li>
  <li>Swift 3.2+ or Swift 4.0+</li>
  <li>Carthage</li>
</ul>

It is recommenced to use [Carthage](https://github.com/Carthage/Carthage) to manage dependencies, and build the Watson Swift SDK for your application. If you are new to Carthage, you can install it with [Homebrew](http://brew.sh/):

```bash
$ brew update
$ brew install carthage
```
{: codeblock}

## Step 1. Creating an instance of Tone Analyzer
{: #create-and-configure-an-instance-of-tone-analyzer}

Provision an instance of the {{site.data.keyword.toneanalyzershort}} service:

1. In the {{site.data.keyword.cloud_notm}} catalog, select {{site.data.keyword.toneanalyzershort}}. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Select an app from the Connect menu if you'd like to bind your instance to an app.
4. Select a pricing plan and click **Create**.
5. Select the **Credentials** tab to view your service credentials. These values are used to connect to the service from your app.

## Step 2. Downloading and building dependencies
{: ###download-and-build-dependencies}

Using your favorite text editor, create a new file that is called `Cartfile` in the root directory of your project (where your `.xcodeproj` file is located). Then, add a line to specify the Watson Swift SDK as a dependency:

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

For a production app, you might also want to specify a particular [version requirement](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) to avoid unexpected changes from new releases of the Watson Swift SDK.

With your `Cartfile` in place, now you can download and build the dependencies. Use a terminal to navigate to the root directory of your project, then run Carthage:
  
  ```bash
  $ carthage update --platform iOS
  ```
  {: codeblock}

Carthage downloads the Watson Swift SDK, and build its frameworks in the `Carthage/Build/iOS` folder of your project.

## Step 3. Adding frameworks to your app
{: ###add-frameworks-to-your-app}

### Link Tone Analyzer steps

Now that the Watson Swift SDK frameworks are built by Carthage, you must link the Tone Analyzer framework with your app.

1. Open your app in Xcode, and then select your project to open its settings.
2. Select your app target then open the **General tab**.
3. Scroll down to the "Linked Frameworks and Libraries" section, and click the `+` icon.
4. In the window that appears, choose **Add Other...** and navigate to the `Carthage/Build/iOS` directory. Select **ToneAnalyzerV3.framework** to link it with your app.

### Copy Tone Analyzer steps

In addition to _linking_ the Tone Analyzer framework, you must also _copy_ it into the app to make it accessible at run time. Xcode has several different ways to copy or embed a framework, but you can use a Carthage script to avoid a particular [App Store submission bug](http://www.openradar.me/radar?id=6409498411401216).

1. With your app target's settings open in Xcode, navigate to the **Build Phases** tab.
2. Click the `+` icon, and then select **New Run Script Phase**.
3. Add the following command to the run script phase: `/usr/local/bin/carthage copy-frameworks`.
4. Add the Tone Analyzer framework to the "Input Files" list: `$(SRCROOT)/Carthage/Build/iOS/ToneAnalyzerV3.framework`.

Now you are ready to start working with the Watson Swift SDK in your app!

## Step 4. Analyzing text in your app
{: #analyze-text-in-your-app}

1. Open the `ViewController.swift` file in Xcode.
2. Add an import statement for Tone Analyzer: 
    ```swift
    import ToneAnalyzerV3
    ```
    {: codeblock}

3. Create an empty function called `toneAnalyzerExample`, then call it from `viewDidLoad`.
4. Add the following code to the `toneAnalyzerExample` function. Be sure to update your service's username and password.
  ```swift
  import UIKit
  import ToneAnalyzerV3

  class ViewController: UIViewController {

      override func viewDidLoad() {
          super.viewDidLoad()
          toneAnalyzerExample()
      }

      func toneAnalyzerExample() {

          // instantiate service
          let toneAnalyzer = ToneAnalyzer(
              username: "your-username-here",
              password: "your-password-here",
              version: "yyyy-mm-dd"
          )

          // text to analyze
          let review = """
              I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
              took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
              stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
              bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
              over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
              some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
              of them - some had slightly bending wood which caused a minor injury.
          """

          // analyze text
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

When you run your app, you see the following analysis in the console:
```
Sadness: 0.575803
Tentative: 0.867377
```
{: screen}

## Using starter kits
{: #tone_starterkits}

[Starter kits](https://console.bluemix.net/developer/appledevelopment/starter-kits) are one of the fastest ways to leverage the capabilities of {{site.data.keyword.cloud_notm}}. You can use the {{site.data.keyword.toneanalyzershort}} service by selecting the **Tone Analyzer for iOS with Watson** starter kit. This service utilizes deep learning capabilities to evaluate passages of text. The Tone Analyzer application identifies the speaker's tone (happy, sad, confident, and more) as it relates to a number of categories.

To get started with this starter kit:

1. Select the starter kit found [here](https://console.bluemix.net/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson).
2. Create the project with the default services.
3. Download the project by clicking **Download Code**. Service credentials are injected into the `BMSCredentials.plist` file in the corresponding key fields.

## Next steps
{: #tone_next}

Great job! The {{site.data.keyword.toneanalyzershort}} is now added to your app. Keep the momentum by trying one of the following options:

* View the [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* For more information, see [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/).

