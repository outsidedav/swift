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

# クライアント・アプリへの SDK のインストール
{: #install-sdks}

{{site.data.keyword.cloud}} iOS SDK は、よく使用される各種の依存関係マネージャーをサポートしているため、容易にインストールして {{site.data.keyword.cloud_notm}} サービスを独自のアプリケーション内で使用することが可能です。

## CocoaPods によるインストール
{: #install_cocoapods}

CocoaPods を使用して SDK をインストールするには、それを `Podfile` に追加します。 プロジェクトに `Podfile` がまだない場合は、`pod init` コマンドを使用してください。
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

`pod install` を実行し、生成された `.xcworkspace` ファイルを開きます。

詳しくは、[CocoaPods Guides ](https://guides.cocoapods.org/using/index.html){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。

## Carthage によるインストール
{: #install_carthage}

Carthage を使用して SDK をインストールするには、次の行を `Cartfile` に追加します。
```
github "<github org name>/<github project name>"
```
{: codeblock}

`carthage update` を実行して、ビルド・プロセスを開始します。 ビルドが完了したら、生成されたフレームワークをプロジェクトに追加します。 

詳しくは、[Carthage の『Getting started』](https://github.com/Carthage/Carthage#getting-started){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") 資料を参照してください。

## Swift Package Manager によるインストール
{: #install_swift_package}

Swift Package Manager を使用して SDK をインストールするには、`Package.swift` 内の依存関係に次の行を追加します。
```swift
.Package(url: "<SDK git url>")
```
{: codeblock}

`swift build` を実行して、ビルド・プロセスを開始します。

詳しくは、[Swift Package Manager Overview](https://swift.org/package-manager/){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。

## 手動でのインストール
{: #install_manually}

手動で SDK をインストールするには、SDK をダウンロードし、手動でソース・ファイルをプロジェクトに追加します。
