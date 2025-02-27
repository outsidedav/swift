---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-19"

keywords: server-side swift, swift cli, swift dependency, swift commands app, create app swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Serverseitige Swift-App mit der Befehlszeilenschnittstelle erstellen
{: #swift_cli}

{{site.data.keyword.cloud}} bietet Lösungen und Services, mit
denen Swift-Entwickler und -Anwendungen dem Kundenbedarf an Sicherheit,
künstlicher Intelligenz und Wertschöpfung Rechnung tragen können. Dank eines
breit aufgestellten Portfolios von Produktangeboten und SDKs können Sie durch
die Nutzung dieser Services eine schnelle Markteinführung Ihrer innovativen
Anwendungen erreichen.
{: shortdesc}

Der folgende Leitfaden soll Ihnen bei der Erstellung, lokalen Ausführung
und Bereitstellung einer serverseitigen Swift-App helfen. Sie erfahren, wie Sie
diese Aktionen unter Verwendung von
{{site.data.keyword.dev_cli_long}} mit Standardbefehlen ausführen.

Mit {{site.data.keyword.dev_cli_short}} können Sie über ein
Dutzend Befehle für die Verwaltung Ihrer serverseitigen Anwendungen nutzen. Weitere Informationen zu den `ibmcloud dev`-Befehlen finden Sie unter [{{site.data.keyword.dev_cli_notm}}-Befehlszeilenschnittstelle](/docs/cli/idt?topic=cloud-cli-idt-cli).

## Schritt 1. Voraussetzungen für die Entwicklung
{: #prereqs-swift-cli}

Damit Sie in die Verwendung von
{{site.data.keyword.cloud_notm}} einsteigen können, müssen die
folgenden Voraussetzungen erfüllt sein.

### Betriebssystem
{: #swift-cli-os-reqs}

Es hat sich bewährt, zur Entwicklung von Swift-Apps die neueste
unterstützte Mac OS-Hardware zu verwenden und bei Tests die neuesten
iOS-Releases zu nutzen. Melden Sie sich für ein [Apple Developer](https://developer.apple.com/){: new_window}-Konto ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") an, um das Testen auf physischen Einheiten zu ermöglichen.

### iOS und Xcode
{: #swift-cli-ios_xcode}

- [Installieren Sie Xcode 8+ (oder höher)](https://developer.apple.com/xcode/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")
- [Führen Sie die Bereitstellung auf Einheiten mit iOS 8 (oder höher) durch](https://support.apple.com/downloads/ios){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")
- Prüfen Sie vor der Übergabe von Apps an Apple die [Richtlinien für die Übergabe an den App-Store](https://developer.apple.com/app-store/resources/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")

### SDKs und Abhängigkeitsmanagement
{: #swift-cli-sdk-dependency}

Die folgenden Tools gewährleisten, dass Sie die nativen SDKs zur
Verwendung mit den verschiedenen
{{site.data.keyword.cloud_notm}}-Services installieren können.

- [CocoaPods für {{site.data.keyword.cloud_notm}}-SDKs installieren](https://cocoapods.org/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Installieren Sie Homebrew als Unterstützung für die Installation von Carthage](https://brew.sh/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Installieren Sie Carthage für Watson-SDKs](https://github.com/Carthage/Carthage){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")
  ```
  brew install carthage
  ```
  {: codeblock}

## Schritt 2. Tools für die lokale Entwicklung installieren
{: #swift-cli-install-tools}

{{site.data.keyword.cloud}} stellt lokale Tools für die
Befehlszeilenschnittstelle bereit, die Ihnen bei der Arbeit mit
unterschiedlichen Aspekten von {{site.data.keyword.cloud_notm}} helfen. Weitere Informationen finden Sie in [{{site.data.keyword.dev_cli_long}}-Informationen](/docs/cli?topic=cloud-cli-getting-started). Sie können die Tools zum Testen eines Swift-Back-End-Programms vor der Cloudbereitstellung in einem
lokalen Docker-Image zu installieren.

* Führen Sie bei Mac OS und Linux den folgenden Befehl aus:
  ```
  curl -sL http://ibm.biz/idt-installer | bash
  ```
  {: codeblock}

* Führen Sie bei Windows 10 den folgenden Befehl als Administrator aus:
  ```
  Set-ExecutionPolicy Unrestricted; iex(New-Object Net.WebClient).DownloadString('http://ibm.biz/idt-win-installer')
  ```
  {: codeblock}

  Klicken Sie mit der rechten Maustaste auf das Symbol für Windows
PowerShell und wählen Sie die Option **Als Administrator
ausführen** aus.
  {: tip}

## Schritt 3. Swift-App erstellen
{: #create-swift-app-cli}

1. Führen Sie den Befehl `ibmcloud dev create` über die
Befehlszeilenschnittstelle von
{{site.data.keyword.dev_cli_short}} aus, um einen vorkonfigurierten
Starter zu generieren. 
  ```
  ibmcloud dev create
  ```
  {: codeblock}

  Achten Sie darauf, sich zum Erstellen eines Projekts mit einem
{{site.data.keyword.cloud_notm}}-Konto anzumelden. Erstbenutzer können sich für ein gebührenfreies Konto [registrieren](https://{DomainName}/registration){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link"). Melden Sie sich mit
dem Befehl `ibmcloud login` über die Befehlszeile an.
  {: tip}

2. Wählen Sie bei der entsprechenden Aufforderung die im
folgenden Beispiel gezeigten Optionen 1, dann 6 und zuletzt 2 aus:
  ```
  ? Select a service type:                  
  1. Backend Service / Web App
  2. Mobile Client
  Enter a number> 1

  ? Select a language:
  1. Java - MicroProfile / Java EE
  2. Java - Spring
  3. Node
  4. Python - Django
  5. Python - Flask
  6. Swift
  Enter a number> 6

  ? Select a Starter Kit:
  1. Backend for Frontend - Swift Kitura - A starter for building 
  backend-for-frontend APIs in Swift, using the Kitura framework.
  2. Web App - Create Project
  3. Web App - Swift Kitura Basic - A starter that provides a basic web serving 
  application in Swift, using the Kitura framework.
  Enter a number> 2
  ```
  {: screen}

3. Geben Sie einen Namen für Ihre Anwendung an:
  ```
  ? Enter a name for your application> swift_project

  Generating application ...
  ```
  {: screen}

## Schritt 4. Anwendung erstellen, ausführen und bereitstellen
{: #swift-cli-deploy}

Jetzt können Sie Ihre Anwendung mit
{{site.data.keyword.dev_cli_short}} erstellen, ausführen und
bereitstellen.

### App erstellen
{: #swift-build-cli}

Sie können jetzt Ihre Anwendung erstellen; dies ist eine Voraussetzung
für die Ausführung der Anwendung. Verwenden Sie den folgenden Befehl im
Stammverzeichnis des Anwendungsverzeichnisses, um Ihre App zu erstellen:
```
ibmcloud dev build
```
{: codeblock}

### App ausführen
{: #swift-run-cli}

Nach einer erfolgreichen Erstellung können Sie Ihre Anwendung mit dem
folgenden Befehl in einem lokalen Container ausführen:
```
ibmcloud dev run
```
{: codeblock}

Wenn der Befehl erfolgreich ausgeführt wird, werden ein lokaler Host
und ein lokaler Port zum Anzeigen der Landing-Page Ihrer Anwendung angezeigt.

### App bereitstellen
{: #swift-deploy-cli}

Stellen Sie Ihre Anwendung in {{site.data.keyword.cloud_notm}}
mit dem Befehl `deploy` bereit:
```
ibmcloud dev deploy
```
{: codeblock}

## Nächste Schritte
{: #swift-cli-next notoc}

Informieren Sie sich über die Verwendung der
{{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple, mit deren
Hilfe Entwickler ausgehend von verschiedenen Starter-Kits Apps erstellen,
bereitstellen und mit wichtigen
{{site.data.keyword.cloud_notm}}-optimierten Services verbinden und danach umgehend funktionsfähigen Code
herunterladen oder dessen kontinuierliche Bereitstellung konfigurieren können. Benutzer können Ihre App erstellen, anzeigen, konfigurieren und verwalten sowie
den Code Ihrer App herunterladen. Die Verwendung der Entwicklerkonsole für
Apple ermöglicht es
Ihnen, {{site.data.keyword.cloud_notm}}-Services mit einer brandneuen
App zügig auszuwerten und zu testen.

Wollen Sie gleich loslegen? In der [{{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") können Sie sofort anfangen.
{: tip}

Weitere Informationen finden Sie im Abschnitt
[Swift-Apps mit
Starter-Kits entwickeln](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro).
