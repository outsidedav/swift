---

copyright:
  years: 2018
lastupdated: "2018-02-015"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}

# Server versus serverless
{: #serverless}

What is serverless? The serverless development pattern refers to application development where server-side logic is run in stateless containers that are event-triggered, ephemeral (lasting for a single execution), and fully managed by a third party. This paradigm, also known as Functions as a Service (FaaS), is where the developer provides a stateless function that can be triggered and executed without explicitly provisioning or managing a server.

By abstracting away the infrastructure and frameworks necessary for server-side development, serverless architecture allows developers to focus on building their application and writing code required to run reactively to change data. This paradigm is  particularly valuable within mobile development.

IBM's FaaS offering, [{{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/), strives to provide a seamless server-side development experience without needing any specialized server-side knowledge. This allows you to quickly develop scalable backend solutions to meet practically any demand in workload without the need to provision resources ahead of time. For applications that have unpredictable load patterns or high server down time, {{site.data.keyword.openwhisk_short}} can be an excellent cloud solution for improved performance while its "pay for what you use" system can help decrease costs.

### Architectural Changes
{: #comparison}

To help you understand the architectural benefits of switching to FaaS, let's compare a traditional and a FaaS architecture for a simple iOS application linked to a database.

In a more traditional architecture, your iOS application will likely offload network intensive tasks or process data remotely on a centralized server, which itself is connected to its own services or storage options.

With this system, the heavy lifting is placed on a singular server that will handle any authentication or processing intensive tasks to minimize stress on the client and provide synchronization across its user base.

A serverless architecture might alter this structure to look more like the following image.

![](./images/Architecture.png) Figure 1. Serverless architecture

Rather than handling all of the processing and authentication logic inside a single server, a serverless architecture leverages highly scalable functions that encapsulate much of the server-side logic while moving some logic to the client and external services.

When looking at the schematic, we see:

1. The client authenticates against an Identity Provider such as AppID.
2. Calls to the FaaS backend API including the access token.
3. The backend is implementated with {{site.data.keyword.openwhisk_short}}. The serverless actions, exposed as web actions, expect the token to be sent in the request headers and verify its validity (signature and expiration date) before allowing access to the actual API.
4. When the client submits data, the feedback is stored in a {{site.data.keyword.cloudant_short_notm}}.
5. The feedback text is processed with {{site.data.keyword.toneanalyzershort}}.
6. Based on the analysis result, a notification is sent back to the client by {{site.data.keyword.mobilepushshort}}.
7. The client receives the notification.

When using a purely serverless model, the client often is required to take on additional responsibilities because of the inability to store the user state. For instance, in this case, authorization is handled by the client and the {{site.data.keyword.appid_short_notm}} identity provider service.

While serverless architectures are not always ideal, they can provide substantial benefits under the right team and usage conditions. Check out some specific examples to learn about a few of the most common [use cases](#use_cases).

## Benefits of serverless
{: #benefits}

### Reduced Cost

Outsourcing the time and monetary cost associated with system administration reduces overall costs associated with traditional backend servers. Additionally, {{site.data.keyword.openwhisk_short}} is different from traditional computing technologies because you only pay for the time your code takes to satisfy requests, rounded up to the nearest 100ms. This means that you might be able to see considerable cost savings relative to other technologies like VMs and containers, which are not likely to be 100% utilized and take up memory on your cloud provider's system.

### High availability and scalability

Serverless architectures inherently provide instant scalability with near constant availability.

### Speed and simplify development

By eliminating the need for system administration and providing simple interfaces for deployment, the serverless paradigm accelerates application development. This enables developers to quickly build apps with action sequences that execute in response to our event-driven world.

## Example use cases
{: #use_cases}

### Mobile Backend
![](./images/cloud-functions-rest-api-trigger.png)

Mobile developers can easily access server-side logic and outsource computationally intensive tasks to a scalable cloud platform. You can implement functions in languages like Swift and easily consume server-side functions by using our iOS SDK without any server-side experience needed.

### Data Processing

![](./images/cloud-functions-cloudant-trigger.png)

You can execute code whenever data is updated in your datastore through built-in triggers. You can also easily automate processes like audio normalization, image rotation, sharpening, noise reduction, thumbnail generation, or video transcoding through a functional server-side programming model.

### Cognitive Data Processing

You can analyze data as soon as it becomes available. Let your function make use of powerful cognitive services like IBM Watson to detect objects or people appearing in images or videos.

### Scheduled Tasks

Execute your functions periodically and define schedules following a cron-like syntax to specify when actions are supposed to be executed.

## API Reference
{: #openwhisk_start_api notoc}

<!-- * [REST API Documentation](./openwhisk_reference.html#openwhisk_ref_restapi)-->
* [REST API](https://console.{DomainName}/apidocs/98)

## Related Links
{: #general notoc}

* [Discover: {{site.data.keyword.openwhisk_short}}](http://www.ibm.com/cloud-computing/bluemix/openwhisk/)
<!-- redirects to link above * [{{site.data.keyword.openwhisk_short}} on IBM developerWorks](https://developer.ibm.com/openwhisk/)-->
* [Apache OpenWhisk project website](http://openwhisk.org)
* [More on Serverless](https://martinfowler.com/articles/serverless.html)
