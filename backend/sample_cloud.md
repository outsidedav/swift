---

copyright:
  years: 2018
lastupdated: "2018-02-06"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}

# iOS & Cloud Logic Use Case
{: #sample_cloud}

An example of an iOS application using a Backend For Frontend (BFF) can be seen through the BluePic photo and image sharing sample application. BluePic utilizes the following:

* Object Storage and Cloudant to store the image data.
* Watson Visual Recognition and the IBM Weather Company service to add additional information to the images.
* Kitura and {{site.data.keyword.openwhisk}} to provide backend logic and control authentication and push notifications, which is all written in Swift.

![BluePic](images/cloudlogic.png "BluePic Flow")

In the BluePic example, when an image is posted, its data is recorded in Cloudant and the image binary is stored in Object Storage. From there, a Cloud Functions sequence is invoked and causes weather data such as temperature and current condition (e.g. sunny, cloudy, etc.) to be calculated based on the location where an image was uploaded from. Watson Visual Recognition is also used in the IBM Cloud Functions sequence to analyze the image and extract text tags based on the content of the image. A push notification is finally sent to the user, informing them their image has been processed and now includes weather and tag data.
