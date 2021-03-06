<properties
	pageTitle="Übersicht: Verwaltungstools für SQL-Datenbank"
	description="Vergleicht die Tools und Optionen zum Verwalten von Azure SQL-Datenbank"
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/19/2016"
	ms.author="sstein"/>

# Übersicht: Verwaltungstools für SQL-Datenbank

In diesem Thema werden Tools und Optionen für die Verwaltung von Azure SQL-Datenbanken untersucht und verglichen, damit Sie das Tool auswählen können, das für den Auftrag, für Ihr Unternehmen und für Sie selbst am besten geeignet ist. Die Auswahl des richtigen Tools hängt von der Anzahl der Datenbanken, den Aufgaben und der Ausführungshäufigkeit einer Aufgabe ab.

## Azure-Portal

Das [Azure-Portal](https://portal.azure.com) ist eine webbasierte Anwendung, in der Sie Datenbanken und logische Server erstellen, aktualisieren und löschen sowie Datenbankaktivitäten überwachen können. Dieses Tool eignet sich hervorragend, wenn Sie gerade erst mit der Verwendung von Azure begonnen haben, nur eine kleine Anzahl von Datenbanken verwalten oder bestimmte Aktionen schnell ausführen müssen.

Ausführlichere Informationen zum Verwenden des Portals finden Sie unter [Verwalten von SQL-Datenbanken über das klassische Azure-Portal](sql-database-manage-portal.md).

## SQL Server Management Studio und SQL Server Data Tools in Visual Studio

SQL Server Management Studio (SSMS) und SQL Server Data Tools (SSDT) in Visual Studio sind Clienttools, die auf Ihrem Computer ausgeführt werden und es ermöglichen, Ihre Datenbank mit der Cloud zu verbinden, darin zu verwalten und zu entwickeln. Wenn Sie ein Anwendungsentwickler sind, der mit Visual Studio oder anderen integrierten Entwicklungsumgebungen (IDEs) vertraut ist, sollten Sie [SSDT in Visual Studio ausprobieren](https://msdn.microsoft.com/library/mt204009.aspx). Viele Datenbankadministratoren sind vertraut mit SSMS, das mit Azure SQL-Datenbanken eingesetzt werden kann. [Laden Sie die neueste Version von SSMS herunter](https://msdn.microsoft.com/library/mt238290), und verwenden Sie bei der Arbeit mit Azure SQL-Datenbank stets die neueste Version. Weitere Informationen zum Verwalten von Azure SQL-Datenbanken mit SSMS finden Sie unter [Verwalten von SQL-Datenbanken mit SSMS](sql-database-manage-azure-ssms.md).

> [AZURE.IMPORTANT] Sie müssen immer die neueste Version von SQL Server Management Studio und den SQL Server Data Tools verwenden, damit Visual Studio mit Updates von Microsoft Azure und SQL-Datenbank synchronisiert wird. Ältere Versionen dieser Tools funktionieren nicht mit SQL-Datenbank.

## Befehlszeilentools

Sie können Befehlszeilentools wie z. B. PowerShell zur Verwaltung von Datenbanken und Pools für elastische Datenbanken sowie zur Automatisierung von Azure-Ressourcenbereitstellungen verwenden. Microsoft empfiehlt, dieses Tool zur Verwaltung einer großen Anzahl von Datenbanken und zur Automatisierung von Bereitstellungs- und Ressourcenänderungen in einer Produktionsumgebung zu verwenden.

Weitere Informationen zum Verwalten von Azure SQL-Datenbanken mit Befehlszeilentools finden Sie unter [Verwalten von SQL-Datenbanken mit PowerShell](sql-database-command-line-tools.md)

## Tools für elastische Datenbanken
Verwenden Sie Tools für elastische Datenbanken, um folgende Aktionen auszuführen:

* Ausführen eines T-SQL-Skripts für einen Satz von Datenbanken mithilfe eines [elastischen Auftrags](sql-database-elastic-jobs-overview.md)
* Verschieben von Datenbanken aus einem mehrinstanzenfähigen Modell zu einem Modell mit einem einzelnen Mandanten mithilfe des [Split-Merge-Tools](sql-database-elastic-scale-overview-split-and-merge.md)
* Verwalten von Datenbanken in einem Modell mit einem einzelnen Mandanten oder einem mehrinstanzenfähigen Modell mithilfe der [Clientbibliothek für die elastische Skalierung](sql-database-elastic-database-client-library.md)
 

<!---HONumber=AcomDC_0720_2016-->