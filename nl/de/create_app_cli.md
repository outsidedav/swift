---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-01"

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
Dutzend Befehle für die Verwaltung Ihrer serverseitigen Anwendungen nutzen. Weitere
Informationen zu den Befehlen `ibmcloud dev` finden Sie unter
[Befehlszeilenschnittstelle von IBM
Cloud Developer Tools](/docs/cli/idt/commands.html).

## Schritt 1. Voraussetzungen für die Entwicklung
{: #prereqs-swift-cli}

Damit Sie in die Verwendung von
{{site.data.keyword.cloud_notm}} einsteigen können, müssen die
folgenden Voraussetzungen erfüllt sein.

### Betriebssystem
{: #swift-cli-os-reqs}

Es hat sich bewährt, zur Entwicklung von Swift-Apps die neueste
unterstützte Mac OS-Hardware zu verwenden und bei Tests die neuesten
iOS-Releases zu nutzen. Registrieren Sie sich für ein Konto des Typs
[Apple Developer](https://developer.apple.com/), um Tests auf
einer physischen Einheit zu ermöglichen.

### iOS und Xcode
{: #swift-cli-ios_xcode}

- [Installieren Sie Xcode
8+ (oder höher)](https://developer.apple.com/xcode/)
- [Nehmen Sie
die Bereitstellung für iOS-Geräte 8 (oder höher) vor.](https://support.apple.com/downloads/ios)
- Prüfen Sie vor der Übergabe von Apps an Apple die Richtlinien für die
Übergabe an den App-Store, die Sie der Seite
[App Store
Submission Guidelines](https://developer.apple.com/app-store/guidelines/) entnehmen können.

### SDKs und Abhängigkeitsmanagement
{: #swift-cli-sdk-dependency}

Die folgenden Tools gewährleisten, dass Sie die nativen SDKs zur
Verwendung mit den verschiedenen
{{site.data.keyword.cloud_notm}}-Services installieren können.

- [Installieren Sie CocoaPods für IBM
Cloud-SDKs.](https://cocoapods.org/)
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Installieren Sie Homebrew zur
Unterstützung der Carthage-Installation.](https://brew.sh/)
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Installieren Sie Carthage für Watson-SDKs.](https://github.com/Carthage/Carthage)
  ```
  brew install carthage
  ```
  {: codeblock}

## Schritt 2. Tools für die lokale Entwicklung installieren
{: #swift-cli-install-tools}

{{site.data.keyword.cloud}} stellt lokale Tools für die
Befehlszeilenschnittstelle bereit, die Ihnen bei der Arbeit mit
unterschiedlichen Aspekten von {{site.data.keyword.cloud_notm}} helfen. Weitere Angaben enthält der Abschnitt [Informationen zu {{site.data.keyword.dev_cli_long}}](/docs/cli/index.html). Sie können die Tools zum Testen eines Swift-Back-End-Programms vor der Cloudbereitstellung in einem
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
{{site.data.keyword.cloud_notm}}-Konto anzumelden. Erstbenutzer können
sich für ein gebührenfreies Konto
[registrieren
![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen
Link")](https://cloud.ibm.com/registration/?cm_sp=dw-bluemix-_-swift-_-devcenter). Melden Sie sich mit
dem Befehl `ibmcloud login` über die Befehlszeile an.
  {: tip}

2. Wählen Sie bei der entsprechenden Aufforderung die im
folgenden Beispiel gezeigten Optionen 1, dann 6 und zuletzt 2 aus:
  ```
  ? Select a resource type:                  
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
  ```
  {: screen}

4. Wählen Sie aus, dass die Unterstützung von OpenAPI 2.0
aktiviert werden soll:
  ```
  ? Enable your application based on an OpenAPI 2.0 Specification document? [y/n]> y
  ```
  {: screen}

  Wenn die Unterstützung von OpenAPI 2.0 aktiviert ist, müssen Sie einen
Pfad zum Dokument von
OpenAPI 2.0 als URL angeben:
  ```
  ? Path to OpenAPI 2.0 document as a url (both yaml and json formats supported)> http://hostname.domain.com/path/to/file.json

  Generating application ...
  ```
  {: screen}

## Schritt 4. Anwendung erstellen, ausführen und bereitstellen
{: #swift-cli-deploy}

Jetzt können Sie Ihre Anwendung mit
{{site.data.keyword.dev_cli_short}} erstellen, ausführen und
bereitstellen.

1. **Erstellung**

  Sie können jetzt Ihre Anwendung erstellen; dies ist eine Voraussetzung
für die Ausführung der Anwendung. Verwenden Sie den folgenden Befehl im
Stammverzeichnis des Anwendungsverzeichnisses, um Ihre App zu erstellen:
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **Ausführung**

  Nach einer erfolgreichen Erstellung können Sie Ihre Anwendung mit dem
folgenden Befehl in einem lokalen Container ausführen:
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  Wenn der Befehl erfolgreich ausgeführt wird, werden ein lokaler Host
und ein lokaler Port zum Anzeigen der Landing-Page Ihrer Anwendung angezeigt.

3. **Bereitstellung**

  Stellen Sie Ihre Anwendung in {{site.data.keyword.cloud_notm}}
mit dem Befehl `deploy` bereit:
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}

## Nächste Schritte
{: #swift-cli-next}

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

Wollen Sie gleich loslegen? In der
[{{site.data.keyword.cloud_notm}}-Entwicklerkonsole
für Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits) können Sie sofort anfangen.
{: tip}

Weitere Informationen finden Sie im Abschnitt
[Swift-Apps mit
Starter-Kits entwickeln](/docs/swift/starter_kit/starter_kits.html).
