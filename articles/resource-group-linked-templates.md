<properties
   pageTitle="Verknüpfte Vorlagen mit Resource Manager | Microsoft Azure"
   description="Beschreibt, wie verknüpfte Vorlagen in einer Azure-Ressourcen-Manager-Vorlage zum Erstellen einer modularen Vorlagenprojektmappe verwendet werden. Zeigt, wie Parameterwerte übergeben, eine Parameterdatei festgelegt und URLs dynamisch erstellt werden."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# Verwenden von verknüpften Vorlagen mit Azure-Ressourcen-Manager

Aus einer Azure Resource Manager-Vorlage heraus können Sie einen Link zu einer anderen Vorlage herstellen, sodass Sie die Bereitstellung in eine Reihe von ausgewählten und zweckgebundenen Vorlagen zerlegen können. Wie das Zerlegen einer Anwendung in mehrere Codeklassen bietet diese Zerlegung Vorteile in Bezug auf Tests, Wiederverwendung und Lesbarkeit.

Sie können Parameter aus einer Hauptvorlage an eine verknüpfte Vorlage übergeben, und diese Parameter können direkt Parametern oder Variablen zugeordnet werden, die von der aufrufenden Vorlage verfügbar gemacht werden. Die verknüpfte Vorlage kann auch eine Ausgabevariable zurück an die Quellvorlage übergeben, wodurch ein bidirektionaler Datenaustausch zwischen Vorlagen ermöglicht wird.

## Verknüpfen mit einer Vorlage

Sie erstellen einen Link zwischen zwei Vorlagen durch Hinzufügen einer Bereitstellungsressource innerhalb der Hauptvorlage, die auf die verknüpfte Vorlage verweist. Sie legen die **templateLink**-Eigenschaft für den URI der verknüpften Vorlage fest. Für die verknüpfte Vorlage können Sie Parameterwerte angeben, indem Sie die Werte entweder direkt in der Vorlage oder per Verknüpfung mit einer Parameterdatei angeben. Im folgenden Beispiel wird die **parameters**-Eigenschaft verwendet, um direkt einen Parameterwert anzugeben.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Der Resource Manager-Dienst muss auf die verknüpfte Vorlage zugreifen können. Sie können keine lokale Datei und keine Datei, die nur in Ihrem lokalen Netzwerk verfügbar ist, für die verknüpfte Vorlage angeben. Sie müssen einen URI-Wert bereitstellen, der entweder **http** oder **https** enthält. Eine Option besteht darin, Ihre verknüpfte Vorlage in einem Speicherkonto zu platzieren und den URI für dieses Element zu verwenden, wie im folgenden Beispiel gezeigt.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Obwohl die verknüpfte Vorlage extern verfügbar sein muss, muss sie der Öffentlichkeit nicht allgemein zur Verfügung stehen. Sie können Ihre Vorlage einem privaten Speicherkonto hinzufügen, auf das nur der Speicherkontobesitzer Zugriff hat. Anschließend erstellen Sie ein SAS-Token (Shared Access Signature), um den Zugriff während der Bereitstellung zu ermöglichen. Sie fügen dieses SAS-Token dem URI für die verknüpfte Vorlage hinzu. Schritte zum Einrichten einer Vorlage in einem Speicherkonto und zum Generieren eines SAS-Tokens finden Sie unter [Bereitstellen von Ressourcen mit dem Resource Manager-Vorlagen und Azure PowerShell](resource-group-template-deploy.md) oder [Bereitstellen von Ressourcen mit Resource Manager-Vorlage und Azure-CLI](resource-group-template-deploy-cli.md).

Im folgenden Beispiel wird eine übergeordnete Vorlage gezeigt, die mit einer anderen Vorlage verknüpft ist. Der Zugriff auf die verknüpfte Vorlage erfolgt mithilfe eines SAS-Tokens, das als Parameter übergeben wird.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Obwohl das Token als sichere Zeichenfolge übergeben wird, wird der URI der verknüpften Vorlage samt SAS-Token in den Bereitstellungsvorgängen für diese Ressourcengruppe protokolliert. Legen Sie ein Ablaufdatum für das Token fest, um den Zugriff zu beschränken.

## Verknüpfen mit einer Parameterdatei

Im nächsten Beispiel wird die **parametersLink**-Eigenschaft genutzt, um eine Verknüpfung mit einer Parameterdatei herzustellen.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

Der URI-Wert für die verknüpfte Parameterdatei darf keine lokale Datei sein und muss entweder **http** oder **https** enthalten. Für die Parameterdatei kann auch die Einschränkung gelten, dass der Zugriff nur mithilfe eines SAS-Tokens möglich ist.

## Verwenden von Variablen für das Verknüpfen von Vorlagen

Die vorherigen Beispiele zeigen hartcodierte URL-Werte für die Vorlagenlinks. Dieser Ansatz funktioniert u. U. für eine einfache Vorlage, aber er funktioniert nicht gut bei der Arbeit mit einer großen Anzahl von modularen Vorlagen. Stattdessen können Sie eine statische Variable erstellen, die eine Basis-URL für die Hauptvorlage speichert, und dann aus dieser Basis-URL dynamisch URLs für die verknüpften Vorlagen erstellen. Der Vorteil dieses Ansatzes besteht darin, dass Sie die Vorlage problemlos verschieben oder verzweigen können, da Sie nur die statische Variable in der Hauptvorlage ändern müssen. Die Hauptvorlage übergibt die richtigen URIs an die zerlegten Vorlagen.

Das folgende Beispiel zeigt, wie Sie eine Basis-URL verwenden können, um zwei URLs für verknüpfte Vorlagen (**sharedTemplateUrl** und **vmTemplate**) zu erstellen.

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

Sie können auch [Bereitstellung()](resource-group-template-functions.md#deployment) verwenden, um die Basis-URL für die aktuelle Vorlage zu erhalten. Mit dieser können Sie die URL für die anderen Vorlagen am gleichen Speicherort abrufen. Diese Vorgehensweise ist hilfreich, wenn sich der Speicherort der Vorlage ändert (möglicherweise aufgrund einer Versionsverwaltung) oder wenn Sie es vermeiden möchten, URLs in der Vorlagendatei fest programmieren zu müssen.

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## Bedingtes Verknüpfen mit Vorlagen

Sie können eine Verknüpfung mit verschiedenen Vorlagen erstellen, indem Sie einen Parameterwert übergeben, der zum Erstellen des URIs der verknüpften Vorlage verwendet wird. Dieser Ansatz eignet sich gut, wenn Sie während der Bereitstellung angeben müssen, welche verknüpfte Vorlage verwendet werden soll. Geben Sie beispielsweise eine Vorlage an, die für ein vorhandenes Speicherkonto verwendet werden soll, und geben Sie eine andere Vorlage an, die für ein neues Speicherkonto verwendet werden soll.

Im folgenden Beispiel sehen Sie einen Parameter für einen Speicherkontonamen und einen Parameter, der angibt, ob das Speicherkonto neu oder bereits vorhanden ist.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

Sie erstellen eine Variable für den Vorlagen-URI mit dem Wert des neuen oder vorhandenen Parameters.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

Sie geben diesen Variablenwert für die Bereitstellungsressource an.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

Der URI wird in eine Vorlage aufgelöst, die entweder den Namen **existingStorageAccount.json** oder **newStorageAccount.json** hat. Erstellen Sie Vorlagen für diese URIs.

Das folgende Beispiel zeigt die Vorlage **existingStorageAccount.json**:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

Das nächste Beispiel zeigt die Vorlage **newStorageAccount.json**. Beachten Sie, dass das Speicherkontoobjekt wie die Vorlage für das vorhandene Speicherkonto in den Ausgaben zurückgegeben wird. Die Mastervorlage kann mit beiden verknüpften Vorlagen verwendet werden.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## Vollständiges Beispiel

Die folgenden Beispielvorlagen zeigen eine vereinfachte Anordnung verknüpfter Vorlagen zum Erläutern verschiedener Konzepte in diesem Artikel. Es wird davon ausgegangen, dass die Vorlagen demselben Container in einem Speicherkonto mit aktiviertem öffentlichen Zugriff hinzugefügt wurden. Im Abschnitt **outputs** übergibt die verknüpfte Vorlage einen Wert zurück an die Hauptvorlage.

Die Datei **parent.json** besteht aus Folgendem:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

Die Datei **helloworld.json** besteht aus Folgendem:

    {
	  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
	  "contentVersion": "1.0.0.0",
	  "parameters": {},
	  "variables": {},
	  "resources": [],
	  "outputs": {
		"result": {
			"value": "Hello World",
			"type" : "string"
		}
	  }
    }
    
In PowerShell rufen Sie ein Token für den Container ab und stellen die Vorlagen mit folgendem Code bereit:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

An der Azure-Befehlszeilenschnittstelle (CLI) rufen Sie ein Token für den Container ab und stellen die Vorlagen mit folgendem Code bereit. Derzeit müssen Sie einen Namen für die Bereitstellung angeben, wenn Sie eine URI-Vorlage nutzen, die ein SAS-Token enthält.

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

Sie werden aufgefordert, das SAS-Token als Parameter anzugeben. Sie müssen dem Token **?** voranstellen.

## Nächste Schritte
- Informationen zum Definieren der Bereitstellungsreihenfolge Ihrer Ressourcen finden Sie unter [Definieren von Abhängigkeiten in Azure-Ressourcen-Manager-Vorlagen](resource-group-define-dependencies.md).
- Informationen, wie Sie eine Ressource definieren und von dieser viele Instanzen erstellen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).

<!---HONumber=AcomDC_0907_2016-->