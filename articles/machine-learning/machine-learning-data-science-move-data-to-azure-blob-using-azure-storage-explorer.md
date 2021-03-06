<properties 
	pageTitle="Verschieben von Daten in den und aus dem Azure-Blobspeicher mithilfe des Azure-Speicher-Explorers | Microsoft Azure" 
	description="Verschieben von Daten in den und aus dem Azure-Blobspeicher mithilfe des Azure-Speicher-Explorers" 
	services="machine-learning,storage" 
	documentationCenter="" 
	authors="bradsev" 
	manager="jhubbard" 
	editor="cgronlun" />

<tags 
	ms.service="machine-learning" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/31/2016"
	ms.author="bradsev" />

# Verschieben von Daten in den und aus dem Azure-Blobspeicher mithilfe des Azure-Speicher-Explorers

Der Azure-Speicher-Explorer ist ein kostenloses Tool von Microsoft, das Ihnen das Arbeiten mit Azure Storage-Daten unter Windows, MacOS und Linux ermöglicht. Dieses Thema beschreibt Verwendung des Tools zum Hoch- und Herunterladen von Daten aus einem Azure-Blobspeicher. Das Tool kann von der Seite [Microsoft Azure Storage Explorer](http://storageexplorer.com/) heruntergeladen werden.

Nachstehend finden Sie Links zu Anleitungen für Technologien zum Verschieben von Daten in den und aus dem Azure-BLOB-Speicher:
 
[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

 
> [AZURE.NOTE] Wenn Sie einen virtuellen Computer verwenden, der mit den von den [virtuellen Data Science-Computern in Azure](machine-learning-data-science-virtual-machines.md) bereitgestellten Skripts eingerichtet wurde, ist der Azure-Speicher-Explorer bereits auf dem virtuellen Computer installiert.
 
> [AZURE.NOTE] Eine umfassende Einführung in Azure-Blobspeicher finden Sie unter [Grundlagen zu Azure-Blobspeicher](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure-Blobdienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).

## Voraussetzungen

In diesem Dokument wird davon ausgegangen, dass Sie über ein Azure-Abonnement, ein Speicherkonto und den zugehörigen Speicherschlüssel für dieses Konto verfügen. Bevor Sie Daten hoch- und herunterladen können, müssen Sie den Namen Ihres Azure-Speicherkontos und den Kontoschlüssel kennen.

- Informationen zum Einrichten eines Azure-Abonnements finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).
- Anleitungen zum Erstellen eines Speicherkontos und zum Abrufen von Konto- und Schlüsselinformationen finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md). Notieren Sie sich den Zugriffsschlüssel für Ihr Speicherkonto, da Sie diesen Schlüssel benötigen, um über das Tool Azure-Speicher-Explorer eine Verbindung mit dem Konto herzustellen.
- Der Azure-Speicher-Explorer kann von der Seite [Microsoft Azure Storage Explorer](http://storageexplorer.com/) heruntergeladen werden. Übernehmen Sie während der Installation die Standardwerte.


<a id="explorer"></a>
## Verwenden von Azure Storage Explorer 

Die folgenden Schritte beschreiben das Hoch- und Herunterladen von Daten mithilfe von Azure Storage Explorer.

1.  Starten Sie den Microsoft Azure-Speicher-Explorer.
2.  Um den Assistenten **Bei Ihrem Konto anmelden...** zu öffnen, wählen Sie das Symbol **Azure-Kontoeinstellungen** und dann die Option **Konto hinzufügen** aus, und geben Sie Ihre Anmeldeinformationen ein.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3.  Um den Assistenten **Verbindung mit Azure-Speicher herstellen** zu öffnen, wählen Sie das Symbol **Verbindung mit Azure-Speicher herstellen** aus.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Geben Sie den Zugriffsschlüssel Ihres Azure-Speicherkontos in den Assistenten **Verbindung mit Azure-Speicher herstellen** ein, und klicken Sie dann auf **Weiter**.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Geben Sie im Feld **Kontoname** den Namen des Speicherkontos ein, und wählen Sie **Weiter** aus.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Das hinzugefügte Speicherkonto sollte jetzt aufgeführt werden. Um in einem Speicherkonto einen Blobcontainer zu erstellen, klicken Sie mit der rechten Maustaste auf den Knoten **Blobcontainer** in diesem Konto, wählen Sie die Option **Blobcontainer erstellen** aus, und geben Sie einen Namen ein.
7. Um Daten in einen Container hochzuladen, wählen Sie den Zielcontainer aus, und klicken Sie auf die Schaltfläche **Hochladen**.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Klicken Sie auf die Schaltfläche **...** rechts neben dem Feld **Dateien**, wählen Sie aus dem Dateisystem eine oder mehrere Dateien zum Hochladen aus, und klicken Sie auf **Hochladen**, um mit dem Hochladen der Dateien zu beginnen.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
7. Um Daten herunterzuladen, wählen Sie das Blob im entsprechenden Container aus und klicken auf **Herunterladen**.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

<!---HONumber=AcomDC_0914_2016-->