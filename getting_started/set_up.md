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

# Requirements for Developers
{: #set_up}

To get started with {{site.data.keyword.cloud_notm}}, make sure that you meet the following minimum requirements.

## Operating System

IBM recommends developing on the latest machines running MacOS and testing with the lastest iOS releases. IBM also recommends signing up for an [Apple Developer](https://developer.apple.com/) account to enable testing on a physical device.

## iOS & Xcode
{: #ios_and_xcode}

- [Install Xcode 8+ (or higher)](https://developer.apple.com/xcode/)
- [Deploy to iOS devices 8 (or higher)](https://support.apple.com/downloads/ios)
- Before app submission to Apple, review the [App Store Submission Guidelines](https://developer.apple.com/app-store/guidelines/)

## SDKs and Dependency management

The following tools are required to make sure you can install the native SDKs to work with the various IBM Cloud Services.

- [Install CocoaPods for IBM Cloud SDKs](https://cocoapods.org/)
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Install Homebrew to help install Carthage](https://brew.sh/)
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Install Carthage for Watson SDKs](https://github.com/Carthage/Carthage)
  ```
  brew install carthage
  ```
  {: codeblock}

## Tools for local development

The IBM Cloud has a set of local CLI tools that help you work with various aspects of the {{site.data.keyword.cloud_notm}}. For more information, see [IBM Cloud Developer Tools Information](https://www.ibm.com/cloud/cli). It is recommend to install these as they will help with testing a Swift backend in a local Docker image before deploying to the cloud.

-  Install IBM Cloud Developer Tools
  ```
  curl -sL https://ibm.biz/idt-installer | bash
  ```
  {: codeblock}
  
- [Install Docker engine](https://www.docker.com/docker-mac)

  Installing the Docker engine on MacOS enables you to test the building of a Swift backend app into a docker image before you try and deploy to {{site.data.keyword.containershort_notm}} with Kubernetes.
