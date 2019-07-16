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

# Instalación de SDK en las apps cliente
{: #install-sdks}

Los SDK de iOS de {{site.data.keyword.cloud}} dan soporte a varios gestores de dependencias populares, lo que le permite instalar fácilmente y utilizar los servicios de {{site.data.keyword.cloud_notm}} dentro de sus propias aplicaciones.

## Instalación con CocoaPods
{: #install_cocoapods}

Para instalar un SDK utilizando CocoaPods, añádalo a su `Podfile`. Si el proyecto no tiene todavía un `Podfile`, utilice el mandato `pod init`.
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

Ejecute `pod install`, y abra el archivo `.xcworkspace` generado.

Para obtener más información, consulte las [Guías de CocoaPods](https://guides.cocoapods.org/using/index.html){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

## Instalación con Carthage
{: #install_carthage}

Para instalar un SDK con Carthage, añada esta línea a su `Cartfile`:
```
github "<github org name>/<github project name>"
```
{: codeblock}

Ejecute `carthage update` para iniciar el proceso de compilación. Una vez finalizada la compilación, añada las infraestructuras generadas a su proyecto. 

Para obtener más información, consulte la documentación de [iniciación a Carthage](https://github.com/Carthage/Carthage#getting-started){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

## Instalación con el gestor de paquetes de Swift
{: #install_swift_package}

Para instalar un SDK utilizando el Gestor de paquetes de Swift, añada la línea siguiente a las dependencias de `Package.swift`:
```swift
.Package(url: "<SDK git url>")
```
{: codeblock}

Ejecute `swift build` para iniciar el proceso de compilación.

Para obtener más información, consulte la [Visión general del gestor de paquetes de Swift](https://swift.org/package-manager/){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

## Instalación manual
{: #install_manually}

Para instalar un SDK manualmente, descargue el SDK y añada manualmente los archivos de origen en el proyecto.
