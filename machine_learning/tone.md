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

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

The IBM Watson Tone Analyzer service enables your app to understand emotions and tones in text. You can use the service to better understand your user's conversations or help users understand how their written communication will be perceived.

## How it works
{: ##how-it-works}

1. Your app chooses a selection of text to analyze (e.g. a text message or Twitter feed).
1. Your app sends the text to the {{site.data.keyword.toneanalyzershort}} service using the Watson Swift SDK.
1. The service analyzes the text using linguistic analysis to identify emotions and tones.
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
{: codeblock}

## Step1. Create and configure an instance of Tone Analyzer
{: ###create-and-configure-an-instance-of-tone-analyzer}

Provision an instance of the {{site.data.keyword.toneanalyzershort}} service:

1. In the {{site.data.keyword.cloud_notm}} catalog, select {{site.data.keyword.toneanalyzershort}}. The service configuration screen opens.
1. Give your service instance a name, or use the preset name.
1. Select an app from the Connect menu if you'd like to bind your instance to an app.
1. Select a pricing plan and click **Create**.
1. Select the **Credentials** tab to view your service credentials. We will use these values to connect to the service from your app.

## Step 2. Download and build dependencies
{: ###download-and-build-dependencies}

Using your favorite text editor, create a new file called `Cartfile` in the root directory of your project (where your `.xcodeproj` file is located). Then add a line to specify the Watson Swift SDK as a dependency:

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

For a production app, you might also want to specify a particular [version requirement](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement) to avoid unexpected changes from new releases of the Watson Swift SDK.

With your `Cartfile` in place, let's download and build the dependencies. Use a terminal to navigate to the root directory of your project then run Carthage:
  
  ```bash
  $ carthage update --platform iOS
  ```
  {: codeblock}

Carthage will download the Watson Swift SDK and build its frameworks in the `Carthage/Build/iOS` folder of your project.

## Step 3. Add frameworks to your app
{: ###add-frameworks-to-your-app}

Now that the Watson Swift SDK frameworks have been built by Carthage, we need to link the Tone Analyzer framework with your app.

1. Open your app in Xcode then select your project at the top of the Navigator to open its settings.
1. Select your app target then open the General tab.
1. Scroll down to "Linked Frameworks and Libraries" section and click the `+` icon.
1. In the window that appears, choose "Add Other..." and navigate to the `Carthage/Build/iOS` directory. Select `ToneAnalyzerV3.framework` to link it with your app.

In addition to _linking_ the Tone Analyzer framework we also need to _copy_ it into the app to make it accessible at runtime. Xcode has several different ways to copy or embed a framework, but we'll use a Carthage script to avoid a particular [App Store submission bug](http://www.openradar.me/radar?id=6409498411401216).

1. With your app target's settings open in Xcode, navigate to the "Build Phases" tab.
1. Click the `+` icon then select "New Run Script Phase."
1. Add the following command to the run script phase: `/usr/local/bin/carthage copy-frameworks`
1. Add the Tone Analyzer framework to the "Input Files" list: `$(SRCROOT)/Carthage/Build/iOS/ToneAnalyzerV3.framework`.

Now we're ready to start working with the Watson Swift SDK in your app!

## Step 4. Analyze text in your app
{: ###analyze-text-in-your-app}

1. Open your `ViewController.swift` in Xcode.
1. Add an import statement for Tone Analyzer: 
    ```swift
    import ToneAnalyzerV3
    ```
1. Create an empty function called `toneAnalyzerExample` then call it from `viewDidLoad`.
1. Add the code below for the `toneAnalyzerExample` function. Be sure to update your service's username and password!

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

When you run your app, you should see the following analysis in the console:

```
Sadness: 0.575803
Tentative: 0.867377
```

## Quick start with starter kits
{: #tone_starterkits}

[Starter kits](https://console.bluemix.net/developer/appledevelopment/starter-kits) are one of the fastest way to leverage the capabilities of {{site.data.keyword.cloud_notm}}. You can make use of the {{site.data.keyword.toneanalyzershort}} service by selecting the **Tone Analyzer for iOS with Watson** starter kit.  This service utilizes deep learning capabilities to evaluate your passages of text.  The Tone Analyzer application identifies the speaker's tone (happy, sad, confident, and more) as it relates to a number of categories.

To get started with this starter kit:

1. Select the starter kit found [here](https://console.bluemix.net/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson).
2. Create the project with the default services.
3. Download the project by clicking **Download Code**. Service credentials will be injected into the `BMSCredentials.plist` file in the corresponding key fields.


## Next steps
{: #tone_next}

Great job! You've added {{site.data.keyword.toneanalyzershort}} to your app. Keep the momentum going by trying one of the following options:

* View the [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk).
* Learn more about and take advantage of all of the features that [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/) has to offer!

