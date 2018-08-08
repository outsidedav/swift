---

copyright:
  years: 2017-2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Storing documents in {{site.data.keyword.cloud_notm}}
{: #cloudant}

{{site.data.keyword.cloudantfull}} is a document-oriented DataBase as a Service (DBaaS). It stores data as documents in JSON format. It is built with scalability, high availability, and durability in mind, and is easy to configure for use in Swift applications. It comes with a wide variety of indexing options that include MapReduce, {{site.data.keyword.cloudant_short_notm}} Query, full-text indexing, and geospatial indexing. The replication capabilities make it easy to keep data in sync between database clusters, desktop PCs, and mobile devices. 
{:shortdesc}

For all of the ways that you can use {{site.data.keyword.cloudant_short_notm}}, see [{{site.data.keyword.cloudant_short_notm}} Basics](/docs/services/Cloudant/basics/index.html#cloudant-nosql-db-basics).

## Before you begin

First, be sure that you have the following prerequisites ready to go:
 * CocoaPods (version 1.1.0 or higher
 * iOS (version 9 or higher)
 * MacOS (version 10.11.5 or higher)
 * Xcode (version 9.0.1 or higher)

The [{{site.data.keyword.cloudant_short_notm}} SDK for Swift![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudant/swift-cloudant) is built with Swift 3.2. If you are planning to use {{site.data.keyword.cloudant_short_notm}} with Kitura, check out [Kitura-CouchDB Library ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Swift/Kitura-CouchDB), which is built with Swift 4.0.
{: tip}

## Step 1. Creating an instance of {{site.data.keyword.cloudant_short_notm}}

See [Creating a Cloudant NoSQL DB instance on IBM Cloud tutorial ![External link icon](../images/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/Cloudant/tutorials/create_service.html#creating-a-cloudant-nosql-db-instance-on-ibm-cloud){:new_window} to provision an instance of the service.


## Step 2. Installing the SDK

### Installing the iOS Swift SDK

Use the Swift Cloudant SDK to help make coding your app easier. The SDK must be installed in your app code.

1. Open your existing Xcode project directory to the `Podfile`.
2. Under your projects target, add a dependency for the `SwiftCloudant` pod. Be sure the `use_frameworks!` command is also under your target as shown in the following example.
    ```
    target '<yourTarget>' do
      use_frameworks!
        pod 'SwiftCloudant', :git => 'https://github.com/cloudant/swift-cloudant.git'
    end
    ```
    {: screen}
3. Download the `SwiftCloudant` dependency.
    ```
    pod install
    ```
    {: pre}

### Installing the server-side Swift SDK

To use with the Swift Package Manager for server-side development, add the following line to your dependencies in your `Package.swift`:
```swift
.Package(url: "https://github.com/cloudant/swift-cloudant.git")
```
{: pre}

## Step 3. Initializing the SDK

After you initialize the SDK in your app, you can begin leveraging {{site.data.keyword.cloudant_short_notm}} to store data.

1.  Add the following import to your `AppDelegate.swift` or server-side swift file.
    ```
    import SwiftCloudant
    ```
    {:pre}
2. Initialize the connection to the database.
    ```swift
    let cloudantURL = NSURL(string: "https://username.cloudant.com")!
    let client = CouchDBClient(url: cloudantURL, username: "username", password: "password")
    let dbName = "database"
    ```
    {: codeblock}

### Basic Operations
These basic operations illustrate the fundamental actions to create, read, and destroy your documents by using the initialized client.

#### Create a document
```swift
let create = PutDocumentOperation(id: "doc1", body: ["hello": "world"], databaseName: dbName) {(response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while creating a document. Error: \(error)")
    } else {
        print("Created document \(response?["id"]) with revision id \(response?["rev"])")
    }
}
client.add(operation: create)
```
{: codeblock}

#### Read a document
```swift
let read = GetDocumentOperation(id: "doc1", databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while reading a document. Error: \(error)")
    } else {
        print("Read document: \(response)")
    }   
}
client.add(operation: read)
```
{: codeblock}

#### Delete a document
```swift
let delete = DeleteDocumentOperation(id: "doc1",
    revision: "1-revisionidhere",
    databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while deleting a document. Error: \(error)")
    } else {
        print("Document deleted")
    }   
}
client.add(operation: delete)
```
    {: codeblock}


## Step 4. Testing your app
{: #cloudant_testing}

Is everything set-up correctly? Test it out!

1. Run your application, making sure to invoke the initialization and respective operations, such as creating a document.
2. Return to the {{site.data.keyword.cloudant_short_notm}} service instance previously created in your web browser, and open the service dashboard.
3. Select the database that is used, and you can see the documents in the dashboard.

Having trouble? Check out the [{{site.data.keyword.cloudant_short_notm}} API Reference](/docs/services/Cloudant/api/index.html#api-reference-overview).


## Next steps
{: #cloudant_next notoc}

Great job! You added a level of secure persistence to your app. Keep the momentum by trying one of the following options:

* View the [{{site.data.keyword.cloudant_short_notm}} SDK for Swift![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudant/swift-cloudant) source code.
* Starter Kits are one of the fastest ways to leverage the capabilities of IBM Cloud. The **Infinite Scrolling with Cloudant NoSQL for iOS** starter kit illustrates how to extend a ViewController to display data by using pagination. This pattern of app is common for iOS developers, and is a great example for illustrating the capabilities of {{site.data.keyword.cloudant_short_notm}}. View the available starter kits in the [Mobile developer dashboard](https://console.bluemix.net/developer/mobile/dashboard). Download the code. Run the App!
* Learn more about and take advantage of all of the features that {{site.data.keyword.cloudant_short_notm}} has to offer, [check out the docs](/docs/services/Cloudant/index.html)!
