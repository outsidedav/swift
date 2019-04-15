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

# Installazione di SDK nelle applicazioni client
{: #install-sdks}

Gli SDK iOS {{site.data.keyword.cloud}} supportano diversi gestori dipendenze molto diffusi, consentendoti di installare facilmente e utilizzare i servizi {{site.data.keyword.cloud_notm}} nelle tue applicazioni.

## Installazione con CocoaPods
{: #install_cocoapods}

Per installare un SDK utilizzando CocoaPods, aggiungilo al tuo `Podfile`. Se il tuo progetto non dispone ancora di un `Podfile`, utilizza il comando `pod init`.
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

Esegui `pod install` e apri il file `.xcworkspace` generato.

Per ulteriori informazioni, consulta le [guide di CocoaPods](https://guides.cocoapods.org/using/index.html){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").

## Installazione con Carthage
{: #install_carthage}

Per installare un SDK con Carthage, aggiungi questa riga al tuo `Cartfile`:
```
github "<github org name>/<github project name>"
```
{: codeblock}

Esegui `carthage update` per avviare il processo di creazione. Una volta terminata la creazione, aggiungi i framework generati al tuo progetto. 

Per ulteriori informazioni, vedi la documentazione di [introduzione a Carthage](https://github.com/Carthage/Carthage#getting-started){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").

## Installazione con il gestore pacchetti Swift
{: #install_swift_package}

Per installare un SDK utilizzando il gestore pacchetti Swift, aggiungi la seguente riga alle tue dipendenze nel tuo `Package.swift`:
```swift
.Package(url: "<SDK git url>")
```
{: codeblock}

Esegui `swift build` per avviare il processo di creazione.

Per ulteriori informazioni, vedi la [panoramica relativa al gestore pacchetti Swift](https://swift.org/package-manager/){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").

## Installazione manuale
{: #install_manually}

Per installare un SDK manualmente, scarica l'SDK e aggiungi manualmente i file di origine nel tuo progetto.
