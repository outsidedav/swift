---

copyright:
  years: 2018
lastupdated: "2018-06-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}

# iOS 及 Cloud Logic 使用案例
{: #sample_cloud}

透過 [BluePic 照片及影像共用範例應用程式](https://github.com/IBM/BluePic)，可看到使用 Backend For Frontend (BFF) 的 iOS 應用程式範例。BluePic 應用程式使用下列技術：

* Object Storage 及 Cloudant，儲存影像資料。
* Watson Visual Recognition 及 IBM Weather Company 服務，新增其他資訊至影像。
* Kitura 及 {{site.data.keyword.openwhisk}}，提供後端邏輯，並控制鑑別與推送通知，這全都以 Swift 撰寫。

![BluePic](images/cloudlogic.png "BluePic 流程")

在 BluePic 範例中，當張貼影像時，其資料會記錄在 Cloudant 中，而影像二進位則儲存在「物件儲存體」。從那裡，呼叫 {{site.data.keyword.openwhisk_short}} 序列，並導致根據影像的上傳位置來計算天氣資料，例如，溫度及目前狀況（例如，晴天、陰天）。{{site.data.keyword.openwhisk_short}} 序列中也會使用 Watson Visual Recognition，根據影像的內容來分析影像並擷取文字標記。最後，傳送推送通知給使用者，通知使用者已處理影像，且現在已包含天氣及標記資料。
