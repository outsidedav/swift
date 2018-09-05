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

# Installazione di SDK nelle applicazioni client
{: #installing}

Gli SDK iOS {{site.data.keyword.cloud}} supportano diversi gestori dipendenze molto diffusi, consentendoti di installare facilmente e utilizzare i servizi {{site.data.keyword.cloud_notm}} nelle tue applicazioni.

## Installazione con CocoaPods
{: #installing_with_cocoapods}

Per installare un SDK utilizzando Cocoapods, aggiungilo al tuo `Podfile`. Se il tuo progetto non dispone ancora di un `Podfile`, utilizza il comando `pod init`.
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

Esegui `pod install` e apri il file `.xcworkspace` generato.

Per ulteriori informazioni, consulta le [guide di Cocoapods](https://guides.cocoapods.org/using/index.html).

## Installazione con Carthage
{: #installing_with_carthage}

Per installare un SDK con Carthage, aggiungi questa riga al tuo `Cartfile`:
```
github "<github org name>/<github project name>"
```
{: codeblock}

Esegui `carthage update` per avviare il processo di creazione. Una volta terminata la creazione, aggiungi i framework generati al tuo progetto. 

Per ulteriori informazioni, vedi il [README di Carthage](https://github.com/Carthage/Carthage#getting-started).

## Installazione con il gestore pacchetti Swift
{: #installing_with_swift_package_manager}

Per installare un SDK utilizzando il gestore pacchetti Swift, aggiungi la seguente riga alle tue dipendenze nel tuo ` Package.swift `:
```
.Package(url: "<SDK git url>")
```
{: codeblock}

Esegui `swift build` per avviare il processo di creazione.

Per ulteriori informazioni, vedi la [panoramica relativa al gestore pacchetti Swift](https://swift.org/package-manager/).

## Installazione manuale
{: #installing_manually}

Per installare un SDK manualmente, scarica l'SDK e aggiungi manualmente i file di origine nel tuo progetto.
