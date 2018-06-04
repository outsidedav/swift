---

copyright:
  years: 2018
lastupdated: "2018-05-29"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Installing SDKs into client apps
{: #installing}

{{site.data.keyword.cloud_notm}} iOS SDKs support various of popular dependency managers, enabling you to easily install, and use {{site.data.keyword.cloud}} services within your own applications.

## Installing with CocoaPods
{: #installing_with_cocoapods}

To install an SDK by using Cocoapods, add it to your `Podfile`. If your project does not have a `Podfile` yet, use the `pod init` command.
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

Then run `pod install`, and open the generated `.xcworkspace` file.

For more information, see the [Cocoapods Guides](https://guides.cocoapods.org/using/index.html).

## Installing with Carthage
{: #installing_with_carthage}

To install an SDK with Carthage, add this line to your `Cartfile`:
```
github "<github org name>/<github project name>"
```
{: codeblock}

Then run `carthage update`. After the build is finished, add the generated frameworks to your project. 

For more information, see the [Carthage README](https://github.com/Carthage/Carthage#getting-started).

## Installing with Swift package manager
{: #installing_with_swift_package_manager}

To install an SDK using Swift Package Manager, add the following line to your dependencies in your `Package.swift`:
```
.Package(url: "<SDK git url>")
```
{: codeblock}

Then run `swift build`.

For more information, see the [Swift Package Manager Overview](https://swift.org/package-manager/).

## Installing manually
{: #installing_manually}

To install an SDK manually, download the SDK and manually add the source files into your project.
