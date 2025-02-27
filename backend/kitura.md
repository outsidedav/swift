---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-19"

keywords: foodtrackerbackend, kitura swift, urlsession sdk, alamofire, restkit, kiturakit, kitura, xcode kitura, meals swift, rest api kitura, rest api swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Creating an app with Kitura
{: #kitura}

[Kitura](https://www.kitura.io){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon") is a server-side Swift framework for building iOS backends and web applications. This framework creates REST APIs that can be invoked from the iOS application by using URLSession SDKs such as Alamofire, RestKit, or the [KituraKit](https://github.com/ibm-swift/kiturakit){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon") SDK provided by Kitura itself.

Kitura is able to integrate with all of the services and features that are provided by {{site.data.keyword.cloud}}, including {{site.data.keyword.appid_short}}, {{site.data.keyword.mobilepushshort}}, and {{site.data.keyword.mobileanalytics_short}}, as well as databases, machine learning, and other services. Kitura can then be deployed and automatically scaled by using either of the Cloud Foundry or Docker (Kubernetes-based) platforms in {{site.data.keyword.cloud}}.

Kitura provides a `kitura` [command line interface (CLI)](https://www.kitura.io/guides/kituracli/gettingstarted.html){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon") that simplifies creating, building, testing, and deploying Kitura applications. Applications that are built by using the Kitura CLI include full support for deploying to any cloud that supports Cloud Foundry, Docker, and Kubernetes technologies. However, if you are building specifically for {{site.data.keyword.cloud_notm}}, it is recommended to use the IBM Apple Development Console in the browser or use the {{site.data.keyword.dev_cli_notm}}. Additionally, while both methods share underlying technology, the Apple Development Console and the IBM Developer Tools create a hosted app and deployment pipeline for you, as well as provision the services that your application needs.

## Before you begin
{: #prereqs-kitura}

First, be sure that you have the following prerequisites ready to go:  

* iOS 11.0+  
* Xcode 9.0  
* Swift 4.0+  
* CocoaPods  

## Step 1. Creating a Kitura app by using the browser
{: #create_kitura}

1. Go to the Starter Kits section of the Apple Development Console. Select a pre-defined starter, such as the **Swift for Backend for Frontend API**, or create a custom app by using the **Create app** tile. Click **Create app**.

2. Give your app a name and select where you want your app to be deployed. If you're unsure where the application is to be deployed, use the default values, as they can be changed later.

3. Select the Swift language. A server-side Swift app is created. Also displayed, is the Host and Domain options, which forms the URL for the application. If you're unsure, use the defaults and you can also provide your own custom domain from a domain provider where the application is to reside.

4. Click **Create app**.

An app is created, but one that doesn't yet use any additional services. You can add services by clicking **Add service** or **Download code** button to get the code for the app. You can also easily add services to an existing app.

## Step 2. Adding services
{: #add_services-kitura}

1. Click **Add service** to add services. Service categories are displayed. For example, select **Data** to look at the available databases, and select **Cloudant NoSQL DB**.
2. Select a pricing plan for the service, for example Lite, and click **Create**.

An instance of the service is created that provides credentials for the application, and in some cases adds the necessary code to your app to include the correct connection to the service. You can now add more services by using the **Add service** button, or by clicking **Download code** to get the code for the app.

After downloading your app, you can begin working with it.

## Step 3. Developing your application with Xcode
{: #develop_xcode-kitura}

After you download your app code, you can modify and develop it by using Xcode, and then upload your modified application for deployment to the cloud.

1. Create an Xcode project.  
  Before you can use your project in Xcode, you must create an Xcode project file by using the Swift Package Manager command. Run the following command from the root directory of your downloaded project, where the `Package.swift` file resides:
  ```
  swift package generate-xcodeproj
  ```
  {: codeblock}

  An Xcode project is created in the same directory that you can then open.

2. Set the Xcode target for the project.  
  To run the Kitura server, you must edit the scheme by clicking the **project_name-Package** section on the toolbar and selecting the **project_name** target from the menu. Check that the target device is set to **My Mac**.

3. Run the Kitura server locally. 
  Click **Run** or use the `⌘+R` key shortcut to start the Kitura server. Once the server is started, you can check that the following standard Kitura URLs are running:
  * Kitura Monitoring: [http://localhost:8080/swiftmetrics-dash/](http://localhost:8080/swiftmetrics-dash/)
  * Kitura Health check: [http://localhost:8080/health](http://localhost:8080/health)

## Step 4. Adding REST APIs
{: #add_restapi-kitura}

A skeleton Kitura server is created, but it doesn't provide any REST APIs that can be used by an iOS application. Add REST APIs in Kitura with minimal coding. Use the following steps to add a REST API for `GET` requests on `/meals`, which is designed to return the `Meal` objects that are stored by the Kitura server.

1. Add a `Meal` struct to the `Sources/Application/Application.swift` file:  
  Before the declaration of the `App` class, add the following to create a Meal struct that conforms to the Codable protocol:  
  ```swift
	struct Meal: Codable {    
	    var name: String
	    var photo: Data
	    var rating: Int
	}
  ```
  {: codeblock}

2. Add a Dictionary to the `Sources/Application/Application.swift` file to store `Meal` objects by adding `let cloudEnv = CloudEnv()` to the following code section:
  ```swift
  private var mealStore: [String: Meal] = [:]
  ```
  {: codeblock}

3. Add a handler for `GET` requests on `/meals` to the `Sources/Application/Application.swift` file by adding the following code into the `postInit()` function:  
  ```swift
  router.get("/meals", handler: loadHandler)
  ```
  {: codeblock}

4. Implement the loadHandler function to the `Sources/Application/Application.swift` file by adding the following code as another function in the `App` class:
  ```swift
  func loadHandler(completion: ([Meal]?, RequestError?) -> Void ) {
      let meals: [Meal] = self.mealStore.map({ $0.value })
    completion(meals, nil)
  }
  ```
  {: codeblock"}

You now have a REST API for `GET` requests on `/meals` that responds with an array of `Meals` or an error.

5. Run the project.  
  Click **Run** or use the `⌘+R` key shortcut. If prompted, select **Allow incoming network connections**.

6. Test the REST API by using a `GET` request, which matches how web browsers request data from a server. 

  You can test the REST API by using the following URL:  
  ```swift
  * `GET /meals`:	[http://localhost:8080/meals](http://localhost:8080/meals)
  ```
  {: codeblock}

  An empty array is returned `[]` because no `Meal` objects are currently stored in the `mealStore`. 

You can use the [FoodTracker Backend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon") tutorial, which helps you build a set of REST APIs for storing, fetching, and deleting `Meal` objects from an iOS application, including storing the data in a database.
{: tip}

## Step 5. Installing KituraKit into your iOS application
{: #install-kiturakit}

The REST APIs built by using the Kitura server are standard web APIs, usable from any application regardless of client library that is used or which language the client is written in. Meaning that you can use Alamofire, RestKit, or URLSession to make connections to the server. Kitura also provides a bespoke, optimized client connector in order to simplify calling its REST APIs from iOS, in the form of KituraKit. 

KituraKit provides a mirror image of the router handler APIs used in Kitura, making it possible to share Swift types between the client and the server with little effort. This feature removes the need to explicitly carry out JSON encoding and decoding of the data that is sent to, or received from, the server.

The following steps show how to install KituraKit into your iOS application, and use it to call the `GET /meals` REST API created by using Kitura:

1. Create a Podfile in the root of your iOS application directory if you don't have one already:
  ```
  pod init
  ```
  {: codeblock}

2. Edit the Podfile to set a global platform of iOS 11 for your project by replacing the following line:
  ```pod
  # platform :ios, '9.0'
  ```
  {: codeblock}

  With the following code:
	```pod
  platform :ios, '11.0'
  ```
  {: codeblock}

3. Add KituraKit to the Podfile by adding the following under `# Pods for <application name>`:
  ```pod
  pod 'KituraKit', :git => 'https://github.com/IBM-Swift/KituraKit.git', :branch => 'pod'
  ```
  {: codeblock}

4. Save the Podfile and install KituraKit by using the following command:
  ```
  pod install
  ```
  {: codeblock}

5. Reopen the application in Xcode by opening the workspace (not project). You can now add the same `Meal` definition to your iOS application as used in the Kitura server:
  ```swift
  struct Meal: Codable {    
      var name: String
      var photo: Data
      var rating: Int
  }
  ```
  {: codeblock}

  And use the following code to make a connection to the Kitura server:
  
  ```swift
  import KituraKit
  
  /* Connect to the Kitura server on http://localhost:8080 */
  guard let client = KituraKit(baseURL: "http://localhost:8080") else {
      print("Error creating KituraKit client")
      return
  }
  
  /* Make a request to `GET /meals`, expecting a [Meal] or a RequestError */
  client.get("/meals") { (meals: [Meal]?, error: RequestError?) in
      /* Check for an error */
      guard error == nil else {
          print("Error saving meal to Kitura: \(error!)")
          return
      }
      /* Check for meals */
      guard let meals = meals else {
          print("Received Meals!")
          return
      }
  }
  ```
  {: codeblock}

The Kitura server is called by using `client.get("/meals")` with a callback that matches the parameters from Kitura's completion handler.

You can use the [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon") tutorial, which helps you build a set of REST APIs for storing, fetching, and deleting `Meal` objects from an iOS application, including storing the data in a database.
{: tip}

## Next Steps
{: #next-kitura notoc}

Now that you have a Kitura server that provides a REST API that can be called by your iOS application, you're ready to deploy your server to {{site.data.keyword.cloud_notm}}. [Deployments](/docs/swift?topic=swift-deploy_apps-swift#deploy_apps-swift) can be done by using containers with Kubernetes, secure containers, Cloud Foundry, Cloud Foundry Enterprise Environment, or virtual instances.
