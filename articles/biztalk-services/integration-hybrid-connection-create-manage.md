<properties 
	pageTitle="Erstellen und Verwalten von Hybridverbindungen | Microsoft Azure" 
	description="Erfahren Sie, wie Sie eine Hybridverbindung herstellen, die Verbindung verwalten und den Hybridverbindungs-Manager installieren. MABS, WABS" 
	services="biztalk-services" 
	documentationCenter="" 
	authors="MandiOhlinger" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="biztalk-services" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/23/2016" 
	ms.author="mandia"/>


# Erstellen und Verwalten von Hybridverbindungen


## Übersicht über die Schritte
1. Erstellen Sie eine Hybridverbindung, indem Sie den **Hostnamen** oder den **FQDN** der lokalen Ressource im privaten Netzwerk eingeben.
2. Verknüpfen Sie Ihre Azure-Web-Apps bzw. mobile Azure-Apps mit der Hybridverbindung.
3. Installieren Sie den Hybrid Connection Manager auf der lokalen Ressource, und stellen Sie eine Verbindung zur betreffenden Hybridverbindung her. Das Azure-Portal stellt ein Ein-Klick-Verfahren für die Installation und Verbindung bereit.
4. Verwalten Sie Hybridverbindungen und deren Verbindungsschlüssel.

In diesem Thema werden diese Schritte aufgeführt.

> [AZURE.IMPORTANT] Sie können einen Hybridverbindungsendpunkt auf eine IP-Adresse festlegen. Bei Verwendung einer IP-Adresse hängt es vom Client ab, ob die lokale Ressource erreichbar ist. Die Hybridverbindung ist darauf angewiesen, dass der Client eine DNS-Suche ausführt. In den meisten Fällen handelt es sich beim __Client__ um den Anwendungscode. Falls der Client keine DNS-Suche ausführt (er versucht nicht, die IP-Adresse aufzulösen, als handele es sich um einen Domänennamen (x.x.x.x)), wird der Datenverkehr nicht über die Hybridverbindung gesendet.
>
> Beispiel: Sie legen **10.4.5.6** als lokalen Host fest:
> 
> **Das folgende Szenario funktioniert:** `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Das folgende Szenario funktioniert nicht:** `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>Erstellen einer Hybridverbindung

Eine Hybridverbindung kann im Azure-Portal anhand von Web-Apps **oder** BizTalk Services erstellt werden.

**Informationen zum Erstellen von Hybridverbindungen mit Web-Apps** finden Sie unter [Verbinden von Azure-Web-Apps mit einer lokalen Ressource](../app-service-web/web-sites-hybrid-connection-get-started.md). Sie können auch in Ihrer Web-App den Hybrid Connection Manager (HCM) installieren, was die bevorzugte Vorgehensweise ist.

**So erstellen Sie Hybridverbindungen in BizTalk Services**:

1. Melden Sie sich beim [klassischen Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885) an.
2. Wählen Sie im linken Navigationsbereich die Option **BizTalk Services** aus, und wählen Sie dann Ihren BizTalk Service aus.

	Wenn Sie keinen vorhandenen BizTalk Service haben, können Sie [einen BizTalk Service erstellen](biztalk-provision-services.md).
3. Wählen Sie die Registerkarte **Hybridverbindungen** aus: ![Registerkarte "Hybridverbindungen"][HybridConnectionTab]

4. Wählen Sie **Hybridverbindung erstellen** aus, oder klicken Sie auf die Schaltfläche **HINZUFÜGEN** in der Taskleiste. Geben Sie Folgendes ein:

	Eigenschaft | Beschreibung
--- | ---
Name | Der Name der Hybridverbindung muss eindeutig sein, und es darf sich nicht um den Namen des BizTalk Service handeln. Sie können jeden beliebigen Namen eingeben; er sollte aber auf den Zweck hinweisen. Beispiele:<br/><br/>Lohnbuch*SQLServer*<br/>Lieferliste*SharepointServer*<br/>Kunden*OracleServer*
Hostname | Geben Sie den vollständig qualifizierten Hostnamen, nur den Hostnamen oder die IPv4-Adresse der lokalen Ressource ein. Beispiele:<br/><br/>mySQLServer<br/>*mySQLServer*.*Domain*.corp.*yourCompany*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*.*yourCompany*.com<br/>10.100.10.10<br/><br/>Beachten Sie bei Verwendung der IPv4-Adresse, dass Ihr Client- oder Anwendungscode die IP-Adresse unter Umständen nicht auflöst. Lesen Sie den wichtigen Hinweis am Anfang dieses Themas.
Port | Geben Sie die Portnummer der lokalen Ressource ein. Wenn Sie Web-Apps verwenden, geben Sie beispielsweise Port 80 oder Port 443 ein. Wenn Sie SQL Server verwenden, geben Sie Port 1433 ein.

5. Wählen Sie das Häkchen aus, um das Setup abzuschließen.

#### Zusätzlich

- Es können mehrere Hybridverbindungen erstellt werden. Die Anzahl der zulässigen Verbindungen finden Sie unter [BizTalk Services: Editionsübersicht](biztalk-editions-feature-chart.md) für .
- Jede Hybridverbindung wird mit einem Paar von Verbindungszeichenfolgen erstellt: Anwendungsschlüsseln, die SENDEN, und lokalen Schlüsseln, die LAUSCHEN. Jedes Paar hat einen primären und einen sekundären Schlüssel.


## <a name="LinkWebSite"></a>Verknüpfen von Azure-Web-Apps oder mobilen Azure-Apps

Um die Azure-Web-Apps mit einer vorhandenen Hybridverbindung zu verknüpfen, wählen Sie auf dem Blatt "Hybridverbindungen" die Option **Vorhandene Hybridverbindung verwenden** aus. Weitere Informationen finden Sie unter [Verbinden von Azure-Web-Apps mit einer lokalen Ressource](../app-service-web/web-sites-hybrid-connection-get-started.md).

Um die mobilen Azure-Apps mit einer vorhandenen Hybridverbindung zu verknüpfen, wählen Sie die Option **Hybridverbindung hinzufügen** aus, wenn Sie einen mobilen Dienst ändern oder erstellen. Weitere Informationen finden Sie unter [Azure Mobile Services und Hybridverbindungen](../mobile-services/mobile-services-dotnet-backend-hybrid-connections-get-started.md).


## <a name="InstallHCM"></a>Lokale Installation des Hybrid Connection Manager

Nachdem eine Hybridverbindung erstellt wurde, installieren Sie den Hybrid Connection Manager auf der lokalen Ressource. Er kann aus Azure-Web-Apps oder BizTalk Service heruntergeladen werden. Schritte für BizTalk Services:

1. Melden Sie sich beim [klassischen Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885) an.
2. Wählen Sie im linken Navigationsbereich die Option **BizTalk Services** aus, und wählen Sie dann Ihren BizTalk Service aus.
3. Wählen Sie die Registerkarte **Hybridverbindungen** aus: ![Registerkarte "Hybridverbindungen"][HybridConnectionTab]
4. Wählen Sie auf der Taskleiste **Lokale Einrichtung** aus: ![Lokale Einrichtung][HCOnPremSetup]
5. Wählen Sie **Installieren und konfigurieren** aus, um den Hybrid Connection Manager auf dem lokalen System herunterzuladen oder auszuführen.
6. Wählen Sie das Häkchen, um die Installation zu starten.

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### Zusätzlich
- Hybrid Connection Manager kann unter den folgenden Betriebssystemen installiert werden:

	- Windows Server 2008 R2 (.NET Framework 4.5 und Windows Management Framework 4.0 mindestens erforderlich)
	- Windows Server 2012 (Windows Management Framework 4.0 mindestens erforderlich)
	- Windows Server 2012 R2


- Nachdem Sie den Hybrid Connection Manager installiert haben, geschieht Folgendes:

	- Die auf Azure gehostete Hybridverbindung wird automatisch so konfiguriert, dass sie die primäre Anwendungsverbindungszeichenfolge verwendet.
	- Die lokale Ressource wird automatisch so konfiguriert, dass sie die primäre lokale Verbindungszeichenfolge verwendet.

- Der Hybrid Connection Manager muss eine gültige lokale Verbindungszeichenfolge für die Autorisierung verwenden. Die Azure-Web-Apps bzw. die mobilen Apps müssen eine gültige Anwendungsverbindungszeichenfolge für die Autorisierung verwenden.
- Sie können Hybridverbindungen skalieren, indem Sie eine andere Instanz von Hybrid Connection Manager auf einem anderen Server installieren. Konfigurieren Sie den lokalen Listener so, dass er die gleiche Adresse wie der erste lokale Listener verwenden. In diesem Fall wird der Datenverkehr zufällig (Roundrobin) zwischen den aktiven lokalen Listenern verteilt.


## <a name="ManageHybridConnection"></a>Verwalten von Hybridverbindungen
Zum Verwalten der Hybridverbindungen haben Sie folgende Möglichkeiten:

- Verwenden Sie das Azure-Portal und gehen Sie zu Ihrem BizTalk Service.
- Verwenden Sie [REST-APIs](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### Kopieren Sie die Hybridverbindungszeichenfolgen oder generieren Sie diese erneut.

1. Melden Sie sich beim [klassischen Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885) an.
2. Wählen Sie im linken Navigationsbereich die Option **BizTalk Services** aus, und wählen Sie dann Ihren BizTalk Service aus.
3. Wählen Sie die Registerkarte **Hybridverbindungen** aus: ![Registerkarte "Hybridverbindungen"][HybridConnectionTab]
4. Wählen Sie die Hybridverbindung aus. Wählen Sie auf der Taskleiste **Verbindung verwalten** aus: ![Verwalten von Optionen][HCManageConnection]

	**Verbindungen verwalten** führt die Anwendung und die Vor-Ort-Verbindungszeichenfolgen auf. Sie können die Verbindungszeichenfolgen kopieren oder den Zugriffsschlüssel in der Verbindungszeichenfolge regenerieren.

	**Wenn Sie "Regenerieren" auswählen**, verändert sich der freigegebene Zugriffsschlüssel innerhalb der Verbindungszeichenfolge. Gehen Sie wie folgt vor:
	- Wählen Sie im klassischen Azure-Portal in der Azure-Anwendung **Synchronisierungsschlüssel**.
	- Führen Sie das **lokale Setup** erneut aus. Wenn Sie das lokale Setup erneut ausführen, wird die lokale Ressource automatisch für die Verwendung der aktualisierten primären Verbindungszeichenfolge konfiguriert.


#### Verwenden Sie Gruppenrichtlinien, um die von einer Hybridverbindung verwendeten lokalen Ressourcen zu steuern.

1. Laden Sie die [administrativen Vorlagen des Hybrid Connection Manager](http://www.microsoft.com/download/details.aspx?id=42963) herunter.
2. Extrahieren Sie die Dateien.
3. Gehen Sie auf dem Computer, der die Gruppenrichtlinie ändert, wie folgt vor:

	- Kopieren Sie die ADMX-Dateien in den Ordner *%WINROOT%\\PolicyDefinitions*.
	- Kopieren Sie die ADML-Dateien in den Ordner *%WINROOT%\\PolicyDefinitions\\de-DE*.

Nach dem Kopieren können Sie den Gruppenrichtlinien-Editor verwenden, um die Richtlinie zu ändern.




## Weiter

[Verbinden von Azure-Web-Apps mit einer lokalen Ressource](../app-service-web/web-sites-hybrid-connection-get-started.md) [Verbinden mit einem lokalen SQL-Server über Azure-Web-Apps](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) [Azure Mobile Services und Hybridverbindungen](../mobile-services/mobile-services-dotnet-backend-hybrid-connections-get-started.md) [Übersicht über Hybridverbindungen](integration-hybrid-connection-overview.md)


## Weitere Informationen

[REST-API für die Verwaltung von BizTalk Services in Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx) [BizTalk Services: Editionsübersicht](biztalk-editions-feature-chart.md) [Erstellen eines BizTalk Service mit dem klassischen Azure-Portal](biztalk-provision-services.md) [BizTalk Services: Registerkarten „Dashboard“, „Überwachen“ und „Skalieren“](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png

<!---HONumber=AcomDC_0824_2016-->