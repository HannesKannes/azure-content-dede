<properties
	pageTitle="Alle Themen über den SQL-Datenbankdienst | Microsoft Azure"
	description="Tabelle mit allen Themen für den Azure-Dienst namens „SQL-Datenbank“ auf https://azure.microsoft.com/de-de/documentation/articles/, Titel und Beschreibung."
	services="sql-database"
	documentationCenter=""
	authors="MightyPen"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.workload="sql-database"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/21/2016"
	ms.author="genemi"/>


# Alle Themen für den Azure SQL-Datenbankdienst

Dieses Thema führt alle Themen mit direktem Bezug zum **SQL-Datenbankdienst** in Azure auf. Sie können auf dieser Webseite mit **STRG+F** nach Schlüsselwörtern suchen, um aktuell interessante Themen zu finden.




## Neu

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 1 | [Getting started with JSON features in Azure SQL Database](sql-database-json-features.md) (Erste Schritte mit JSON-Features in der Azure SQL-Datenbank) | Mit Azure SQL-Datenbank können Sie Daten in JavaScript Object Notation (JSON) analysieren, abfragen und formatieren. |
| 2 | [SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse](sql-database-ssms-mfa-authentication.md) | Verwenden Sie die Multi-Factor Authentication mit SSMS für SQL-Datenbank und SQL Data Warehouse. |


## Aktualisierte Artikel

In diesem Abschnitt werden Artikel aufgelistet, die kürzlich mit einem sehr umfangreichen oder wesentlichen Update aktualisiert wurden. Für jeden aktualisierten Artikel wird ein grober Ausschnitt des hinzugefügten Markdowntexts angezeigt. Die Artikel wurden zwischen dem **26.07.2016** und **21.08.2016** aktualisiert.

| &nbsp; | Artikel | Aktualisierter Text, Ausschnitt |
| --: | :-- | :-- |
| 3 | [Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell](sql-database-export-powershell.md) | $subscriptionId = "IHRE AZURE-ABONNEMENT-ID" Login-AzureRmAccount Set-AzureRmContext -SubscriptionId $subscriptionId Zu exportierende Datenbank $DatabaseName = "DATENBANKNAME" $ResourceGroupName = "RESSOURCENGRUPPENNAME" $ServerName = "SERVERNAME" $serverAdmin = "ADMINISTRATORNAME" $serverPassword = "ADMINISTRATORKENNWORT" $securePassword = ConvertTo-SecureString ΓÇôString $serverPassword ΓÇôAsPlainText -Force $creds = New-Object ΓÇôTypeName System.Management.Automation.PSCredential ΓÇôArgumentList $serverAdmin, $securePassword Generieren eines eindeutigen Dateinamens für die BACPAC-Datei $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac" Speicherkontoinformationen für die BACPAC-Datei $BaseStorageUri = "https://SPEICHERNAME.blob.core.windows.net/BLOBCONTAINERNAME/" $BacpacUri = $BaseStorageUri + $bacpacFilename $StorageKeytype = "StorageAccessKey" $StorageKey = "IHR SPEICHERSCHLÜSSEL" $exportRequest = New-AzureRmSqlDatabaseExpo |
| 4 | [SQL-Datenbankoptionen und -leistung: Grundlegendes zum Angebot in den einzelnen Tarifen](sql-database-service-tiers.md) | **Auswahl einer Dienstebene** Ermitteln Sie zum Auswählen einer Dienstebene zunächst, ob die Datenbank eine eigenständige Datenbank oder Teil eines elastischen Pools sein soll. ** Auswählen einer Dienstebene für eine eigenständige Datenbank** Ermitteln Sie zum Auswählen einer Dienstebene für eine eigenständige Datenbank zunächst, welche Datenbankfeatures Sie benötigen, um Ihre SQL-Datenbank-Edition auszuwählen: / Datenbankgröße (5 GB maximal für Basic, 250 GB maximal für Standard und 500 GB bis 1 TB maximal für Premium – je nach Leistungsstufe) / Aufbewahrungszeitraum der Datenbanksicherung (7 Tage für Basic, 35 Tage für Standard und 35 Tage für Premium) Nachdem Sie die SQL-Datenbank-Edition bestimmt haben, können Sie die Leistungsstufe für die Datenbank (Anzahl der DTUs) bestimmen. Sie können dies schätzen und dann anhand der tatsächlichen Erfahrungen dynamisch zentral hoch- oder herunterskalieren („sql-database-scale-up.md“). Sie können auch den DTU Calculator (http://dtucalculator.azurewebsites.net/) verwenden, um die geschätzte Anzahl von erforderlichen DTUs zu ermitteln.** C |





## Erste Schritte

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 5 | [Erstellt mehrinstanzenfähige Apps mit Azure SQL-Datenbank mit Isolation und Effizienz](sql-database-build-multi-tenant-apps.md) | Erfahren Sie, wie Sie mithilfe von Azure SQL-Datenbank mehrinstanzenfähige Apps entwickeln können. |
| 6 | [Herstellen einer Verbindung mit einer SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md) | Erfahren Sie, wie Sie über SQL Server Management Studio (SSMS) eine Verbindung mit einer SQL-Datenbank in Azure herstellen. Führen Sie dann eine Beispielabfrage mithilfe von Transact-SQL (T-SQL) aus. |
| 7 | [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von .NET (C#)](sql-database-develop-dotnet-simple.md) | Verwenden Sie den Beispielcode in diesem Schnelleinstieg zum Erstellen einer modernen Anwendung in C#, die mit Azure SQL-Datenbank durch eine leistungsfähige relationale Datenbank in der Cloud unterstützt wird. |
| 8 | [Erstellen eines neuen Pools für elastische Datenbanken mit dem Azure-Portal](sql-database-elastic-pool-create-portal.md) | Enthält Informationen zum Hinzufügen eines skalierbaren Pools für elastische Datenbanken zu Ihrer SQL-Datenbankkonfiguration, um eine einfachere Verwaltung und Ressourcenfreigabe zwischen zahlreichen Datenbanken zu ermöglichen. |
| 9 | [Kennenlernen der Azure SQL-Datenbank-Tutorials](sql-database-explore-tutorials.md) | Erfahren Sie mehr über die Features und Funktionalität von SQL-Datenbank |
| 10 | [SQL-Datenbank-Tutorial: Erstellen einer SQL-Datenbank in Minuten mit dem Azure-Portal](sql-database-get-started.md) | Hier erfahren Sie, wie Sie einen logischen SQL-Datenbankserver, eine Firewallregel auf Serverebene, eine SQL-Datenbank und Beispieldaten einrichten, eine Verbindung mit Clienttools herstellen sowie Benutzer und eine Firewallregel auf Datenbankebene konfigurieren. |
| 11 | [Azure SQL-Datenbank sichert und schützt](sql-database-helps-secures-and-protects.md) | Erfahren Sie, wie SQL-Datenbank hilft, sichert und schützt |
| 12 | [Azure SQL-Datenbank – Lernen und Anpassungen](sql-database-learn-and-adapt.md) | Erfahren Sie, wie SQL-Datenbank lernt und sich anpasst. |
| 13 | [Wählen Sie eine SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure-VMs (IaaS)](sql-database-paas-vs-sql-server-iaas.md) | Erfahren Sie, welche SQL Server-Cloudoption sich am besten für Ihre Anwendung eignet: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure Virtual Machines. |
| 14 | [Spontane Skalierung von Azure SQL-Datenbank](sql-database-scale-on-the-fly.md) | Lernen Sie die spontane Skalierung von Azure SQL-Datenbank kennen |
| 15 | [Explore Azure SQL Database Solution Quick Starts (Entdecken Sie die lösungsbezogenen Schnellstarts für Azure SQL-Datenbank)](sql-database-solution-quick-starts.md) | Informationen zu Azure SQL-Datenbanklösungen |
| 16 | [Azure SQL-Datenbank funktioniert in Ihrer Umgebung](sql-database-works-in-your-environment.md) | Erfahren Sie, wie SQL-Datenbank hilft, sichert und schützt |



## Eine App erstellen

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 17 | [Durchführen der Problembehandlung, Diagnose und Verhinderung von SQL-Verbindungsfehlern und vorübergehenden Fehlern für SQL-Datenbank](sql-database-connectivity-issues.md) | Erfahren Sie, wie Sie einen SQL-Verbindungsfehler oder vorübergehenden Fehler in Azure SQL-Datenbank behandeln, diagnostizieren und verhindern. |
| 18 | [Herstellen der Verbindung mit einer SQL-Datenbank mit Visual Studio](sql-database-connect-query.md) | Schreiben Sie ein Programm in C# zum Abfragen und Herstellen einer Verbindung mit einer SQL-Datenbank. Informationen zu IP-Adressen, Verbindungszeichenfolgen, sicherer Anmeldung und kostenlosem Visual Studio. |
| 19 | [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md) | Bei Clientverbindungen von ADO.NET mit Azure SQL-Datenbank V12 wird der Proxy manchmal umgangen und direkt mit der Datenbank interagiert. Andere Ports als 1433 werden wichtig. |
| 20 | [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbankverbindungsfehler und andere Probleme](sql-database-develop-error-messages.md) | Erfahren Sie mehr über SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen, beispielsweise zu häufigen Datenbankverbindungsfehlern, Datenbankkopiefehlern und allgemeinen Fehlern. |
| 21 | [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von PHP unter Windows](sql-database-develop-php-simple.md) | Enthält ein PHP-Beispielprogramm, das eine Verbindung von einem Windows-Client mit Azure SQL-Datenbank herstellt, sowie Links zu den auf dem Client erforderlichen Softwarekomponenten. |
| 22 | [Ausprobieren von SQL-Datenbank: Verwenden von C# zum Erstellen einer SQL-Datenbank mithilfe der SQL-Datenbankbibliothek für .NET](sql-database-get-started-csharp.md) | Probieren Sie SQL-Datenbank für das Entwickeln von SQL- und C#-Apps aus, und erstellen Sie eine Azure SQL-Datenbank mithilfe der SQL-Datenbankbibliothek für .NET. |
| 23 | [Beheben von Verbindungsproblemen mit der Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md) | Schritte zum Ermitteln und Behandeln von häufigen Verbindungsproblemen für Azure SQL-Datenbank. |
| 24 | [Fehler „Datenbank auf dem Server ist zurzeit nicht verfügbar.“ beim Herstellen einer Verbindung mit SQL-Datenbank](sql-database-troubleshoot-connection.md) | Problembehandlung des Fehlers „Datenbank auf dem Server ist zurzeit nicht verfügbar.“, wenn eine Anwendung eine Verbindung mit SQL-Datenbank herstellt. |



## Entwickeln

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 25 | [Abrufen der Client-ID und des Schlüssels für die Verbindung mit der SQL-Datenbank aus dem Code](sql-database-client-id-keys.md) | Abrufen der Client-ID und des Schlüssels für den Zugriff auf die SQL-Datenbank aus dem Code. |
| 26 | [Herstellen von Verbindungen mit der SQL-Datenbank mithilfe von Java mit JDBC unter Windows](sql-database-develop-java-simple.md) | Zeigt ein Java-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank. Das Beispiel verwendet JDBC und wird auf einem Clientcomputer unter Windows ausgeführt. |
| 27 | [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Node.js](sql-database-develop-nodejs-simple.md) | Zeigt ein Node.js-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank. |
| 28 | [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md) | Hier erfahren Sie mehr über verfügbare Verbindungsbibliotheken und bewährte Methoden für Anwendungen, die eine Verbindung mit SQL-Datenbank herstellen. |
| 29 | [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Python](sql-database-develop-python-simple.md) | Zeigt ein Python-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank. |
| 30 | [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Ruby](sql-database-develop-ruby-simple.md) | Geben Sie ein Ruby-Codebeispiel an, das zum Herstellen einer Verbindung mit Azure SQL-Datenbank ausgeführt werden kann. |
| 31 | [Erste Schritte mit temporalen Tabellen in der Azure SQL-Datenbank](sql-database-temporal-tables.md) | Lernen Sie die ersten Schritte mit temporalen Tabellen in der Azure SQL-Datenbank. |



## Leistung und Skalierbarkeit

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 32 | [SQL-Datenbankratgeber](sql-database-advisor.md) | Der Azure SQL-Datenbank-Ratgeber stellt Empfehlungen für Ihre vorhandenen SQL-Datenbanken bereit, die die aktuelle Abfrageleistung verbessern können. |
| 33 | [SQL-Datenbankratgeber](sql-database-advisor-portal.md) | Mit dem Azure SQL-Datenbankratgeber im Azure-Portal können Sie Empfehlungen für Ihre vorhandenen SQL-Datenbanken anzeigen und umsetzen und so die derzeitige Abfrageleistung verbessern. |
| 34 | [Übersicht über das Durchführen von Vergleichstests für Azure SQL-Datenbank](sql-database-benchmark-overview.md) | Dieses Thema beschreibt den Azure SQL Database Benchmark-Vergleichstest zum Messen der Leistung von Azure SQL-Datenbank. |
| 35 | [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md) | Ein elastischer Datenbankpool stellt eine Sammlung der verfügbaren Ressourcen dar, die von einer Gruppe von elastischen Datenbanken gemeinsam verwendet werden. Dieses Dokument enthält Anleitungen, die Ihnen helfen sollen, die Eignung eines elastischen Datenbankpools für eine Gruppe von Datenbanken einzuschätzen. |
| 36 | [Erste Schritte mit Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md) | Grundlegende Erläuterung der Funktion „Tools für elastische Datenbanken“ von Azure SQL, einschließlich einer einfachen Beispiel-App. |
| 37 | [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md) | Antworten auf häufig gestellte Fragen von Kunden zu Cloud-Datenbanken, zur Azure SQL-Datenbank, das Microsoft Managementsystem für relationale Datenbanken (RDBMS) und Database as a Service in der Cloud. |
| 38 | [Erste Schritte mit In-Memory (Vorschau) in SQL-Datenbank](sql-database-in-memory.md) | SQL In-Memory-Technologien verbessern die Leistung von Transaktions- und Analyseworkloads erheblich. Erfahren Sie, wie Sie von diesen Technologien profitieren. |
| 11,9 | [Verwenden von In-Memory-OLTP (Vorschau) zur Verbesserung der Anwendungsleistung in SQL-Datenbank](sql-database-in-memory-oltp-migration.md) | Verwenden von In-Memory OLTP zum Verbessern der Transaktionsleistung in einer vorhandenen SQL-Datenbank. |
| 40 | [Überwachen des In-Memory-OLTP-Speichers](sql-database-in-memory-oltp-monitoring.md) | Einschätzen und Überwachen der XTP-In-Memory-Speicherverwendung und -kapazität; Beheben des Kapazitätsfehlers 41823 |
| 41 | [Verwenden des Abfragespeichers in Azure SQL-Datenbank](sql-database-operate-query-store.md) | Erfahren Sie, wie Sie den Abfragespeicher in Azure SQL-Datenbank verwenden. |
| 42 | [Einblicke in die SQL-Datenbankleistung](sql-database-performance.md) | Die Azure SQL-Datenbank bietet Leistungstools, mit denen Sie Bereiche identifizieren können, in denen die aktuelle Abfrageleistung verbessert werden kann. |
| 43 | [Leitfaden zur Azure SQL-Datenbankleistung für Einzeldatenbanken](sql-database-performance-guidance.md) | Dieses Thema enthält eine Anleitung, mit deren Hilfe Sie ermitteln können, welche Dienstebene für Ihre Anwendung geeignet ist. Außerdem enthält es Empfehlungen zur Optimierung der Anwendung, damit Sie alle Vorteile von Azure SQL-Datenbank ausnutzen können. |
| 44 | [Query Performance Insight für Azure SQL-Datenbank](sql-database-query-performance.md) | Mit der Überwachung der Abfrageleistung werden für eine Azure SQL-Datenbank die Abfragen mit der höchsten CPU-Auslastung identifiziert. |
| 45 | [Ändern der Dienstebene und Leistungsstufe (Tarif) einer SQL-Datenbank](sql-database-scale-up.md) | In „Ändern der Dienstebene und Leistungsstufe einer Azure SQL-Datenbank“ wird beschrieben, wie Sie die Dienstebene und Leistungsstufe Ihrer SQL-Datenbank zentral hoch- oder herunterstufen. Ändern des Tarifs einer Azure SQL-Datenbank. |
| 46 | [Überwachen der Datenbankleistung mithilfe von Azure SQL-Datenbank](sql-database-single-database-monitor.md) | Lernen Sie die Optionen zum Überwachen Ihrer Datenbank mithilfe von Azure-Tools und dynamischen Verwaltungssichten kennen. |
| 47 | [Tipps zur Optimierung der SQL-Datenbankleistung](sql-database-troubleshoot-performance.md) | Tipps für die Leistungsoptimierung in Azure SQL-Datenbank durch Auswertung und Verbesserung. |
| 48 | [Gewusst wie: Verbessern der Leistung von SQL-Datenbankanwendungen mithilfe von Batchverarbeitung](sql-database-use-batching-to-improve-performance.md) | Dieses Thema belegt, dass die Verwendung der Batchverarbeitung bei Datenbankvorgängen erheblich zur Verbesserung der Geschwindigkeit und Skalierbarkeit Ihrer Azure SQL-Datenbankanwendungen beiträgt. Die Batchverarbeitungstechniken können zwar für jede beliebige SQL Server-Datenbank verwendet werden, der Artikel konzentriert sich jedoch auf Azure. |



## Tools für das horizontale Skalieren

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 49 | [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md) | In diesem Artikel werden die Anforderungen und allgemeinen Datenarchitekturmuster von mehrinstanzenfähigen Datenbankanwendungen, die in einer Cloudumgebung ausgeführt werden, sowie die verschiedenen Vor- und Nachteile dieser Muster erläutert. Außerdem wird beschrieben, wie diese Anforderungen mit den Pools für elastische Datenbanken und elastischen Tools von Azure SQL-Datenbank ohne Kompromisse erfüllt werden können. |
| 50 | [Migrieren von vorhandenen Datenbanken zu horizontaler Hochskalierung](sql-database-elastic-convert-to-use-elastic-tools.md) | Erstellen eines Shardzuordnungs-Managers, um Sharddatenbanken für die Verwendung der Tools für elastische Datenbanken umzuwandeln |
| 51 | [Erstellen skalierbarer Clouddatenbanken](sql-database-elastic-database-client-library.md) | Erstellen skalierbarer .NET-Datenbank-Apps mit der Clientbibliothek für elastische Datenbanken |
| 52 | [Leistungsindikatoren für den Shardzuordnungs-Manager](sql-database-elastic-database-perf-counters.md) | ShardMapManager-Klasse und datenabhängiges Routing – Leistungsindikatoren |
| 53 | [Hinzufügen eines Shards mithilfe der Tools für elastische Datenbanken](sql-database-elastic-scale-add-a-shard.md) | Erfahren Sie, wie Sie einem Shard-Satz mithilfe der Elastic Scale-APIs neue Shards hinzufügen. |
| 54 | [Bereitstellen eines Split-Merge-Diensts](sql-database-elastic-scale-configure-deploy-split-and-merge.md) | Aufteilen und Zusammenführen mit Tools für elastische Datenbanken |
| 55 | [Datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md) | Erfahren Sie, wie Sie die ShardMapManager-Klasse in .NET-Apps für das datenabhängige Routing, einem Feature von elastischen Datenbanken für Azure SQL-Datenbank, verwenden können. |
| 56 | [Tools für elastische Datenbanken – Häufig gestellte Fragen](sql-database-elastic-scale-faq.md) | Häufig gestellte Fragen zur Elastic Scale-Funktion der Azure SQL-Datenbank. |
| 57 | [Tools für elastische Datenbanken – Glossar](sql-database-elastic-scale-glossary.md) | Erläuterung von Begriffen, die für die Tools für flexible Datenbanken verwendet werden |
| 58 | [Übersicht über Features für elastische Datenbanken](sql-database-elastic-scale-introduction.md) | SaaS-Entwickler (Software-as-a-Service) können mit diesen Tools problemlos elastische und skalierbare Datenbanken in der Cloud erstellen. |
| 59 | [Anmeldeinformationen für den Zugriff auf die Clientbibliothek für elastische Datenbanken](sql-database-elastic-scale-manage-credentials.md) | So richten Sie Anmeldeinformationen mit den passenden Berechtigungen (von Administrator- bis Leseberechtigungen) für Apps für elastische Datenbanken ein |
| 60 | [Abfragen von mehreren Shards](sql-database-elastic-scale-multishard-querying.md) | Führen Sie mithilfe der Clientbibliothek für elastische Datenbanken Abfragen mehrerer Shards durch. |
| 61 | [Skalierung mit dem Split-Merge-Tool für elastische Datenbanken](sql-database-elastic-scale-overview-split-and-merge.md) | Hier wird erläutert, wie Sie Shards manipulieren und Daten über einen selbst gehosteten Dienst mithilfe von APIs für elastische Datenbanken verschieben. |
| 62 | [Horizontales Skalieren von Datenbanken mit dem Shardzuordnungs-Manager](sql-database-elastic-scale-shard-map-management.md) | Erfahren Sie, wie Sie "ShardMapManager" und die Clienbtbibliothek für elastische Datenbanken verwenden. |
| 63 | [Split-Merge-Sicherheitskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md) | Einrichten von x409-Zertifikaten für die Verschlüsselung |
| 64 | [Upgrade einer App auf die neueste Clientbibliothek für elastische Datenbanken](sql-database-elastic-scale-upgrade-client-library.md) | Aktualisieren von Apps und Bibliotheken mithilfe von Nuget |
| 65 | [Clientbibliothek für elastische Datenbanken mit Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) | Verwenden der Clientbibliothek für elastische Datenbanken und von Entity Framework für die Programmierung von Datenbanken |
| 66 | [Verwenden der Clientbibliothek für elastische Datenbanken](sql-database-elastic-scale-working-with-dapper.md) | Verwenden der Clientbibliothek für elastische Datenbanken. |
| 67 | [Mehrinstanzenfähige Anwendungen mit elastischen Datenbanktools und zeilenbasierter Sicherheit](sql-database-elastic-tools-multi-tenant-row-level-security.md) | Erfahren Sie, wie Sie mithilfe von elastischen Datenbanktools in Verbindung mit zeilenbasierter Sicherheit eine Anwendung mit einer hochgradig skalierbaren Datenebene in einer Azure SQL-Datenbank erstellen, die mehrinstanzenfähige Shards unterstützt. |



## Elastische Aufträge

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 68 | [Erstellen und Verwalten von horizontal hochskalierten Azure SQL-Datenbanken mithilfe von elastischen Aufträgen (Vorschau)](sql-database-elastic-jobs-create-and-manage.md) | Führen Sie die Schritte zur Erstellung und Verwaltung eines elastischen Datenbankauftrags aus. |
| 69 | [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md) | Verwenden von Aufträgen für die elastische Datenbank |
| 70 | [Verwalten horizontal hochskalierter Clouddatenbanken](sql-database-elastic-jobs-overview.md) | Veranschaulicht den Dienst für elastische Datenbankaufträge |
| 71 | [Erstellen und Verwalten von Aufträgen für die elastische SQL-Datenbank mithilfe von PowerShell (Vorschau)](sql-database-elastic-jobs-powershell.md) | PowerShell, verwendet zum Verwalten von Pools für Azure SQL-Datenbanken |
| 72 | [Installieren von Aufträgen für die elastische Datenbank – Übersicht](sql-database-elastic-jobs-service-installation.md) | Schrittweise Anleitung für die Installation der Funktion für elastische Aufträge |
| 73 | [Deinstallieren von Komponenten der Aufträge für die elastische Datenbank](sql-database-elastic-jobs-uninstall.md) | So deinstallieren Sie das Tool für elastische Datenbankaufträge |



## Elastische Pools

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 74 | [Was ist ein Azure-Pool für elastische Datenbanken?](sql-database-elastic-pool.md) | Verwalten Sie hunderte oder tausende Datenbanken mithilfe eines Pools. Ein Preis für einen Satz Leistungseinheiten kann über den Pool verteilt werden. Verschieben Sie die Datenbanken je nach Wunsch in den oder aus dem Pool. |
| 75 | [Erstellen eines neuen Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-create-csharp.md) | Verwenden Sie C#-Datenbankentwicklungstechniken, um einen skalierbaren Pool für elastische Datenbanken in Azure SQL-Datenbank zu erstellen, damit Sie Ressourcen für zahlreiche Datenbanken freigeben können. |
| 76 | [Erstellen eines neuen Pools für elastische Datenbanken mit PowerShell](sql-database-elastic-pool-create-powershell.md) | Erfahren Sie, wie Sie mithilfe von PowerShell Ressourcen für Azure SQL-Datenbanken horizontal hochskalieren können, indem Sie für die Verwaltung mehrerer Datenbanken einen skalierbaren Pool für elastische Datenbanken erstellen. |
| 77 | [C# database development: Create and configure an elastic database pool for SQL database (C#-Datenbankentwicklung: Erstellen und Konfigurieren eines Pools für elastische Datenbanken für die SQL-Datenbank)](sql-database-elastic-pool-csharp.md) | Verwenden Sie C#-Datenbankentwicklungstechniken, um einen Pool für elastische Datenbanken in Azure SQL-Datenbank zu erstellen, damit Sie Ressourcen für zahlreiche Datenbanken freigeben können. |
| 78 | [PowerShell script for identifying databases suitable for an elastic database pool (PowerShell-Skript zum Ermitteln von Datenbanken mit Eignung für einen Pool für elastische Datenbanken)](sql-database-elastic-pool-database-assessment-powershell.md) | Ein elastischer Datenbankpool stellt eine Sammlung der verfügbaren Ressourcen dar, die von einer Gruppe von elastischen Datenbanken gemeinsam verwendet werden. Dieses Dokument enthält ein PowerShell-Skript, das Ihnen helfen soll, die Eignung eines Pools für elastische Datenbanken für eine Gruppe von Datenbanken einzuschätzen. |
| 79 | [Überwachen und Verwalten eines Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-manage-csharp.md) | Methoden zur Entwicklung von C#-Datenbanken verwenden, um einen Pool für elastische Azure SQL-Datenbanken zu verwalten |
| 80 | [Überwachen und Verwalten eines Pools für elastische Datenbanken über das Azure-Portal](sql-database-elastic-pool-manage-portal.md) | Erfahren Sie, wie Sie mithilfe der im Azure-Portal und in der SQL-Datenbank integrierten Intelligenz einen skalierbaren Pool für elastische Datenbanken verwalten, überwachen und skalieren, um die Leistung der Datenbank und die Kostenverwaltung zu optimieren. |
| 81 | [Überwachen und Verwalten eines Pools für elastische Datenbanken mit PowerShell](sql-database-elastic-pool-manage-powershell.md) | Erfahren Sie, wie Sie PowerShell zum Verwalten eines Pools für elastische Datenbanken nutzen |
| 82 | [Überwachen und Verwalten eines Pools für elastische Datenbanken per Transact-SQL](sql-database-elastic-pool-manage-tsql.md) | Es wird beschrieben, wie Sie T-SQL verwenden, um eine Azure SQL-Datenbank in einem elastischen Pool zu erstellen. Außerdem können Sie T-SQL nutzen, um die Datenbank in Pools bzw. aus Pools zu verschieben. |
| 83 | [Abrechnungs- und Preisinformationen für Pools für elastische Datenbanken](sql-database-elastic-pool-price.md) | Hier finden Sie Preisinformationen zu Pools für elastische Datenbanken. |



## Elastische Abfrage

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 84 | [Erstellen von Berichten für horizontal hochskalierte Clouddatenbanken](sql-database-elastic-query-getting-started.md) | Verwenden datenbankübergreifender Datenbankabfragen |
| 85 | [Erste Schritte mit datenbankübergreifenden Abfragen (vertikale Partitionierung, Vorschau)](sql-database-elastic-query-getting-started-vertical.md) | Sie erfahren, wie Sie Abfragen für elastische Datenbanken für vertikal partitionierte Datenbanken verwenden. |
| 86 | [Horizontal hochskalierte Clouddatenbanken übergreifende Berichte (Vorschau)](sql-database-elastic-query-horizontal-partitioning.md) | Informationen zum Einrichten elastischer Abfragen, die horizontal partitionierte Datenbanken übergreifen |
| 87 | [Übersicht über elastische Datenbankenabfragen in Azure SQL-Datenbank (Vorschau)](sql-database-elastic-query-overview.md) | Übersicht über das Abfragefeature für elastische Datenbanken |
| 88 | [Ausführen von Abfragen über Clouddatenbanken mit unterschiedlichen Schemas hinweg (Vorschau)](sql-database-elastic-query-vertical-partitioning.md) | Einrichten von datenbankübergreifenden Abfragen über vertikale Partitionen |



## Elastische Transaktion

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 89 | [Übersicht über elastische Datenbanktransaktionen mit Azure SQL-Datenbank](sql-database-elastic-transactions-overview.md) | Übersicht über elastische Datenbanktransaktionen mit Azure SQL-Datenbank |



## Sicherung und Wiederherstellung

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 90 | [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md) | Informieren Sie sich über integrierte Sicherungen der SQL-Datenbank, mit denen Sie einen früheren Zustand einer Azure SQL-Datenbank wiederherstellen oder eine Datenbank in eine neue Datenbank in einer geografischen Region kopieren können (bis zu 35 Tage). |
| 91 | [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md) | Erfahren Sie, wie Azure SQL-Datenbank die Geschäftskontinuität in der Cloud sowie die Datenbankwiederherstellung unterstützt und dafür sorgt, dass geschäftskritische Cloudanwendungen in Betrieb gehalten werden. |
| 92 | [Wiederherstellen einer einzelnen Tabelle aus einer Azure SQL-Datenbanksicherung](sql-database-cloud-migrate-restore-single-table-azure-backup.md) | Erfahren Sie, wie Sie eine einzelne Tabelle aus einer Azure SQL-Datenbanksicherung wiederherstellen. |
| 93 | [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung mithilfe der aktiven Georeplikation in SQL-Datenbank](sql-database-designing-cloud-solutions-for-disaster-recovery.md) | Erfahren Sie, wie Sie cloudbasierte Notfallwiederherstellungslösungen entwerfen, bei denen die Geschäftskontinuität unter Verwendung der Georeplikation für die App-Datensicherung mit Azure SQL-Datenbank erzielt wird. |
| 94 | [Wiederherstellen einer Azure SQL-Datenbank oder Failover auf eine sekundäre Datenbank](sql-database-disaster-recovery.md) | Erfahren Sie, wie Sie eine Datenbank nach Störungen in einem regionalen Rechenzentrum oder dessen Ausfall mithilfe der aktiven Georeplikation oder der Geowiederherstellung von Azure SQL-Datenbank wiederherstellen. |
| 95 | [Ausführen von Notfallwiederherstellungsverfahren](sql-database-disaster-recovery-drills.md) | Hier erhalten Sie Anleitungen und bewährte Methoden für die Verwendung von Azure SQL-Datenbank zum Ausführen von Notfallwiederherstellungsverfahren, mit denen Sie wichtige Geschäftsanwendungen vor Fehlern und Ausfällen schützen. |
| 96 | [Strategien für die Notfallwiederherstellung für Anwendungen mit elastischem SQL-Datenbankpool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) | Erfahren Sie, wie Sie durch Wahl des richtigen Failovermusters Ihre Cloud-Lösung für die Notfallwiederherstellung entwerfen können. |
| 97 | [Verwenden der RecoveryManager-Klasse zur Behebung von Problemen mit der Shard-Zuordnung](sql-database-elastic-database-recovery-manager.md) | Verwenden der RecoveryManager-Klasse zum Beheben von Problemen mit Shardzuordnungen |
| 98 | [Importieren einer BACPAC-Datei zum Erstellen einer neuen Azure SQL-Datenbank](sql-database-import.md) | Erstellen Sie eine neue Azure SQL-Datenbank, indem Sie eine vorhandene BACPAC-Datei importieren. |
| 99 | [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal](sql-database-point-in-time-restore-portal.md) | Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt |
| 100 | [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell](sql-database-point-in-time-restore-powershell.md) | Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt |
| 101 | [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md) | Point-in-Time-Wiederherstellung, Microsoft Azure SQL-Datenbank, Datenbank wiederherstellen, Datenbankwiederherstellung, klassisches Azure-Portal, klassisches Azure-Portal |
| 102 | [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](sql-database-recovery-using-backups.md) | Hier finden Sie Informationen zur Point-in-Time-Wiederherstellung, mit der Sie ein Rollback für eine Azure SQL-Datenbank durchführen können (bis zu 35 Tage). |
| 103 | [Wiederherstellen einer gelöschten Azure SQL-Datenbank im Azure-Portal](sql-database-restore-deleted-database-portal.md) | Wiederherstellen einer gelöschten Azure SQL-Datenbank (Azure-Portal) |
| 104 | [Wiederherstellen einer gelöschten Azure SQL-Datenbank mit PowerShell](sql-database-restore-deleted-database-powershell.md) | Wiederherstellen einer gelöschten Azure SQL-Datenbank (PowerShell) |
| 105 | [Wiederherstellen einer Datenbank zu einem früheren Zeitpunkt, einer gelöschten Datenbank oder eines Rechenzentrums nach einem Ausfall](sql-database-troubleshoot-backup-and-restore.md) | Erfahren Sie, wie eine Clouddatenbank bei Fehlern oder Ausfällen mithilfe von Sicherungen und Replikaten in Azure SQL-Datenbank wiederhergestellt wird. |



## Migrieren

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 106 | [Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei per SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank exportieren, BACPAC-Datei exportieren, sqlpackage |
| 107 | [Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei mit SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank exportieren, BACPAC-Datei exportieren, Assistent „Datenebenenanwendung exportieren“ |
| 108 | [Importieren aus einer BACPAC-Datei in SQL-Datenbank per SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank importieren, BACPAC-Datei importieren, sqlpackage |
| 109 | [Importieren aus BACPAC zur SQL-Datenbank mithilfe von SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Microsoft Azure SQL-Datenbank, Datenbankbereitstellung, Datenbankmigration, Datenbank importieren, Datenbank exportieren, Migrations-Assistent |
| 110 | [Migrieren von SQL Server-Datenbank auf SQL-Datenbank mit dem Assistenten zum Bereitstellen einer Datenbank unter Microsoft Azure-Datenbank](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Assistent für Microsoft Azure-Datenbank |
| 111 | [Migrieren von SQL Server-Datenbank zu Azure SQL-Datenbank per Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank importieren, Transaktionsreplikation |
| 112 | [Ermitteln der SQL-Datenbankkompatibilität mithilfe von SqlPackage.exe](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, SQL-Datenbankkompatibilität, SqlPackage |
| 113 | [Verwenden Sie SQL Server Management Studio zum Ermitteln der Kompatibilität von SQL-Datenbank vor der Migration zu Azure SQL-Datenbank.](sql-database-cloud-migrate-determine-compatibility-ssms.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, SQL-Datenbankkompatibilität, Assistent „Datenebenenanwendung exportieren“ |
| 114 | [Verwenden Sie den SQL Azure-Migrations-Assistenten, um vor der Migration zu Azure SQL-Datenbank Kompatibilitätsprobleme mit SQL Server-Datenbank zu beheben.](sql-database-cloud-migrate-fix-compatibility-issues.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, Migrations-Assistent für SQL Azure |
| 115 | [Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank mithilfe von SQL Server Data Tools für Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, Migrations-Assistent für SQL Azure, SSDT |
| 116 | [Beheben von Kompatibilitätsproblemen der SQL Server-Datenbank mit SQL Server Management Studio vor der Migration zur SQL-Datenbank](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) | Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, Migrations-Assistent für SQL Azure |
| 117 | [Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell](sql-database-export-powershell.md) | Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell |
| 118 | [Importieren einer BACPAC-Datei zum Erstellen einer neuen Azure SQL-Datenbank mithilfe von PowerShell](sql-database-import-powershell.md) | Importieren einer BACPAC-Datei zum Erstellen einer neuen Azure SQL-Datenbank mithilfe von PowerShell |
| 119 | [Laden von Daten aus einer CSV-Datei in Azure SQL Data Warehouse (Flatfiles)](sql-database-load-from-csv-with-bcp.md) | Für kleinere Datenmengen wird BCP zum Importieren von Daten in Azure SQL-Datenbank verwendet. |
| 120 | [Verschieben von Datenbanken zwischen Servern und Abonnements sowie in und aus Azure](sql-database-troubleshoot-moving-data.md) | Kurzanweisungen zum Kopieren, Verschieben und Migrieren von Daten und Datenbanken in Azure SQL-Datenbank. |



## Verschieben von Daten in und aus Azure

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 121 | [Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank in der Cloud](sql-database-cloud-migrate.md) | Erfahren Sie, wie lokale SQL Server-Datenbanken zu Azure SQL-Datenbank in der Cloud migriert werden. Verwenden Sie Datenbankmigrationstools, um vor der Datenbankmigration die Kompatibilität zu testen. |
| 122 | [Kopieren einer Azure SQL-Datenbank](sql-database-copy.md) | Erstellen der Kopie einer Azure SQL-Datenbank |
| 123 | [Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals](sql-database-export.md) | Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals |



## Sicherheit

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 124 | [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) | Erfahren Sie, wie Sie unter Verwendung der Azure Active Directory-Authentifizierung eine Verbindung mit SQL-Datenbank herstellen. |
| 125 | [Always Encrypted: Schützen von vertraulichen Daten in SQL-Datenbank und Speichern der Verschlüsselungsschlüssel im Windows-Zertifikatspeicher](sql-database-always-encrypted.md) | Schützen Sie vertrauliche Daten in der SQL-Datenbank in wenigen Minuten. |
| 126 | [Always Encrypted – Schützen von vertraulichen Daten in SQL-Datenbank und Speichern der Verschlüsselungsschlüssel in Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md) | Schützen Sie vertrauliche Daten in der SQL-Datenbank in wenigen Minuten. |
| 127 | [SQL-Datenbank – Unterstützung für kompatible Clients und IP-Endpunkt-Änderungen für die Überwachung](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) | Erfahren Sie mehr über die Unterstützung für kompatible Clients und IP-Endpunkt-Änderungen für die Überwachung bei SQL-Datenbank. |
| 128 | [Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe von PowerShell](sql-database-configure-firewall-settings-powershell.md) | Sie erfahren, wie Sie die Firewall für IP-Adressen mit Zugriff auf Azure SQL-Datenbanken konfigurieren. |
| 129 | [Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe der REST-API](sql-database-configure-firewall-settings-rest.md) | Sie erfahren, wie Sie die Firewall für IP-Adressen mit Zugriff auf Azure SQL-Datenbanken konfigurieren. |
| 130 | [Konfigurieren von Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank mithilfe von T-SQL](sql-database-configure-firewall-settings-tsql.md) | Sie erfahren, wie Sie die Firewall für IP-Adressen mit Zugriff auf Azure SQL-Datenbanken konfigurieren. |
| 131 | [Erste Schritte mit der dynamischen Datenmaskierung für SQL-Datenbanken (Azure-Portal)](sql-database-dynamic-data-masking-get-started.md) | Einstieg in die dynamische Datenmaskierung für SQL-Datenbanken im Azure-Portal |
| 132 | [Erste Schritte mit der dynamischen Datenmaskierung für SQL-Datenbanken (klassisches Azure-Portal)](sql-database-dynamic-data-masking-get-started-portal.md) | Einstieg in die dynamische Datenmaskierung für SQL-Datenbanken im klassischen Azure-Portal |
| 133 | [Konfigurieren von Firewallregeln für Azure SQL-Datenbank – Übersicht](sql-database-firewall-configure.md) | Erfahren Sie, wie eine SQL-Datenbankfirewall mit der Firewallregeln auf Server- und Datenbankebene zum Verwalten des Zugriffs konfigurieren. |
| 134 | [SQL Datenbank-Tutorial: Erstellen von SQL-Datenbankbenutzerkonten für den Zugriff auf und die Verwaltung von Datenbanken](sql-database-get-started-security.md) | Erfahren Sie, wie Sie Benutzerkonten für den Zugriff auf und die Verwaltung von Datenbanken erstellen. |
| 135 | [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md) | Erfahren Sie mehr über die Sicherheitsverwaltung für SQL-Datenbank und insbesondere dazu, wie der Datenbankzugriff und die Anmeldesicherheit über das Prinzipalkonto auf Serverebene verwaltet wird. |
| 136 | [Sicherheitsrichtlinien und Einschränkungen von Azure SQL-Datenbank](sql-database-security-guidelines.md) | Enthält Informationen zu den Richtlinien und Einschränkungen von Microsoft Azure SQL-Datenbank, die sich auf die Sicherheit beziehen. |
| 137 | [Erste Schritte mit der Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection-get-started.md) | Erste Schritte mit der Bedrohungserkennung der SQL-Datenbank im Azure-Portal |
| 138 | [Gewusst wie: Ausführen allgemeiner Verwaltungsaufgaben in Azure SQL-Datenbank](sql-database-troubleshoot-permissions.md) | Hier wird beschrieben, wie allgemeine Verwaltungsaufgaben in SQL-Datenbank ausgeführt werden. Sie erfahren beispielsweise, wie Sie das Administratorkennwort zurücksetzen und Zugriff gewähren bzw. entfernen. |



## Verwalten Ihrer Datenbank

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 139 | [Erste Schritte bei der Überwachung von SQL-Datenbank](sql-database-auditing-get-started.md) | Erste Schritte bei der Überwachung von SQL-Datenbank |
| 140 | [Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md) | Hier erfahren Sie, wie Sie die Firewall für IP-Adressen mit Zugriff auf Azure SQL Server konfigurieren. |
| 141 | [Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md) | Erstellen der Kopie einer Azure SQL-Datenbank |
| 142 | [Verwalten von Azure SQL-Datenbanken mit Azure Automation](sql-database-manage-automation.md) | Erfahren Sie, wie der Azure Automation-Dienst zur angemessenen Verwaltung von Azure SQL-Datenbanken verwendet werden kann. |
| 143 | [Übersicht: Verwaltungstools für SQL-Datenbank](sql-database-manage-overview.md) | Vergleicht die Tools und Optionen zum Verwalten von Azure SQL-Datenbank |
| 144 | [Überwachen der Azure SQL-Datenbank mit dynamischen Verwaltungssichten](sql-database-monitoring-with-dmvs.md) | Erfahren Sie, wie Sie allgemeine Leistungsprobleme mithilfe der dynamischen Verwaltungssichten zum Überwachen von Microsoft Azure SQL-Datenbank ermitteln und diagnostizieren. |
| 145 | [Sichern der SQL-Datenbank](sql-database-security.md) | Erfahren Sie mehr über die Sicherheit von Azure SQL-Datenbank und SQL Server, einschließlich der Unterschiede zwischen einem SQL-Server in der Cloud und einem SQL-Server vor Ort hinsichtlich Authentifizierung, Autorisierung, Verbindungssicherheit, Verschlüsselung und Compliance. |



## Erweiterte Ereignisse

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 146 | [Code des Ziels "Ereignisdatei" für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-event-file.md) | Stellt ein PowerShell- und ein Transact-SQL-Skript für ein zweiphasiges Codebeispiel zur Veranschaulichung des Ereignisdateiziels in einem erweiterten Ereignis in Azure SQL-Datenbank bereit. Azure Storage ist ein erforderlicher Bestandteil in diesem Szenario. |
| 147 | [Code des Ringpufferziels für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-ring-buffer.md) | Zeigt ein Transact-SQL-Codebeispiel (für Azure SQL-Datenbank), das durch die Verwendung des Ringpufferziels eine einfache und schnelle Methode zum Erfassen und Ausgeben von Daten bietet. |
| 148 | [Erweiterte Ereignisse in Azure SQL-Datenbank](sql-database-xevent-db-diff-from-svr.md) | Beschreibt erweiterte Ereignisse (XEvents) in Azure SQL-Datenbank und wie sich Ereignissitzungen im Vergleich zu Microsoft SQL Server geringfügig unterscheiden. |



## Georeplikation

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 149 | [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-failover-portal.md) | Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit dem Azure-Portal |
| 150 | [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit PowerShell](sql-database-geo-replication-failover-powershell.md) | Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit PowerShell |
| 151 | [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit Transact-SQL |
| 152 | [Übersicht: Aktive Georeplikation in Azure SQL-Datenbank](sql-database-geo-replication-overview.md) | Die aktive Georeplikation ermöglicht Ihnen das Einrichten von vier Replikaten Ihrer Datenbank in einem beliebigen Azure-Rechenzentrum. |
| 153 | [Konfigurieren der Georeplikation für Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-portal.md) | Konfigurieren der Georeplikation für eine Azure SQL-Datenbank mit dem Azure-Portal |
| 154 | [Konfigurieren der Georeplikation für die Azure SQL-Datenbank mit PowerShell](sql-database-geo-replication-powershell.md) | Konfigurieren der aktiven Georeplikation für die Azure SQL-Datenbank mit PowerShell |
| 155 | [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md) | In diesem Thema werden Sicherheitsaspekte bei der Verwaltung der Sicherheit nach einer Wiederherstellung oder einem Failover einer Datenbank erläutert. |
| 156 | [Konfigurieren der Georeplikation für Azure SQL-Datenbank mit Transact-SQL](sql-database-geo-replication-transact-sql.md) | Konfigurieren der Georeplikation für Azure SQL-Datenbank mithilfe von Transact-SQL |
| 157 | [Geowiederherstellung einer Azure SQL-Datenbank aus einer georedundanten Sicherung über das Azure-Portal](sql-database-geo-restore-portal.md) | Geowiederherstellung einer Azure SQL-Datenbank aus einer georedundanten Sicherung (Azure-Portal) |
| 158 | [Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung mit PowerShell](sql-database-geo-restore-powershell.md) | Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung auf einem neuen Server |



## Upgrade

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 159 | [Verbesserte Abfrageleistung mit Kompatibilitätsgrad 130 in Azure SQL-Datenbank](sql-database-compatibility-level-query-performance-130.md) | Enthält eine Beschreibung der Schritte und Tools, mit denen ermittelt werden kann, welcher Kompatibilitätsgrad für Ihre Datenbank unter Azure SQL-Datenbank oder Microsoft SQL Server am besten geeignet ist. |
| 160 | [Verwalten von parallelen Upgrades von Cloudanwendungen mithilfe der aktiven Georeplikation von SQL-Datenbank](sql-database-manage-application-rolling-upgrade.md) | Erfahren Sie, wie Sie die Georeplikation von Azure SQL-Datenbank verwenden, um Onlineupgrades Ihrer Cloudanwendung zu unterstützen. |
| 161 | [Upgrade auf Azure SQL-Datenbank V12 mithilfe des Azure-Portals](sql-database-upgrade-server-portal.md) | Erläutert das Upgrade auf Azure SQL-Datenbank V12, einschließlich der Aktualisierung von Web- und Business-Datenbanken, sowie das Upgrade eines V11-Servers mit direkter Migration der Datenbanken in einen Pool für elastische Datenbanken mithilfe des Azure-Portals. |
| 162 | [Upgrade auf Azure SQL-Datenbank V12 mithilfe von PowerShell](sql-database-upgrade-server-powershell.md) | Erläutert das Upgrade auf Azure SQL-Datenbank V12, einschließlich der Aktualisierung von Web- und Business-Datenbanken, sowie das Upgrade eines V11-Servers mit direkter Migration der zugehörigen Datenbanken in einen Pool für elastische Datenbanken mithilfe von PowerShell. |
| 163 | [Plan and prepare to upgrade to SQL Database V12 (Planen und Vorbereiten des Upgrades auf die SQL-Datenbank V12; in englischer Sprache)](sql-database-v12-plan-prepare-upgrade.md) | Beschreibt die Vorbereitungen und Einschränkungen im Zusammenhang mit einem Upgrade auf Version V12 der Azure SQL-Datenbank. |
| 164 | [Neuerungen in SQL-Datenbank V12](sql-database-v12-whats-new.md) | Beschreibt, warum Geschäftssysteme, die eine Azure SQL-Datenbank in der Cloud verwenden von einem Upgrade auf Version V12 profitieren. |
| 165 | [Häufig gestellte Fragen zur Einstellung von Web Edition und Business Edition](sql-database-web-business-sunset-faq.md) | Erfahren Sie, wann die Web- und Business-Datenbanken in Azure SQL-Datenbank eingestellt werden und welche Features und Funktionen die neuen Dienstebenen bieten. |



## Tools

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 166 | [Verwalten von Azure SQL-Datenbanken mit PowerShell](sql-database-command-line-tools.md) | Verwalten von Azure SQL-Datenbank mit PowerShell. |
| 167 | [Tutorial zu SQL-Datenbank: Verbinden von Excel mit einer Azure SQL-Datenbank und Erstellen eines Berichts](sql-database-connect-excel.md) | Erfahren Sie, wie Sie Microsoft Excel mit Azure SQL-Datenbank in der Cloud verbinden. Importieren Sie Daten zwecks Berichterstellung und Untersuchung in Excel. |
| 168 | [Erstellen einer SQL-Datenbank und Ausführen gängiger Datenbankeinrichtungsaufgaben mithilfe von PowerShell-Cmdlets](sql-database-get-started-powershell.md) | Es wird beschrieben, wie Sie eine SQL-Datenbank mithilfe von PowerShell erstellen. Allgemeine Datenbankeinrichtungsaufgaben können mithilfe von PowerShell-Cmdlets verwaltet werden. |
| 169 | [Erste Schritte mit Azure SQL-Datensynchronisierung (Vorschauversion)](sql-database-get-started-sql-data-sync.md) | Dieses Lernprogramm unterstützt Sie bei den ersten Schritten mit Azure SQL-Datensynchronisierung (Vorschau). |
| 170 | [Verwalten von Azure SQL-Datenbank mit SQL Server Management Studio](sql-database-manage-azure-ssms.md) | Erfahren Sie, wie Sie SQL Server Management Studio für die Verwaltung von SQL-Datenbankservern und -Datenbanken verwenden. |
| 171 | [Verwalten von Azure SQL-Datenbanken über das Azure-Portal](sql-database-manage-portal.md) | Erfahren Sie, wie Sie das Azure-Portal verwenden, um eine relationale Datenbank mithilfe des Azure-Portals in der Cloud zu verwalten. |
| 172 | [Ändern der Dienstebene und Leistungsstufe (Tarif) einer SQL-Datenbank mit PowerShell](sql-database-scale-up-powershell.md) | In „Ändern der Dienstebene und Leistungsstufe einer Azure SQL-Datenbank“ wird beschrieben, wie Sie die Dienstebene und Leistungsstufe Ihrer SQL-Datenbank mit PowerShell zentral hoch- oder herunterstufen. Ändern des Tarifs einer Azure SQL-Datenbank mit PowerShell. |
| 173 | [Verwenden von SQL Server Management Studio in Azure RemoteApp zum Herstellen einer Verbindung mit SQL-Datenbank](sql-database-ssms-remoteapp.md) | Sie erfahren in diesem Tutorial, wie Sie SQL Server Management Studio in Azure RemoteApp für Sicherheit und Leistung beim Verbinden mit SQL-Datenbank verwenden. |



## Referenz

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 174 | [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md) | Listet die mindestens erforderliche Versionsnummer für jeden Treiber auf, mit der Clientprogramme Verbindungen mit Azure SQL-Datenbank oder Microsoft SQL Server herstellen können. Es wird ein Link zu Versionsinformationen für die Treiber bereitgestellt, die nicht von Microsoft, sondern von der Community freigegeben werden. |
| 175 | [Ressourceneinschränkungen für Azure SQL-Datenbanken](sql-database-resource-limits.md) | Diese Seite beschreibt einige allgemeine Ressourceneinschränkungen für Azure SQL-Datenbanken. |
| 176 | [Azure SQL-Datenbank – Abweichungen bei Transact-SQL](sql-database-transact-sql-information.md) | Transact-SQL-Anweisungen, die in Azure SQL-Datenbank nicht vollständig unterstützt werden |



## Verschiedenes

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 177 | [Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md) | Erstellen der Kopie einer Azure SQL-Datenbank mithilfe von PowerShell |
| 178 | [Kopieren einer Azure SQL-Datenbank mithilfe von Transact-SQL](sql-database-copy-transact-sql.md) | Erstellen der Kopie einer Azure SQL-Datenbank mithilfe von Transact-SQL |
| 179 | [Azure SQL-Datenbanken – Allgemeine Einschränkungen und Leitlinien](sql-database-general-limitations.md) | Auf dieser Seite werden einige allgemeine Einschränkungen von Azure SQL-Datenbank sowie Aspekte der Interoperabilität und Unterstützung beschrieben. |
| 180 | [Tarifempfehlungen für SQL-Datenbank](sql-database-service-tier-advisor.md) | Beim Ändern von Tarifen im Azure-Portal werden Tarifempfehlungen bereitgestellt, die den am besten geeigneten Tarif für die Ausführung des Workloads einer vorhandenen Azure SQL-Datenbank empfehlen. Tarife beschreiben die Dienstebene und die Leistungsebene einer SQL-Datenbank. |


&nbsp;

<hr/>

<a name="extras_append"></a>

## Extras

<a name="extra_related_articles"></a>

### Verwandte Artikel von anderen Azure-Diensten

- [Sichern von Web-Apps in Azure App Service](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### Dokumentationstools

- [Für Azure SQL-Datenbank angekündigte Updates](http://azure.microsoft.com/updates/?service=sql-database)

- [Durchsuchen aller Dokumentationstypen von Microsoft Azure, mit Filtern](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### Lernpfadgrafiken

- [Lernpfaddiagramme für alle Microsoft Azure-Dienste](http://azure.microsoft.com/documentation/learning-paths/)

- [Lernpfad: „SQL-Datenbank-Schulung: Erlernen von Azure SQL Database“](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)

- [Lernpfad: „SQL-Datenbank – Features und Tools elastischer Datenbanken verwenden“](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)


<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->

<!---HONumber=AcomDC_0824_2016-->