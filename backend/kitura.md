---

copyright:
  years: 2018
lastupdated: "2018-02-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Creating an application with Kitura
{: #kitura}

[Kitura](http://www.kitura.io) is a server-side Swift framework for building iOS backends and web applications. This is used to create REST APIs that can be invoked from the iOS application using URLSession, SDKs such as Alamofire or RestKit, or the [KituraKit](https://github.com/ibm-swift/kiturakit) SDK provided by Kitura itself.

Kitura is able to integrate with all of the services and capabilties provided by {{site.data.keyword.cloud}}, including {{site.data.keyword. appid_short}}, {{site.data.keyword.mobilepushshort}}, and {{site.data.keyword.mobileanalytics_short}}, as well as databases, machine learning, and other services. Kitura can then be deployed and automatically scaled using either of the Cloud Foundry or Docker and Kubernetes plaforms in {{site.data.keyword.cloud}}.

Kitura provides a `kitura` [command line interface (CLI)](http://www.kitura.io/en/starter/gettingstarted.html) that simplifies creating, building, testing, and deploying Kitura applications. Applications built by using the Kitura CLI include full support for deploying to any cloud that supports any of Cloud Foundry, Docker and Kubernetes technologies. However if you are building specifically for {{site.data.keyword.cloud}} it is recommended that you use either the IBM Apple Development Console in the browser, or use the {{site.data.keyword.dev_cli_notm}}. These all share the same underlying technology, but the Apple Development Console and the IBM Developer Tools will additionally create a hosted project and deployment pipeline for you, as well as provision the services your application needs.

## Before you begin

First, be sure you have the following prerequisites ready to go:  

* iOS 11.0+  
* Xcode 9.0  
* Swift 4.0+  
* Cocoapods  


## Step 1. Create a Kitura project by using the browser

1. Go to the Starter Kits section of the Apple Development Console. Select a pre-defined starter, such as the "Swift for Backend for Frontend API" or create a custom project by using the **Create Project** tile. Click **Create Project**.
2. Give your project a name and select where you want your project to be deployed. If you are unsure where the application should be deployed use the default values. These can be changed later.
3. Select the Swift language. A server-side Swift project is created.
  This also displays the Host and Domain options, which forms the URL for the application. If you are unsure, use the defaults and you can also provide your own custom domain from a domain provider where the application will appear.

4. Optionally, provide an OpenAPI (also known as Swagger) definition for the REST API that you want to build. If you have a definition, a REST API is created including the corresponding handler functions in Kitura. If you don't have an OpenAPI definition, don't worry as Kitura makes it easy for you to manually create REST APIs by using its Router APIs.
5. Click **Create Project**.

A project is created, but one that does not yet use any additional services or capabilities. You can optionally add those by using the **Add Resource** button, or by clicking the **Download Code** button to get the code for the project. You can also easily add services to an existing project.

## (Optional) Step 2. Add additional services

1. Click the **Add Resource** button to add additional services. This provide a panel of service categories. For example, select **Data** to look at the available databases, and select **Cloudant NoSQL DB**.
2. Select a pricing plan for the service, for example Lite, and click **Create**.

This creates an instance of the service and provides you with the service credentials for use in your application, and in some cases also adds the necessary code to your project to set up the connection to the service with the correct configuration.  You can now optionally add more services by using the **Add Resource** button, or by clicking the **Download Code** button to get the code for the project.

After you have downloaded your project you can begin working with your app.

## Step 3. Developing your application with Xcode
After you have downloaded your project, you are able to modify and develop it by using Xcode, and then upload your modified application for deployment to the cloud.

1. Create an Xcode project.  
  Before using your project in Xcode, you need to create an Xcode project file. This can be done using the following Swift Package Manager command from the root directory of your downloaded project (which will contain a `Package.swift` file):
  ```
  swift package generate-xcodeproj
  ```
  {: codeblock}

  This creates an Xcode project in the same directory that you can then open.

2. Set the Xcode target for the project.  
  To run the Kitura server, you must edit the scheme by clicking on the **project_name-Package** section on the toolbar and selecting the **project_name** target from the menu. You should also check that the target device is set to **My Mac**.

3. Run the Kitura server locally  
  Click on the **Run** button or use the `⌘+R` key shortcut to start the Kitura server. After the server has started you can check that some of the standard Kitura URLs are running:
  * Kitura Monitoring: [http://localhost:8080/swiftmetrics-dash/]()
  * Kitura Health check: [http://localhost:8080/health]()


## Step 4. Adding additional REST APIs
A skeleton Kitura server is created, but it does not provide any REST APIs that can be used by an iOS application. Adding REST APIs in Kitura requires very little code. The following steps show how to add a REST API for `GET` request on `/meals`, which is designed to return the `Meal` objects stored by the Kitura server.

1. Add a `Meal` struct to the `Sources/Application/Application.swift` file:  
  Before the declaration of the App class, add the following to create a Meal struct that conforms to the Codable protocol:  
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

3. Add a handler for `GET` requests on `/meals` to the `Sources/Application/Application.swift` file by adding the following into the postInit() function:  

  ```swift
  router.get("/meals", handler: loadHandler)
  ```
  {: codeblock}

4. Implement the loadHandler function to the `Sources/Application/Application.swift` file by adding the following as another function in the `App` class:  

  ```swift
  func loadHandler(completion: ([Meal]?, RequestError?) -> Void ) {
      let meals: [Meal] = self.mealStore.map({ $0.value })
    completion(meals, nil)
  }
  ```
  {: codeblock"}

You now have a REST API for `GET` requests on `/meals` that responds with an array of `Meals` or an error.

5. Run the project.  
  Press the **Run** button or use the `⌘+R` key shortcut. If prompted, select **Allow incoming network connections**.

6. Test the REST API. 
  As this is a `GET` request, which matches how web browsers request data from a server, you can test the REST API by using the following URL:  

  ```
  * `GET /meals`:	[http://localhost:8080/meals]()
  ```

  This should return an empty array, eg. `[]` as there are no `Meal` objects currently stored in the `mealStore`. 

A full tutorial called [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend) is available that helps you build a set of REST APIs for storing, fetching, and deleting `Meal` objects from an iOS application, including storing the data in a database.
{: tip}

## Step 5. Install KituraKit into your iOS application
The REST APIs built by using the Kitura server are standard web APIs, usable from any application regardless of client library used or which language the client is written in. This means that you can use Alamofire, RestKit or URLSession to make connections to the server. Kitura also provides a bespoke, optimized client connector in order to simplify calling its REST APIs from iOS, in the form of KituraKit. 

KituraKit provides a mirror image of the router handler APIs used in Kitura, making it possible to share Swift types between the client and the server with little effort, and removes the need to explicitly carry out JSON encoding and decoding of the data being sent to or received from the server.

The following steps show how to install KituraKit into your iOS application, and use it to call the `GET /meals` REST API created by using Kitura:

1. Create a Podfile in the root of your iOS application directory if you do not have one already:
  ```
  pod init
  ```
  {: codeblock}

2. Edit the Podfile to set a global platform of iOS 11 for your project by replacing the following line:
  ```
  # platform :ios, '9.0'
  ```

  with
	
  ```
  platform :ios, '11.0'
  ```

3. Add KituraKit to the Podfile by adding the following under `# Pods for <application name>`:
  ```
  pod 'KituraKit', :git => 'https://github.com/IBM-Swift/KituraKit.git', :branch => 'pod'
  ```
  {: codeblock}

4. Save the Podfile and install KituraKit by using the following on the command line:
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
  
  // Connect to the Kitura server on http://localhost:8080
  guard let client = KituraKit(baseURL: "http://localhost:8080") else {
      print("Error creating KituraKit client")
      return
  }
  
  // Make a request to `GET /meals`, expecting a [Meal] or a RequestError
  client.get("/meals") { (meals: [Meal]?, error: RequestError?) in
      // Check for an error
      guard error == nil else {
          print("Error saving meal to Kitura: \(error!)")
          return
      }
      // Check for meals
      guard let meals = meals else {
          print("Received Meals!")
          return
      }
  }
  ```
  {: codeblock}

This calls the Kitura server byusing `client.get("/meals")` with a callback that matches the parameters from Kitura's completion handler.

A full tutorial called [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend) is available that helps you build a set of REST APIs for storing, fetchin,g and deleting `Meal` objects from an iOS application, including storing the data in a database.
{: tip}

## Next Steps
Now that you have a Kitura server providing a REST API that is being called by your iOS application, you are ready to deploy your server to IBM Cloud. This can be done using Containers with Kubernetes, Secure Containers, or Cloud Foundry.
