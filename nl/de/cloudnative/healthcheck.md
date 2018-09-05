---

copyright:
  years: 2018
lastupdated: "2018-08-17"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Statusprüfung in einer Swift-App verwenden
{: #healthcheck}

Statusprüfungen stellen einen einfachen Mechanismus zur Verfügung, mit
dem festgestellt werden kann, ob eine serverseitige Anwendung ordnungsgemäß
funktioniert. Viele Bereitstellungsumgebungen wie
[Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) und
[Kubernetes](https://www.ibm.com/cloud/container-service)
können so konfiguriert werden, dass durch eine regelmäßige Abfrage der
Statusendpunkte ermittelt wird, ob eine Instanz Ihres Service
Datenverkehr akzeptieren kann oder nicht.

Statusprüfungen werden normalerweise über HTTP verarbeitet und verwenden
Standardrückgabecodes, um den Status `UP` (= aktiv) oder
`DOWN` (= inaktiv) mitzuteilen. Beispiele sind die Rückgabe
von
`200` für `UP` und von `5xx`
für `DOWN`. Zur Präzisierung wird `503`
verwendet, wenn die Anwendung Anforderungen nicht verarbeiten kann oder noch
nicht gestartet wurde (der Service also nicht verfügbar ist), und
`500`, wenn der Server eine Fehlerbedingung festgestellt hat. Der
Rückgabewert einer Statusprüfung ist zwar variabel, aber durch eine minimale
JSON-Antwort wie `{“status”: “UP”}` wird die Konsistenz
gewährleistet.

Cloud Foundry verwendet einen Statusendpunkt, um anzugeben, ob eine
Serviceinstanz Anforderungen verarbeiten kann oder nicht. In Kubernetes ist ein
Statusprüfungsendpunkt äquivalent zu einem
Endpunkt `readiness`. Ihre Anwendung muss diesen Endpunkt
definieren, damit das Treffen automatischer Routing-Entscheidungen unterstützt
wird. Der Erfolg oder Fehler dieses Endpunkts kann die Berücksichtigung
erforderlicher nachgelagerter Services für den Fall beinhalten, dass es keine
akzeptable Rückübertragung gibt. Falls Sie nachgelagerte Services überprüfen,
kann eine Zwischenspeicherung des Ergebnisses manchmal sinnvoll sein, um die
Auslastung des Gesamtsystems aufgrund von Statusüberprüfungen
zu minimieren.

Kubernetes definiert den zusätzlichen Endpunkt
[liveness](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/),
mit dem die Anwendung angeben kann, ob ein Prozess erneut gestartet werden soll
oder nicht. Hier wird eine Untergruppe von Berücksichtigungen angewendet;
beispielsweise kann eine Überprüfung von
`liveness` fehlschlagen, sobald ein bestimmter Schwellenwert
für die Speicherauslastung erreicht ist. Falls Ihre App unter Kubernetes
ausgeführt wird, kann es sinnvoll sein, einen Endpunkt
`liveness` hinzuzufügen, damit ein Neustart des Prozesses bei
Bedarf gewährleistet wird.

## Statusprüfung zu einer vorhandenen Swift-App hinzufügen
{: #add-healthcheck-existing}

Die
Bibliothek [Health](https://github.com/IBM-Swift/Health)
macht das Hinzufügen einer Statusprüfung zu einer Swift-Anwendung ganz einfach. Statusprüfungen
können erweitert werden. Zusätzliche Angaben über das
[Caching](https://github.com/IBM-Swift/Health#caching) zur
Verhinderung von Denial-of-Service-Attacken oder das Hinzufügen von
[angepassten
Prüfungen](https://github.com/IBM-Swift/Health#implementing-a-health-check) können Sie der Bibliothek
[Health](https://github.com/IBM-Swift/Health) entnehmen.

Um die Bibliothek "Health" zu einer vorhandenen Swift-App hinzuzufügen,
geben Sie sie im Abschnitt
*dependencies:* der Datei `Package.swift` an;
achten Sie hierbei darauf, sie zu allen Zielen hinzuzufügen, bei denen sie
verwendet wird:
```swift
  .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
```
{: codeblock}

Anschließend fügen Sie den folgenden Initialisierungscode zu Ihrer
Anwendung hinzu:
```swift
import Health

let health = Health()
```
{: codeblock}

Danach fügen Sie die Routendefinition hinzu, um den Endpunkt für die
Statusprüfung zu definieren:
```
router.get("/health") { request, response, next in
  // let status = health.status.toDictionary()
  let status = health.status.toSimpleDictionary()
  if health.status.state == .UP {
    try response.send(json: status).end()
  } else {
    try response.status(.serviceUnavailable).send(json: status).end()
  }
}
```
{: codeblock}

Überprüfen Sie den Status der App, indem Sie mit einem Browser auf den
Endpunkt
`/health` zugreifen. Der Code gibt Nutzdaten im Format
`{"status": "UP"}` wie durch das einfache Wörterverzeichnis
definiert zurück.

Informationen zu alternativen Implementierungen wie beispielsweise die
Verwendung von
**Codable** oder des Standardwörterverzeichnisses finden Sie
auf der Seite
[Health library examples](https://github.com/IBM-Swift/Health#usage).

## Aus serverseitigen Swift-Starter-Kit-Apps auf die Statusprüfung
zugreifen
{: #healthcheck-starterkit}

Wenn Sie eine Kitura-basierte Swift-App mithilfe eines Starter-Kits
generieren, ist standardmäßig ein Basisendpunkt für die Statusprüfung namens
`/health` enthalten. Der Endpunkt nutzt das in Swift 4
verfügbare Codable-Protokoll, das durch die Bibliothek
[Health](https://github.com/IBM-Swift/Health) unterstützt wird.

Der Basisinitialisierungscode, wie beispielsweise die Initialisierung
des Health-Objekts, findet in `Sources/Application.swift`
statt, während der Endpunkt für die Statusprüfung von der Datei `/Sources/Application/Routes/HealthRoutes.swift`  bereitgestellt wird,
die den folgenden Code enthält:
```swift
import LoggerAPI
import Health
import KituraContracts

func initializeHealthRoutes(app: App) {

    app.router.get("/health") { (respondWith: (Status?, RequestError?) -> Void) -> Void in
        if health.status.state == .UP {
            respondWith(health.status, nil)
        } else {
            respondWith(nil, RequestError(.serviceUnavailable, body: health.status))
        }
    }

}
```
{: codeblock}

Das Beispiel verwendet das Standardwörterverzeichnis, das Nutzdaten wie
beispielsweise
`{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}`
zurückgibt, wenn Sie auf den Endpunkt `/health`
zugreifen.
