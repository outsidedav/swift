---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: install sdk swift, sdk client swift, carthage, cocoapods, swift package manager, ios sdk

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# SDKs in Client-Apps installieren
{: #install-sdks}

Die iOS-SDKs von {{site.data.keyword.cloud}} unterstützen
verschiedene gängige Abhängigkeitsmanager, weshalb Sie
{{site.data.keyword.cloud_notm}}-Services problemlos in Ihren eigenen
Anwendungen installieren und einsetzen können.

## Installation mit CocoaPods
{: #install_cocoapods}

Um ein SDK mithilfe von CocoaPods zu installieren, fügen Sie es zu Ihrer
`Poddatei` hinzu. Falls Ihr Projekt noch keine
`Poddatei` enthält, verwenden Sie den Befehl `pod
init`.
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

Führen Sie den Befehl `pod install` aus und öffnen Sie
die generierte Datei
`.xcworkspace`.

Weitere Informationen finden Sie in den [Handbüchern zu CocoaPods](https://guides.cocoapods.org/using/index.html){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link").

## Installation mit Carthage
{: #install_carthage}

Um ein SDK mithilfe von Carthage zu installieren, fügen Sie die folgende
Zeile zur Datei
`Cartfile` hinzu:
```
github "<github org name>/<github project name>"
```
{: codeblock}

Führen Sie den Befehl `carthage update` aus, um den
Buildprozess zu starten. Fügen Sie nach Abschluss des Builds die generierten
Frameworks zu Ihrem Projekt hinzu. 

Weitere Informationen finden Sie in der [einführenden Dokumentation zu Carthage](https://github.com/Carthage/Carthage#getting-started){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link").

## Installation mit dem Swift-Paketmanager
{: #install_swift_package}

Um ein SDK mithilfe des Swift-Paketmanagers zu installieren, fügen Sie
die folgende Zeile zu den Abhängigkeiten
in der Datei `Package.swift` hinzu:
```swift
.Package(url: "<SDK git url>")
```
{: codeblock}

Führen Sie den Befehl `swift build` aus, um den
Buildprozess zu starten.

Weitere Informationen finden Sie in der [Übersicht über den Swift-Paketmanager](https://swift.org/package-manager/){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link").

## Manuelle Installation
{: #install_manually}

Um ein SDK manuell zu installieren, laden Sie das SDK herunter und fügen
Sie die Quellendateien manuell zu Ihrem Projekt hinzu.
