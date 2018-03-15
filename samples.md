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

Check out our featured sample apps for the Swift programming language.

## Share photos and images with BluePic sample app
{: #bluepic}

[BluePic](https://github.com/IBM/BluePic) is a photo and image sharing sample application that enables you to take photos and share them with other BluePic users. This sample application demonstrates how to leverage, in a mobile iOS 10 application, a Kitura-based server application written in Swift.

BluePic takes advantage of Swift in a typical iOS client setting, but also on the server-side by using the new Swift web framework and HTTP Server, Kitura. An interesting feature of Bluepic, is the way it handles photos on the server. When an image is posted, its data is recorded in {{site.data.keyword.cloudant}} and the image binary is stored in {{site.data.keyword.cos_short}}. From there, a {{site.data.keyword.openwhisk_short}} sequence is invoked causing weather data like temperature and current condition, for example sunny, cloudy, and so on, to be calculated based on the location an image was uploaded from. {{site.data.keyword.visualrecognitionshort}} is also used in the {{site.data.keyword.openwhisk_short}} sequence to analyze the image and extract text tags based on the content of the image. A push notification is finally sent to the user, informing them their image has been processed and now includes weather and tag data.

## Learn more about Kitura 2 with the TodoList sample app
{: #todolist}

[ToDo List](https://github.com/IBM-Swift/iOSSampleKituraKit) demonstrates how Kitura 2s new Codable Routing features can help developers leverage Swift 4's Codable protocol and write code that works on both the front and back-end. A new client library is used in the app called [KituraKit](https://github.com/IBM-Swift/KituraBuddy), which is open source so feel free to contribute!

It is built against a set of [ToDoBackend tests](http://www.todobackend.com/) to showcase the power of Kitura 2 and its API.

You can import KituraKit into your own iOS project by following the [KituraKit Usage](https://github.com/IBM-Swift/KituraKit/blob/master/README.md).

To view this sample app with an SQL database connected, see the [persistentiOSKituraKit branch](https://github.com/IBM-Swift/iOSSampleKituraKit/tree/persistentiOSKituraKit).

## Swift enterprise demo
{: #swift-enterprise-demo}

The Swift-Enterprise-Demo sample app is designed to highlight new enterprise capabilities that you can leverage when you deploy your Swift applications to {{site.data.keyword.cloud_notm}}. Specifically, this application showcases the following {{site.data.keyword.cloud_notm}} services and new libraries for the Swift language:

- [{{site.data.keyword.autoscaling}}](/docs/services/Auto-Scaling/index.html)
- [{{site.data.keyword.alertnotificationshort}}](/docs/services/AlertNotification/index.html#alert_gettingstarted)
- [Circuit Breaker](https://github.com/IBM-Swift/CircuitBreaker)
- [SwiftMetrics](https://github.com/RuntimeTools/SwiftMetrics)

By using Swift-Enterprise-Demo you can see how the application can scale in and out according to rules defined in the {{site.data.keyword.autoscaling}} service, receive alerts when important events occur in the application, and see how the Circuit Breaker pattern prevents the application from executing actions that are bound to fail.

The browser-based component of this application provides UI widgets that you can use to trigger actions that will cause stress on the server component of the application. These actions can increase or decrease the memory usage, increase or decrease the HTTP response time by adding or removing a delay, and increase or decrease the number of HTTP requests per second.
