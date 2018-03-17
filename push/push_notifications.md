---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Sending {{site.data.keyword.mobilepushshort}}
{: #push_notifications}

Enhance your app capability by using {{site.data.keyword.mobilepushshort}} service on {{site.data.keyword.cloud}}. This service enables you to send real-time notifications to mobile devices and web applications.

 - Notifications can be delivered to either all application users, or to a selected set of users or devices.
 - Supports both interactive and silent notifications.
 - Customers can choose to subscribe to specific tags or topics for notification.
 - Enables the app owner to analyze the number of devices that are registered to receive notifications and the number of notifications sent.

You can choose to use the {{site.data.keyword.mobilepushshort}} service either as a part of MobileFirst Services Starter Boilerplate or as {{site.data.keyword.cloud_notm}} [Dedicated Services](/docs/dedicated/index.html).  You can also use an SDK (software development kit) and [REST APIs ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mobile.{DomainName}/imfpush/){: new_window} to further develop your client applications.

![Push Overview](images/push_notification_lifecycle.jpg) Figure 1. Overview of the {{site.data.keyword.mobilepushshort}} service life cycle

## Before you begin

First, be sure you have the following prerequisites ready to go:

 - iOS 8.0+
 - Xcode 7.3, 8.0
 - Swift 2.3 - 4.0
 - Cocoapods or Carthage

## Step 1. Creating an instance of {{site.data.keyword.mobilepushshort}}
{: #push_create}

1. In the {{site.data.keyword.cloud_notm}} catalog, click **Mobile** > **{{site.data.keyword.mobilepushshort}}**. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Click **Create**.
4. In the navigation pane, Click **Connections** to select an app and bind it to your service. You can bind the service instance to your app later if you leave it unbound during creation.


## Step 2. Obtain your notification provider credentials

To set up Push Notifications service, you need to obtain the required credentials from the Apple Push Notification Service (APNs). Follow the steps here to [obtain and configure your APNs credentials ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/mobilepush/push_step_1.html#push_step_1_ios){: new_window}.


## Step 3. Configure a service instance
{: #enable-push-ios-notifications}

To use the {{site.data.keyword.mobilepushshort}} service to send notifications, upload the `.p12` certificates that you created. This certificate contains the private key and SSL certificates that are required to build and publish your application. You can also use the REST API to upload an APNs certificate.

After the `.cer` file is in your key chain access, export it to your computer to create a `.p12` certificate.

For more information about using the APNs, see [iOS Developer Library: Local and Push Notification Programming Guide ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1){: new_window}.

To set up APNs on the Push Notification services console, complete the steps:

1. Select **Configure** on the {{site.data.keyword.mobilepushshort}} services console.
2. Choose the **Mobile** option to update the information in the **APNs Push Credentials** form.
3. Choose either of the following options:
	- For **Mobile** option
		1. Select **Sandbox** (development) or **Production** (distribution) as appropriate and then upload the `p.12` certificate that you have created.
		  ![Set {{site.data.keyword.mobilepushshort}} console](images/wizard.jpg)

		1. In the **Password** field, enter the password that is associated with the `.p12` certificate file, then click **Save**.
	- For **Web** option
		- In the Safari Push section, update the form with the required information.
		- **Website Name**: This is the name that you have provided in the Notification center.
		- **Website Push ID**: Update with the reverse-domain string for your Website Push ID. For example, web.com.acmebanks.www.
		- **Website URL**: Provide the URL of the website that should be subscribed to push notifications. For example, https://www.acmebanks.com.
		- **Allowed Domains**: This is optional parameter. This is the list of websites that requests permission from the user. Ensure that the URLs are comma separated values. Note that the value in Website URL will be used if this is not provided.
		- **URL Format String**: The URL to resolve when the notification is clicked. For example, ["https://www.acmebanks.com"]. Ensure that the URL use the http or https scheme.
		-**Safari web push certificate**: Upload the .p12 certificate and provide the password.
4. Click **Save**.
	![{{site.data.keyword.mobilepushshort}} console](images/push_configure_safari.jpg)


## Step 4. Set up service client SDK

To enable iOS applications to receive push notifications to your devices, you need to configure the iOS SDK for the {{site.data.keyword.mobilepushshort}} service.

The IBM Cloud Mobile Services Swift SDKs can be installed with either Cocoapods or Carthage. For more information on installation, see [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application).


## Step 5. Sending a notification

After you have developed your applications, you can send basic push notifications.

To send basic push notifications, complete the following steps:

1. Select **Send Notifications**, and compose a message by choosing a **Send to** option. The supported options are **Device by Tag**, **Device Id**, **User Id**, **iOS devices**, **Web Notifications**, and **All Devices**.
**Note**: When you select the **All Devices** option, all devices subscribed to {{site.data.keyword.mobilepushshort}} will receive notifications.

	![Notifications screen](images/tag_notification.jpg)

2. In the **Message** field, compose your message. Choose to configure the optional settings as required.
3. Click **Send**.
3. Verify that your devices or browser has received the notification.

The following screen shot shows an alert box handling a push notification in the foreground on the device.
	![Foreground push notification on Android](images/Android_Screenshot.jpg)

The following screen shot shows a push notification in the background.
	![Background push notification on Android](images/background.png)

### Optional settings
{: #push_step_4_ios}

You can customize the {{site.data.keyword.mobilepushshort}} settings for sending notifications to iOS devices. The following optional customization options are supported.

- **Badge**:  Indicates the number that is displayed on the application badge. The default value is zero (0), and this would not display a badge.
- **Sound**: Indicates a sound clip to be played on the receipt of a notification. Supports default or the name of a sound resource bundled in the app.
- **Additional payload**: Specifies the custom payload values for your notifications.

You can also choose to enable [interactive notifications](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#interactive-notifications) and [rich media notifications](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enabling-rich-media-notifications).

## Step 6. Monitoring for delivered notifications
{: #push_step_4_monitor}

The {{site.data.keyword.mobilepushshort}} service provides a monitoring utility to help you check the status of messages that are sent. To configure your monitoring utility, see [Enable monitoring for iOS applications](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enable-monitoring).

## Next steps

 - To learn more about the service and take advantage of all of the features, read through our [documentation](/docs/services/mobilepush/c_overview_push.html#overview-push).

 - For an introduction to working with Mobile services and {{site.data.keyword.Bluemix_notm}}, see [Getting started with Mobile apps on IBM Cloud](/docs/services/mobile/index.html).

 - Starter Kits are one of the fastest way to leverage the capabilities of IBM Cloud. View our available starter kits in the [Mobile developer dashboard](https://console.bluemix.net/developer/mobile/dashboard). Download the code. Run the App!

 - You can use the [Swagger UI](https://mobile.ng.bluemix.net/imfpush/) to quickly review REST API documentation.
