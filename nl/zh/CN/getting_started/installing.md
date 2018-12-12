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

# 将 SDK 安装到客户端应用程序中
{: #installing}

{{site.data.keyword.cloud}} iOS SDK 支持各种常用依赖项管理器，使您可以在自己的应用程序中轻松安装和使用 {{site.data.keyword.cloud_notm}} 服务。

## 使用 CocoaPods 进行安装
{: #installing_with_cocoapods}

要使用 CocoaPods 安装 SDK，请将其添加到 `Podfile` 中。如果您的项目还没有 `Podfile`，请使用 `pod init` 命令来进行创建。
```ruby
use_frameworks!target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

运行 `pod install`，然后打开生成的 `.xcworkspace` 文件。

有关更多信息，请参阅 [CocoaPods 指南](https://guides.cocoapods.org/using/index.html)。

## 使用 Carthage 进行安装
{: #installing_with_carthage}

要使用 Carthage 安装 SDK，请将以下行添加到 `Cartfile` 中：
```
github "<github org name>/<github project name>"
```
{: codeblock}

运行 `carthage update` 以启动构建过程。构建完成后，将生成的框架添加到项目。 

有关更多信息，请参阅 [Carthage 入门](https://github.com/Carthage/Carthage#getting-started)文档。

## 使用 Swift Package Manager 进行安装
{: #installing_with_swift_package_manager}

要使用 Swift Package Manager 安装 SDK，请将以下行添加到 `Package.swift` 中的 dependencies：
```
.Package(url: "<SDK git url>")
```
{: codeblock}

运行 `swift build` 以启动构建过程。

有关更多信息，请参阅 [Swift Package Manager 概述](https://swift.org/package-manager/)。

## 手动安装
{: #installing_manually}

要手动安装 SDK，请下载 SDK 并将源文件手动添加到项目中。
