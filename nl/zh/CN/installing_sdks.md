---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-21"

keywords: install sdk swift, sdk client swift, carthage, cocoapods, swift package manager, ios sdk

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 将 SDK 安装到客户端应用程序中
{: #install-sdks}

{{site.data.keyword.cloud}} iOS SDK 支持各种常用依赖项管理器，使您可以在自己的应用程序中轻松安装和使用 {{site.data.keyword.cloud_notm}} 服务。

## 使用 CocoaPods 进行安装
{: #install_cocoapods}

要使用 CocoaPods 安装 SDK，请将其添加到 `Podfile` 中。如果您的项目还没有 `Podfile`，请使用 `pod init` 命令来进行创建。
```ruby
use_frameworks!target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

运行 `pod install`，然后打开生成的 `.xcworkspace` 文件。

有关更多信息，请参阅 [CocoaPods 指南](https://guides.cocoapods.org/using/index.html){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。

## 使用 Carthage 进行安装
{: #install_carthage}

要使用 Carthage 安装 SDK，请将以下行添加到 `Cartfile` 中：
```
github "<github org name>/<github project name>"
```
{: codeblock}

运行 `carthage update` 以启动构建过程。构建完成后，将生成的框架添加到项目。 

有关更多信息，请参阅 [Carthage 入门](https://github.com/Carthage/Carthage#getting-started){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 文档。

## 使用 Swift Package Manager 进行安装
{: #install_swift_package}

要使用 Swift Package Manager 安装 SDK，请将以下行添加到 `Package.swift` 中的 dependencies：
```swift
.Package(url: "<SDK git url>")
```
{: codeblock}

运行 `swift build` 以启动构建过程。

有关更多信息，请参阅 [Swift Package Manager 概述](https://swift.org/package-manager/){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")。

## 手动安装
{: #install_manually}

要手动安装 SDK，请下载 SDK 并将源文件手动添加到项目中。
