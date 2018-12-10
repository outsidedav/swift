---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# Funktionen von Mobile Backend as a Service
{: #mbaas}

Ihre Anwendung kann zur Nutzung von externer Funktionen eines von
zwei Verfahren verwenden:
* Direkte Nutzung aus der Anwendung heraus für ihre Services, die
üblicherweise im Rahmen einer Plattform des Typs "Mobile Backend as a
Service"
(MBaas) gehostet werden.
* Nutzung über eine gehostete BFF-Komponente (BFF = Backend For
Frontend), die die Zusammensetzung und Steuerung der Services bereitstellen
sowie Back-End-Logik für die Anwendung beisteuern kann.

In der Regel ist das direkte Hinzufügen von externer Funktionen aus
Ihrer mobilen Anwendung heraus mit der MBaaS-Methode ein unkomplizierterer
Ansatz, denn in das erste Servicepaket müssen weniger Komponenten und
Infrastruktur einbezogen werden. Allerdings fehlt bei dieser Methode jene
ultimative Flexibilität und Bereichsvariabilität, die durch die Bereitstellung
einer BFF-Komponente ermöglicht wird.

Einer der Vorzüge der {{site.data.keyword.cloud_notm}}-Plattform
ist, dass Sie sich nicht im Vorfeld festlegen müssen. Alle bereitgestellten
Services können mithilfe der verfügbaren SDKs auf Swift-Basis direkt vom
Mobilgerät aus genutzt werden. Die
{{site.data.keyword.cloud_notm}}-Plattform unterstützt ebenfalls das
Schreiben ihrer eigenen Back-End-Logik in Swift, entweder über ein
serverunabhängiges Modell mit
{{site.data.keyword.openwhisk_short}} oder über serverseitiges Swift
mit Frameworks wie beispielsweise Kitura. Dank dieser Funktionalität können Sie
Back-End-Logik über ein BFF-Modell in Swift hinzufügen und auch die Nutzung der
Swift-SDKs durch die Anwendung auf BFF migrieren. Daher können Sie nach und
nach ohne großen Extra-Aufwand von der MBaaS-Methode auf eine BFF-Lösung umsteigen. 
