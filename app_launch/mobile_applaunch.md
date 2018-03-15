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

**modified**

# Manage app feature release
{: #mobile_applaunch}

{{site.data.keyword.engage_full}} lets developers build engaging apps by controlling reach and roll out of App features while measuring the defined metrics. The service helps developers to remove the coupling that exists today between App feature rollout and App updates to production. You can now publish features without exposing it to Production. This enables the developers to gradually release new versions of an app in a controlled manner. With the App Launch service, app owners have full control over feature rollout for a targeted segment.

The App Launch service lets you define a feature, create audiences based on device platforms, create custom audience attributes, and finally define an engagement that choreographs the timing and placement of the feature. Once you have used the SDKs along with the feature and metrics attributes that are incorporated in the application, the service starts measuring audience experiences. You can now leverage your app based on this information to create customized customer engagements across various categories of your app users.

The salient features of the App Launch services are:

 - **Accelerate feature deployment**

    Accelerate delivery of features to your app through controlled release by mitigating risks. Release features to subset of audience segments and make larger rollout or roll back decisions based on real time feedback. Separate feature rollouts from regular release cycles.

 - **Segment Audience**

    User segments can be defined based on demographic, contextual and behavioral attributes. Alternatively features can be rolled out to certain percentage of the entire user base. Key Performance indicators can be defined for each feature and client side code instrumented to measure the results.

 - **Adapt application based on context**

    Application behavior, user interface, notifications can be customized for specific audience segments. For example, app background can be changed based on user location. This user personalization results in higher engagement of users with the application.

 - **A / B test features**

    Gain confidence by experimentation. Two variants of application features can be compared against each other by rolling out both at the same time. Decisions can be made based on hard data.

 - **Increase Customer Engagement**

    Promote user engagement. Notifications can be targeted to all application users or to a specific set of users and devices. A message schedule can be defined. User interaction plays a vital role in customer relationships.


The following image gives you an overview of the {{site.data.keyword.engage_short}} service life cycle.

![Cognitive Engage overview](images/process_app_launch.png)

## Getting started with the service
{: #getting-started-app-launch}

The below steps will help you get started with the service.

## Before you begin

First, be sure you have the following prerequisites ready to go:

 - iOS 10+
 - Xcode 9
 - Swift 3.2 - 4
 - Cocoapods or Carthage

## Creating an instance of {{site.data.keyword.engage_short}}
{: ##app_launch_create}

1. In the {{site.data.keyword.cloud_notm}} catalog, click **Mobile** > **App Launch**. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Click Create.
4. In the navigation pane, Click on **Connections** to select an app and bind it to your service. You can leave always bind the service instance to your app later if you leave it unbound during creation.

## Initializing your app
{: #prerequisites_app}
The service provides Platform specific SDKs to simplify application development.

The IBM Cloud Mobile Services Swift SDKs can be installed with either Cocoapods or Carthage.

1. Click **Settings**.
2. Install the [SDK](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-applaunch) . Refer the 'README' document in the SDK for detailed information on installation and technical concepts.
3. Copy the configuration keys to initialize your App. Use the App Secret, App GUID, and Client Secret to configure your app and create engagements.

![SDK and Keys](images/engagement_settings.gif)

##Pre-requisites

###Creating a feature
{: #prerequisites_2}

The {{site.data.keyword.engage_short}} service allows you to create and test responses to features.

To create a feature, complete the following steps: :

![Feature Details](images/feature_creation_animated.gif)

1. In the navigation pane, click **Features** > **Create New Feature**

2. Update the Create New Feature and Metrics form with an appropriate feature name and description. You can also define the feature properties and add metrics to measure the impact of your engagement. Click **Bulk edit** to add multiple properties by editing the JSON.

3. Click **Create**. The new feature now appears on the Features panel.

4. Enable the feature once it is developed.

5. To enable a feature to be used as an engagement, click the Feature that you have created.

6. In the Feature Details window, choose to Update Status of your feature to **Ready**.

7. Click **Update** Status.

8. Update your app to include the newly created attributes and feature codes in your iOS App.

9. The feature is now ready to be used.

The Feature Details window has an option to export the feature as a JSON file which can be used in the client application to load the default values.

###Creating an audience
{: #prerequisites_2}

To create an audience, complete the following steps:

![Create Audience](images/create_audience_animated.gif)

1. Create an **audience attribute**.

	Click **Audience** > **Create Attribute**.

	Provide the following values:

	- **Name**: Provide an appropriate name for the attribute.
	- **Description**: A brief description on the attribute.
	- **Type**:	Choose the attribute type.
	- **Allowed values**: Enter the attribute values that you would want to use.

    You can choose to create multiple audience attributes, as listed in the following image, based on your requirement.


2. Create an **audience**.

	a. Click **Create Audience**.

	b. Provide an appropriate name and description on the New Audience window.

	c. Select an attribute, and click **Add**.

    d. Choose the required options from the listed attributes.

	e. Click **Save**.


You can now create an Engagement.

## Create an Engagement

An **Engagement** is an instantiation of a Feature with properties initialized and attaching one of the pre-defined audiences.

You can either create an engagement using **Feature Control** or **In-App Messaging**.

###Enabling Feature Control Capability

Through this engagement an app owner can control the visibility of a feature by enabling or disabling it during runtime.

A feature can be enabled/disabled to all the application users or to a specific set of users and devices.

Feature roll outs can be scheduled and co-ordinated by defining a start/end time and date. You can also choose a specific day on which a defined feature should be enabled/disabled.

Complete the following steps to create an engagement using the Feature Control:

![animated gif](images/create_engagement.gif)

1. You can create an engagement using either of the following methods:
	- Click **Engagements** in the navigation pane.
	- Select **Create Engagements** on the new Feature that you have created.
	- In the navigation pane, click **Overview** > **Create New Engagement** .

2. The New Engagement window appears.

3. Provide a name and description to your new engagement. Ensure that you give a unique engagement name, and not one that is already listed in Engagements.

     **Select Engagement type** as **Feature Control**

	To do a controlled experiment with multiple variants of the feature, select **A/B testing** on the **Select Experimentation Type**. Click **Next**.

4. Choose the Feature that you have created. You can also choose to add and define the variants that you might want to experiment with. Click **Next**.

5. Select an audience. Click **Next**.

6. Define a trigger by choosing Time and the **Start** date/time and an **End** date/time. Click **Save**.

7. The new engagement now appears in the Engagement Details window.

You can now measure the [performance](https://console.bluemix.net/docs/services/app-launch/app_measure_performance.html#applaunch_type) of your engagement.

###Enabling In-App Messaging Capability

Through this engagement an app owner can send notifications to the app users while they are actively using the application.

Messages can be targeted to all application users or to a specific set of users and devices. For every message that you submit to the service, the intended audience receives a notification.

In-app messages can be scheduled by defining a start/end date and time. You may also schedule it based on an event.

These messages are more cutomized as they are based on analytics insights about the user's choice, user's interactions, device, application logs etc.

In-app messages can be used to:

- Send customized messages
- Send messages to users who have turned off push notifications
- Request a feedback or engage users in a conversation
- Send relevant messages after knowing what the user is looking for
- Engage active and loyal customers
- Inform users on the app updates or launch of a new feature
and so on.

Complete the following steps to create an engagement using the Messaging option:

![animated gif](images/in-app-engagement_animated.gif)

1. You can create an engagement using either of the following methods:
	- Click **Engagements** in the navigation pane.
	- Select **Create Engagement** on the new Feature that you have created.
	- In the navigation pane, click **Overview** > **Create New Engagement**.

2. The New Engagement window appears.

3. Provide a name and description to your new engagement. Ensure that you give a unique engagement name and not one that is already listed in Engagements.

    **Select Engagement type** as **In-App Messaging**

	To do a controlled experiment with multiple variants of the messaging feature, select **A/B testing** on the **Select Experimentation Type**. Click **Next**.

4. Fill in the message properties and click **Next**.

5. **Select Audience** and the percentage of audience you want to reach out to. Click **Next**.

6. Define a trigger by selecting a **Start/End Date and Time**.

    **Select Event.**  Click **Next**.

7. Map the elements to the metrics you want to measure through them. Select the element and fill in the metric details. Click **Save**.

The new engagement now appears in the Engagement Details window.

You can now measure the [performance](https://console.bluemix.net/docs/services/app-launch/app_measure_performance.html#applaunch_type) of your engagement.

##Quick Links

Below are some quick links to gain more insight and understand the features of App launch:

 - Try out the [Service](https://console.bluemix.net/catalog/services/app-launch)
 - [Blogs and Videos](https://console.bluemix.net/docs/services/app-launch/relatedlinks.html#blogs-and-videos)
 - For more information, make sure to look through our [documentation](https://console.bluemix.net/docs/services/app-launch/index.html#gettingstartedtemplate)
