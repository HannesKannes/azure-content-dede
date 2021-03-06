<properties
	pageTitle="Versionshinweise für Visual Studio-Erweiterung für Developer Analytics"
	description="Neueste Updates für Visual Studio-Tools für Developer Analytics"
	services="application-insights"
    documentationCenter=""
	authors="acearun"
	manager="douge"/>
<tags
	ms.service="application-insights"
	ms.workload="tbd"
	ms.tgt_pltfrm="ibiza"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/09/2016"
	ms.author="acearun"/>

# Versionshinweise für Developer Analytics Tools
Neuerungen: Application Insights- und HockeyApp-Analysen in Visual Studio.
## Version 7.0
### Visual Studio Application Insights-Trends
Visual Studio Application Insights ist ein neues Tool in Visual Studio, mit dem Sie den Betrieb Ihrer App über die Zeit besser analysieren können. Wählen Sie zuerst über die Symbolleistenschaltfläche **Application Insights** oder im Suchfenster von Application Insights die Option **Telemetrietrends untersuchen**. Alternativ dazu können Sie auch im Menü **Ansicht** auf die Option **Andere Fenster** und dann auf **Application Insights-Trends** klicken. Wählen Sie eine der fünf allgemeinen Abfragen aus, um zu beginnen. Sie können unterschiedliche Datasets basierend auf Telemetrietypen, Zeiträumen und anderen Eigenschaften analysieren. Wählen Sie zum Ermitteln von Anomalien in Ihren Daten in der Dropdownliste **Ansichtstyp** eine Anomalieoption. Mit den Filteroptionen am unteren Rand des Fensters ist es einfach, bestimmte Teilmengen Ihrer Telemetriedaten anzuzeigen.

![Application Insights-Trends](./media/app-insights-release-notes-vsix/Trends.PNG)

### Ausnahmen in CodeLens
Telemetriedaten zu Ausnahmen werden jetzt in CodeLens angezeigt. Wenn Sie Ihr Projekt mit dem Application Insights-Dienst verknüpft haben, wird die Anzahl von Ausnahmen angezeigt, die in den letzten 24 Stunden in den einzelnen Methoden in der Produktion aufgetreten sind. Von CodeLens können Sie zur Suche oder zu den Trends wechseln, um die Ausnahmen genauer zu untersuchen.

![Ausnahmen in CodeLens](./media/app-insights-release-notes-vsix/ExceptionsCodeLens.png)

### Unterstützung für ASP.NET Core
Application Insights unterstützt jetzt ASP.NET Core RC2-Projekte in Visual Studio. Sie können Application Insights neuen ASP.NET Core RC2-Projekten hinzufügen, indem Sie wie im folgenden Screenshot das Dialogfeld **Neues Projekt** verwenden. Sie können Application Insights auch einem vorhandenen Projekt hinzufügen, im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt klicken und dann auf **Application Insights-Telemetrie hinzufügen** klicken.

![Unterstützung für ASP.NET Core](./media/app-insights-release-notes-vsix/NetCoreSupport.PNG)

Für ASP.NET 5 RC1- und ASP.NET Core RC2-Projekte steht im Fenster „Diagnosetools“ ebenfalls neue Unterstützung bereit. Während des lokalen Debuggens werden Application Insights-Ereignisse wie z.B. Anforderungen und Ausnahmen aus Ihrer ASP.NET-App angezeigt. Sie können für ein Ereignis jeweils auf **Suche** klicken, um weitere Informationen anzuzeigen.

![Unterstützung für Diagnosetools](./media/app-insights-release-notes-vsix/DiagnosticTools.PNG)

### HockeyApp für universelle Windows-Apps
Zusätzlich zur Betaverteilung und zum Benutzerfeedback bietet HockeyApp Absturzberichte in Form von Symbolen für Ihre universellen Windows-Apps. Das Hinzufügen des HockeyApp SDK ist jetzt noch einfacher: Klicken Sie mit der rechten Maustaste auf Ihr universelles Windows-Projekt, und klicken Sie dann auf **HockeyApp – Absturzanalysen aktivieren**. Dadurch wird das SDK installiert, die Absturzsammlung eingerichtet und eine HockeyApp-Ressource in der Cloud bereitgestellt. Ihre App wird dabei nicht in den HockeyApp-Dienst hochgeladen.

Weitere neue Features:

* Wir haben die Benutzeroberfläche der Application Insights-Suche schneller und intuitiver gestaltet. Zeitbereiche und Detailfilter werden jetzt automatisch angewendet, wenn Sie sie auswählen.
* Außerdem verfügt die Application Insights-Suche jetzt über eine Option, mit der Sie aus der Anforderungstelemetrie direkt in den Code springen können.
* Wir haben die Benutzeroberfläche zur Anmeldung bei HockeyApp verbessert.
* In Diagnosetools werden Produktionstelemetriedaten für Ausnahmen angezeigt.

## Version 5.2
Wir freuen uns, die Einführung von HockeyApp-Szenarien in Visual Studio ankündigen zu können. Die erste Integration ist Teil der Beta-Distribution von universellen Windows-Apps und Windows Forms-Apps in Visual Studio.

Mit der Beta-Distribution laden Sie frühe Versionen Ihrer Apps für die Verteilung an eine ausgewählte Gruppe von Kunden oder Testpersonen in HockeyApp hoch. Die Beta-Distribution kann vor einer größeren Veröffentlichung in Kombination mit der HockeyApp-Absturzerfassung und den Funktionen für Benutzerfeedback wertvolle Informationen zu Ihrer App liefern. Sie können diese Informationen verwenden, um Probleme mit Ihrer App zu behandeln. So können Sie zukünftige Probleme vermeiden oder eindämmen, z.B. schlechte App-Bewertungen, negatives Feedback usw.

Informieren Sie sich darüber, wie einfach es ist, Builds für die Beta-Distribution aus Visual Studio hochzuladen.
### Universelle Windows-Apps
Das Kontextmenü eines Projektknotens für universelle Windows-Apps enthält jetzt eine Option zum Hochladen Ihres Builds in HockeyApp.

![Kontextmenü des Projekts für universelle Windows-Apps](./media/app-insights-release-notes-vsix/UniversalContextMenu.png)

Wenn Sie die Option wählen, wird das Dialogfeld für den HockeyApp-Upload angezeigt. Sie benötigen ein HockeyApp-Konto, um den Build hochzuladen. Machen Sie sich keine Sorgen, wenn Sie ein neuer Benutzer sind. Das Erstellen eines Kontos ist ein einfacher Prozess.

Nachdem die Verbindung hergestellt wurde, wird das Formular für den Upload im Dialogfeld angezeigt.

![Dialogfeld „Upload“ für universelle Windows-Apps](./media/app-insights-release-notes-vsix/UniversalUploadDialog.png)

Wählen Sie den Inhalt für den Upload aus (APPXBUNDLE- oder APPX-Datei), und wählen Sie im Assistenten dann die Veröffentlichungsoptionen aus. Auf der nächsten Seite können Sie optional die Versionshinweise hinzufügen. Wählen Sie die Option **Fertig stellen**, um den Upload zu starten.

Nach Abschluss des Uploads wird eine HockeyApp-Benachrichtigung mit der Bestätigung und einem Link zur App im HockeyApp-Portal angezeigt.

![Benachrichtigung „Upload abgeschlossen“](./media/app-insights-release-notes-vsix/UploadComplete.png)

Fertig! Sie haben mit wenigen Klicks einen Build für die Beta-Distribution hochgeladen.

Sie können die Anwendung im HockeyApp-Portal auf verschiedene Arten verwalten. Hierzu gehören das Einladen von Benutzern, das Anzeigen von Absturzberichten und Feedback, das Ändern von Details usw.

![HockeyApp-Portal](./media/app-insights-release-notes-vsix/HockeyAppPortal.png)

Weitere Informationen zur App-Verwaltung finden Sie unter [HockeyApp Knowledge Base](http://support.hockeyapp.net/kb/app-management-2).

### Windows Forms-Apps
Das Kontextmenü für einen Windows Forms-Projektknoten enthält jetzt eine Option zum Hochladen Ihres Builds in HockeyApp.

![Kontextmenü des Projekts für Windows Forms-Apps](./media/app-insights-release-notes-vsix/WinFormContextMenu.png)

Das Dialogfeld für den HockeyApp-Upload wird geöffnet. Es ähnelt dem Dialogfeld in einer universellen Windows-App.

![Dialogfeld „Upload“ für Windows Forms-Apps](./media/app-insights-release-notes-vsix/WinFormsUploadDialog.png)

Beachten Sie, dass dieser Assistent ein neues Feld zum Angeben der App-Version enthält. Für universelle Windows-Apps werden die Informationen aus dem Manifest eingefügt. Für Windows Forms-Apps ist leider keine Funktion dieser Art vorhanden. Sie müssen die Informationen manuell angeben.

Der Rest des Ablaufs ähnelt dem Ablauf für universelle Windows-Apps: Build und Optionen für die Veröffentlichung auswählen, Versionshinweise hinzufügen, Daten hochladen und die Verwaltung im HockeyApp-Portal durchführen.

So einfach ist dieser Vorgang. Probieren Sie es aus, und teilen Sie uns Ihre Meinung mit.
## Version 4.3
### Suchen von Telemetriedaten aus lokalen Debugsitzungen
In dieser Version können Sie nun nach den Application Insights-Telemetriedaten suchen, die in der Visual Studio-Debugsitzung generiert wurden. Bisher konnten Sie die Suche nur verwenden, wenn Sie die App bei Application Insights registriert haben. Jetzt muss für die App nur das Application Insights-SDK installiert werden, um nach lokalen Telemetriedaten suchen zu können.

Führen Sie zur Verwendung der Suche die folgenden Schritte aus, wenn Sie eine ASP.NET-Anwendung mit dem Application Insights-SDK verwenden:

1. Debuggen Sie die Anwendung.
2. Öffnen Sie die Application Insights-Suche mit einer der folgenden Vorgehensweisen:
	- Klicken Sie im Menü **Ansicht** auf **Andere Fenster** und dann auf **Application Insights-Suche**.
	- Klicken Sie auf die Symbolleistenschaltfläche **Application Insights**.
	- Erweitern Sie im Projektmappen-Explorer die Option **ApplicationInsights.config**, und klicken Sie auf **Nach Telemetriedaten der Debugsitzung suchen**.
3. Wenn Sie nicht bei Application Insights registriert sind, wird das Suchfenster im Modus „Telemetriedaten der Debugsitzung“ geöffnet.
4. Klicken Sie auf das Symbol **Suche**, um Ihre lokalen Telemetriedaten anzuzeigen.

![Upload abgeschlossen](./media/app-insights-release-notes-vsix/LocalSearch.png)

## Version 4.2
In dieser Version haben wir Features zum Suchen nach Daten im Ereigniskontext hinzugefügt (mit der Möglichkeit, aus mehr Datenereignissen in den Code zu springen) und das Senden von Protokolldaten an Application Insights vereinfacht. Diese Erweiterung wird monatlich aktualisiert. Feedback oder Funktionswünsche können Sie an aidevtools@microsoft.com senden.
### Protokollierung ohne Klicks
Falls Sie bereits NLog, log4net oder System.Diagnostics.Tracing nutzen, müssen Sie sich keine Sorgen machen, dass Sie alle Ablaufverfolgungen nach Application Insights verschieben müssen. In dieser Version haben wir die Application Insights-Protokollierungsadapter in die normale Benutzeroberfläche für die Konfiguration integriert. Wenn Sie bereits eines dieser Protokollierungsframeworks konfiguriert haben, helfen Ihnen die Funktionen zur Verwendung im folgenden Abschnitt weiter. **Wenn Sie Application Insights bereits hinzugefügt haben:**
1. Klicken Sie mit der rechten Maustaste auf den Projektknoten, und klicken Sie dann auf **Application Insights** und **Application Insights konfigurieren**. Vergewissern Sie sich, dass die Option zum Hinzufügen des richtigen Adapters im Konfigurationsfenster angezeigt wird.
2. Alternativ dazu können Sie beim Erstellen der Projektmappe auch das oben rechts angezeigte Popupfenster verwenden und darin auf **Konfigurieren** klicken.

![Benachrichtigung über Protokollierung](./media/app-insights-release-notes-vsix/LoggingToast.png)

Nachdem Sie den Protokollierungsadapter installiert haben, können Sie die Anwendung ausführen und überprüfen, ob die Daten wie folgt auf der Registerkarte der Diagnosetools angezeigt werden:

![Ablaufverfolgungen](./media/app-insights-release-notes-vsix/Traces.png)

### Springen zum Code oder Suchen von Code, an den die Telemetrieereigniseigenschaft ausgegeben wurde
In der neuen Version können Benutzer dann auf einen beliebigen Wert in den Ereignisdetails klicken, um nach einer übereinstimmenden Zeichenfolge in der aktuell geöffneten Projektmappe zu suchen. Die Ergebnisse werden in Visual Studio in der Liste „Suchergebnisse“ wie unten dargestellt angezeigt:

![Übereinstimmung suchen](./media/app-insights-release-notes-vsix/FindMatch.png)

### Neues Suchfenster ohne aktive Anmeldung
Wir haben das Aussehen des Fensters für die Application Insights-Suche verbessert, damit Sie leichter nach Daten suchen können, wenn sich die App in der Produktion befindet.

![Fenster „Suchen“](./media/app-insights-release-notes-vsix/SearchWindow.png)

### Anzeigen aller Telemetrieereignisse, die dem Ereignis zugeordnet sind
Wir haben eine neue Registerkarte mit vordefinierten Abfragen für alle Daten hinzugefügt, die sich auf das vom Benutzer angezeigte Telemetrieereignis beziehen. Sie befindet sich neben der Registerkarte mit den Ereignisdetails. Eine Anforderung enthält beispielsweise das Feld **Vorgangs-ID**. Jedes Ereignis, das dieser Anforderung zugeordnet ist, weist für **Vorgangs-ID** den gleichen Wert auf. Wenn eine Ausnahme auftritt, während der Vorgang die Anforderung verarbeitet, wird für die Ausnahme die gleiche Vorgangs-ID wie für die Anforderung vergeben, um das Auffinden zu erleichtern. Klicken Sie beim Anzeigen einer Anforderung auf **Die gesamte Telemetrie für diesen Vorgang**, um eine neue Registerkarte zu öffnen, auf der die neuen Suchergebnisse angezeigt werden.

![Verwandte Elemente](./media/app-insights-release-notes-vsix/RelatedItems.png)

### Vorwärts/Rückwärts-Funktion im Suchverlauf
Sie können jetzt zwischen den Suchergebnissen wechseln.

![Zurück navigieren](./media/app-insights-release-notes-vsix/GoBAck.png)

## Version 4.1
Diese Version enthält eine Reihe neuer Features und Updates. Update 1 muss installiert sein, um diese Version installieren zu können.

### Springen von einer Ausnahme zu einer Methode in Quellcode
Wenn Sie jetzt Ausnahmen aus der Produktions-App im Fenster der Application Insights-Suche anzeigen, können Sie im Code zu der Methode springen, für die die Ausnahme aufgetreten ist. Sie müssen nur das richtige Projekt laden, und Application Insights kümmert sich dann um den Rest! (Weitere Informationen zum Fenster der Application Insights-Suche finden Sie unter den Versionshinweisen für Version 4.0 in den folgenden Abschnitten.)

Wie funktioniert Application Insights? Sie können die Application Insights-Suche auch verwenden, wenn keine Projektmappe geöffnet ist. Im Stapelüberwachungsbereich wird eine Meldung mit Informationen angezeigt, und viele Elemente der Stapelüberwachung sind nicht verfügbar.

Falls Dateiinformationen verfügbar sind, kann es sich bei einigen Elemente ggf. um Links handeln, aber das Element mit den Projektmappeninformationen ist weiterhin sichtbar.

Wenn Sie auf den Hyperlink klicken, springen Sie an die Position der ausgewählten Methode im Code. Es kann ein Unterschied in Bezug auf die Versionsnummer bestehen. Die Funktion zum Springen zur richtigen Version des Codes wird in zukünftigen Versionen enthalten sein.

![Auf Ausnahmedetails klicken](./media/app-insights-release-notes-vsix/jumptocode.png)

### Neue Einstiegspunkte für Suchvorgänge im Projektmappen-Explorer
Sie können jetzt über den Projektmappen-Explorer auf die Suche zugreifen.

![Suche im Projektmappen-Explorer](./media/app-insights-release-notes-vsix/searchentry.png)

### Anzeigen einer Benachrichtigung nach Abschluss der Veröffentlichung
Ein Popupdialogfeld wird angezeigt, nachdem das Projekt online veröffentlicht wurde, damit Sie Ihre Application Insights-Daten in der Produktion anzeigen können.

![Benachrichtigung „Veröffentlichung abgeschlossen“](./media/app-insights-release-notes-vsix/publishtoast.png)

## Version 4.0

### Durchsuchen von Application Insights-Daten in Visual Studio
Genau wie bei der Suchfunktion im Application Insights-Portal können Sie in Visual Studio jetzt filtern und nach Ereignistypen, Eigenschaftswerten und Text suchen und dann einzelne Ereignisse überprüfen.

![Fenster „Suchen“](./media/app-insights-release-notes-vsix/search.png)

### Anzeigen von Daten von Ihrem lokalen Computer in den Diagnosetools

Sie können Ihre Telemetriedaten zusätzlich zu anderen Debugdaten auf der Seite mit den Visual Studio-Diagnosetools anzeigen. Nur ASP.NET 4.5 wird unterstützt.

![Seite „Diagnosetools“](./media/app-insights-release-notes-vsix/diagtools.png)

### Hinzufügen des SDK zum Projekt ohne Anmeldung bei Azure

Sie müssen sich nicht mehr bei Azure anmelden, um Application Insights-Pakete Ihrem Projekt hinzuzufügen, egal ob im Dialogfeld **Neues Projekt** oder im Kontextmenü des Projekts. Wenn Sie sich anmelden, wird das SDK wie üblich installiert und für das Senden von Telemetriedaten konfiguriert. Wenn Sie sich nicht anmelden, wird das SDK Ihrem Projekt hinzugefügt, und es werden Telemetriedaten für den Diagnosehub generiert. Sie können dies später konfigurieren, wenn Sie möchten.

![Dialogfeld "Neues Projekt"](./media/app-insights-release-notes-vsix/newproject.png)

### Unterstützung für Geräte

Im Rahmen der *Connect();* 2015 haben wir [angekündigt](https://azure.microsoft.com/blog/deep-diagnostics-for-web-apps-with-application-insights/), dass HockeyApp unsere mobile Entwicklerumgebung für Geräte ist. HockeyApp unterstützt Sie beim Verteilen der Beta-Builds an Ihre Tester, beim Sammeln und Analysieren von allen Abstürzen Ihrer App und beim Sammeln von Feedback direkt von Ihren Kunden. HockeyApp unterstützt Ihre App auf jeder Plattform, die Sie für die Erstellung wählen. Dies kann iOS, Android oder Windows oder eine plattformübergreifende Lösung wie Xamarin, Cordova oder Unity sein.

In zukünftigen Versionen der Application Insights-Erweiterung werden wir eine stärkere Integration zwischen HockeyApp und Visual Studio ermöglichen. Sie können vorerst mit der Verwendung von HockeyApp beginnen, indem Sie einfach den NuGet-Verweis hinzufügen. Weitere Informationen finden Sie in der [Dokumentation](http://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone).

<!---HONumber=AcomDC_0727_2016-->