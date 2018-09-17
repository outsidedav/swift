---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 向应用程序添加机器学习
{: #overview}

通过 [Core ML](https://developer.apple.com/documentation/coreml){:new_window}，可以将各种机器学习模型类型集成到应用程序中。Core ML 除了支持丰富的 30 多种层类型的深度学习外，还支持标准模型，例如树整体、SVM 和广义线性模型。Core ML 并不是远程发送数据进行分析，而是通过低级别技术（例如，Metal 和 Accelerate）无缝地利用 CPU 和 GPU 来提供最大性能和效率。

## 开始之前
{: #before-you-begin}

要使用 Core ML 和 Watson Visualization，需要以下组件：

  * iOS 11.0+
  * Xcode 9.0+
  * Swift 4.0+
  * Carthage

要安装 Carthage，请使用以下 `brew` 命令：
```
$ brew update
$ brew install carthage
```
{: codeblock}

## 步骤 1. 使用 {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}} 培训模型
{: #training-your-model}

如果当前模型不存在，那么将使用远程找到的第一个模型或使用本地存在的任何模型。以下 gif 和随附的指示信息说明了如何将服务链接到 Watson Studio，并培训模型。

![Core ML 模型全程指导](images/CoreMLWalkthrough.gif)

### 在 Core ML 应用程序仪表板中设置服务

1. 通过在入门模板工具包仪表板中选择**启动工具**，启动 Visual Recognition 工具。
2. 通过选择**创建模型**开始创建模型。
2. 如果项目尚未与您创建的 Visual Recognition 实例相关联，那么会创建项目。否则，请跳至**创建模型**部分。
3. 对项目命名，然后单击**创建**。
  **提示**：如果未定义任何存储器，请单击“刷新”。

### 将服务绑定到项目

1. 创建项目后，将显示项目仪表板。
2. 转至“设置”选项卡，向下滚动到**关联服务**，选择**添加服务** -> **Watson**。
3. 在“Watson 服务”页面中，选择 **Visual Recognition**。
4. 选择服务信息页面上的**现有**选项卡，然后从仪表板中选择服务实例。
5. 现在，服务已绑定，可以通过在“项目”仪表板中选择**资产**，然后单击**添加 Visual Recognition 模型**开始创建模型。

### 创建模型

1. 通过模型创建工具，根据需要修改分类器名称。缺省情况下，应用程序会使用第一个可用项，除非指定了要使用的项。如果要使用特定模型，请确保修改主视图控制器中的 `defaultClassifierID` 字段。

2. 在侧边栏中，上传 .zip 文件形式的模型培训课程。然后，从下拉菜单中选择每个数据集并将其添加到模型。请随意添加更多类，以使用自己的图像集来增强分类器！

![添加类](images/add_classes.png)

3. 选择**培训模型**，然后等待模型完全培训。

一切准备就绪！现在，您已准备就绪，可下载 Core ML 模型，并使用 [{{site.data.keyword.watson}} Developer Cloud Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window} 将其集成到应用程序中。

## 步骤 2. 下载并构建依赖项
{: #installing-dependencies}

1. 使用您最喜欢的文本编辑器，在项目的根目录（**.xcodeproj** 文件所在的位置）中创建名为 `Cartfile` 的新文件。

2. 添加一行以将 {{site.data.keyword.watson}} Developer Cloud Swift SDK 指定为依赖项，例如：

  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: pre}

  对于生产应用程序，您可能还要指定特定的版本需求，以避免 SDK 新发行版中的意外更改。
  {: tip}

3. 使用终端浏览至项目的根目录，然后运行 Carthage：

  ```
  $ carthage update --platform iOS
  ```
  {: pre}

  随后会下载 {{site.data.keyword.watson}} Developer Cloud Swift SDK，并在项目的 `Carthage/Build/iOS` 文件夹中构建其框架。

## 步骤 3. 向应用程序添加图像分类
{: #adding-image-classification}

```Swift
//
//  ViewController.swift
//  CoreMLImageClassificationExample
//
//  Created by Aaron Liberatore on 3/1/18.
//  Copyright © 2018 Aaron Liberatore. All rights reserved.
//

import UIKit
import VisualRecognitionV3

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        initializeCoreMLExample()
    }

    func failureHandler(_ error: Error) {
        print(error)
    }

    func initializeCoreMLExample() {
        // 配置

        // 您的 Watson Visual Recognition 服务 API 密钥
        let apiKey = "your-apiKey-here"

        // 要使用的分类器的名称
        let classifierID = "your-classifier-here"

        // 图像识别的最低置信度阈值
        let threshold = 0.5

        // 初始化服务
        let visualRecognition = VisualRecognition(apiKey: apiKey, version: "03-01-2018")

        // 更新或下载模型
        visualRecognition.updateLocalModel(classifierID: classifierID,
                                           failure: failureHandler) {

            // 使用模型对图像分类
            visualRecognition.classifyWithLocalModel(image: image,
                                                     classifierIDs: [classifierID],
                                                     threshold: threshold,
                                                     failure: self.failureHandler) { classifiedImages in

                 print(classifiedImages)
             }            
        }
    }
}
```
{: codeblock}

## 步骤 4. 使用入门模板工具包
{: #coreml_starterkits}

通过入门模板工具包，可以快速、轻松地利用 {{site.data.keyword.cloud_notm}} 和 Core ML 的功能。可以使用入门模板工具包将 {{site.data.keyword.visualrecognitionshort}} 添加到任何客户端 iOS 应用程序或服务器端后端。Custom Vision Model for Core ML with {{site.data.keyword.watson}} 入门模板工具包说明了如何创建定制 {{site.data.keyword.visualrecognitionshort}} 模型，并将其实例化为可以在设备上本地运行的 Core ML 模型。

要将 {{site.data.keyword.visualrecognitionshort}} 添加到入门模板工具包，请完成以下步骤：

1. 选择要使用的[入门模板工具包](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window}。
2. 使用缺省服务创建项目。
3. 单击**添加资源 > Watson > {{site.data.keyword.visualrecognitionshort}}**。
4. 通过单击**下载代码**来下载项目。对于 iOS 项目，凭证会插入到 `BMSCredentials.plist` 文件中的相应密钥字段中。对于服务器端 Swift 项目，可以在 `config/local-dev.json` 文件中找到这些凭证。

## 后续步骤
{: #coreml_next}

现在，可以使用定制生成的 Core ML 模型来分析图像。请一鼓作气，尝试下列其中一个选项：

* 添加云逻辑。首先[开发无服务器应用程序](/docs/swift/backend/functions.html)。
* 利用 {{site.data.keyword.visualrecognitionshort}} 可以提供的所有功能。有关更多详细信息，请参阅[文档](/docs/services/visual-recognition/index.html)。
