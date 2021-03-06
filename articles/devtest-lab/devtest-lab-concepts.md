<properties
	pageTitle="DevTest Labs-Konzepte | Microsoft Azure"
	description="Lernen Sie die grundlegenden Konzepte von DevTest Labs kennen, und erfahren Sie, wie Sie virtuelle Azure-Computer damit einfach erstellen, verwalten und überwachen können."
	services="devtest-lab,virtual-machines"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/25/2016"
	ms.author="tarcher"/>

#DevTest Labs-Konzepte

> [AZURE.NOTE]
Dieser Artikel ist Teil 3 einer dreiteiligen Reihe:
> 
> 1. [Was ist DevTest Labs?](devtest-lab-overview.md)
> 1. [Warum DevTest Labs?](devtest-lab-why.md)
> 1. **[DevTest Labs-Konzepte](devtest-lab-concepts.md)**

##Übersicht

Die folgende Liste enthält wichtige DevTest Labs-Konzepte und Definitionen:

##Artefakte
Artefakte werden zum Bereitstellen und Konfigurieren Ihrer Anwendung nach der Bereitstellung eines virtuellen Computers verwendet. Bei Artefakten kann es sich um Folgendes handeln:

- Tools, die Sie auf dem virtuellen Computer installieren möchten, z. B. Agents, Fiddler oder Visual Studio.
- Aktionen, die auf dem virtuellen Computer ausgeführt werden sollen, z. B. das Klonen eines Repositorys.
- Anwendungen, die Sie testen möchten.

Artefakte sind [Azure Resource Manager](../resource-group-overview.md)-JSON-Dateien, die Anweisungen zum Ausführen der Bereitstellung und Übernehmen der Konfiguration enthalten.

##Artefaktrepositorys
Artefaktrepositorys sind Git-Repositorys, in denen Artefakte eingecheckt werden. Die gleichen Artefaktrepositorys können mehreren Labs in Ihrer Organisation hinzugefügt werden, sodass sie wiederverwendet und geteilt werden können.

## Basisimages
Basisimages sind ein VM-Images, in denen alle Tools und Einstellungen vorinstalliert und so konfiguriert sind, dass Sie schnell einen virtuellen Computer erstellen können. Sie können einen virtuellen Computer bereitstellen, indem Sie eine vorhandene Basis auswählen ein Artefakt hinzufügen, um Ihren Test-Agent zu installieren. Anschließend können Sie dann die bereitgestellte VM als Basis speichern, die dann verwendet werden kann, ohne den Test-Agent bei jeder Bereitstellung der VM erneut installieren zu müssen.

##Formeln 
Formeln bieten neben Basisimages einen Mechanismus für die schnelle Bereitstellung virtueller Computer. Eine Formel in DevTest Labs ist eine Liste standardmäßiger Eigenschaftswerte, die zum Erstellen eines virtuellen Labcomputers verwendet werden. Mit Formeln können virtuelle Computer mit dem gleichen Satz von Eigenschaften (z.B. Basisimage, VM-Größe, virtuelles Netzwerk und Artefakte) erstellt werden, ohne dass diese Eigenschaften jedes Mal angeben werden müssen. Beim Erstellen eines virtuellen Computers aus einer Formel können die Standardwerte wie vorhanden verwendet oder geändert werden.

##Obergrenzen
Obergrenzen sind ein Mechanismus zum Minimieren unnötiger Ressourcennutzung im Lab. Beispielsweise können Sie eine Obergrenze für die Anzahl der virtuellen Computer festlegen, die pro Benutzer oder in einem Lab erstellt werden können.

##Richtlinien
Richtlinien helfen dabei, die Kosten im Lab zu kontrollieren. Beispielsweise können Sie eine Richtlinie erstellen, um virtuelle Computer automatisch anhand eines definierten Zeitplans herunterzufahren.

##Sicherheitsstufen
Der Sicherheitszugriff wird durch die rollenbasierte Zugriffssteuerung von Azure (Role-Based Access Control, RBAC) bestimmt. Die Funktionsweise des Zugriffs ist leichter zu verstehen, wenn Sie die Unterschiede zwischen Berechtigungen, Rollen und Bereichen in RBAC kennen.

- Berechtigung: Eine Berechtigung ist ein definierter Zugriff auf eine bestimmte Aktion (z.B. Lesezugriff auf alle virtuellen Computer).
- Rolle: Eine Rolle ist ein Satz von Berechtigungen, die gruppiert und einem Benutzer zugewiesen werden können. Zum Beispiel die Rolle *Abonnementbesitzer* verfügt über Zugriff auf alle Ressourcen in einem Abonnement.
- Bereich: Ein Bereich ist eine Ebene innerhalb der Azure-Ressourcenhierarchie (z.B. eine Ressourcengruppe, ein einzelnes Lab oder das gesamte Abonnement).
 
Innerhalb des Bereichs von DevTest Labs gibt es zwei Arten von Rollen zum Definieren von Benutzerberechtigungen: Labbesitzer und Labbenutzer.

- Labbesitzer: Ein Labbesitzer verfügt über Zugriff auf alle Ressourcen innerhalb des Labs. Daher kann ein Labbesitzer Richtlinien und das virtuelle Netzwerk ändern, verfügt über Lese- und Schreibberechtigungen für alle virtuellen Computer usw.
- Labbenutzer: Ein Labbenutzer kann alle Labressourcen anzeigen (z.B. virtuelle Computer, Richtlinien und virtuelle Netzwerke), aber keine Richtlinien oder von anderen Benutzern erstellte virtuelle Computer ändern.


Informationen zum Erstellen benutzerdefinierter Rollen in DevTest Labs finden Sie im Artikel [Gewähren von Benutzerberechtigungen für bestimmte Labrichtlinien](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Da Bereiche hierarchisch sind, gelten die Berechtigungen, die einem Benutzer für einen bestimmten Bereich gewährt werden, automatisch für alle darin enthaltenen Bereiche auf niedrigerer Ebene. Ist ein Benutzer beispielsweise der Rolle „Abonnementbesitzer“ zugewiesen, hat er Zugriff auf alle Ressourcen in einem Abonnement, d.h. auf alle virtuellen Computer, alle virtuellen Netzwerke und alle Labs. Der Abonnementbesitzer erbt somit automatisch die Rolle des Labbesitzers. Umgekehrt ist dies jedoch nicht der Fall. Ein Labbesitzer hat Zugriff auf ein Lab, d.h. auf einen Bereich, der sich innerhalb der Hierarchie auf einer niedrigeren Ebene befindet als das Abonnement. Aus diesem Grund kann ein Labbesitzer keine virtuellen Computer, virtuellen Netzwerke oder anderen Ressourcen außerhalb des Labs anzeigen.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

##Nächste Schritte

[Erstellen eines Labs in Azure DevTest Labs](devtest-lab-create-lab.md)

<!---HONumber=AcomDC_0831_2016-->