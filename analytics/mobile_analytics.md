---

copyright:
  years: 2018
lastupdated: "2018-06-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Collecting mobile analytics
{: #mobile_analytics}

{{site.data.keyword.mobileanalytics_short}} on {{site.data.keyword.cloud_notm}} provides developers, IT administrators, and business stakeholders insight into how their mobile apps are performing and how it is being used. With the {{site.data.keyword.mobileanalytics_short}} service you can:

 - **Gain immediate insight**

    See performance and usage metrics in real time.

 - **Implement in minutes**

    Create a service instance in {{site.data.keyword.cloud_notm}}, add the SDK to your project, paste two lines of code into your application and you are ready to collect dozens of pre-defined metrics.

 - **Collect data you want**

    Instrument apps with custom events, discover how users are interacting with your app, track purchases, and monitor app activity.

 - **See metrics for all of your applications at-a-glance**

    The {{site.data.keyword.mobileanalytics_short}} console offers ready-made charts, without the need to write queries.

 - **Focus on what is important to you**

    Filter metrics by time, adapter, application, application version, OS, OS version, or device model.

 - **Rapidly discover issues**

    Monitor crash status. Set alert triggers on critical metrics and route alerts to any REST endpoint.

 - **Troubleshoot to root cause**

    Use custom logging in your application and automatically upload the logs and search them from the console. Drill down on crash events to see stack traces.

## Before you begin

First, be sure you have the following prerequisites ready to go:

 - iOS 8.0+ / watchOS 2.0+
 - Xcode 7.3, 8.0
 - Swift 2.2 - 3.0
 - Cocoapods or Carthage

## Step 1. Creating an instance of {{site.data.keyword.mobileanalytics_short}}
{: ##mobile_analytics_create}

1. In the {{site.data.keyword.cloud_notm}} catalog, click **Mobile** > **{{site.data.keyword.mobileanalytics_short}}**. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Click **Create**.
4. In the navigation pane, Click **Connections** to select an app and bind it to your service. You can bind the service instance to your app later if you leave it unbound during creation.

## Step 2. Installing the iOS Swift SDK

The service provides platform-specific SDKs to simplify application development. The {{site.data.keyword.cloud_notm}} Mobile Services Swift SDKs can be installed with either Cocoapods or Carthage. For more information, see [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics).

The {{site.data.keyword.mobileanalytics_full}} SDK enables you to instrument your mobile application. The Swift SDK is available for iOS and watchOS.

1. Make sure that you correctly set up Xcode. To learn how to set up your iOS development environment, see the [Apple Developer website ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.apple.com/support/xcode/){: new_window}. Read about the [Xcode requirements ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#requirements){: new_window} for Client SDK Swift Analytics.

2. Enable the location API by adding a property in the `Info.plist` file in the project folder of your app. For example,  `Privacy - Location Usage Description` and give proper justification for adding the location API, such as "The app requires location service to be enabled".

The {{site.data.keyword.mobileanalytics_short}} SDK is distributed with [CocoaPods ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cocoapods.org/){: new_window} and [Carthage ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/Carthage/Carthage#getting-started){: new_window}, which are dependency managers for Cocoa projects. CocoaPods and Carthage automatically download artifacts from repositories to make them available to your application. Select CocoaPods or Carthage:

### CocoaPods
{: #cocoapods notoc}

1. Follow the [{{site.data.keyword.Bluemix_notm}} Mobile Services Swift SDK instructions ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#cocoapods){: new_window} on GitHub to install `BMSAnalytics` by using Cocoapods and add it to your Podfile.

2. After installing the iOS Client SDK, [import and initialize](sdk.html#initalize-ma-sdk) the Analytics Client SDK.   

### Carthage
{: #carthage notoc}

If you are not using CocoaPods, you can add frameworks to your project by using [Carthage ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos){: new_window}.

1. Follow the [Carthage installation instructions ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#carthage){: new_window} on GitHub to install `BMSAnalytics`.

2. After installing the iOS Client SDK, import and initialize the Analytics Client SDK.

## Step 3. Initializing the SDK

{{site.data.keyword.mobileanalytics_short}} enables you to collect the following categories of data, and each requires a different degree of instrumentation:

- **Pre-defined data**

	This category includes generic usage and device information that applies to all applications. It indicates the volume, frequency, or duration of application use. Predefined data is collected automatically after you initialize the {{site.data.keyword.mobileanalytics_short}} SDK in your application.

 - **Application log messages**

	This category enables the developer to add lines of code throughout the application that can log custom messages to assist with development and debugging. The developer assigns a severity/verbosity level to each log message.

 - **Custom events**

	This category includes data that you define yourself and that is specific to your app. This data represents events that occur within your app, such as page views, button taps, or in-app purchases. In addition to initializing the {{site.data.keyword.mobileanalytics_short}} SDK in your app, you must add a line of code for each custom event that you want to track.

## Step 4. Identifying your service credentials API Key value
{: #analytics-clientkey}

Identify your **API Key** value before you set up the client SDK. The API key is required for initializing the client SDK.

1. Open your {{site.data.keyword.mobileanalytics_short}} service dashboard.
2. Expand **View Credentials** to reveal your API Key value. You need the API key value when you initialize the {{site.data.keyword.mobileanalytics_short}} Client SDK.

## Step 5.  Initializing your application to collect analytics
{: #initalize-ma-sdk}

Initialize your application to enable sending logs to the {{site.data.keyword.mobileanalytics_short}} service.

1. Import the Client SDK.

    The Swift SDK is available for iOS and watchOS. Import the `BMSCore` and `BMSAnalytics` frameworks by adding the following `import` statements to the beginning of your `AppDelegate.swift` project file:
		```swift
		import BMSCore
		import BMSAnalytics
		```
		{: codeblock}  

2. Initialize the {{site.data.keyword.mobileanalytics_short}} Client SDK in your application.

	First initialize the `BMSClient` class by placing the initialization code in the `application(_:didFinishLaunchingWithOptions:)` method of your application delegate, or in a location that works best for your project.
		```swift
		BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Make sure that you point to your region
		```
		{: codeblock}

	You must initialize the `BMSClient` with the **bluemixRegion** parameter. In the initializer, the **bluemixRegion** value specifies which {{site.data.keyword.Bluemix_notm}} deployment you are using.

3. Initialize Analytics by using your application object and giving it your application’s name.

	The name that you select for your application (`your_app_name_here`) displays in the {{site.data.keyword.mobileanalytics_short}} console as the application name. The application name is used as a filter to search for application logs in the dashboard. When you use the same application name across platforms (for example, iOS), you can see all logs from that application under the same name, regardless of which platform the logs were sent from.

	You also need the [API Key](#analytics-clientkey) value.

	```swift
	Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, collectLocation: true, deviceEvents: .lifecycle, .network)
	```
    {: codeblock}

4. The application is now initialized and ready to collect analytics. Next, you can [send analytics data] to the {{site.data.keyword.mobileanalytics_short}} service.		

## Step 6. Gathering usage analytics
{: #app-monitoring-gathering-analytics}

You can configure the {{site.data.keyword.mobileanalytics_short}} client SDK to record usage analytics and send the recorded data to the {{site.data.keyword.mobileanalytics_short}} service.

Use the following APIs to start recording and sending usage analytics:
	```
	// Disable recording of usage analytics (eg: to save disk space)
	// Recording is enabled by default

	Analytics.isEnabled = false

	// Enable recording of usage analytics

	Analytics.isEnabled = true

	// Send recorded usage analytics to the Mobile Analytics Service

	Analytics.send(completionHandler: { (response: Response?, error: Error?) in
	    if let response = response {

		  // Handle Analytics send success here.
	    }
	    if let error = error {
	        // Handle Analytics send failure here.
	    }
	})
	```
	{: codeblock}

	Sample usage analytics for logging an event:

	```swift
	// Log a custom analytics event
	let eventObject = ["customProperty": "propertyValue"]
	Analytics.log(metadata: eventObject)
	```
	{: codeblock}

## Step 7. Enabling, configuring, and using Logger

The {{site.data.keyword.mobileanalytics_full}} Client SDK provides a logging framework that is similar to other log frameworks that you might be familiar with, such as `java.util.logging` or `log4j`. The logging framework supports multiple per-package logger instances, different log levels, capturing of stack traces for an application crash, and more.

You can also configure the logged data to be stored on the device where the application is running and send the device logs to the {{site.data.keyword.mobileanalytics_short}} Service at a later time.

The {{site.data.keyword.mobileanalytics_short}} Client SDK logging framework supports the following log levels, which are listed from least to most verbose, with recommended use guidelines:

  * `FATAL` - Use for unrecoverable crashes or hangs. The `FATAL` level is reserved for logging unrecoverable errors, which appear to users as an application crash
  * `ERROR` - Use for unexpected exceptions or unexpected network protocol errors
  * `WARN` - Use to log usage warnings that are not considered critical errors, such as usage of deprecated APIs or slow network response
  * `INFO` - Use for reporting initialization events and other data that might be important, but not urgent
  * `DEBUG` - Use for reporting debug statements to help developers resolve application defects

### Log level scenario
{: #log-level-scenario notoc}

When the logger level is configured to `FATAL`, the logger captures uncaught exceptions, but does not capture any logs that lead up to the crash event. You can set a more verbose logger level to ensure that logs that might lead to a `FATAL` logger entry, such as `WARN` and `ERROR`, are also captured.

When the logger level is set `DEBUG`, you also get Mobile Analytics Client SDK logs, which are included when you send logs.

### Sample Logger usage
{: #sample-logger-usage notoc}

**Note:** Make sure that your application is configured to use the {{site.data.keyword.mobileanalytics_short}} Client SDK before using the logging framework.

The following code snippets show sample Logger usage:
	```
	// Configure Logger. Disabled by default;

	Logger.isLogStorageEnabled = true

	// Change the minimum log level (optional). Default is - LogLevel.debug

	Logger.logLevelFilter = LogLevel.info

	// Multiple log instances can be created to organize your logs

	let logger1 = Logger.logger(name: "feature1Logger")
	let logger2 = Logger.logger(name: "feature2Logger")

	// Log messages with different levels

	logger1.debug(message: "debug message for feature 1")

	// logger1.debug message isn't logged as logLevelFilter is set to info

	logger2.info(message: "info message for feature 2")

	// Send logs to the Mobile Analytics Service

	Logger.send(completionHandler: { (response: Response?, error: Error?) in
	        if let response = response {
	        logger.debug(message: "Status code: \(response.statusCode)")
	        logger.debug(message: "Response: \(response.responseText)")
	    }
	    if let error = error {
	        logger.error(message: "Error: \(error)")
	    }
	})
	```
	{: codeblock}

	For privacy concerns, you can disable Logger output for applications that are built in release mode. By default, the Logger class prints logs to the Xcode console. In the build settings for your target, add a `-D RELEASE_BUILD` flag to the **Other Swift Flags** section of the release build configuration.
	{: tip}

## Step 8. Location Data Logging
{: #location-logging}

Location of the mobile device might be logged from the app through this provided API:
```
Analytics.logLocation();
```

This API enables the app to send its location as latitude, longitude to the server in between app sessions. Other than these explicit calls to the location-logging API, the SDK sends device location for every App-Session, both for initial app session context and user switch app session (that is, a switch of a user between an app session) context. The location API must be enabled in the initialization of SDK.

To call this location logging API, set the `collectLocation` parameter to true in SDK initialization:
```
Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
```

## Step 9. Making a network request
{: #network-requests}

You can configure the {{site.data.keyword.mobileanalytics_short}} Client SDK to [make a network request](/docs/mobile/sdk_network_request.html). Be sure that you have already initialized `BMSClient` and `BMSAnalytics` and imported the Client SDKs.

## Step 10. Reporting crash analytics
{: #report-crash-analytics}

You can see [application crash data](app-monitoring.html#monitor-app-crash) by sending analytics and log information to {{site.data.keyword.mobileanalytics_short}}.

The `Analytics.send()` method populates the **Crash Overview** and **Crashes** tables on the **Crashes** page. Charts in this section are enabled by using the initialization and sending process for analytics; no special configuration is necessary.

The `Logger.send()` method populates the the **Crash Summary** and **Crash Details** tables on the **Troubleshooting** page. You must enable your application to store and send logs to populate the charts in this section, by adding an additional statement in your application code:
	```
	Logger.isLogStorageEnabled = true
	Logger.logLevelFilter = LogLevel.Fatal // or greater
	```

	See the iOS [sample logger usage](sdk.html##sample-logger-usage).

## Step 11. Tracking active users
{: #trackingusers}

If your application can recognize unique users on a device, you can optionally track how many users are actively using your application by passing the user name of the active user to {{site.data.keyword.mobileanalytics_short}}.

Enable user tracking by initializing {{site.data.keyword.mobileanalytics_short}} with `hasUserContext=true`. Otherwise, {{site.data.keyword.mobileanalytics_short}} captures only one user per device.

	Add the following code to track when the user logs in:
	```
	Analytics.userIdentity = "username"
	```
	{: codeblock}

## Step 12. Testing your app
{: #appid_testing}

Is everything set-up correctly? Time to test it out!

1. Open your app. If you have a web application, use a browser. If you have an iOS client application with the Xcode emulator.
2. Compile and run the application on your emulator or device.
3. Using the GUI, walk through the process of signing into your application.
4. Go to the {{site.data.keyword.mobileanalytics_short}} console to see usage analytics for your application. You can also monitor your application by [setting alerts](/docs/services/mobileanalytics/app-monitoring.html#alerts) and [monitoring app crashes](/docs/services/mobileanalytics/app-monitoring.html#monitor-app-crash).

## What to do next
{: #what-to-do-next notoc}

 - To learn more about the service, read through the [documentation](/docs/services/mobileanalytics/index.html#getting-started-tutorial).

 - For an introduction to working with mobile services and {{site.data.keyword.Bluemix_notm}}, see [Getting started with Mobile apps on IBM Cloud](/docs/services/mobile/index.html).

 - Starter Kits are one of the fastest ways to leverage the capabilities of {{site.data.keyword.cloud_notm}}. View all the available starter kits in the [Mobile developer dashboard](https://console.bluemix.net/developer/mobile/dashboard). Download the code. Run the App!

 - You can use the [Swagger UI](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/) to quickly review REST API documentation.
