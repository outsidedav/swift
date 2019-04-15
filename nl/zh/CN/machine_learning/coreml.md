---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: coreml swift, core ml swift, watson swift core, create model swift, image classification swift, version parameter swift, swift coreml watson

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 搭配使用 Core ML 和 Watson
{: #swift-coreml}

通过 [Core ML](https://developer.apple.com/documentation/coreml){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，可以将各种机器学习模型类型集成到应用程序中。Core ML 除了支持丰富的 30 多种层类型的深度学习外，还支持标准模型，例如树整体、SVM 和广义线性模型。Core ML 并不是远程发送数据进行分析，而是通过低级别技术（例如，Metal 和 Accelerate）无缝地利用 CPU 和 GPU 来提供最大性能和效率。

您可以通过使用以下步骤训练和创建模型、下载和构建依赖项以及添加图像分类，从而将 Core ML 和 Watson Visual Recognition 添加到 Swift 应用程序。
{: shortdesc}

## 开始之前
{: #prereqs-coreml}

要将 Core ML 和 Watson Visual Recognition 用于 Swift，您需要以下组件：

* iOS 11.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods、Carthage 或 Swift Package Manager

您可以使用 [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，[Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 或 [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 安装 [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。通过使用 CocoaPods 管理依赖关系，您仅获得所需框架，而不是整个 Watson Swift SDK。如果您未使用过 CocoaPods，您可以轻松进行安装：

```console
sudo gem install cocoapods
```
{: codeblock}

## 步骤 1. 使用 {{site.data.keyword.watson}} {{site.data.keyword.visualrecognitionshort}} 训练模型
{: #train-module-coreml}

如果当前模型不存在，那么将使用远程发现的第一个模型或使用本地存在的任何模型。以下 gif 和随附的指示信息说明了如何将服务链接到 Watson Studio，并训练模型。

![Core ML 模型全程指导](images/CoreMLWalkthrough.gif)

### 在 Core ML 应用程序仪表板中设置服务
{: #service-coreml}

1. 通过在入门模板工具包仪表板中选择**启动工具**，启动 Visual Recognition 工具。
2. 通过选择**创建模型**开始创建模型。
2. 如果项目尚未与您创建的 Visual Recognition 实例相关联，那么会创建项目。否则，请跳至**创建模型**部分。
3. 对项目命名，然后单击**创建**。
  
  
  如果未定义任何存储器，请单击“刷新”。
  {: tip}

### 将服务绑定到项目
{: #bind-service-coreml}

1. 创建项目后，将显示项目仪表板。
2. 转至“设置”选项卡，向下滚动到**关联服务**，选择**添加服务** -> **Watson**。
3. 在“Watson 服务”页面中，选择 **Visual Recognition**。
4. 选择服务信息页面上的**现有**选项卡，然后从仪表板中选择服务实例。
5. 现在，服务已绑定，可以通过在“项目”仪表板中选择**资产**，然后单击**添加 Visual Recognition 模型**开始创建模型。

### 创建模型
{: #create-model-coreml}

1. 通过模型创建工具，可以更新分类器名称。如果要使用特定模型，请确保修改主视图控制器中的 `defaultClassifierID` 字段。

2. 在侧边栏，上传 `.zip` 压缩文件格式的模型训练课程。然后，从下拉菜单中选择每个数据集并将其添加到模型。请随意添加更多类，以使用自己的图像集来增强分类器！

![添加类](images/add_classes.png)

3. 选择**训练模型**，然后等待模型完成训练。

一切准备就绪！现在，您已准备就绪，可下载 Core ML 模型，并使用 [Watson Developer Cloud Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 将其集成到应用程序中。

## 步骤 2. 下载并构建依赖项
{: #install-depend-coreml}

使用您最喜欢的文本编辑器，通过运行 `pod init`，在项目的根目录（`.xcodeproj` 文件所在的位置）中创建新的 `Podfile`。然后，添加行以将 Watson Swift SDK 的
{{site.data.keyword.visualrecognitionshort}} 框架指定为依赖项：

```pod
use_frameworks!target 'MyApp' do
    pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

对于生产应用程序，您可能还要指定特定[版本需求](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，以避免 Watson Swift SDK 新发行版中的意外更改。

`Podfile` 已就位，现在可以下载依赖项。使用终端导航至项目的根目录，然后运行 CocoaPods：

```console
pod install
```
{: codeblock}

Cocoapods 下载 {{site.data.keyword.visualrecognitionshort}} 框架，并在项目的 `Pods/` 文件夹中进行构建。

要防止 Pod 构建失败，在 Xcode 中打开项目时，请打开以 `.xcworkspace` 而不是 `.xcodeproj` 结尾的文件。
{: tip}

## 步骤 3. 向应用程序添加图像分类
{: #add-image-coreml}

以下样本帮助您将 {{site.data.keyword.visualrecognitionshort}} Core ML 功能添加到应用程序，通常在 `ViewController.swift` 中。使用以下示例，您可以扩展用例的本地模型调用。

1. 添加用于 Visual Recognition 的 import 语句：
    
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. 传递 API 密钥和版本来初始化 SDK：
    
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  查看[版本参数文档](https://cloud.ibm.com/apidocs/visual-recognition#versioning){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 或者使用创建 {site.data.keyword.visualrecognitionshort}} 服务的日期。
  {: tip}

3. 添加以下代码以下载本地 Core ML 模型或者使用 Watson 分类器更新该模型：
  ```swift
  // 要使用的分类器的名称
  let classifierID = "your-classifier-ID-here"

  // 图像识别的最低置信度阈值
        let threshold = 0.5

        // 更新或下载模型
  visualRecognition.updateLocalModel(classifierID: classifierID) { _, error in
      if let error = error {
          print(error)
      } else {
          print("model successfully updated")
      }                
  }
  ```
  {: codeblock}

4. 通过使用本地 Core ML 模型添加以下代码以对图像分类：
  ```swift
  let image = UIImage(named: "your-image-filename")
  let classifierID = "your-classifier-ID-here"
  let threshold = 0.5

  // 使用模型对图像分类
  visualRecognition.classifyWithLocalModel(image: image, classifierIDs: [classifierID], threshold: threshold) { classifiedImages, error in
      if let error = error {
          print(error)
    }

    guard let classifiedImages = classifiedImages? else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

5. 探索 Watson SDK 支持的其他 [Core ML 分类功能](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。

## 步骤 4. 使用入门模板工具包
{: #coreml_starterkits}

通过入门模板工具包，可以快速、轻松地利用 {{site.data.keyword.cloud_notm}} 和 Core ML 的功能。可以使用入门模板工具包将 {{site.data.keyword.visualrecognitionshort}} 添加到任何客户端 iOS 应用程序或服务器端后端。Custom Vision Model for Core ML with {{site.data.keyword.watson}} 入门模板工具包说明了如何创建定制 {{site.data.keyword.visualrecognitionshort}} 模型，并将其实例化为可以在设备上本地运行的 Core ML 模型。

要将 {{site.data.keyword.visualrecognitionshort}} 添加到入门模板工具包，请完成以下步骤：

1. 选择要使用的[入门模板工具包](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。
2. 使用缺省服务创建应用程序。
3. 单击**添加服务 > Watson > {{site.data.keyword.visualrecognitionshort}}**。
4. 通过单击**下载代码**来下载项目。对于 iOS 项目，凭证会插入到 `BMSCredentials.plist` 文件中的相应密钥字段中。对于服务器端 Swift 项目，可以在 `config/local-dev.json` 文件中找到这些凭证。

## 后续步骤
{: #coreml_next}

现在，可以使用您自己定制生成的 Core ML 模型来分析图像。请一鼓作气，尝试下列其中一个选项：

* 查看 [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")，并探索其他受支持的 Watson 服务。
* 添加云逻辑。首先[开发无服务器应用程序](/docs/swift/backend?topic=swift-serverless-dev-swift#serverless-dev-swift)。
* 利用 {{site.data.keyword.visualrecognitionshort}} 提供的所有功能。有关更多详细信息，请参阅[文档](/docs/services/visual-recognition?topic=visual-recognition-index#index)。
