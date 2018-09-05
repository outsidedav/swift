---

copyright:
  years: 2018
lastupdated: "2018-06-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# SDKs in Client-Apps installieren
{: #installing}

Die iOS-SDKs von {{site.data.keyword.cloud}} unterstützen
verschiedene gängige Abhängigkeitsmanager, weshalb Sie
{{site.data.keyword.cloud_notm}}-Services problemlos in Ihren eigenen
Anwendungen installieren und einsetzen können.

## Installation mit CocoaPods
{: #installing_with_cocoapods}

Um ein SDK mithilfe von Cocoapods zu installieren, fügen Sie es zu Ihrer
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

Weitere Informationen finden Sie in den
[Handbüchern zu Cocoapods](https://guides.cocoapods.org/using/index.html).

## Installation mit Carthage
{: #installing_with_carthage}

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

Weitere Informationen enthält die
[Readme-Datei
von Carthage](https://github.com/Carthage/Carthage#getting-started).

## Installation mit dem Swift-Paketmanager
{: #installing_with_swift_package_manager}

Um ein SDK mithilfe des Swift-Paketmanagers zu installieren, fügen Sie
die folgende Zeile zu den Abhängigkeiten
in der Datei `Package.swift` hinzu:
```
.Package(url: "<SDK git url>")
```
{: codeblock}

Führen Sie den Befehl `swift build` aus, um den
Buildprozess zu starten. 

Weitere Informationen finden Sie auf der Seite mit der
[Übersicht über den
Swift-Paketmanager](https://swift.org/package-manager/).

## Manuelle Installation
{: #installing_manually}

Um ein SDK manuell zu installieren, laden Sie das SDK herunter und fügen
Sie die Quellendateien manuell zu Ihrem Projekt hinzu.
