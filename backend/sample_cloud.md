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

# iOS and Cloud Logic Use Case
{: #sample_cloud}

An example of an iOS application that uses a Backend For Frontend (BFF) can be seen through the [BluePic image share sample application](https://github.com/IBM/BluePic). The BluePic app uses the following technologies:

* Object Storage and Cloudant to store the image data.
* Watson Visual Recognition and the IBM Weather Company service to add additional information to the images.
* Kitura and {{site.data.keyword.openwhisk}} to provide backend logic and control authentication and push notifications, which is all written in Swift.

![BluePic](images/cloudlogic.png "BluePic Flow")

In the BluePic example, when an image is posted, its data is recorded in Cloudant, and the image binary is stored in Object Storage. From there, a {{site.data.keyword.openwhisk_short}} sequence is invoked, and causes weather data such as temperature and current condition (for example, sunny, cloudy) to be calculated based on the location where an image was uploaded from. Watson Visual Recognition is also used in the {{site.data.keyword.openwhisk_short}} sequence to analyze the image and extract text tags based on the content of the image. Finally, a push notification is sent to the user, informing them that the image was processed, and now includes weather and tag data.
