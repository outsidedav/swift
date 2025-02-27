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

# Instalando SDKs nos apps do cliente
{: #install-sdks}

Os SDKs do iOS do {{site.data.keyword.cloud}} suportam vários gerenciadores de dependência populares, permitindo a instalação fácil e o uso de serviços do {{site.data.keyword.cloud_notm}} dentro de seus próprios aplicativos.

## Instalando com CocoaPods
{: #install_cocoapods}

Para instalar um SDK usando CocoaPods, inclua-o em seu `Podfile`. Se o seu projeto ainda não tiver um `Podfile`, use o comando `pod init`.
```ruby
use_estruturas!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

Execute `pod install` e abra o arquivo `.xcworkspace` gerado.

Para obter mais informações, consulte os [Guias do CocoaPods](https://guides.cocoapods.org/using/index.html){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Instalando com Cartago
{: #install_carthage}

Para instalar um SDK com o Carthage, inclua esta linha no `Cartfile`:
```
github "<github org name>/<github project name>"
```
{: codeblock}

Execute `carthage update` para iniciar o processo de construção. Depois que a construção for concluída, inclua as estruturas geradas em seu projeto. 

Para obter mais informações, consulte a documentação de [Introdução do Carthage](https://github.com/Carthage/Carthage#getting-started){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Instalando com o gerenciador de pacote do Swift
{: #install_swift_package}

Para instalar um SDK usando o Swift Package Manager, inclua a linha a seguir em suas dependências no `Package.swift`:
```swift
.Package (url: "< SDK git url>")
```
{: codeblock}

Execute `swift build` para iniciar o processo de construção.

Para obter mais informações, consulte a [Visão geral do Swift Package Manager](https://swift.org/package-manager/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Instalando Manualmente
{: #install_manually}

Para instalar um SDK manualmente, faça download do SDK e inclua manualmente os arquivos de origem em seu projeto.
