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

# Server with Kitura
{: #kitura}

[Kitura](http://www.kitura.io) is a server-side Swift framework for building iOS backends and web applications. This is used to create REST APIs that can be invoked from the iOS application using URLSession, SDKs such as Alamofire or RestKit, or the [KituraKit](https://github.com/ibm-swift/kiturakit) SDK provided by Kitura itself.

Kitura is able to integrate with all of the services and capabilties provided by IBM Cloud, including AppID, Push Notifications and Mobile Analytics as well as databases, machine learning, and other services. Kitura can then be deployed and automatically scaled using either of the Cloud Foundry or Docker and Kubernetes plaforms in IBM Cloud.

##Develop

Kitura provides its own `kitura` [command line interface (CLI)](http://www.kitura.io/en/starter/gettingstarted.html) that simplifies creating, building, testing and deploying Kitura applications. Applications built using the Kitura CLI include full support for deploying to any cloud that supports any of Cloud Foundry, Docker and Kubernetes technologies. However if you are building specifically for IBM Cloud it is recommended that you use either the IBM Apple Development Console in the browser, or use the IBM Developer Tools CLI. These all share the same underlying technology, but the Apple Development Console and the IBM Developer Tools will additionally create a hosted project and deployment pipeline for you, as well as provision the services your application needs.

## Before you begin

First, be sure you have the following prerequisites ready to go:  

* iOS 11.0+  
* Xcode 9.0  
* Swift 4.0+  
* Cocoapods  


##Creating a Kitura project using the Browser

1. Go to the "Starter Kits" section of the Apple Development Console. Here you can either select a pre-defined starter such as the "Swift for Backend for Frontend API", or create a custom project using the "Create Project" tile. Select "Create Project"

2.  Give your project a name and select where you would like your project to be deployed. If you are unsure where the application should be deployed use the default values, as these can be changed later.
3. Select a language of Swift, which will cause a server side Swift project to be created

This will also cause Host and Domain options to appear for Host and Domain, which will form the URL that the application will appear on. If you are unsure use the defaults, as these can be changed later, and you can also provide your own custom domain from a domain provider where the application will appear.

4. You can also optionally provide an OpenAPI (aka Swagger) definition for the REST API you want to build. If you have a definition this will create the REST API and the corresponding handler functions in Kitura for you. If you don't have an OpenAPI definition, don't worry as Kitura makes it easy for you to manually create REST APIs using its Router APIs.

5. Click Create Project to create a project.
This has created a project, but one that does not yet use any additional services or capabilities. You can optionally add those using the "Add Resource" button, or click the "Download Code" button to get the code for the project. Note that you can also easily add services to an existing project.

### Add additional services (optional)
1. Click the "Add Resource" button to add additional services. This provide a panel of service categories. For example, select Data to look at the available databases, and select Cloudant NoSQL DB.
2. Select a pricing plan for the service, eg. Lite and click "Create"
This both creates an instance of the service and provides you with the service credentials for use in your application, and in some cases also adds the necessary code to your project to set up the connection to the service with the correct configuration.  You can now optionally add more services using the "Add Resource" button, or click the "Download Code" button to get the code for the project.

Once you have downloaded your project you can begin working with your app.

##Working with your application
One you have downloaded your project, you are able to modify and develop it using Xcode, and then upload your modified application for deployment to the cloud.

1. Create an Xcode project.  
Before using your project in Xcode, you need to create an Xcode project file. This can be done using the following Swift Package Manager command from the root directory of your downloaded project (which will contain a `Package.swift` file):

	```
	    swift package generate-xcodeproj
	```
This creates an Xcode project in the same directory that you can then open.

2. Set the Xcode target for the project  
In order to run the Kitura server, you need to edit the scheme by clicking on the "<project name>-Package" section on the top-left the toolbar and selecting the "<project name>" target from the pull down menu of options. You should also check that the target device is set to "My Mac"

3. Run the Kitura server locally  
You can now click on the Run button or use the `⌘+R` key shortcut to start the Kitura server. Once the server has started you can check that some of the standard Kitura URLs are running:

	* Kitura Monitoring: [http://localhost:8080/swiftmetrics-dash/]()
	* Kitura Health check: [http://localhost:8080/health]()


##Adding Additional REST APIs
This has created a skeleton Kitura server, but it does not get provide any REST APIs that can be used by an iOS application. Adding REST APIs in Kitura is however is designed to require very little code. The following steps show how to add a REST API for `GET` request on `/meals` which is designed to return the `Meal` objects stored by the Kitura server.

1. Add a Meal struct to the `Sources/Application/Application.swift` file:  
Before the declaration of the App class, add the following to create a Meal struct that conforms to the Codable protocol:  

	```swift
	struct Meal: Codable {    
	    var name: String
	    var photo: Data
	    var rating: Int
	}
	```

2. Add a Dictionary to the `Sources/Application/Application.swift` file in which to store `Meal` objects:  
Add the following on the line below `let cloudEnv = CloudEnv()`:
 
	```swift
	private var mealStore: [String: Meal] = [:]
	```

3. Add a handler for `GET` requests on `/meals` to the `Sources/Application/Application.swift` file:  
Add the following into the postInit() function:  

	```swift
		router.get("/meals", handler: loadHandler)
	```

4. Implement the loadHandler function to the `Sources/Application/Application.swift` file:  
Add the following as another function in the `App` class:  

	```swift
	    func loadHandler(completion: ([Meal]?, RequestError?) -> Void ) {
		    let meals: [Meal] = self.mealStore.map({ $0.value })
	       completion(meals, nil)
	    }
	```

	This has implemented a REST API for `GET` requests on `/meals` that responds with an array of `Meals` or an error.

5. Run the project.  
Press the Run button or use the `⌘+R` key shortcut. If prompted, select "Allow incoming network connections".

6. Test the REST API. 
As this is a GET request, which matches how web browsers request data from a server, you can test the REST API using the following URL:  

	* `GET /meals`:	[http://localhost:8080/meals]()

	This should return an empty array, eg. `[]` as there are no Meal objects currently stored in the mealStore. 

7. Try out the FoodTrackerBackend Tutorial.  
A full tutorial called [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend) is available that helps you build a set of REST APIs for storing, fetching and deleting Meal objects from an iOS application, including storing the data in a database.

##Mobile SDK
The REST APIs built using the Kitura server are standard web APIs, usable from any application regardless of client library used or which language the client is written in. This means that you can use Alamofire, RestKit or URLSession to make connections to the server. Kitura also provides a bespoke, optimized client connector in order to simplify calling its REST APIs from iOS, in the form of KituraKit. 

KituraKit provides a mirror image of the router handler APIs used in Kitura, making it possible to share Swift types between the client and the server with little effort, and removes the need to explicitly carry out JSON encoding and decoding of the data being sent to or received from the server.

The following steps show how to install KituraKit into your iOS application, and use it to call the `GET /meals` REST API created using Kitura in the steps above:

1. Create a Podfile in the root of your iOS application directory if you do not have one already:

	```
	pod init
	```

2. Edit the Podfile to set a global platform of iOS 11 for your project by replacing the following line

	```
	# platform :ios, '9.0'
	```
	
	with
	
	```
	platform :ios, '11.0'
	```

3. Add KituraKit to the Podfile by adding the following under `# Pods for <application name>`

	```
	pod 'KituraKit', :git => 'https://github.com/IBM-Swift/KituraKit.git', :branch => 'pod'
	
	```

4. Save the Podfile and install KituraKit using the following on the command line:

	```
	pod install
	```

5. Reopen the application in Xcode by opening the workspace (not project!)

You can now add the same Meal definition to your iOS application as used in the Kitura server:

```swift
struct Meal: Codable {    
    var name: String
    var photo: Data
    var rating: Int
}
```

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

This calls the Kitura server using `client.get("/meals")` with a callback that matches the parameters from Kitura's completion handler.

A full tutorial called [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend) is available that helps you build a set of REST APIs for storing, fetching and deleting Meal objects from an iOS application, including storing the data in a database.

##Deploying the Kitura Application
Now that you have a Kitura server providing a REST API that is being called by your iOS application, you are ready to deploy your server to IBM Cloud. This can be done using Containers with Kubernetes, Secure Containers, or Cloud Foundry.
