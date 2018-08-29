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

# Instalando SDKs nos apps do cliente
{: #installing}

Os SDKs do iOS do {{site.data.keyword.cloud}} suportam vários gerenciadores de dependência populares, permitindo a instalação fácil e o uso de serviços do {{site.data.keyword.cloud_notm}} dentro de seus próprios aplicativos.

## Instalando com CocoaPods
{: #installing_with_cocoapods}

Para instalar um SDK usando o Cocoapods, inclua-o no `Podfile`. Se o seu projeto ainda não tiver um `Podfile`, use o comando `pod init`.
```ruby
use_estruturas!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

Execute `pod install` e abra o arquivo `.xcworkspace` gerado.

Para obter mais informações, consulte os [Guias do Cocoapods](https://guides.cocoapods.org/using/index.html).

## Instalando com Cartago
{: #installing_with_carthage}

Para instalar um SDK com o Carthage, inclua esta linha no `Cartfile`:
```
github "<github org name>/<github project name>"
```
{: codeblock}

Execute `carthage update` para iniciar o processo de construção. Depois que a construção for concluída, inclua as estruturas geradas em seu projeto. 

Para obter mais informações, consulte o [LEIA-ME do Carthage](https://github.com/Carthage/Carthage#getting-started).

## Instalando com o gerenciador de pacote do Swift
{: #installing_with_swift_package_manager}

Para instalar um SDK usando o Swift Package Manager, inclua a linha a seguir em suas dependências no `Package.swift`:
```
.Package (url: "< SDK git url>")
```
{: codeblock}

Execute `swift build` para iniciar o processo de construção.

Para obter mais informações, consulte a [Visão Geral do Swift Package Manager](https://swift.org/package-manager/).

## Instalando Manualmente
{: #installing_manually}

Para instalar um SDK manualmente, faça download do SDK e inclua manualmente os arquivos de origem em seu projeto.
