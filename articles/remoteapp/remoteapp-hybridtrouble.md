
<properties
    pageTitle="Problembehandlung bei der Erstellung von RemoteApp-Hybridsammlungen | Microsoft Azure"
    description="Erfahren Sie, wie Sie Probleme beim Erstellen von RemoteApp-Hybrid-Sammlungen beheben."
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# Problembehandlung beim Erstellen von Azure RemoteApp-Hybridsammlungen

> [AZURE.IMPORTANT]
Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148).

Eine Hybrid-Sammlung wird in der Cloud gehostet und speichert die Daten in der Azure-Cloud. Benutzer können aber auch auf die Daten und Ressourcen zugreifen, die im lokalen Netzwerk gespeichert sind. Benutzer können sich mit den Anmeldeinformationen des Unternehmens, die mit Azure Active Directory verbunden sind oder synchronisiert werden, anmelden und auf die Apps zugreifen. Sie können eine Hybridsammlung für ein vorhandenes virtuelles Azure-Netzwerk bereitstellen oder ein neues virtuelles Netzwerk erstellen. Es wird empfohlen, dass Sie ein Subnetz des virtuellen Netzwerks mit einem CIDR-Bereich, der für das erwartete Wachstum für Azure RemoteApp groß genug ist, erstellen oder verwenden.

Haben Sie Ihre Sammlung noch nicht erstellt? Die Schritte finden Sie unter [Erstellen einer Hybridsammlung](remoteapp-create-hybrid-deployment.md).

Wenn Sie Probleme beim Erstellen einer Sammlung haben oder wenn die Sammlung nicht wie gewünscht funktioniert, lesen Sie die folgenden Informationen.

## Ihr Image ist ungültig. ##
Wenn eine Meldung wie „GoldImageInvalid“ angezeigt wird, während Sie darauf warten, dass Azure Ihre Sammlung bereitstellt, bedeutet dies, dass Ihr Vorlagenimage nicht die [definierten Anforderungen für Images](remoteapp-imagereqs.md) erfüllt. Gehen Sie deshalb diese [Anforderungen](remoteapp-imagereqs.md) durch, korrigieren Sie Ihr Image, und versuchen Sie erneut, Ihre Sammlung zu erstellen.



## Sind in Ihrem VNET Netzwerksicherheitsgruppen definiert? ##
Wenn in dem Subnetz, das Sie für die Sammlung verwenden, Netzwerksicherheitsgruppen definiert sind, stellen Sie sicher, dass aus dem Subnetz auf diese [URLs und Ports](remoteapp-ports.md) zugegriffen werden kann.

Sie können den im Subnetz bereitgestellten virtuellen Computern für eine bessere Kontrolle zusätzliche Netzwerksicherheitsgruppen hinzufügen.

## Verwenden Sie eigene DNS-Server? Kann darauf aus dem VNET-Subnetz zugegriffen werden? ##
>[AZURE.NOTE] Sie müssen sicherstellen, dass die DNS-Server in Ihrem VNET jederzeit verfügbar und in der Lage sind, die im VNET gehosteten virtuellen Computer aufzulösen. Verwenden Sie hierfür nicht Google DNS.


Für Hybridsammlungen verwenden Sie eigene DNS-Server. Sie geben sie in Ihrem Netzwerkkonfigurationsschema oder über das Verwaltungsportal beim Erstellen des virtuellen Netzwerks an. DNS-Server werden in der Reihenfolge verwendet, in der sie zum Failover angegeben sind (im Gegensatz zu Roundrobin). Lesen Sie unter [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) nach, um sicherzustellen, dass Ihre DNS-Server richtig konfiguriert sind.

Stellen Sie sicher, dass auf die DNS-Server für die Sammlung zugegriffen werden kann und dass sie über das VNET-Subnetz, dass Sie für diese Sammlung angegeben haben, verfügbar sind.

Beispiel:

	<VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
	</VirtualNetworkConfiguration>

![Definieren des DNS-Servers](./media/remoteapp-hybridtrouble/dnsvpn.png)

## Verwenden Sie einen Active Directory-Domänencontroller in Ihrer Sammlung? ##
Derzeit kann nur eine Active Directory-Domäne Azure RemoteApp zugeordnet werden. Die Hybridsammlung unterstützt nur Azure Active Directory-Konten, die von einer Windows Server Active Directory-Bereitstellung synchronisiert wurden (mithilfe des Tools "DirSync"); hier besonders Konten, die entweder mit der Option "Kennwortsynchronisierung" oder mit Active Directory Federation Services (AD FS) in einer Verbundkonfiguration synchronisiert wurden. Sie müssen eine benutzerdefinierte Domäne erstellen, die dem UPN-Domänensuffix Ihrer lokalen Domäne entspricht, und die Verzeichnisintegration einrichten.

Weitere Informationen finden Sie unter [Konfigurieren von Active Directory für Azure RemoteApp](remoteapp-ad.md).

Stellen Sie sicher, dass die Domänendetails gültig sind und dass der Domänencontroller vom virtuellen Computer im für Azure RemoteApp verwendeten Subnetz erreichbar ist. Stellen Sie außerdem sicher, dass die bereitgestellten Anmeldeinformationen für das Dienstkonto über Berechtigungen zum Hinzufügen von Computern zur angegebenen Domäne verfügen und dass der angegebene AD-Name vom im VNET bereitgestellten DNS aufgelöst werden kann.

## Welchen Domänennamen haben Sie angegeben, als Sie Ihre Sammlung erstellt haben? ##

Der erstellte oder hinzugefügte Domänenname muss ein interner Domänenname sein (nicht Ihr Azure AD-Domänenname), und er muss in das DNS-Format auflösbar sein (contoso.local). Angenommen, Sie verfügen über einen internen Active Directory-Namen (contoso.local) und eine Active Directory-UPN (contoso.com), dann müssen Sie den internen Namen verwenden, wenn Sie eine Sammlung erstellen.

<!---HONumber=AcomDC_0817_2016-->