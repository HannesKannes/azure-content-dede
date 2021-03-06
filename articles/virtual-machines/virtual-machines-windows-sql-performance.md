<properties
	pageTitle="Bewährte Methoden für die Leistung von SQL Server | Microsoft Azure"
	description="Dieses Thema enthält bewährte Methoden zur Optimierung der Leistung von SQL Server in Microsoft Azure Virtual Machines."
	services="virtual-machines-windows"
	documentationCenter="na"
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-service-management" />

<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="09/07/2016"
	ms.author="jroth" />

# Bewährte Methoden zur Leistung für SQL Server auf virtuellen Azure-Computern

## Übersicht

Dieses Thema enthält bewährte Methoden zur Optimierung der Leistung von SQL Server in Microsoft Azure Virtual Machines. Es empfiehlt sich, beim Ausführen von SQL Server in Azure Virtual Machines weiterhin die gleichen Optionen zur Optimierung der Datenbankleistung zu verwenden, die für SQL Server in der lokalen Server-Umgebung gelten. Die Leistung einer relationalen Datenbank in einer öffentlichen Cloud hängt jedoch von vielen Faktoren ab, z. B. der Größe eines virtuellen Computers und der Konfiguration der Datenträger für Daten.

Wenn Sie SQL Server-Images erstellen, [empfiehlt sich die Bereitstellung der virtuellen Computer im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md). Über virtuelle SQL Server-Computer, die im Portal mit Resource Manager bereitgestellt werden, werden alle diese optimalen Verfahren implementiert, darunter auch die Speicherkonfiguration.

Dieser Artikel konzentriert sich auf die *optimale* Leistung von SQL Server auf Azure-VMs. Wenn Ihre Workload weniger anspruchsvoll ist, sind möglicherweise nicht alle unten aufgeführten Optimierungen erforderlich. Berücksichtigen Sie bei der Evaluierung dieser Empfehlungen Ihre Leistungsanforderungen und Workloadmuster.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## Checkliste

Im folgenden finden eine kurze Checkliste für die optimale Leistung von SQL Server in Azure Virtual Machines:

|Bereich|Optimierungen|
|---|---|
|[Größe des virtuellen Computers](#vm-size-guidance)|[DS3](virtual-machines-windows-sizes.md#standard-tier-ds-series) oder höher für SQL Enterprise Edition.<br/><br/>[DS2](virtual-machines-windows-sizes.md#standard-tier-ds-series) oder höher für SQL Standard und Web Edition.|
|[Speicher](#storage-guidance)|Verwenden Sie [Storage Premium](../storage/storage-premium-storage.md). Storage Standard empfiehlt sich nur für Entwicklungs- und Testumgebungen.<br/><br/>Das [Speicherkonto](../storage/storage-create-storage-account.md) und der virtuelle SQL Server-Computer müssen sich in derselben Region befinden.<br/><br/>Deaktivieren Sie auf dem Speicherkonto den [georedundanten Azure-Speicher](../storage/storage-redundancy.md) (Georeplikation).|
|[Datenträger](#disks-guidance)|Verwenden Sie mindestens 2 [P30-Datenträger](../storage/storage-premium-storage.md#scalability-and-performance-targets-whde-DEing-premium-storage) (1 für Protokolldateien, 1 für Datendateien und „tempdb“).<br/><br/>Verwenden Sie keine Betriebssystem- oder temporären Datenträger für die Datenbankspeicherung oder -protokollierung.<br/><br/>Aktivieren Sie das Caching von Lesevorgängen auf den Datenträgern, auf denen die Datendateien und „tempdb“ gehostet werden.<br/><br/>Aktivieren Sie Caching nicht auf Datenträgern, auf denen die Protokolldatei gehostet wird.<br/><br/>Wichtig: Beenden Sie den SQL Server-Dienst beim Ändern der Cacheeinstellungen für einen Azure-VM-Datenträger.<br/><br/>Kombinieren Sie mehrere Azure-Datenträger über Striping, um einen höheren E/A-Durchsatz zu erhalten.<br/><br/>Formatieren Sie mit dokumentierten Zuordnungsgrößen.|
|[E/A](#io-guidance)|Aktivieren Sie die Datenbankseitenkomprimierung.<br/><br/>Aktivieren Sie die sofortige Dateiinitialisierung für Datendateien.<br/><br/>Begrenzen oder deaktivieren Sie die automatische Vergrößerung für die Datenbank.<br/><br/>Deaktivieren Sie die automatische Verkleinerung für die Datenbank.<br/><br/>Verschieben Sie alle Datenbanken auf Datenträger für Daten, einschließlich der Systemdatenbanken.<br/><br/>Verschieben Sie Verzeichnisse für SQL Server-Fehlerprotokoll- und -Ablaufverfolgungsdateien auf Datenträger für Daten.<br/><br/>Richten Sie Standardspeicherorte für Sicherungen und Datenbankdateien ein.<br/><br/>Aktivieren Sie gesperrte Seiten.<br/><br/>Wenden Sie SQL Server-Leistungs-Hotfixpakete an.|
|[Funktionsspezifisch](#feature-specific-guidance)|Sichern Sie direkt in den Blobspeicher.|

Weitere Informationen dazu, *wie* und *warum* diese Optimierungen vorgenommen werden, finden Sie in den Details und den Leitfäden in den folgenden Abschnitten.

## Leitfaden für die Größe virtueller Computer

Für leistungsabhängige Anwendungen empfiehlt sich die Verwendung der folgenden [Größen für virtuelle Computer](virtual-machines-windows-sizes.md):

- **SQL Server Enterprise Edition**: DS3 oder höher

- **SQL Server Standard und Web Edition**: DS2 oder höher


## Leitfaden für Speicher

Virtuelle Computer der DS-Serie (sowie der DSv2- und GS-Serie) unterstützen [Storage Premium](../storage/storage-premium-storage.md). Für alle Produktionsworkloads wird Storage Premium empfohlen.

> [AZURE.WARNING] Storage Standard verfügt über unterschiedliche Latenzen und Bandbreiten und sollte nur für Entwicklungs-/Testworkloads verwendet werden. Für Produktionsworkloads sollte Storage Premium verwendet werden.

Außerdem sollten Sie Ihr Azure-Speicherkonto im selben Rechenzentrum wie Ihre SQL Server-VMs erstellen, um Übertragungsverzögerungen zu verringern. Wenn Sie ein Speicherkonto erstellen, deaktivieren Sie die Georeplikation, da keine konsistente Schreibreihenfolge über mehrere Datenträger gewährleistet werden kann. Konfigurieren Sie stattdessen eine SQL Server-Notfallwiederherstellungstechnologie zwischen zwei Azure-Rechenzentren. Weitere Informationen finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## Leitfaden für Datenträger

Es gibt drei Haupttypen von Datenträgern auf einer Azure-VM:

- **Betriebssystemdatenträger**: Wenn Sie einen virtuellen Azure-Computer erstellen, fügt die Plattform mindestens einen Datenträger (mit der Bezeichnung Laufwerk **C:**) für das Betriebssystem zum virtuellen Computer hinzu. Bei diesem Datenträger handelt es sich um eine VHD, die als Seitenblob gespeichert wird.
- **Temporärer Datenträger**: Virtuelle Azure-Computer enthalten einen weiteren Datenträger, der als temporärer Datenträger bezeichnet wird (mit der Bezeichnung Laufwerk **D:**). Dies ist ein Datenträger für temporären Speicherbereich auf dem Knoten.
- **Datenträger für Daten**: Sie können auch weitere Datenträger für Daten zum virtuellen Computer hinzufügen. Diese werden als Seitenblobs im Speicher gespeichert.

In den folgenden Abschnitten werden Empfehlungen zur Verwendung dieser unterschiedlichen Datenträger erläutert.

### Betriebssystem-Datenträger

Ein Betriebssystem-Datenträger ist eine virtuelle Festplatte, die Sie als aktive Version eines Betriebssystems starten und einbinden können, und die als Laufwerk **C** bezeichnet ist.

Die für den Betriebssystem-Datenträger verwendete Caching-Standardrichtlinie ergibt **Lesen/Schreiben**. Für leistungsabhängige Anwendungen empfiehlt sich die Verwendung von Datenträgern für Daten anstelle des Betriebssystem-Datenträgers. Weitere Informationen finden Sie im Abschnitt über Datenträger für Daten weiter unten.

### Temporärer Datenträger

Das temporäre Speicherlaufwerk, das als Laufwerk **D**: bezeichnet wird, wird nicht für Azure-BLOB-Speicher beibehalten. Speichern Sie die Datenbank- oder Transaktionsprotokolldateien für Ihre Benutzer nicht auf Laufwerk **D:**.

Das temporäre Laufwerk auf virtuellen Computern der D-Serie, Dv2-Serie und G-Serie ist SSD-basiert. Bei hoher Workload von „tempdb“ (z.B. für temporäre Objekte oder komplexe Verknüpfungen) kann das Speichern von „tempdb“ auf Laufwerk **D:** zu einem höheren Durchsatz und einer geringeren Latenz von „tempdb“ führen.

Für virtuelle Computer, die Storage Premium unterstützen (DS-Serie, DSv2-Serie und GS-Serie), sollten Sie „tempdb“ und/oder Pufferpoolerweiterungen auf einem Datenträger speichern, der Storage Premium mit aktiviertem Lesecache unterstützt. Es gibt eine Ausnahme von dieser Empfehlung: Wenn die „tempdb“-Auslastung schreibintensiv ist, können Sie eine höhere Leistung erzielen, indem Sie „tempdb“ auf dem lokalen Laufwerk **D:** speichern, das für diese Computergrößen auch SSD-basiert ist.

### Datenträger

- **Verwenden Sie Datenträger für Daten für Daten- und Protokolldateien**: Verwenden Sie mindestens 2 Storage Premium [P30-Datenträger](../storage/storage-premium-storage.md#scalability-and-performance-targets-whde-DEing-premium-storage), wobei sich auf einem Datenträger die Protokolldateien und auf dem anderen die Datendateien und „tempdb“ befinden.

- **Datenträgerstriping**: Für einen höheren Durchsatz können Sie zusätzliche Datenträger für Daten hinzufügen und Datenträgerstriping verwenden. Um die Anzahl der Datenträger für Daten zu bestimmen, müssen Sie die Anzahl der für Ihre Daten- und Protokolldatei-Datenträger verfügbaren IOPS analysieren. Diese Informationen finden Sie in den Tabellen zu IOPS pro [Größe des virtuellen Computers](virtual-machines-windows-sizes.md) und Datenträgergröße in folgendem Artikel: [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../storage/storage-premium-storage.md). Verwenden Sie die folgenden Richtlinien:

	- Verwenden Sie [Speicherplätze](https://technet.microsoft.com/library/hh831739.aspx) für Windows 8 und Windows Server 2012 oder höher. Legen Sie die Stripesetgröße für OLTP-Workloads auf 64 KB und für Data Warehousing-Workloads auf 256 KB fest, um Leistungseinbußen durch falsche Partitionsausrichtung zu vermeiden. Legen Sie außerdem Anzahl der Spalten = Anzahl der physischen Datenträger fest. Um einen Speicherplatz mit mehr als 8 Datenträgern zu konfigurieren, müssen Sie PowerShell (nicht die Server-Manager-Benutzeroberfläche) verwenden, um die Anzahl der Spalten explizit so festzulegen, dass sie der Anzahl der Datenträger entspricht. Weitere Informationen zum Konfigurieren von [Speicherplätzen](https://technet.microsoft.com/library/hh831739.aspx) finden Sie unter [Speicherplatz-Cmdlets in Windows PowerShell](https://technet.microsoft.com/library/jj851254.aspx).

	- Für Windows 2008 R2 oder früher können Sie dynamische Datenträger (Betriebssystem-Stripesetvolumes) verwenden, und die Stripesetgröße ist immer 64 KB. Beachten Sie, dass diese Option seit Windows 8 / Windows Server 2012 veraltet ist. Informationen hierzu finden Sie in der Supporterklärung unter [Virtual Disk Service is transitioning to Windows Storage Management API](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx) (Übergang des Diensts für virtuelle Datenträger in die Windows-Speicherverwaltungs-API, in englischer Sprache).

	- Wenn Ihre Workload nicht protokollintensiv ist und keine dedizierten IOPS benötigt werden, können Sie einen einzigen Speicherpool konfigurieren. Andernfalls erstellen Sie zwei Speicherpools, einen für die Protokolldatei(en) und einem weiteren für die Datendatei(en) und TempDB. Bestimmen Sie die Anzahl der jedem Speicherpool zugeordneten Datenträger basierend auf der erwarteten Auslastung. Bedenken Sie, dass verschiedene VM-Größen unterschiedlich viele angefügte Datenträger für Daten unterstützen. Weitere Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-windows-sizes.md).

	- Wenn Sie Storage Premium nicht verwenden (Entwicklungs-/Testszenarien), lautet die Empfehlung, die für Ihre [Größe des virtuellen Computers](virtual-machines-windows-sizes.md) maximal unterstützte Anzahl von Datenträgern für Daten hinzuzufügen und Datenträgerstriping zu verwenden.

- **Cacherichtlinie**: Aktivieren Sie für Storage Premium-Datenträger für Daten das Zwischenspeichern für Lesevorgänge nur für die Datenträger, auf denen Ihre Datendateien und „tempdb“ gehostet werden. Wenn Sie Storage Premium nicht verwenden, aktivieren Sie Caching für keinen Datenträger für Daten. Informationen zum Konfigurieren des Datenträgercachings finden Sie in den folgenden Themen: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) und [Set-AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

	>[AZURE.WARNING] Beenden Sie den SQL Server-Dienst beim Ändern der Cacheeinstellung der Azure-VM-Datenträger, um eine Beschädigung der Datenbank zu vermeiden.

- **Größe der NTFS-Zuordnungseinheiten**: Es empfiehlt sich, beim Formatieren des Datenträgers für Daten als Größe der Zuordnungseinheiten für Daten- und Protokolldateien sowie für TempDB einen Wert von 64 KB zu verwenden.

- **Bewährte Methoden für die Datenträgerverwaltung**: Beenden Sie den SQL Server-Dienst, wenn Sie einen Datenträger für Daten entfernen oder dessen Cachetyp ändern. Wenn die Cacheeinstellungen auf dem Betriebssystemdatenträger geändert werden, beendet Azure den virtuellen Computer, ändert den Cachetyp und startet den virtuellen Computer neu. Wenn die Cacheeinstellungen eines Datenträgers für Daten geändert werden, wird der virtuelle Computer nicht beendet, aber der Datenträger für Daten wird während der Änderung vom virtuellen Computer getrennt und dann erneut angefügt.

	>[AZURE.WARNING] Beenden Sie den SQL Server-Dienst während dieser Vorgänge nicht, kann eine Beschädigung der Datenbank die Folge sein.

## E/A-Leitfaden

- Die besten Ergebnisse mit Storage Premium werden erzielt, wenn Sie Ihre Anwendung und Anforderungen parallelisieren. Storage Premium ist für Szenarios bestimmt, in denen die E/A-Warteschlangentiefe größer als 1 ist, sodass nur geringe oder gar keine Leistungssteigerungen für serielle Singlethread-Anforderungen erreicht werden (auch wenn sie speicherintensiv sind). Dies kann sich z. B. auf die Singlethread-Testergebnisse der Leistungsanalysetools, z. B. SQLIO, auswirken.

- Ziehen Sie die Verwendung der [Datenbankseitenkomprimierung](https://msdn.microsoft.com/library/cc280449.aspx) in Betracht, weil Sie dadurch die Leistung von E/A-intensiven Workloads verbessern können. Die Datenkomprimierung erhöht jedoch möglicherweise den CPU-Verbrauch auf dem Datenbankserver.

- Erwägen Sie die Aktivierung der sofortigen Dateiinitialisierung, um die für die anfängliche Dateizuordnung erforderliche Zeit zu verringern. Um die sofortige Dateiinitialisierung nutzen zu können, gewähren Sie dem SQL Server-Dienstkonto (MSSQLSERVER) die Berechtigung SE\_MANAGE\_VOLUME\_NAME, und fügen Sie es zur Sicherheitsrichtlinie **Durchführen von Volumewartungsaufgaben** hinzu. Wenn Sie ein SQL Server-Plattformimage für Azure verwenden, wird das Standarddienstkonto ("NT Service\\MSSQLSERVER") nicht der Sicherheitsrichtlinie **Durchführen von Volumewartungsaufgaben** hinzugefügt. Anders ausgedrückt: Die sofortige Dateiinitialisierung wird in einem SQL Server Azure-Plattformimages nicht aktiviert. Starten Sie den SQL Server-Dienst neu, nachdem Sie das SQL Server-Dienstkonto der Sicherheitsrichtlinie **Durchführen von Volumewartungsaufgaben** hinzugefügt haben. Möglicherweise sind bei Verwendung dieses Features bestimmte Sicherheitsaspekte zu berücksichtigen. Weitere Informationen finden Sie unter [Datenbankdatei-Initialisierung](https://msdn.microsoft.com/library/ms175935.aspx).

- Die **automatische Vergrößerung** gilt lediglich als Behelfslösung für unerwartetes Wachstum. Führen Sie die tägliche Verwaltung Ihres Daten- und Protokolldateiwachstums nicht durch automatische Vergrößerung. Wenn die automatische Vergrößerung verwendet wird, vergrößern Sie die Datei vorab mit dem Switch "Größe".

- Stellen Sie sicher, dass **Automatische Verkleinerung** deaktiviert ist, um unnötigen Mehraufwand zu vermeiden, der sich negativ auf die Leistung auswirken kann.

- Verschieben Sie alle Datenbanken, einschließlich der Systemdatenbanken, auf Datenträger für Daten. Weitere Informationen finden Sie unter [Verschieben von Systemdatenbanken](https://msdn.microsoft.com/library/ms345408.aspx).

- Verschieben Sie die Verzeichnisse für das SQL Server-Fehlerprotokoll und die Ablaufverfolgungsdateien auf die Datenträger für Daten. Dies können Sie mit dem SQL Server-Konfigurations-Manager vornehmen, indem Sie mit der rechten Maustaste auf die SQL Server-Instanz klicken und „Eigenschaften“ auswählen. Die Einstellungen für Fehlerprotokolle und Ablaufverfolgungsdateien können auf der Registerkarte **Startparameter** geändert werden. Das Sicherungsverzeichnis wird auf der Registerkarte **Erweitert** angegeben. Der folgende Screenshot zeigt, wo sich der Startparameter für Fehlerprotokolle befindet.

	![Screenshot eines SQL-Fehlerprotokolls](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

- Richten Sie standardmäßige Dateispeicherorte für Sicherungen und Datenbanken ein. Verwenden Sie die Empfehlungen in diesem Thema, und nehmen Sie die Änderungen im Fenster mit den Servereigenschaften vor. Eine Anleitung finden Sie unter [Anzeigen oder Ändern der Standardspeicherorte für Daten- und Protokolldateien (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). Der folgende Screenshot zeigt, wo diese Änderungen vorgenommen werden.

	![Datenprotokoll- und Sicherungsdateien für SQL](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)

- Aktivieren Sie gesperrte Seiten, um E/A- und Auslagerungsaktivitäten zu verringern. Weitere Informationen finden Sie unter [Aktivieren der Option zum Sperren von Seiten im Speicher (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

- Wenn Sie SQL Server 2012 ausführen, installieren Sie Service Pack 1, kumulatives Update 10. Dieses Update enthält ein Fix für schlechte E/A-Leistung beim Ausführen von SELECT in temporären Tabellenanweisungen in SQL Server 2012 Informationen hierzu finden Sie in diesem [Knowledge Base-Artikel](http://support.microsoft.com/kb/2958012).

- Es könnte auch sinnvoll sein, alle Datendateien beim Übertragen in bzw. aus Azure zu komprimieren.

## Funktionsspezifischer Leitfaden

Für manche Bereitstellungen können durch Verwendung erweiterter Konfigurationsmethoden zusätzliche Leistungsvorteile erzielt werden. In der folgenden Liste sind einige SQL Server-Features zusammengestellt, mit denen Sie eine bessere Leistung erzielen können:

- **Sicherung in Azure Storage**: Informationen zum Durchführen von Sicherungen für SQL Server, die in Azure Virtual Machines ausgeführt werden, finden Sie unter [SQL Server-Sicherung über URLs](https://msdn.microsoft.com/library/dn435916.aspx). Dieses Feature steht ab SQL Server 2012 SP1 CU2 zur Verfügung und wird für Sicherungen auf die angefügten Datenträgern für Daten empfohlen. Bei Sicherungen/Wiederherstellungen nach/von Azure Storage folgen Sie den Empfehlungen unter [SQL Server-URL-Sicherung – bewährte Methoden und Problembehandlung](https://msdn.microsoft.com/library/jj919149.aspx) und „Wiederherstellen von in Microsoft Azure gespeicherten Sicherungen“. Darüber hinaus können Sie diese Sicherungen mit [automatisierten Sicherungen für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-classic-sql-automated-backup.md) automatisieren.

	In Versionen vor SQL Server 2012 können Sie das [Microsoft SQL Server Backup to Microsoft Azure Tool](https://www.microsoft.com/download/details.aspx?id=40740) verwenden. Dieses Tool hilft, den Sicherungsdurchsatz durch Verwenden mehrerer Sicherungsstripeset-Ziele zu erhöhen.

- **SQL Server-Datendateien in Azure**: Das neue Feature [SQL Server-Datendateien in Microsoft Azure](https://msdn.microsoft.com/library/dn385720.aspx) ist ab SQL Server 2014 verfügbar. Das Ausführen von SQL Server mit Datendateien in Azure zeigt vergleichbare Leistungseigenschaften wie die Verwendung von Azure-Datenträgern.

## Nächste Schritte

Ausführliche Informationen zu SQL Server und Storage Premium finden Sie im Artikel [Verwenden von Azure Storage Premium mit SQL Server auf virtuellen Computern](virtual-machines-windows-classic-sql-server-premium-storage.md).

Bewährte Methoden für die Sicherheit finden Sie unter [Sicherheitsüberlegungen für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-security.md).

Weitere Themen zu virtuellen SQL Server-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

<!---HONumber=AcomDC_0907_2016-->