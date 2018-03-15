---

copyright:
  years: 2018
lastupdated: "2018-03-01"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Installing SDKs into Client Apps
{: #installing}

{{site.data.keyword.cloud_notm}} iOS SDKs support a variety of popular dependency managers, allowing you to easily install and use {{site.data.keyword.cloud}} services within your own applications.

## Installing with CocoaPods
{: #installing_with_cocoapods}

To install an SDK using Cocoapods, add it to your Podfile. If your project does not have a Podfile yet, use the `pod init` command.

```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: screen}

Then run `pod install`, and open the generated `.xcworkspace` file.

For more information on using Cocoapods, refer to the [Cocoapods Guides](https://guides.cocoapods.org/using/index.html).

## Installing with Carthage
{: #installing_with_carthage}

To install an SDK with Carthage, simply add this line to your Cartfile:

```
github "<github org name>/<github project name>"
```
{: screen}

Then run `carthage update`. Once the build is finished, add the generated frameworks to your project. 

For more information on using Carthage, refer to the [Carthage README](https://github.com/Carthage/Carthage#getting-started).

## Installing with Swift Package Manager
{: #installing_with_swift_package_manager}

To install an SDK using Swift Package Manager, simply add the following line to your dependencies in your Package.swift:

```
.Package(url: "<SDK git url>")
```
{: screen}

Then run `swift build`.

For more information on using Swift Package Manager, refer to the [Swift Package Manager Overview](https://swift.org/package-manager/).

## Installing Manually
{: #installing_manually}

To install an SDK manually, download the SDK and manually add the source files into your project.
