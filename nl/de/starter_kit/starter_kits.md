---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: swift starter kit, apple developer console, download code swift, app details swift, create swift app

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Swift-Apps mit Starter-Kits erstellen
{: #starterkits-intro}

In der {{site.data.keyword.cloud_notm}}-Entwicklerkonsole für
Apple
können Apple-Entwickler ausgehend von verschiedenen Starter-Kits Apps
erstellen, bereitstellen, mit wichtigen
{{site.data.keyword.cloud_notm}}-optimierten Services verbinden und
anschließend umgehend funktionsfähigen Code herunterladen oder die
kontinuierliche Bereitstellung konfigurieren. Benutzer können Ihre App
erstellen, anzeigen, konfigurieren und verwalten sowie den Code Ihrer App
herunterladen. Die Verwendung der Starter-Kits ermöglicht es Ihnen,
{{site.data.keyword.cloud_notm}}-Services mit einer brandneuen App
zügig auszuwerten und zu testen.

Wollen Sie gleich loslegen? In der [{{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") können Sie sofort anfangen.
{: tip}

## Was ist ein Starter-Kit?
{: #starterkits-what}

Mit der {{site.data.keyword.cloud_notm}}-Entwicklerkonsole können Sie aus verschiedenen Starter-Kits auswählen. Starter-Kits weisen
{{site.data.keyword.cloud_notm}} an, in der Sprache Ihrer Wahl ein Gerüst
für eine einsatzfähige App dynamisch zu assemblieren, das für die
Cloudbereitstellung vorbereitet ist. Jedes Starter-Kit deckt eine Sprache, ein
Framework und ein Muster für einen bestimmten realistischen Anwendungsfall ab,
wodurch Sie Code wiederverwenden können, statt ihn neu erfinden zu müssen.

Starter-Kits sind einsatzbereit und dienen schwerpunktmäßig zur
Demonstration einer wichtigen Musterimplementierung unter Verwendung einer
Laufzeit (z. B. Swift). In einigen Fällen bieten Starter-Kits eine einfache
Funktionalität für den Benutzer, um die Integration des Service
hervorzuheben. In anderen Fällen stellen Starter-Kits eine anpassbare
Implementierung für einen komplexeren Anwendungsfall dar.

Starter-Kits enthalten Anweisungen, anhand derer {{site.data.keyword.cloud_notm}} automatisch Apps erstellen kann, die mit einem Gerüst und mit portierbarem Code versehen sind, sowie die Services angeben kann, die automatisch bereitgestellt werden müssen, wenn Sie eine App aus dem Starter-Kit erstellen.

## {{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple
verwenden
{: #starterkits-journey}

In der {{site.data.keyword.cloud_notm}}-Entwicklerkonsole für
Apple erhalten Sie einen nahtlosen Pfad zur Erstellung einer
Swift-Starter-App für Ihren speziellen Anwendungsfall. Die Schritte, die Sie
hierzu ausführen könnten, sind nachfolgend beschrieben.

### Übersichtsanzeige
{: #overview_screen}

Die Übersichtsanzeige bietet Ihnen Inhalt, der auf eine Reihe von
Anwendungsfällen wie Watson, Wetter und anderes abgestimmt ist. Ausgehend von
der Übersichtsanzeige können Sie die Dokumentation einsehen, auf
Ausbildungsressourcen zugreifen, Services durchsuchen, unterstützte
Starter-Kits anzeigen oder über einen Link auf eine größere Sammlung von
Starter-Kits zugreifen. Wählen Sie im Navigationsbereich **Starter-Kits** aus, um die Ansicht "Starter-Kits" aufzurufen.

![{{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple - Übersichtsanzeige](images/overview_screen.png "Übersichtsanzeige")

### Ansicht 'Starter-Kits'
{: #starter_kits_view}

Die Ansicht 'Starter-Kits' zeigt die Sammlung der Starter-Kits für einen bestimmten Bereich von Anwendungsfällen an. Über verschiedene Links auf der Karte eines
Starter-Kits können Sie Demos und weitere Informationen anzeigen. Durch die
Auswahl eines Starter-Kits können Sie in die Ansicht "App erstellen" wechseln.

![{{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple - Ansicht 'Starter-Kits'](images/starter_kits_screen.png "Ansicht 'Starter-Kits'")

### App-Ansicht erstellen
{: #create_new_app_view}

In der Ansicht **App erstellen** können Sie einen Namen für Ihre App angeben sowie Bereitstellungs- und Routing-Informationen zur Verfügung stellen. Darüber hinaus werden die Services, die bei der Erstellung Ihrer App bereitgestellt werden, zusammen mit den für sie geltenden Preisstrukturplänen und Bedingungen angezeigt. Wählen Sie **Erstellen** aus, um die Ansicht "App-Details" aufzurufen. Falls Sie noch nicht bei {{site.data.keyword.cloud_notm}} angemeldet sind, müssen Sie dies jetzt nachholen.

![{{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple - Ansicht 'Neue App erstellen'](images/create_new_project_screen.png "Ansicht 'Neue App erstellen'")

## Ansicht 'App-Details'
{: #app_details_view}

In der Ansicht 'App-Details' wird eine Liste der Services angezeigt, die
für Ihre App konfiguriert sind. Für jeden Listeneintrag können Sie den
Servicenamen, Links zu weiterführenden Informationen sowie eine Schaltfläche
**Aktionen** anzeigen, auf der drei vertikal angeordnete
Punkte zu sehen sind. Zu den Optionen, die über die Schaltfläche
**Aktionen** angeboten werden, gehören das Entfernen von
Services aus einer App, das Öffnen des Dashboards für einen Service und das
Löschen eines Service. Beim Entfernen einer Serviceinstanz wird die Zuordnung
zu dieser App entfernt; die Serviceinstanz selbst wird nicht gelöscht. In
dieser Ansicht sind darüber hinaus die Serviceberechtigungsnachweise
konsolidiert; Sie müssen daher nicht die Ansichten für jede
einzelne Serviceinstanz aufrufen, um sie abzurufen.

![{{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple - Ansicht 'App-Details'](images/project_details_screen.png "Ansicht 'App-Details'")

Mithilfe der Seite **App-Details** können Sie neue oder vorhandene Services zu Ihrer App hinzufügen, die nicht Bestandteil des ursprünglichen Starter-Kits waren. Klicken Sie auf **Service hinzufügen**, um Services hinzuzufügen. Welche Services verfügbar
sind, richtet sich nach dem Typ der App sowie den Services, die in einer Region
verfügbar sind. Daher können nicht alle Services allen Apps zugeordnet werden.

![{{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple - Dialog 'Ressource hinzufügen'](images/add_resource_screen.png "Dialog 'Ressource hinzufügen'")

### Eigenen Code herunterladen

Auf der Seite _App-Details_ können Sie auf Ihren Code zugreifen, indem Sie **Code herunterladen** auswählen, um den Code für Ihre App zu generieren und herunterzuladen.

### Ansicht 'App-Liste'
{: #app_list_view}

In der Ansicht _App-Liste_ können Sie alle erstellten Apps auflisten. Ausgehend
von dieser Ansicht können Sie Ihre Apps umbenennen oder löschen. Wenn Sie auf
einen App-Namen klicken, werden Sie zur Ansicht "App-Details" zurückgeführt.

![{{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple - Ansicht 'App-Liste'](images/project_list_screen.png "Ansicht 'App-Liste'")

Weitere Informationen finden Sie im Abschnitt zu [Lernressourcen für die {{site.data.keyword.cloud_notm}}-Entwicklerkonsole für Apple](https://{DomainName}/developer/appledevelopment/learning-resources){: new_window} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link").
{: tip}
