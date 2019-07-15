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

# 클라이언트 앱에 SDK 설치
{: #install-sdks}

{{site.data.keyword.cloud}} iOS SDK는 자주 사용되는 여러 종속성 관리자를 지원하므로 고유한 애플리케이션 내의 {{site.data.keyword.cloud_notm}} 서비스를 쉽게 설치하고 사용할 수 있습니다.

## CocoaPods를 사용하여 설치
{: #install_cocoapods}

CocoaPods를 사용하여 SDK를 설치하려면 CocoaPods를 `Podfile`에 추가하십시오. 프로젝트에 아직 `Podfile`이 없으면 `pod init` 명령을 사용하십시오.
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

`pod install`을 실행하고 생성된 `.xcworkspace` 파일을 여십시오.

자세한 정보는 [CocoaPods 안내서](https://guides.cocoapods.org/using/index.html){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 참조하십시오.

## Carthage를 사용하여 설치
{: #install_carthage}

Carthage를 사용하여 SDK를 설치하려면 다음 행을 `Cartfile`에 추가하십시오.
```
github "<github org name>/<github project name>"
```
{: codeblock}

`carthage update`를 실행하여 빌드 프로세스를 시작하십시오. 빌드가 완료되면 생성된 프레임워크를 프로젝트에 추가하십시오. 

자세한 정보는 [Carthage 시작하기](https://github.com/Carthage/Carthage#getting-started){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘") 문서를 참조하십시오.

## Swift 패키지 관리자를 사용하여 설치
{: #install_swift_package}

Swift Package Manager를 사용하여 SDK를 설치하려면 `Package.swift`의 종속성에 다음 행을 추가하십시오.
```swift
.Package(url: "<SDK git url>")
```
{: codeblock}

`swift build`를 실행하여 빌드 프로세스를 시작하십시오.

자세한 정보는 [Swift Package Manager 개요](https://swift.org/package-manager/){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 참조하십시오.

## 수동으로 설치
{: #install_manually}

SDK를 수동으로 설치하려면 SDK를 다운로드하고 소스 파일을 프로젝트에 수동으로 추가하십시오.
