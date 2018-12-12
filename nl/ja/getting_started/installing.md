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

# クライアント・アプリへの SDK のインストール
{: #installing}

{{site.data.keyword.cloud}} iOS SDK は、よく使用される各種の依存関係マネージャーをサポートしているため、容易にインストールして {{site.data.keyword.cloud_notm}} サービスを独自のアプリケーション内で使用することが可能です。

## CocoaPods によるインストール
{: #installing_with_cocoapods}

CocoaPods を使用して SDK をインストールするには、それを `Podfile` に追加します。 プロジェクトに `Podfile` がまだない場合は、`pod init` コマンドを使用してください。
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

`pod install` を実行し、生成された `.xcworkspace` ファイルを開きます。

詳しくは、[CocoaPods Guides](https://guides.cocoapods.org/using/index.html) を参照してください。

## Carthage によるインストール
{: #installing_with_carthage}

Carthage を使用して SDK をインストールするには、次の行を `Cartfile` に追加します。
```
github "<github org name>/<github project name>"
```
{: codeblock}

`carthage update` を実行して、ビルド・プロセスを開始します。 ビルドが完了したら、生成されたフレームワークをプロジェクトに追加します。 

詳しくは、[Carthage の『Getting started』](https://github.com/Carthage/Carthage#getting-started)資料を参照してください。

## Swift Package Manager によるインストール
{: #installing_with_swift_package_manager}

Swift Package Manager を使用して SDK をインストールするには、`Package.swift` 内の依存関係に次の行を追加します。
```
.Package(url: "<SDK git url>")
```
{: codeblock}

`swift build` を実行して、ビルド・プロセスを開始します。

詳しくは、[Swift Package Manager Overview](https://swift.org/package-manager/) を参照してください。

## 手動でのインストール
{: #installing_manually}

手動で SDK をインストールするには、SDK をダウンロードし、手動でソース・ファイルをプロジェクトに追加します。
