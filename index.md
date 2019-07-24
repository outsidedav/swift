---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-07"

keywords: getting started swift, custom app, create app swift, stater kit swift, apple app swift, swift dependency, ios development

subcollection: Swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Getting started tutorial
{: #getting-started}

{{site.data.keyword.cloud}} offers solutions and services to enable Swift Developers to build applications that are integrated with the security, AI, and value that your customers demand. With a broad portfolio of offerings and SDKs, you can use these services, and bring cutting-edge applications to market quickly. This Swift programming explains how to add services to a new or existing Swift application, whether it's an iOS client or server-side Swift.
{: shortdesc}

The following tutorial shows how to easily create a Swift mobile app with {{site.data.keyword.mobileanalytics_full}} by using an empty Starter Kit from the [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon"). From the console, you add the {{site.data.keyword.mobileanalytics_short}} service, download the code, run the iOS app locally in Xcode, configure, and monitor the app.

## Step 1. Requirements for developers
{: #dev-requirements-swift}

To get started with iOS development on {{site.data.keyword.cloud_notm}}, make sure that you meet the following requirements.

### Operating system
{: #swift-os-requirements}

The best practice for developing Swift apps is by using the latest MacOS supported hardware, and testing with the latest iOS releases. Sign up for an [Apple Developer](https://developer.apple.com/){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") account to enable testing on a physical device.

### iOS and Xcode
{: #ios_and_xcode}

- Install [Xcode 8+](https://developer.apple.com/xcode/){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") (or higher).
- Deploy to [iOS 8 devices](https://support.apple.com/downloads/ios){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") (or higher).
- Review the [App Store Submission Guidelines](https://developer.apple.com/app-store/resources/){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") before you submit apps to Apple.

### SDKs and dependency management
{: #swift-sdk-management}

The following tools ensure that you can install the native SDKs to work with the various {{site.data.keyword.cloud_notm}} Services. You can use CocoaPods or Carthage for dependency management.

* **Using CocoaPods** - Install [CocoaPods](https://cocoapods.org/) to support {{site.data.keyword.cloud_notm}} SDKs:
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}

* **Using Carthage** - Some SDKs are also available through the [Carthage](https://github.com/Carthage/Carthage){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") or [Swift Package Manager](https://swift.org/package-manager/){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") dependency managers. To use Carthage for dependency management, do the following steps:

  Install [Homebrew](https://brew.sh/){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") to assist Carthage installation:
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

  Install Carthage:
  ```
  brew install carthage
  ```
  {: codeblock}

## Step 2. Creating a custom iOS Swift app
{: #create-ios-app-swift}

1. Log in to the [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").
2. Click **Create app**.
3. On the [Empty Starter](https://{DomainName}/developer/appledevelopment/create-app){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") page, you can use the default configuration, or update the fields as needed. Ensure that **iOS Swift** is the selected language. Click **Create**.

## Step 3. Adding the {{site.data.keyword.cloudant_short_notm}} service
{: #resources-swift}

You can now add services to your Swift application. For this tutorial, add the {{site.data.keyword.cloudant_short_notm}} service to your Swift app, which creates a fully managed, distributed `JSON` document database. Cloudant powers your app with scalability, high availability, and durability in a lightweight framework that keeps your data safe and in sync.

1. From the **App details** page, click **Add service**.
2. Select **Data**, and click **Next**.
3. Select **Cloudant**, and click **Next**.
4. Click **Create**.
5. Once the service is created, click it to start it. On this new page, select **Launch Cloudant Dashboard** to begin creating a database and JSON documents.  Alternatively, this can be done programmatically.

## Step 4. Downloading the code and setting up client SDKs
{: #run-locally-swift}

To download the code, click **Download code** under `Apps` > `Your App`. The downloaded code comes with the [SwiftCloudant SDK](https://github.com/cloudant/swift-cloudant){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") included, as well as some basic initialization code. The client SDKs are available on CocoaPods and Swift Package Manager. This solution uses CocoaPods.

1. Extract the downloaded code. Then, using a terminal, navigate to the extracted folder.
  ```
  cd <Name of Project>
  ```

2. The folder includes a podfile with the required dependencies. Run the following command to install the dependencies (Client SDKs):
  ```
  pod install
  ```
  {: codeblock}

## Step 5. Configure the app to use your new database
{: #config-db-swift}

1. Open the file name that ends in `.xcworkspace` with Xcode, and navigate to `ViewController.swift`. `SwiftCloudant` is the core Cloudant SDK. `CouchDBClient` is a class of `SwiftCloudant` and initialized in `ViewController.swift`.

  Always open the new Xcode workspace, instead of the original Xcode project file: `MyApp.xcworkspace`.
  {: tip}

2. The database initialization code is already included as shown in the following snippet:
  ```swift
  /* Read the Cloudant credentials and intialize database connection */
  if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"), let dictionary = NSDictionary(contentsOfFile: contents) {
            let url = URL(string: dictionary["cloudantUrl"] as! String)
            let client = CouchDBClient(url:url!,
                                       username:dictionary["cloudantUsername"] as? String,
                                       password:dictionary["cloudantPassword"] as? String)
        }
  ```
  {: codeblock}

  The service credentials are part of the `BMSCredentials.plist` file.
  {: note}

## Step 6. Build out your database operations
{: #build_ops-swift}

Now that you have a working database connection and SDK set up, you can begin building out the basic [create, read, update, and destroy operations](/docs/swift/data?topic=swift-cloudant) for your particular use case.

## Next steps
{: #next-swift}

### Adding more services
{: #moreresources-swift}

You can add more services to your iOS app directly from the web console, such as the following commonly used services:

* [Adding the Push Notifications service](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate)
* [Adding user authentication with App ID](/docs/services/appid?topic=appid-getting-started)

### Using {{site.data.keyword.cloud_notm}} developer tools
{: #devtools-swift}

You can also learn how to develop Swift apps by using the [{{site.data.keyword.cloud_notm}} Developer tools](/docs/cli?topic=cloud-cli-getting-started), which offer a command line approach to creating, developing, and deploying complete web, mobile, and microservice applications.
