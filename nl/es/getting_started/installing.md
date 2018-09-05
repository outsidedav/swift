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

# Instalación de SDK en las apps cliente
{: #installing}

Los SDK de iOS de {{site.data.keyword.cloud}} dan soporte a varios gestores de dependencias populares, lo que le permite instalar fácilmente y utilizar los servicios de {{site.data.keyword.cloud_notm}} dentro de sus propias aplicaciones.

## Instalación con CocoaPods
{: #installing_with_cocoapods}

Para instalar un SDK utilizando Cocoapods, añádalo a su `Podfile`. Si el proyecto no tiene todavía un `Podfile`, utilice el mandato `pod init`.
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

Ejecute `pod install`, y abra el archivo `.xcworkspace` generado.

Para obtener más información, consulte las [Guías de Cocoapods](https://guides.cocoapods.org/using/index.html).

## Instalación con Carthage
{: #installing_with_carthage}

Para instalar un SDK con Carthage, añada esta línea a su `Cartfile`:
```
github "<github org name>/<github project name>"
```
{: codeblock}

Ejecute `carthage update` para iniciar el proceso de compilación. Una vez finalizada la compilación, añada las infraestructuras generadas a su proyecto. 

Para obtener más información, consulte el [README de Carthage](https://github.com/Carthage/Carthage#getting-started).

## Instalación con el gestor de paquetes de Swift
{: #installing_with_swift_package_manager}

Para instalar un SDK utilizando el Gestor de paquetes de Swift, añada la línea siguiente a las dependencias de `Package.swift`:
```
.Package(url: "<SDK git url>")
```
{: codeblock}

Ejecute `swift build` para iniciar el proceso de compilación.

Para obtener más información, consulte la [Visión general del gestor de paquetes de Swift](https://swift.org/package-manager/).

## Instalación manual
{: #installing_manually}

Para instalar un SDK manualmente, descargue el SDK y añada manualmente los archivos de origen en el proyecto.
