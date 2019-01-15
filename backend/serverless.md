---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip}

# Serverless development
{: #serverless}

What is serverless? The serverless development pattern refers to application development where server-side logic is run in stateless containers. The containers are event-triggered, ephemeral (lasting for a single execution), and fully managed by a third party. This paradigm, also known as Functions as a Service (FaaS), is where the Developer provides a stateless function that can be triggered and run without explicitly creating or managing a server.

By abstracting away the infrastructure and frameworks necessary for server-side development, serverless architecture allows Developers to focus on writing code to run reactively to change data.

IBM's FaaS offering, [{{site.data.keyword.openwhisk}}](https://cloud.ibm.com/openwhisk/), strives to provide a simple, server-side development experience without needing any specialized server-side knowledge. By using serverless technology, you can quickly develop extensible backend solutions to meet practically any workload demand without the need to create resources ahead of time. For applications that have unpredictable load patterns or high server downtime, {{site.data.keyword.openwhisk_short}} can be an excellent cloud solution with improved performance, and its "pay for what you use" system helps to reduce costs.

## Architectural Changes
{: #comparison}

To help you understand the architectural benefits of switching to FaaS, a traditional and FaaS architectures are compared by using a simple iOS application that is linked to a database.

In a more traditional architecture, the iOS application offloads network intensive tasks, or processes data remotely on a centralized server, by which itself is connected to by its own services or storage options. The heavy lifting is placed on a singular server that processes many intensive tasks to minimize stress on the client, and provide synchronization across its user base.

A serverless architecture can alter this structure to look more like the following image.

![](./images/Architecture.png) Figure 1. Serverless architecture

Rather than handle all of the processing and authentication logic inside a single server, a serverless architecture uses functions that encapsulate much of the server-side logic, and offloads some logic to the client (and external services).

Looking at the schematic, you can see the following points:

1. The client authenticates against an Identity Provider such as App ID.
2. Calls to the FaaS backend API including the access token.
3. The backend is implemented with {{site.data.keyword.openwhisk_short}}. The serverless actions are exposed as web actions, and expect tokens to be sent in the request header for verification (signature and expiry date) to provide access to the actual API.
4. When the client submits data, the feedback is stored in a {{site.data.keyword.cloudant_short_notm}}.
5. The feedback text is processed with {{site.data.keyword.toneanalyzershort}}.
6. Based on the analysis result, a notification is sent back to the client by {{site.data.keyword.mobilepushshort}}.
7. The client receives the notification.

In a purely serverless model, the client often takes on extra responsibilities because of the inability to store the user state. Authorization is handled by the client and the {{site.data.keyword.appid_short_notm}} identity provider service.

While serverless architectures aren't always ideal, they can provide profound benefits under the right team and usage conditions. Check out some specific examples to learn about a few of the most common [use cases](#use_cases).

## Serverless benefits
{: #benefits}

### Reduced Cost

Outsourcing the time and monetary cost that is associated with system administration, reduces the overall cost that is associated with traditional backend servers. Additionally, {{site.data.keyword.openwhisk_short}} is different from traditional computing technologies because you pay only for the time your code takes to satisfy requests, rounded up to the nearest 100 ms. Considerable cost savings are possible when you compare to other technologies like VMs and containers, which aren't likely to be 100% used, and take up memory on your cloud provider's system.

### High availability and scalability

Serverless architectures inherently provide instant scalability with near constant availability.

### Speed and simplified development

By eliminating the need for system administration, and providing simple interfaces for deployment, the serverless paradigm accelerates application development. Developers can quickly build apps with action sequences that execute in response to an event-driven world.

## Example use cases
{: #use_cases}

### Mobile Backend
![](./images/cloud-functions-rest-api-trigger.png)

Mobile Developers can easily access server-side logic and outsource computationally intensive tasks to a cloud platform. You can implement functions in languages like Swift and easily consume server-side functions by using the iOS SDK without any server-side experience needed.

### Data Processing

![](./images/cloud-functions-cloudant-trigger.png)

You can execute code whenever data is updated in your data store through built-in triggers. You can also easily automate processes like audio normalization, image rotation, sharpening, noise reduction, thumbnail generation, or video transcoding through a functional server-side programming model.

### Cognitive Data Processing

You can analyze data as soon as it becomes available. Your function can take advantage of powerful cognitive services, like IBM Watson, to detect objects or people in images or videos.

### Scheduled Tasks

Execute your functions periodically, and define schedules that follow a cron-like syntax to specify when actions are to be run.

## API Reference
{: #openwhisk_start_api notoc}

<!-- * [REST API Documentation](./openwhisk_reference.html#openwhisk_ref_restapi)-->
* [REST API](https://cloud.ibm.com/apidocs)

## Related Links
{: #general notoc}

* [Discover {{site.data.keyword.openwhisk_short}}](http://www.ibm.com/cloud-computing/bluemix/openwhisk/)
<!-- redirects to link above * [{{site.data.keyword.openwhisk_short}} on IBM developerWorks](https://developer.ibm.com/openwhisk/)-->
* [Apache OpenWhisk project website](http://openwhisk.org)
* [More on Serverless](https://martinfowler.com/articles/serverless.html)
