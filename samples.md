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

# Sample Apps
{: #samples}

## BluePic
{: #bluepic}

[BluePic](https://github.com/IBM/BluePic) is a photo and image sharing sample application that allows you to take photos and share them with other BluePic users. This sample application demonstrates how to leverage, in a mobile iOS 10 application, a Kitura-based server application written in Swift.

BluePic takes advantage of Swift in a typical iOS client setting, but also on the server-side using the new Swift web framework and HTTP Server, Kitura. An interesting feature of Bluepic, is the way it handles photos on the server. When an image is posted, it's data is recorded in Cloudant and the image binary is stored in Object Storage. From there, a Cloud Functions sequence is invoked causing weather data like temperature and current condition (e.g. sunny, cloudy, etc.) to be calculated based on the location an image was uploaded from. Watson Visual Recognition is also used in the Cloud Functions sequence to analyze the image and extract text tags based on the content of the image. A push notification is finally sent to the user, informing them their image has been processed and now includes weather and tag data.

## TodoList
{: #todolist}

[ToDo List](https://github.com/IBM-Swift/iOSSampleKituraKit) demonstrates how Kitura 2s new Codable Routing features can help developers leverage Swift 4's Codable protocol and write code that works on both the front and back end. A new client library is used in the app called [KituraKit](https://github.com/IBM-Swift/KituraBuddy), which is open source so feel free to contribute!

It is built against a set of [ToDoBackend tests](http://www.todobackend.com/) to showcase the power of Kitura 2 and its API.

If you would like to import KituraKit into your own iOS project please see [KituraKit Usage](https://github.com/IBM-Swift/KituraKit/blob/master/README.md).

If you would like to view this sample app with an SQL database connected view the [persistentiOSKituraKit branch](https://github.com/IBM-Swift/iOSSampleKituraKit/tree/persistentiOSKituraKit).

## Swift Enterprise Demo
{: #swift-enterprise-demo}

Swift-Enterprise-Demo is designed to highlight new enterprise capabilities that you can leverage when you deploy your Swift applications to IBM Cloud. Specifically, this application showcases the following IBM Cloud services and new libraries for the Swift language:

- Auto Scaling
- Alert Notification
- Circuit Breaker
- SwiftMetrics

Using Swift-Enterprise-Demo you can see how the application can scale in and out according to rules defined in the Auto Scaling service, receive alerts when important events occur in the application, and see how the Circuit Breaker pattern prevents the application from executing actions that are bound to fail.

The browser-based component of this application provides UI widgets that you can use to trigger actions that will cause stress on the server component of the application. These actions can increase or decrease the memory usage, increase or decrease the HTTP response time by adding or removing a delay, and increase or decrease the number of HTTP requests per second.
