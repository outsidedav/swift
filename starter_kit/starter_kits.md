---

copyright:
  years: 2018
lastupdated: "2018-05-31"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Developing with Starter Kits
{: #intro}

The {{site.data.keyword.cloud_notm}} Developer Console for Apple enables Apple developers to create apps from a variety of starter kits, provision and connect key {{site.data.keyword.cloud_notm}}-optimized services, and then quickly download working code or set up for continuous delivery. Users are able to create, view, configure, and manage your app, as well as download your app's code. Using starter kits helps you to quickly evaluate, and test {{site.data.keyword.cloud_notm}} services with a brand new app.

Ready to jump in? Visit the [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits) now to get started.
{: tip}

## What is a Starter Kit?
{: #starter_kits}

With the {{site.data.keyword.cloud_notm}} Developer Experience, you can choose from various starter kits. Starter kits instruct {{site.data.keyword.cloud_notm}} to dynamically assemble a skeleton production app, in the language of your choice, ready for cloud deployment. Each starter kit embodies a language, a framework, and a pattern for a specific real-world use case that allows reusing code rather than re-inventing it.

### Comparing Samples and Starter Kits

Developers often rely on pre-written code like snippets, demos, and samples. How are these different from {{site.data.keyword.cloud_notm}} starter kits?

* **snippet** - A few lines of code that is often presented in an IDE. Snippets help a developer integrate with a programming language syntax or support integration with a defined API.

* **demo** - Code that is usually high in quality, fidelity, and uses a range of services and integration points. It often requires setup time, and is used to prove a business problem or demonstrate a platform feature. It is used for evaluating stages of cloud adoption. Sometimes its code is included in production code.

* **sample** - Code that is a small example of a specific feature, function, service, or user journey. A sample can be reused or included in a production application. It is typically used to show technical capabilities, and a possible approach to solving a technical problem.

* **{{site.data.keyword.cloud_notm}} starter kit** - Starter Kits are a production-ready pattern, which can be integrated with a set of services to generate a production-ready asset, that can be deployed directly into a devops pipeline and a Kubernetes cluster. A Starter Kit contains descriptive metadata providing the end user with enough information to know what the kit is and does. It also contains instructions that tell {{site.data.keyword.cloud_notm}} what to produce. The output is production-ready out of the box, and can be iterated on for further enhancements, based on IBM best practices. Starter Kit content is not as complex as a demo and not as trivial as a snippet or sample. They are dynamically created based on the developer’s requirements.

Starter kits are production-ready, and focus on demonstrating a key pattern implementation by using a runtime (for example Swift + Kitura). In some cases, starter kits offer a simple user experience to highlight the integration of the service. In other cases, stater kits represent a customizable implementation of a sophisticated use case.

Starter kits contain instructions that allow {{site.data.keyword.cloud_notm}} to automatically produce scaffolded app projects with portable code, and specify resources to be auto-provisioned when you create a project from the starter kit.

## Auto-provisioned Resources
{: #auto_privisioned_resources}

If a starter kit specifies required resources, {{site.data.keyword.cloud_notm}} automatically creates instances of those resources when you create your project. Note that you can manually provision resources or select existing resource instances to augment your project after it is created. You can see a list of service instances that are associated with your project in the Project Details view, along with credentials in case you need them.

## Generating portable code
{: #portable_code}

Creating an app from a starter kit automatically produces code for you that has consistent format and is runtime agnostic. You can deploy the code in your environment of choice (for example, Kubernetes or Cloud Foundry) without making changes.

### Exactly What App Content Is Produced?

The code that isproduced from an {{site.data.keyword.cloud_notm}} starter kit has four fundamental components: use case logic, language components, service enablement, and cloud enablement. Generating these components saves you valuable time and ensures that you are using best-in-class architecture.

* **Use case logic**, as the name indicates, provides functionality for the core function of a particular use case. Examples include code that is for a Watson Conversation chat bot, or code that is for a mobile visual recognition app.
* **Language components** are code components, and files specific to the language you select for your starter kit. For example, node.js programmers need a `package.json` file for dependency management, and this file is automatically created by the {{site.data.keyword.cloud_notm}} Developer Experience.
* **Service enablement** is code that allows your app to connect to, and use the services that you added to your project. Credential management, initialization code, and service-specific SDKs are examples of service enablement items.
* **Cloud enablement** is code that enables your app to run on {{site.data.keyword.cloud_notm}}. For example, Helm charts that allow your app to run on an {{site.data.keyword.cloud_notm}} Kubernetes cluster would fall in the cloud enablement category.

The app that is produced by {{site.data.keyword.cloud_notm}} is not only architecturally proven, but also reflects best practices for the language you chose for your project. To see details on the files and structure of an app project, see the *Project contents* topic in the Reference section of this document.

The project includes a `README` file that contains technical details of the project, and explains what is needed to get your app to run if it does not run out-of-the-box.

## Using the {{site.data.keyword.cloud_notm}} Developer Console for Apple
{: #journey}

The {{site.data.keyword.cloud_notm}} Developer Console for Apple gives you a seamless path to building a Swift starter app for your specific use case. Let's look at the steps you might take in your journey.

### Overview screen
{: #overview_screen}

The Overview screen gives you content that is tailored to a set of use cases like Watson, Weather, and more. From the overview screen, you can see documentation, access educational resources, browse services, see featured starter kits, or link to a larger collection of starter kits. Click `Starter Kits` in the left hand navigation area to step into the Starter Kits view.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple Overview screen](images/overview_screen.png "Overview screen") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple Overview screen*

### Starter Kits view
{: #starter_kits_view}

The Starter Kits view shows the collection of starter kits specific to a use case area. You can click various links on a starter kit card to see demos and more information. Select a starter kit to move to the Create New Project view.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple Starter Kits view](images/starter_kits_screen.png "Starter Kits view") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple Starter Kits view*

### Create New Project view
{: #create_new_project_view}

From the **Create New Project** view, you can name your app, as well as provide deployment and routing information. On the right, you can also see the services that automatically provision when you create your app, along with pricing plans, and terms for each. Click `Create Project` to move to the Project Details view. If you are not logged in to {{site.data.keyword.cloud_notm}}, you need to do so now.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple Create New Project view](images/create_new_project_screen.png "Create New Project view") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple Create New Project view*

## Project Details view
{: #project_details_view}

The Project Details view displays list of services that are configured for your app. For each item in the list, you can see the service name, links to other information, and an **actions** button with three vertically aligned dots. The **actions** button options are to remove services from an app, open dashboard for service, and delete service. Removing a service instance only removes the association to this app, and does not delete the service instance. Also the service credentials are consolidated on this view, so you don't have to visit each individual service instance views to get them.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple Project Details view](images/project_details_screen.png "Project Details view") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple Project Details view*

The Project Details view also allows you to add new or existing services to your app that were not part of the original starter kit. Click the **Add Resource** link in the service list box to do this. The available services depend on the type of app, and the services that are available in a region, so not all services are available to associate with all apps.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple Add Resource dialog](images/add_resource_screen.png "Add Resource dialog") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple Add Resource dialog*

On the Project Details view, you can access your code in two ways:
*  Select **Download Code** to generate and download the code for your app.
*  Click the **Create Toolchain** button to push your code to a repo and deploy your app to {{site.data.keyword.cloud_notm}}. For more information about the {{site.data.keyword.cloud_notm}} devops toolchain, click [here](../services/ContinuousDelivery/index.html).

### Project List view
{: #project_list_view}

You can see a list of all the apps you have created from the Projects List view. You can rename or delete your apps from here. Click on a project name row to return to the Project Details view.

![{{site.data.keyword.cloud_notm}} Developer Console for Apple Project List view](images/project_list_screen.png "Project List view") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple Project List view*

For more information, visit the [{{site.data.keyword.cloud_notm}} Developer Console for Apple Learning Resources](https://console.bluemix.net/developer/appledevelopment/learning-resources).
{: tip}
