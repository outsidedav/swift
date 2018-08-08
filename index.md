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
{:tip: .tip}

# Getting started tutorial
{: #set_up}

{{site.data.keyword.cloud}} offers solutions and services to empower Swift developers and applications with the security, AI, and value demanded by your customers. With a broad portfolio of offerings and SDKs, you can leverage these services, and bring cutting-edge applications to market quickly. The Swift programming guide teaches you how to add services to a new or existing Swift application, whether it is an iOS client or server-side Swift.
{: shortdesc}

The following tutorial is an entry point to show you how to easily create a Swift mobile app with {{site.data.keyword.mobileanalytics_full}} by using an empty Starter Kit from the [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits). From the console, you add the {{site.data.keyword.mobileanalytics_short}} service, download the code, run the iOS app locally in Xcode, configure, and monitor the app.

## Step 1. Requirements for Developers
{: #dev-requirements}

To get started with iOS development on {{site.data.keyword.cloud_notm}}, make sure that you meet the following requirements.

### Operating System

It is best practice to develop Swift apps by using the latest MacOS supported hardware, and testing with the latest iOS releases. Sign up for an [Apple Developer](https://developer.apple.com/) account to enable testing on a physical device.

### iOS and Xcode
{: #ios_and_xcode}

- Install [Xcode 8+](https://developer.apple.com/xcode/) (or higher).
- Deploy to [iOS 8 devices](https://support.apple.com/downloads/ios) (or higher).
- Review the [App Store Submission Guidelines](https://developer.apple.com/app-store/guidelines/) prior to submitting apps to Apple.

### SDKs and Dependency management

The following tools ensure that you can install the native SDKs to work with the various {{site.data.keyword.cloud_notm}} Services.

1. Install [CocoaPods](https://cocoapods.org/) to support {{site.data.keyword.cloud_notm}} SDKs:
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
2. Install [Homebrew](https://brew.sh/) to assist Carthage installation:
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

3. Install [Carthage](https://github.com/Carthage/Carthage) to support Watson SDKs:
  ```
  brew install carthage
  ```
  {: codeblock}

## Step 2. Creating a custom iOS Swift App
{: #create-ios-app}

1. Log in to the [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits).
2. Click **Create App**.
3. On the [Empty Starter](https://console.bluemix.net/developer/appledevelopment/create-app) page, you can use the default configuration, or update the fields as needed. Ensure that **iOS Swift** is the selected language. Click **Create**.

## Step 3. Adding the {{site.data.keyword.mobileanalytics_short}} service
{: #adding-services}

1. From the App Details page, click the **Add Resource** button.
2. Select **Mobile**, and click **Next**.
3. Select **{{site.data.keyword.mobileanalytics_short}}**, and click **Next**.
4. Click **Create**.

## Step 4. Downloading the code and setting up client SDKs
{: #run-locally}

To download the code, click on **Download Code** under `Apps` > `Your App`. The downloaded code comes with **{{site.data.keyword.mobileanalytics_short}}** Client SDKs included. The Client SDKs are available on CocoaPods and Carthage. For this solution, use CocoaPods.

1. Unzip the downloaded code. Then, using a terminal, navigate to the unzipped folder.
  ```
  cd <Name of Project>
  ```
2. The folder includes a podfile with the required dependencies. Run the following command to install the dependencies (Client SDKs):
  ```
  pod install
  ```
  {: codeblock}

## Step 5. Configuring the app to use {{site.data.keyword.mobileanalytics_short}}
{: #configure-analytics}

1. Open `.xcworkspace` in Xcode and navigate to `AppDelegate.swift`.
  **Note:** Ensure that you always open the new Xcode workspace, instead of the original Xcode project file: `MyApp.xcworkspace`.
   ![Open Xcode](images/Xcode.png)

  `BMSCore` is the Core SDK and is base for the Mobile Client SDKs. `BMSClient` is a class of BMSCore and initialized in AppDelegate.swift. Along with BMSCore, {{site.data.keyword.mobileanalytics_short}} SDK is already imported into the project.
  
2. Analytics initialization code is already included as shown in the following snippet:
  ```
  // Analytics client SDK is configured to record lifecycle events.
         	Analytics.initialize(appName:dictionary["appName"] as? String,
        			     apiKey: dictionary["analyticsApiKey"] as? String,
        	        	     deviceEvents: .lifecycle)

        	// Enable Logger (disabled by default) and set level to ERROR (DEBUG by default).
        	Logger.isLogStorageEnabled = true
        	Logger.logLevelFilter = .error
  ```
  {: codeblock}

  **Note:** The service credentials are part of the `BMSCredentials.plist` file.

3. Gathering usage analytics by using `logger`. Navigate to the `ViewController.swift` file to see the following code.
  ```
   func didBecomeActive(_ notification: Notification) {
       Analytics.send()
       Logger.send()
    }
  ```
  {: codeblock}

   For advanced Analytics and logging capabilities, refer to [Gathering usage Analytics](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#app-monitoring-gathering-analytics) and [logging](https://console.bluemix.net/docs/services/mobileanalytics/sdk.html#enabling-configuring-and-using-logger).
   {:tip}

## Step 6. Monitoring the app with {{site.data.keyword.mobileanalytics_short}}
The {{site.data.keyword.mobileanalytics_short}} service provides key application usage and performance insights for mobile application developers and application owners. By using {{site.data.keyword.mobileanalytics_short}}, application owners and developers can understand what is happening on the user side. They can use this insight to build better applications that are relevant to users, and that stand out in the veritable sea of mobile applications.

The service includes the {{site.data.keyword.mobileanalytics_short}} Console where developers and application owners can monitor mobile application performance, see usage statistics, and search device logs.

1. Open the `{{site.data.keyword.mobileanalytics_short}}` service from the mobile app you created or click on the three vertical dots next to the service and select `Open Dashboard`.
2. You can see LIVE Users, Sessions, and other App Data by disabling `Demo Mode`. You can filter the analytics information by the following criteria:
    * Date.
    * Application.
    * Operating System.
    * Version of the app.
         ![{{site.data.keyword.mobileanalytics_short}}](images/mobile_analytics.png)
3. [Click here](https://console.bluemix.net/docs/services/mobileanalytics/app-monitoring.html#monitoringapps) to set alerts, Monitor App crashes, and Monitor network requests.

## Next steps
{: #next-steps}

### Adding more services
You can add more services to your iOS app directly from the web console, such as the following commonly used services:

* [Adding the Push Notifications service](/push/push_notifications.html)
* [Adding user authentication with App ID](/authenticate/app_id.html)

### Using {{site.data.keyword.cloud_notm}} developer tools
You can also learn how to develop Swift apps by using the [{{site.data.keyword.cloud_notm}} developer tools](../cli/index.html), which offer a command-line approach to creating, developing, and deploying end-to-end web, mobile, and microservice applications.

