<properties
	pageTitle="Anpassen von HDInsight-Clustern mit Skriptaktionen | Microsoft Azure"
	description="Erfahren Sie, wie Sie mit Skriptaktionen benutzerdefinierte Komponenten zu Linux-basierten HDInsight-Clustern hinzufügen. Bei Skriptaktionen handelt es sich um Bash-Skripts auf den Clusterknoten. Sie können zum Anpassen der Clusterkonfiguration oder zum Hinzufügen zusätzlicher Dienste und Hilfsprogramme wie Hue, Solr oder R verwendet werden."
	services="hdinsight"
	documentationCenter=""
	authors="Blackmist"
	manager="jhubbard"
	editor="cgronlun"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/06/2016"
	ms.author="larryfr"/>

# Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen

HDInsight verfügt über eine Konfigurationsoption mit der Bezeichnung **Skriptaktion**, mit der benutzerdefinierte Skripts zum Anpassen des Clusters aufgerufen werden. Diese Skripts können während der Erstellung des Clusters oder in einem bereits ausgeführten Cluster verwendet werden und dienen zum Installieren zusätzlicher Komponenten oder Ändern von Konfigurationseinstellungen.

> [AZURE.NOTE] Die Möglichkeit zum Verwenden von Skriptaktionen in einem bereits ausgeführten Cluster ist nur für Linux-basierte HDInsight-Cluster verfügbar. Informationen zur Verwendung von Skriptaktionen mit Windows-basierten Clustern finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen (Windows)](hdinsight-hadoop-customize-cluster.md).

Skriptaktionen können auch als HDInsight-Anwendung im Azure Marketplace veröffentlicht werden. Einige der Beispiele in diesem Dokument zeigen, wie Sie eine HDInsight-Anwendung mit Skriptaktionsbefehlen aus PowerShell und .NET SDK installieren können. Weitere Informationen zu HDInsight-Anwendungen finden Sie unter [Veröffentlichen von HDInsight-Anwendungen im Azure Marketplace](hdinsight-apps-publish-applications.md).

## Grundlegendes zu Skriptaktionen

Eine Skriptaktion ist einfach ein Bash-Skript, für das Sie eine URL und die Parameter angeben und das dann auf den HDInsight-Clusterknoten ausgeführt wird. Unten sind die Merkmale und Features von Skriptaktionen aufgeführt.

* Sie müssen als URI gespeichert werden, der über den HDInsight-Cluster verfügbar ist. Dies sind zwei mögliche Speicherorte:

    * Ein Blob-Speicherkonto, das entweder das primäre oder das zusätzliche Speicherkonto für den HDInsight-Cluster darstellt. Da HDInsight während der Clustererstellung Zugriff auf beide Typen von Speicherkonten gewährt wird, stellen diese eine Möglichkeit dar, eine nicht öffentliche Skriptaktion zu verwenden.
    
    * Ein öffentlich lesbarer URI wie ein Azure-Blob, GitHub, OneDrive, Dropbox usw.
    
    Beispiele für den URI für Skripts, die im Blobcontainer (öffentlich lesbar) gespeichert werden, finden Sie im Abschnitt [Script Action-Beispielskripts](#example-script-action-scripts).

* Sie können auf die __ausschließliche Ausführung auf bestimmten Knotentypen__ beschränkt werden, z.B. Hauptknoten oder Workerknoten.

    > [AZURE.NOTE] Bei Verwendung mit HDInsight Premium können Sie angeben, dass das Skript auf dem Edgeknoten verwendet werden sollte.

* Es kann __permanent__ oder __ad-hoc__ sein.

    __Permanente__ Skripts sind Skripts, die auf Workerknoten angewendet und automatisch auf neuen Knoten ausgeführt werden, die beim zentralen Hochskalieren eines Clusters erstellt werden.

    Mit einem permanenten Skript können außerdem Änderungen auf einen anderen Knotentyp angewendet werden, z.B. einen Hauptknoten. Aus Sicht der Funktionalität besteht der einzige Grund für die Verwendung eines permanenten Skripts aber darin, dass es auf neue Workerknoten angewendet wird, die beim horizontalen Hochskalieren eines Clusters erstellt werden.

    > [AZURE.IMPORTANT] Permanente Skriptaktionen müssen einen eindeutigen Namen haben.

    __Ad-hoc__-Skripts sind nicht permanent. Sie können ein Ad-hoc-Skript aber nachträglich in ein permanentes Skript oder ein permanentes Skript in ein Ad-hoc-Skript umwandeln.

    > [AZURE.IMPORTANT] Skriptaktionen, die während der Clustererstellung verwendet werden, werden automatisch zu permanenten Skripts.
    >
    > Skripts, deren Ausführung nicht erfolgreich ist, werden nicht zu permanenten Skripts. Dies gilt auch, wenn Sie speziell angeben, dass dies der Fall sein soll.

* Sie können __Parameter__ akzeptieren, die von den Skripts während der Ausführung verwendet werden.

* Sie werden mit __Stammebenenberechtigungen__ auf den Clusterknoten ausgeführt.

* Sie können über das __Azure-Portal__, __Azure PowerShell__, die __Azure-Befehlszeilenschnittstelle (CLI)__ oder das __HDInsight .NET SDK__ verwendet werden.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

Als Hilfe bei der Ermittlung, welche Skripts auf einen Cluster angewendet wurden, und bei der Bestimmung der ID von Skripts für die Höher- oder Herunterstufung wird im Cluster der Verlauf aller ausgeführten Skripts gespeichert.

> [AZURE.IMPORTANT] Es gibt keine automatische Möglichkeit, die von einer Skriptaktion vorgenommenen Änderungen rückgängig zu machen. Falls Sie die Auswirkungen eines Skripts umkehren möchten, müssen Sie wissen, welche Änderungen vorgenommen wurden, und diese manuell rückgängig machen (oder eine Skriptaktion bereitstellen, mit der dieser Vorgang durchgeführt wird).

### Skriptaktionen im Clustererstellungsvorgang

Während der Clustererstellung verwendete Skriptaktionen unterscheiden sich leicht von Skriptaktionen, die in einem vorhandenen Cluster ausgeführt wurden:

* Das Skript wird __automatisch dauerhaft gespeichert__.

* Ein __Fehler__ im Skript kann dazu führen, dass die Clustererstellung nicht erfolgreich ist.

Das folgende Diagramm veranschaulicht, wann Skriptaktionen während des Erstellungsvorgangs ausgeführt werden:

![HDInsight-Clusteranpassung und Phasen während der Clustererstellung][img-hdi-cluster-states]

Das Skript wird ausgeführt, während HDInsight konfiguriert wird. In dieser Phase wird das Skript parallel auf allen angegebenen Knoten im Cluster ausgeführt. Die Ausführung erfolgt dabei mit Stammberechtigungen für die Knoten.

> [AZURE.NOTE] Da das Skript mit Stammebenenberechtigungen auf den Clusterknoten ausgeführt wird, können Sie Vorgänge wie beispielsweise das Beenden und Starten von Diensten, einschließlich Hadoop-bezogener Dienste, durchführen. Wenn Sie Dienste beenden, müssen Sie sicherstellen, dass der Ambari-Dienst und andere Hadoop-bezogene Dienste in Betrieb sind, ehe die Ausführung des Skripts beendet wird. Diese Dienste werden benötigt, um die Integrität und den Status des Clusters erfolgreich zu ermitteln, während dieser erstellt wird.

Während der Clustererstellung können Sie mehrere Skriptaktionen angeben, die in der Reihenfolge des Angebens aufgerufen werden.

> [AZURE.IMPORTANT] Skriptaktionen müssen innerhalb von 60 Minuten abgeschlossen sein, andernfalls tritt ein Timeout ein. Während der Clusterbereitstellung wird das Skript gleichzeitig mit anderen Einrichtungs- und Konfigurationsprozessen ausgeführt. Der Wettbewerb um Ressourcen wie CPU-Zeit oder Netzwerkbandbreite kann dazu führen, dass es länger als in Ihrer Entwicklungsumgebung dauert, bis das Skript abgeschlossen ist.
>
> Um die Ausführungsdauer des Skripts zu minimieren, vermeiden Sie Aufgaben wie das Herunterladen und Kompilieren von Anwendungen aus der Quelle. Führen Sie stattdessen eine Vorkompilierung der Anwendung durch, und speichern Sie die binäre Version im Azure-BLOB-Speicher, damit sie schnell in den Cluster heruntergeladen werden kann.

###Skriptaktion in einem ausgeführten Cluster

Im Gegensatz zu Skriptaktionen, die während der Clustererstellung verwendet werden, gilt hierbei Folgendes: Ein Fehler in einem Skript, das in einem bereits ausgeführten Cluster ausgeführt wird, führt nicht automatisch dazu, dass der Cluster in einen Fehlerstatus versetzt wird. Nachdem ein Skript abgeschlossen wurde, sollte der Cluster wieder in den Ausführungszustand zurückkehren.

> [AZURE.IMPORTANT] Dies bedeutet nicht, dass Ihr ausgeführtes Cluster immun gegen Skripts ist, mit denen Schaden angerichtet werden soll. Mit einem Skript können beispielsweise Dateien gelöscht werden, die vom Cluster benötigt werden, die Konfiguration kann so geändert werden, dass Dienste ausfallen, usw.
>
> Skriptaktionen werden mit Stammberechtigungen ausgeführt. Stellen Sie also sicher, dass Sie die Auswirkungen eines Skripts verstehen, bevor Sie es auf den Cluster anwenden.

Beim Anwenden eines Skripts auf einen Cluster ändert sich der Clusterstatus für erfolgreiche Skripts von __Wird ausgeführt__ in __Akzeptiert__ und über __HDInsight-Konfiguration__ schließlich wieder in __Wird ausgeführt__. Der Status des Skripts wird im Verlauf der Skriptaktion protokolliert, sodass Sie ermitteln können, ob ein Skript erfolgreich ausgeführt wurde. Beispielsweise können Sie das PowerShell-Cmdlet `Get-AzureRmHDInsightScriptActionHistory` verwenden, um den Status eines Skripts anzuzeigen. Die Ausgabe dieses Befehls sieht etwa so aus:

    ScriptExecutionId : 635918532516474303
    StartTime         : 2/23/2016 7:40:55 PM
    EndTime           : 2/23/2016 7:41:05 PM
    Status            : Succeeded

> [AZURE.NOTE] Wenn Sie nach dem Erstellen des Clusters das Kennwort für den Clusterbenutzer (Admin) ändern, können Skriptaktionen, die für diesen Cluster ausgeführt werden, möglicherweise fehlschlagen. Wenn Sie Skriptaktionen beibehalten möchten, deren Ziel Workerknoten sind, können sie fehlschlagen, wenn Sie dem Cluster über eine Größenänderung Knoten hinzufügen.

## Script Action-Beispielskripts

Skriptaktionsskripts können über das Azure-Portal, Azure PowerShell, die Azure-Befehlszeilenschnittstelle (CLI) oder das HDInsight .NET SDK genutzt werden. HDInsight verfügt über Skripts zum Installieren der folgenden Komponenten auf HDInsight-Clustern:

Name | Skript
----- | -----
**Hinzufügen eines Azure Storage-Kontos** | https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh. Siehe [Anwenden einer Skriptaktion auf einen ausgeführten Cluster](#apply-a-script-action-to-a-running-cluster).
**Installieren von Hue** | https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. Siehe [Installieren und Verwenden von Hue in HDInsight-Clustern](hdinsight-hadoop-hue-linux.md).
**Installieren von R** | https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh. Siehe [Installieren und Verwenden von R in HDInsight-Clustern](hdinsight-hadoop-r-scripts-linux.md).
**Installieren von Solr** | https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh. Siehe [Installieren und Verwenden von Solr in HDInsight-Clustern](hdinsight-hadoop-solr-install-linux.md).
**Installieren von Giraph** | https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh. Siehe [Installieren und Verwenden von Giraph in HDInsight-Clustern](hdinsight-hadoop-giraph-install-linux.md).
| **Vorabladen von Hive-Bibliotheken** | https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh. Siehe [Hinzufügen von Hive-Bibliotheken zu HDInsight-Clustern](hdinsight-hadoop-add-hive-libraries.md). |

## Verwenden einer Skriptaktion während der Clustererstellung

Dieser Abschnitt enthält Beispiele zu den verschiedenen Verwendungsmöglichkeiten von Skriptaktionen, wenn Sie einen HDInsight-Cluster mit einer der folgenden Methoden erstellen: über das Azure-Portal, mithilfe einer Azure Resource Manager-Vorlage, mit PowerShell-Cmdlets oder mit dem .NET SDK.

### Verwenden einer Skriptaktion während der Clustererstellung im Azure-Portal

1. Beginnen Sie mit dem Erstellen eines Clusters, wie unter [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-provision-clusters.md#portal) beschrieben.

2. Klicken Sie unter __Optionale Konfiguration__ auf dem Blatt **Skriptaktionen** auf **Skriptaktion hinzufügen**, um wie folgt Details zur Skriptaktion anzugeben:

	![Anpassen eines Clusters mit "Skriptaktion"](./media/hdinsight-hadoop-customize-cluster-linux/HDI.CreateCluster.8.png)

	| Eigenschaft | Wert |
	| -------- | ----- |
	| Name | Geben Sie einen Namen für die Skriptaktion an. |
	| Skript-URI | Geben Sie den URI für das Skript an, das aufgerufen wird, um den Cluster anzupassen. |
	| Haupt-/Workerknoten | Geben Sie die Knoten (**Hauptknoten**, **Workerknoten** oder **ZooKeeper**) an, auf denen das Anpassungsskript ausgeführt wird. |
	| Parameter | Geben Sie die Parameter an, wenn dies für das Skript erforderlich ist. |

	Drücken Sie die EINGABETASTE, um dem Cluster mehr als eine Skriptaktion zum Installieren von mehreren Komponenten hinzuzufügen.

3. Klicken Sie auf **Auswählen**, um die Konfiguration zu speichern und die Clustererstellung fortzusetzen.

### Verwenden einer Skriptaktion über Azure Resource Manager-Vorlagen

In diesem Abschnitt werden Azure Resource Manager-Vorlagen verwendet, um einen HDInsight-Cluster zu erstellen. Zudem werden mithilfe einer Skriptaktion benutzerdefinierte Komponenten (in diesem Beispiel R) im Cluster installiert. In diesem Abschnitt wird eine Beispielvorlage bereitgestellt, um mithilfe von Skriptaktionen einen Cluster zu erstellen.

> [AZURE.NOTE] Die Schritte in diesem Abschnitt veranschaulichen das Erstellen eines Clusters mit einer Skriptaktion. Ein Beispiel für das Erstellen eines Clusters aus einer Vorlage mit einer HDInsight-Anwendung finden Sie unter [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md).

#### Voraussetzungen

* Weitere Informationen zum Konfigurieren einer Arbeitsstation für die Ausführung von HDInsight Powershell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure-PowerShell](../powershell-install-configure.md).
* Anweisungen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md).
* Wenn Sie Azure PowerShell noch nicht mit dem Resource Manager verwendet haben, finden Sie unter [Verwenden von Windows PowerShell mit dem Azure Resource Manager](../powershell-azure-resource-manager.md) weitere Informationen.

#### Erstellen von Clustern mithilfe von Skriptaktionen

1. Kopieren Sie die folgende Vorlage in einen Speicherort auf Ihrem Computer. Mit dieser Vorlage wird Giraph auf den Hauptknoten sowie auf den Workerknoten im Cluster installiert. Sie können zudem überprüfen, ob die JSON-Vorlage gültig ist. Fügen Sie den Vorlageninhalt in [JSONLint](http://jsonlint.com/) (ein JSON-Onlineüberprüfungstool) ein.

			{
		    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
		    "contentVersion": "1.0.0.0",
		    "parameters": {
		        "clusterLocation": {
		            "type": "string",
		            "defaultValue": "West US",
		            "allowedValues": [ "West US" ]
		        },
		        "clusterName": {
		            "type": "string"
		        },
		        "clusterUserName": {
		            "type": "string",
		            "defaultValue": "admin"
		        },
		        "clusterUserPassword": {
		            "type": "securestring"
		        },
		        "sshUserName": {
		            "type": "string",
		            "defaultValue": "username"
		        },
		        "sshPassword": {
		            "type": "securestring"
		        },
		        "clusterStorageAccountName": {
		            "type": "string"
		        },
		        "clusterStorageAccountResourceGroup": {
		            "type": "string"
		        },
		        "clusterStorageType": {
		            "type": "string",
		            "defaultValue": "Standard_LRS",
		            "allowedValues": [
		                "Standard_LRS",
		                "Standard_GRS",
		                "Standard_ZRS"
		            ]
		        },
		        "clusterStorageAccountContainer": {
		            "type": "string"
		        },
		        "clusterHeadNodeCount": {
		            "type": "int",
		            "defaultValue": 1
		        },
		        "clusterWorkerNodeCount": {
		            "type": "int",
		            "defaultValue": 2
		        }
		    },
		    "variables": {
		    },
		    "resources": [
		        {
		            "name": "[parameters('clusterStorageAccountName')]",
		            "type": "Microsoft.Storage/storageAccounts",
		            "location": "[parameters('clusterLocation')]",
		            "apiVersion": "2015-05-01-preview",
		            "dependsOn": [ ],
		            "tags": { },
		            "properties": {
		                "accountType": "[parameters('clusterStorageType')]"
		            }
		        },
		        {
		            "name": "[parameters('clusterName')]",
		            "type": "Microsoft.HDInsight/clusters",
		            "location": "[parameters('clusterLocation')]",
		            "apiVersion": "2015-03-01-preview",
		            "dependsOn": [
		                "[concat('Microsoft.Storage/storageAccounts/', parameters('clusterStorageAccountName'))]"
		            ],
		            "tags": { },
		            "properties": {
		                "clusterVersion": "3.2",
		                "osType": "Linux",
		                "clusterDefinition": {
		                    "kind": "hadoop",
		                    "configurations": {
		                        "gateway": {
		                            "restAuthCredential.isEnabled": true,
		                            "restAuthCredential.username": "[parameters('clusterUserName')]",
		                            "restAuthCredential.password": "[parameters('clusterUserPassword')]"
		                        }
		                    }
		                },
		                "storageProfile": {
		                    "storageaccounts": [
		                        {
		                            "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
		                            "isDefault": true,
		                            "container": "[parameters('clusterStorageAccountContainer')]",
		                            "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
		                        }
		                    ]
		                },
		                "computeProfile": {
		                    "roles": [
		                        {
		                            "name": "headnode",
		                            "targetInstanceCount": "[parameters('clusterHeadNodeCount')]",
		                            "hardwareProfile": {
		                                "vmSize": "Large"
		                            },
		                            "osProfile": {
		                                "linuxOperatingSystemProfile": {
		                                    "username": "[parameters('sshUserName')]",
		                                    "password": "[parameters('sshPassword')]"
		                                }
		                            },
		                            "scriptActions": [
		                                {
		                                    "name": "installGiraph",
		                                    "uri": "https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh",
		                                    "parameters": ""
		                                }
		                            ]
		                        },
		                        {
		                            "name": "workernode",
		                            "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
		                            "hardwareProfile": {
		                                "vmSize": "Large"
		                            },
		                            "osProfile": {
		                                "linuxOperatingSystemProfile": {
		                                    "username": "[parameters('sshUserName')]",
		                                    "password": "[parameters('sshPassword')]"
		                                }
		                            },
		                            "scriptActions": [
		                                {
		                                    "name": "installR",
		                                    "uri": "https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh",
		                                    "parameters": ""
		                                }
		                            ]
		                        }
		                    ]
		                }
		            }
		        }
		    ],
		    "outputs": {
		        "cluster":{
		            "type" : "object",
		            "value" : "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
		        }
		    }
		}

2. Starten Sie Azure PowerShell, und melden Sie sich bei Ihrem Azure-Konto an. Nach der Eingabe Ihrer Anmeldeinformationen gibt der Befehl die Informationen zu Ihrem Konto zurück.

		Add-AzureRmAccount

		Id                             Type       ...
		--                             ----
		someone@example.com            User       ...

3. Wenn Sie über mehrere Abonnements verfügen, geben Sie die Abonnement-ID ein, die Sie für die Bereitstellung verwenden möchten.

		Select-AzureRmSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] Sie können `Get-AzureRmSubscription` verwenden, um eine Liste aller Abonnements anzurufen, die Ihrem Konto zugeordnet sind, einschließlich der Abonnement-ID jedes Abonnements.

5. Erstellen Sie eine neue Ressourcengruppe, wenn noch keine vorhanden ist. Geben Sie den Namen der Ressourcengruppe und des gewünschten Speicherorts ein. Es wird eine Zusammenfassung der neuen Ressourcengruppe zurückgegeben.

		New-AzureRmResourceGroup -Name myresourcegroup -Location "West US"

		ResourceGroupName : myresourcegroup
		Location          : westus
		ProvisioningState : Succeeded
		Tags              :
		Permissions       :
		            		Actions  NotActions
		            		=======  ==========
		            		*
		ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup


6. Führen Sie zum Erstellen einer neuen Bereitstellung für die Ressourcengruppe den Befehl **New-AzureRmResourceGroupDeployment** aus, und geben Sie die erforderlichen Parameter ein. Die Parameter enthalten den Namen der Bereitstellung, den Namen der Ressourcengruppe und den Pfad oder die URL der erstellten Vorlage. Wenn Ihre Vorlage Parameter erfordert, müssen Sie auch diese Parameter übergeben. In diesem Fall sind für die Skriptaktion zum Installieren von R auf dem Cluster keine Parameter erforderlich.


		New-AzureRmResourceGroupDeployment -Name mydeployment -ResourceGroupName myresourcegroup -TemplateFile <PathOrLinkToTemplate>


	Sie werden aufgefordert, Werte für die in der Vorlage definierten Parameter anzugeben.

7. Wenn die Ressourcengruppe bereitgestellt wurde, wird eine Zusammenfassung der Bereitstellung angezeigt.

		  DeploymentName    : mydeployment
		  ResourceGroupName : myresourcegroup
		  ProvisioningState : Succeeded
		  Timestamp         : 8/17/2015 7:00:27 PM
		  Mode              : Incremental
		  ...

8. Wenn die Bereitstellung fehlschlägt, können Sie mithilfe der folgenden Cmdlets Informationen zu den Fehlern abrufen.

		Get-AzureRmResourceGroupDeployment -ResourceGroupName myresourcegroup -ProvisioningState Failed

### Verwenden einer Skriptaktion während der Clustererstellung mit Azure PowerShell

In diesem Abschnitt verwenden wir das Cmdlet [Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx), um Skripts mithilfe von „Skriptaktion“ zum Anpassen eines Clusters aufzurufen. Stellen Sie vor dem Fortfahren sicher, dass Azure PowerShell installiert und konfiguriert ist. Weitere Informationen zum Konfigurieren einer Arbeitsstation für die Ausführung von HDInsight PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Führen Sie die folgenden Schritte aus:

1. Öffnen Sie die Azure PowerShell-Konsole, und verwenden Sie folgende Angaben, um sich an Ihrem Azure-Abonnement anzumelden und einige PowerShell-Variablen zu deklarieren:

        # LOGIN TO ZURE
        Login-AzureRmAccount

		# PROVIDE VALUES FOR THESE VARIABLES
		$subscriptionId = "<SubscriptionId>"		# ID of the Azure subscription
		$clusterName = "<HDInsightClusterName>"			# HDInsight cluster name
		$storageAccountName = "<StorageAccountName>"	# Azure storage account that hosts the default container
		$storageAccountKey = "<StorageAccountKey>"      # Key for the storage account
		$containerName = $clusterName
		$location = "<MicrosoftDataCenter>"				# Location of the HDInsight cluster. It must be in the same data center as the storage account.
		$clusterNodes = <ClusterSizeInNumbers>			# The number of nodes in the HDInsight cluster.
        $resourceGroupName = "<ResourceGroupName>"      # The resource group that the HDInsight cluster will be created in

2. Legen Sie die Konfigurationswerte fest, z. B. Knoten im Cluster und den zu verwendenden Standardspeicher.

		# SPECIFY THE CONFIGURATION OPTIONS
		Select-AzureRmSubscription -SubscriptionId $subscriptionId
		$config = New-AzureRmHDInsightClusterConfig
		$config.DefaultStorageAccountName="$storageAccountName.blob.core.windows.net"
		$config.DefaultStorageAccountKey=$storageAccountKey

3. Rufen Sie das Skript mithilfe des Cmdlets **Add-AzureRmHDInsightScriptAction** auf. Im folgenden Beispiel wird ein Skript zum Installieren von Giraph im Cluster verwendet:

		# INVOKE THE SCRIPT USING THE SCRIPT ACTION FOR HEADNODE AND WORKERNODE
		$config = Add-AzureRmHDInsightScriptAction -Config $config -Name "Install Giraph"  -NodeType HeadNode -Uri https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
        $config = Add-AzureRmHDInsightScriptAction -Config $config -Name "Install Giraph"  -NodeType WorkerNode -Uri https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

	Das Cmdlet **Add-AzureRmHDInsightScriptAction** verarbeitet die folgenden Parameter:

	| Parameter | Definition |
	| --------- | ---------- |
	| Konfiguration | Konfigurationsobjekt, dem Skriptaktionsinformationen hinzugefügt werden. |
	| Name | Name der Skriptaktion. |
	| NodeType | Gibt den Knoten an, auf dem das Anpassungsskript ausgeführt wird. Die gültigen Werte lauten **HeadNode** (für die Installation auf dem Hauptknoten), **WorkerNode** (für die Installation auf allen Datenknoten) oder **ZookeeperNode** (für die Installation auf dem Zookeeper-Knoten). |
	| Parameter | Vom Skript benötigte Parameter. |
	| Uri | Gibt den URI des auszuführenden Skripts an. |

4. Legen Sie den Administrator/HTTPS-Benutzer für den Cluster fest:

        $httpCreds = get-credential

    Geben Sie nach Aufforderung „admin“ als Namen und ein Kennwort ein.

5. Legen Sie die SSH-Anmeldeinformationen fest:

        $sshCreds = get-credential

    Geben Sie nach Aufforderung den SSH-Benutzernamen und das Kennwort ein. Wenn Sie das SSH-Konto mit einem Zertifikat anstatt mit einem Kennworts schützen möchten, verwenden Sie ein leeres Kennwort, und legen Sie `$sshPublicKey` auf den Inhalt des öffentlichen Schlüssels des Zertifikats fest, der verwendet werden soll. Beispiel:

        $sshPublicKey = Get-Content .\path\to\public.key -Raw

4. Erstellen Sie schließlich den Cluster:

        New-AzureRmHDInsightCluster -config $config -clustername $clusterName -DefaultStorageContainer $containerName -Location $location -ResourceGroupName $resourceGroupName -ClusterSizeInNodes $clusterNodes -HttpCredential $httpCreds -SshCredential $sshCreds -OSType Linux

    Wenn Sie einen öffentlichen Schlüssel zum Schutz Ihres SSH-Kontos verwenden, müssen Sie auch `-SshPublicKey $sshPublicKey` als Parameter angeben.

Die Erstellung des Clusters kann einige Minuten in Anspruch nehmen.

### Verwenden einer Skriptaktion während der Clustererstellung im HDInsight .NET SDK

Das HDInsight .NET SDK enthält Clientbibliotheken zur Vereinfachung der Arbeit mit HDInsight in .NET-Anwendungen. Ein Codebeispiel finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mit dem .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).

## Anwenden einer Skriptaktion auf einen ausgeführten Cluster

Dieser Abschnitt enthält Beispiele zu den verschiedenen Möglichkeiten der Anwendung von Skriptaktionen auf einen ausgeführten HDInsight-Cluster – über das Azure-Portal, PowerShell-Cmdlets, die plattformübergreifende Azure-Befehlszeilenschnittstelle (CLI) und das .NET SDK. Die in diesem Abschnitt verwendete permanente Skriptaktion fügt einem ausgeführten Cluster ein vorhandenes Azure-Speicherkonto hinzu. Sie können auch andere Skriptaktionen verwenden, wie in [Script Action-Beispielskripts](#example-script-action-scripts) beschrieben.

### Anwenden einer Skriptaktion auf einen ausgeführten Cluster über das Azure-Portal

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren HDInsight-Cluster aus.

2. Wählen Sie auf dem Blatt für den HDInsight-Cluster die Kachel __Skriptaktionen__ aus.

    ![Kachel „Skriptaktionen“](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

    > [AZURE.NOTE] Sie können auch auf dem Blatt „Einstellungen“ __Alle Einstellungen__ und anschließend __Skriptaktionen__ auswählen.

4. Wählen Sie oben auf dem Blatt „Skriptaktionen“ __Neue übermitteln__ aus.

    ![Symbol „Neue übermitteln“](./media/hdinsight-hadoop-customize-cluster-linux/newscriptaction.png)

5. Geben Sie auf dem Blatt „Skriptaktion hinzufügen“ die folgenden Informationen ein.

    * __Name:__ Der Anzeigename für diese Skriptaktion. In diesem Beispiel ist dies `Add Storage account`.
    * __SKRIPT-URI:__ Der URI für das Skript. In diesem Beispiel ist dies `https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh`.
    * __Hauptknoten__, __Worker__ und __Zookeeper:__ Aktivieren Sie die Knoten, auf die das Skript angewendet werden soll. In diesem Beispiel sind der Haupt-, Worker- und Zookeeperknoten aktiviert.
    * __PARAMETER:__ Falls das Skript Parameter akzeptiert, können Sie diese hier eingeben. In diesem Beispiel geben Sie den Namen des Speicherkontos und den Schlüssel des Speicherkontos ein:

		![HDInsight permanente Skriptaktion Konto Speicherkonto auf ausgeführte Cluster](./media/hdinsight-hadoop-customize-cluster-linux/hdinsight-persisted-script-action-add-storage-account.png)

		Im Screenshot ist `contosodata` ein vorhandenes Azure Storage-Konto, und die zweite Zeile ist der Schlüssel für das Storage-Konto.
    * __PERMANENT:__ Aktivieren Sie diesen Eintrag, wenn das Skript dauerhaft gespeichert werden soll, damit es beim zentralen Hochskalieren des Clusters auf neue Workerknoten angewendet wird.

6. Verwenden Sie abschließend die Schaltfläche __Erstellen__, um das Skript auf den Cluster anzuwenden.

### Anwenden einer Skriptaktion auf einen ausgeführten Cluster mit Azure PowerShell

Stellen Sie vor dem Fortfahren sicher, dass Azure PowerShell installiert und konfiguriert ist. Weitere Informationen zum Konfigurieren einer Arbeitsstation für die Ausführung von HDInsight PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

1. Öffnen Sie die Azure PowerShell-Konsole, und verwenden Sie folgende Angaben, um sich an Ihrem Azure-Abonnement anzumelden und einige PowerShell-Variablen zu deklarieren:

        # LOGIN TO ZURE
        Login-AzureRmAccount

		# PROVIDE VALUES FOR THESE VARIABLES
		$clusterName = "<HDInsightClusterName>"			# HDInsight cluster name
        $saName = "<ScriptActionName>"                  # Name of the script action
        $saURI = "<URI to the script>"                  # The URI where the script is located
        $nodeTypes = "headnode", "workernode"
        
    > [AZURE.NOTE] Wenn Sie einen HDInsight-Premium-Cluster verwenden, können Sie einen Knotentyp `"edgenode"` zum Ausführen des Skripts auf dem Edgeknoten verwenden.

2. Verwenden Sie den folgenden Befehl, um das Skript auf einen Cluster anzuwenden:

        Submit-AzureRmHDInsightScriptAction -ClusterName $clusterName -Name $saName -Uri $saURI -NodeTypes $nodeTypes -PersistOnSuccess

    Nachdem der Auftrag abgeschlossen ist, sollten Sie Informationen erhalten, die in etwa wie folgt aussehen:

        OperationState  : Succeeded
        ErrorMessage    :
        Name            : Giraph
        Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
        Parameters      :
        NodeTypes       : {HeadNode, WorkerNode}

### Anwenden einer Skriptaktion auf einen ausgeführten Cluster über die Azure-Befehlszeilenschnittstelle (CLI)

Stellen Sie vor dem Fortfahren sicher, dass die Azure-CLI installiert und konfiguriert ist. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md).

	[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

1. Öffnen Sie eine Shell-Sitzung, ein Terminal, eine Eingabeaufforderung oder eine andere Befehlszeile für Ihr System, und verwenden Sie den folgenden Befehl, um zum Azure Resource Manager-Modus zu wechseln.

        azure config mode arm

2. Führen Sie den folgenden Befehl aus, um sich bei Ihrem Azure-Abonnement zu authentifizieren.

        azure login

3. Verwenden Sie den folgenden Befehl, um eine Skriptaktion auf einen ausgeführten Cluster anzuwenden:

        azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>

    Wenn Sie die Parameter für diesen Befehl weglassen, werden Sie zur Angabe der Parameter aufgefordert. Wenn das mit `-u` angegebene Skript Parameter akzeptiert, können Sie diese mit dem Parameter `-p` angeben.

    Gültige __Knotentypen__ sind __Hauptknoten__, __Workerknoten__ und __Zookeeper-Knoten__. Wenn das Skript auf mehrere Knotentypen angewendet werden soll, geben Sie die Typen getrennt durch ein Semikolon (;) ein. Beispiel: `-n headnode;workernode`.

    Fügen Sie `--persistOnSuccess` hinzu, um das Skript dauerhaft zu speichern. Sie können das Skript auch mithilfe von `azure hdinsight script-action persisted set` zu einem späteren Zeitpunkt dauerhaft speichern.
    
    Nach Abschluss des Auftrags sollten Sie eine Ausgabe ähnlich der folgenden erhalten:
    
        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### Anwenden einer Skriptaktion auf einen ausgeführten Cluster mithilfe einer REST-API

Informationen hierzu finden Sie unter [Ausführen von Skriptaktionen in einem ausgeführten Cluster](https://msdn.microsoft.com/library/azure/mt668441.aspx).
### Anwenden einer Skriptaktion auf einen ausgeführten Cluster über das HDInsight .NET SDK

Ein Beispiel für die Verwendung des .NET SDK zum Anwenden von Skripts auf einen Cluster finden Sie unter [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

## Anzeigen des Verlaufs und Höherstufen und Herunterstufen von Skriptaktionen

### Verwenden des Azure-Portals

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren HDInsight-Cluster aus.

2. Wählen Sie auf dem Blatt für den HDInsight-Cluster __Einstellungen__ aus.

    ![Symbol „Einstellungen“](./media/hdinsight-hadoop-customize-cluster-linux/settingsicon.png)

3. Wählen Sie auf dem Blatt „Einstellungen“ __Skriptaktionen__ aus.

    ![Link „Skriptaktionen“](./media/hdinsight-hadoop-customize-cluster-linux/settings.png)

4. Auf dem Blatt „Skriptaktionen“ werden eine Liste mit permanenten Skripts sowie Verlaufsinformationen für die Skripts angezeigt, die auf den Cluster angewendet wurden. Im Screenshot unten sehen Sie, dass das Solr-Skript für diesen Cluster ausgeführt wurde, aber keine Skriptaktionen permanent gemacht wurden.

    ![Blatt „Skriptaktionen“](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionhistory.png)

5. Wenn Sie ein Skript aus dem Verlauf auswählen, wird dafür das Blatt „Eigenschaften“ angezeigt. Oben auf dem Blatt können Sie das Skript erneut ausführen oder höherstufen.

    ![Blatt mit Eigenschaften von Skriptaktionen](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionproperties.png)

6. Sie können auf dem Blatt „Skriptaktionen“ auch __...__ rechts neben den Einträgen verwenden, um Aktionen wie das erneute Ausführen, Beibehalten oder (für permanente Aktionen) Löschen durchzuführen.

    ![Skriptaktionen – Nutzung von „...“](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### Verwenden von Azure PowerShell

| Verwendung | Zweck |
| ----- | ----- |
| Get-AzureRmHDInsightPersistedScriptAction | Abrufen von Informationen zu permanenten Skriptaktionen |
| Get-AzureRmHDInsightScriptActionHistory | Abrufen eines Verlaufs der auf den Cluster angewendeten Skriptaktionen oder von Details zu einem bestimmten Skript |
| Set-AzureRmHDInsightPersistedScriptAction | Höherstufen einer Ad-hoc-Skriptaktion zu einer permanenten Skriptaktion |
| Remove-AzureRmHDInsightPersistedScriptAction | Herunterstufen einer permanenten Skriptaktion auf eine Ad-hoc-Aktion |

> [AZURE.IMPORTANT] Mit der Verwendung von `Remove-AzureRmHDInsightPersistedScriptAction` werden die mit einem Skript durchgeführten Aktionen nicht rückgängig gemacht, sondern nur das Flag „Permanent“ entfernt. Auf diese Weise wird das Skript nicht für neue Workerknoten ausgeführt, die dem Cluster hinzugefügt werden.

Im folgenden Beispielskript wird veranschaulicht, wie die Cmdlets zuerst zum Höherstufen und dann zum Herunterstufen eines Skripts verwendet werden.

    # Get a history of scripts
    Get-AzureRmHDInsightScriptActionHistory -ClusterName mycluster

    # From the list, we want to get information on a specific script
    Get-AzureRmHDInsightScriptActionHistory -ClusterName mycluster -ScriptExecutionId 635920937765978529

    # Promote this to a persisted script
    # Note: the script must have a unique name to be promoted
    # if the name is not unique, you will receive an error
    Set-AzureRmHDInsightPersistedScriptAction -ClusterName mycluster -ScriptExecutionId 635920937765978529

    # Demote the script back to ad hoc
    # Note that demotion uses the unique script name instead of
    # execution ID.
    Remove-AzureRmHDInsightPersistedScriptAction -ClusterName mycluster -Name "Install Giraph"

### Verwenden der Azure-Befehlszeilenschnittstelle

| Verwendung | Zweck |
| ----- | ----- |
| `azure hdinsight script-action persisted list <clustername>` | Abrufen einer Liste der persistenten Skriptaktionen |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` | Abrufen von Informationen zu einer bestimmten persistenten Skriptaktion |
| `azure hdinsight script-action history list <clustername>` | Abrufen eines Verlaufs der auf den Cluster angewendeten Skriptaktionen |
| `azure hdinsight script-action history show <clustername> <scriptname>` | Abrufen von Informationen zu einer bestimmten Skriptaktion |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` | Höherstufen einer Ad-hoc-Skriptaktion zu einer permanenten Skriptaktion |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` | Herunterstufen einer permanenten Skriptaktion auf eine Ad-hoc-Aktion |

> [AZURE.IMPORTANT] Mit der Verwendung von `azure hdinsight script-action persisted delete` werden die mit einem Skript durchgeführten Aktionen nicht rückgängig gemacht, sondern nur das Flag „Permanent“ entfernt. Auf diese Weise wird das Skript nicht für neue Workerknoten ausgeführt, die dem Cluster hinzugefügt werden.

### Verwenden des HDInsight .NET SDK

Ein Beispiel für die Verwendung des .NET SDK zum Abrufen des Skriptverlaufs aus einem Cluster sowie zum Höherstufen oder Herabstufen von Skripts finden Sie unter [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

> [AZURE.NOTE] Dieses Beispiel veranschaulicht auch die Installation einer HDInsight-Anwendung mit dem .NET SDK.

## Problembehandlung

Über die Ambari-Webbenutzeroberfläche können Sie Informationen anzeigen, die von Skriptaktionen protokolliert wurden. Wenn das Skript bei der Clustererstellung verwendet wurde und die Erstellung aufgrund eines Fehlers im Skript nicht erfolgreich war, sind die Protokolle auch im Standardspeicherkonto verfügbar, das dem Cluster zugeordnet ist. Dieser Abschnitt enthält Informationen zum Abrufen der Protokolle mit den folgenden zwei Optionen:

### Mithilfe der Ambari-Webbenutzeroberfläche

1. Navigieren Sie in Ihrem Browser zu https://CLUSTERNAME.azurehdinsight.net. Ersetzen Sie CLUSTERNAME durch den Namen Ihres HDInsight-Clusters.

	Geben Sie bei der entsprechenden Aufforderung den Namen (admin) und das Kennwort des Administratorkontos für den Cluster ein. Möglicherweise müssen Sie die Administratoranmeldeinformationen in einem Webformular erneut eingeben.

2. Wählen Sie in der Leiste oben auf der Seite die Option __ops__ aus. Dadurch wird eine Liste der aktuellen und vorherigen Vorgänge angezeigt, die über Ambari auf dem Cluster durchgeführt wurden.

	![Ambari-Webbenutzeroberfläche mit ausgewählter Option "ops"](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. Suchen Sie die Einträge, die __run\_customscriptaction__ in der Spalte __Vorgänge__ enthalten. Diese werden erstellt, wenn die Skriptaktionen ausgeführt werden.

	![Screenshot von Vorgängen](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

	Wählen Sie diesen Eintrag aus, und führen Sie einen Drilldown durch die Links aus, um die Ausgabe für STDOUT und STDERR anzuzeigen, die beim Ausführen des Skripts auf dem Cluster generiert wurde.

### Rufen Sie Protokolle über das Standardspeicherkonto auf.

Wenn die Erstellung des Clusters aufgrund eines Fehlers der Skriptaktion fehlgeschlagen ist, kann auf die Skriptaktionsprotokolle weiterhin direkt über das Standardspeicherkonto zugegriffen werden, das dem Cluster zugeordnet ist.

* Die Speicherprotokolle stehen unter `\STORAGE_ACOCUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE` zur Verfügung.

	![Screenshot von Vorgängen](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

	Hier sind die Protokolle separat für Hauptknoten, Workerknoten und Zookeeper-Knoten aufgeführt. Hier einige Beispiele:
	* **Hauptknoten:** `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`
	* **Workerknoten:** `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`
	* **Zookeeper-Knoten:** `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* Alle stdout- und stderr-Elemente des entsprechenden Hosts werden in das Speicherkonto hochgeladen. Für jede Skriptaktion liegen die Dateien **output-*.txt** und **errors-*.txt** vor. Die Datei „output-*.txt“ enthält Informationen zum URI des Skripts, das auf dem Host ausgeführt wurde. Beispiel:

		'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Es kann sein, dass Sie wiederholt eine Skriptaktion mit demselben Namen erstellen. In diesem Fall können Sie die relevanten Protokolle anhand des Datums im Ordnernamen unterscheiden. Die Ordnerstruktur für einen an verschiedenen Tagen erstellten Cluster (mycluster) lautet beispielsweise wie folgt:
	* `\STORAGE_ACOCUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04`
	* `\STORAGE_ACOCUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Wenn Sie am gleichen Tag einen Skriptaktionscluster mit demselben Namen erstellen, können Sie die relevanten Protokolldateien anhand des eindeutigen Präfixes ermitteln.

* Wenn Sie am Ende des Tages einen Cluster erstellen, umfassen die Protokolldateien unter Umständen zwei Tage. In diesem Fall sehen Sie für den gleichen Cluster zwei Ordner mit unterschiedlichen Datumsangaben.

* Das Hochladen von Protokolldateien in den Standardcontainer kann, insbesondere bei großen Clustern, bis zu 5 Minuten dauern. Wenn Sie also auf die Protokolle zugreifen möchten, sollten Sie nicht sofort den Cluster löschen, falls eine Skriptaktion fehlschlägt.


## Unterstützung für Open-Source-Software in HDInsight-Clustern

Der Microsoft Azure HDInsight-Dienst ist eine flexible Plattform, die es Ihnen ermöglicht, Big Data-Anwendungen in der Cloud innerhalb des Ökosystems der Open-Source-Technologien rund um Hadoop zu erstellen. Microsoft Azure bietet allgemeinen Support für Open Source-Technologien. Informationen finden Sie im Abschnitt **Supportumfang** auf der [FAQ-Website zum Azure-Support](https://azure.microsoft.com/support/faq/). Der HDInsight-Dienst bietet, wie nachstehend beschrieben, zusätzliche Unterstützung für einige der Komponenten.

Es gibt zwei Arten von Open-Source-Komponenten, die im HDInsight-Dienst verfügbar sind:

- **Integrierte Komponenten** – Diese Komponenten sind in HDInsight-Clustern vorinstalliert und stellen Kernfunktionen des Clusters bereit. So gehören beispielsweise Yarn Resource Manager, die Hive-Abfragesprache (HiveQL) und die Mahout Library zu dieser Kategorie. Eine vollständige Liste der Clusterkomponenten finden Sie unter [Neuheiten in den von HDInsight bereitgestellten Hadoop-Clusterversionen](hdinsight-component-versioning.md).

- **Benutzerdefinierte Komponenten** – Als Benutzer des Clusters können Sie in Ihrem Workload eine beliebige in der Community verfügbare oder von Ihnen erstellte Komponente installieren oder verwenden.

> [AZURE.WARNING] Komponenten, die mit dem HDInsight-Cluster bereitgestellt werden, werden vollständig unterstützt, und Microsoft Support hilft Ihnen, Probleme im Zusammenhang mit diesen Komponenten zu isolieren und zu beheben.
>
> Für benutzerdefinierte Komponenten steht kommerziell angemessener Support für eine weiterführende Behebung des Problems zur Verfügung. Auf diese Weise kann das Problem behoben werden, ODER Sie werden aufgefordert, verfügbare Kanäle für Open-Source-Technologien in Anspruch zu nehmen, die über umfassende Kenntnisse für diese Technologien verfügen. So können z. B. viele Communitywebsites verwendet werden, wie: das [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/de-DE/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Für Apache-Projekte gibt es auch Projektwebsites auf [http://apache.org](http://apache.org), z.B. [Hadoop](http://hadoop.apache.org/).

Der HDInsight-Dienst bietet mehrere Möglichkeiten, benutzerdefinierte Komponenten zu verwenden. Unabhängig davon, wie die Komponente verwendet wird oder im Cluster installiert ist, gilt der gleiche Supportumfang. Nachfolgend finden Sie eine Liste der am häufigsten genutzten Möglichkeiten für die Verwendung von benutzerdefinierten Komponenten in HDInsight-Clustern:

1. Senden des Auftrags – Hadoop- oder andere Auftragstypen, die benutzerdefinierte Komponenten ausführen oder verwenden, können zum Cluster gesendet werden.

2. Clusteranpassung – Während der Clustererstellung können Sie zusätzliche Einstellungen und benutzerdefinierte Komponenten angeben, die auf den Clusterknoten installiert werden.

3. Beispiele – Für beliebte benutzerdefinierte Komponenten stellen Microsoft und andere Anbieter u. U. Beispiele dafür bereit, wie diese Komponenten in den HDInsight-Clustern verwendet werden können. Für diese Beispiele wird kein Support bereitgestellt.

##Problembehandlung

###Verlauf zeigt keine Skripts an, die bei der Clustererstellung verwendet wurden

Wenn der Cluster vor dem 15. März 2016 erstellt wurde, wird im Verlauf der Skriptaktionen unter Umständen kein Eintrag für Skripts angezeigt, die während der Clustererstellung verwendet wurden. Falls Sie aber nach dem 15. März 2016 die Größe des Clusters ändern, werden die während der Clustererstellung verwendeten Skripts im Verlauf angezeigt, wenn sie im Rahmen der Größenänderung auf neue Knoten im Cluster angewendet werden.

Hierfür gelten zwei Ausnahmen:

* Ihr Cluster wurde vor dem 1. September 2015 erstellt. Dies ist das Datum, an dem Skriptaktionen eingeführt wurden. Für Cluster, die vor diesem Datum erstellt wurden, können also keine Skriptaktionen für die Clustererstellung verwendet worden sein.

* Sie haben während der Erstellung des Clusters mehrere Skriptaktionen und den gleichen Namen für mehrere Skripts verwendet, oder den gleichen Namen und URI, aber unterschiedliche Parameter für mehrere Skripts. In diesen Fällen erhalten Sie eine Fehlermeldung der folgenden Art:

    Aufgrund von in Konflikt stehenden Skriptnamen in vorhandenen Skripts können in diesem Cluster keine neuen Skriptaktionen ausgeführt werden. Alle bei der Clustererstellung angegebenen Skriptnamen müssen eindeutig sein. Vorhandene Skripts werden beim Ändern der Größe weiterhin ausgeführt.

## Nächste Schritte

Informationen und Beispiele zum Erstellen und Verwenden von Skripts zum Anpassen eines Clusters finden Sie über die folgenden Links:

- [Entwickeln von Script Action-Skripts für HDInsight](hdinsight-hadoop-script-actions-linux.md)
- [Installieren und Verwenden von Solr in HDInsight-Clustern](hdinsight-hadoop-solr-install-linux.md)
- [Installieren und Verwenden von Giraph in HDInsight-Clustern](hdinsight-hadoop-giraph-install-linux.md)



[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Phasen während der Clustererstellung"

<!---HONumber=AcomDC_0914_2016-->