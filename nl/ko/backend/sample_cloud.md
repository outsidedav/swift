---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# iOS 및 클라우드 로직 유스 케이스
{: #sample_cloud}

[BluePic 이미지 공유 샘플 애플리케이션](https://github.com/IBM/BluePic)을 통해 BFF(Backend For Frontend)를 사용하는 iOS 애플리케이션의 예를 볼 수 있습니다. BluePic 앱은 다음 기술을 사용합니다.

* 이미지 데이터를 저장할 Object Storage 및 Cloudant
* 추가 정보를 이미지에 추가할 Watson Visual Recognition 및 IBM Weather Company 서비스
* 백엔드 로직을 제공하고 인증 및 푸시 알림을 제어할 Kitura 및 {{site.data.keyword.openwhisk}}(모두 Swift로 작성됨)

![BluePic](images/cloudlogic.png "BluePic 플로우")

BluePic 예에서는 이미지가 게시되면 데이터가 Cloudant에 기록되고 이미지 바이너리가 Object Storage에 저장됩니다. 그 위치에서부터 {{site.data.keyword.openwhisk_short}} 시퀀스가 호출되고 이미지가 업로드된 위치를 기반으로 온도 및 현재 상태(예: 맑음, 흐림)와 같은 날씨 데이터를 계산하게 됩니다. 또한 {{site.data.keyword.openwhisk_short}} 시퀀스에서 Watson Visual Recognition을 사용하여 이미지를 분석하고 이미지의 컨텍스트를 기반으로 텍스트 태그를 추출합니다. 마지막으로 이미지가 처리되었음을 알려주는 푸시 알림을 사용자에게 전송하며, 알림에는 이제 날씨 및 태그 데이터가 포함됩니다.
