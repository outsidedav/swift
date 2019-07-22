---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: bluepic swift, ios bluepic, bff swift image, sample swift, swift example bff

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# iOS 和云逻辑用例
{: #sample_cloud}

您可以通过 [BluePic 图像共享样本应用程序](https://github.com/IBM/BluePic){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 来查看使用服务于前端的后端 (BFF) 的 iOS 应用程序示例。BluePic 应用程序使用了以下技术：

* Object Storage 和 Cloudant，用于存储图像数据。
* Watson Visual Recognition 和 IBM Weather Company 服务，用于向图像添加更多信息。
* Kitura 和 {{site.data.keyword.openwhisk}}，用于提供后端逻辑、控制认证和推送通知，这些内容全部用 Swift 编写。

![BluePic](images/cloudlogic.png "BluePic 流程")

在 BluePic 示例中，当发布图像时，其数据会记录在 Cloudant 中，并且图像二进制文件会存储在 Object Storage 中。在其中，将调用 {{site.data.keyword.openwhisk_short}} 序列，以根据图像上传位置来计算天气数据，例如温度和当前天气状况（例如，晴、多云）。{{site.data.keyword.openwhisk_short}} 序列中还会使用 Watson Visual Recognition 来分析图像，并基于图像的内容抽取文本标记。最后，向用户发送推送通知，通知用户图像已处理，现在包括天气和标记数据。
