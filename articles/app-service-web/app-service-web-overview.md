<properties
	pageTitle="Web-Apps – Übersicht | Microsoft Azure"
	description="Es wird beschrieben, wie Azure App Service Sie beim Entwickeln und Hosten von Webanwendungen unterstützt."
	services="app-service\web"
	documentationCenter=""
	authors="jaime-espinosa"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="08/26/2016"
	ms.author="rachelap"/>

# Web-Apps – Übersicht

Bei *App Service-Web-Apps* handelt es sich um eine vollständig verwaltete Computeplattform, die für das Hosten von Websites und Webanwendungen optimiert ist. Mit diesem PaaS-Angebot ([Platform as a Service](https://en.wikipedia.org/wiki/Platform_as_a_service)) von Microsoft Azure können Sie sich auf Ihre Geschäftslogik konzentrieren, während Azure sich um die Infrastruktur zum Ausführen und Skalieren Ihrer Apps kümmert.

Im folgenden fünfminütigen Video werden Azure App Service-Web-Apps vorgestellt.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

## Was ist eine Web-App in App Service?

In App Service handelt es sich bei einer *Web-App* um die Computeressourcen, die von Azure zum Hosten einer Website oder einer Webanwendung bereitgestellt werden.

Die Computeressourcen können sich je nach gewähltem Tarif auf freigegebenen oder dedizierten virtuellen Computern (VMs) befinden. Ihr Anwendungscode wird auf einer verwalteten VM ausgeführt, die von anderen Kunden isoliert ist.

Für Ihren Code kann jede Sprache und jedes Framework verwendet werden, die bzw. das von [Azure App Service](../app-service/app-service-value-prop-what-is.md) unterstützt wird, z.B. ASP.NET, Node.js, Java, PHP oder Python. Sie können auch Skripts ausführen, bei denen [PowerShell und andere Skriptsprachen](web-sites-create-web-jobs.md#acceptablefiles) in einer Web-App verwendet werden.

Beispiele für typische Anwendungsszenarien, für die Web-Apps verwendet werden können, finden Sie unter [Szenarien mit Web-Apps](https://azure.microsoft.com/documentation/scenarios/web-app/) und unter [Azure App Service, Azure Cloud Services, Azure Virtual Machines und Azure Service Fabric im Vergleich](choose-web-site-cloud-service-vm.md#scenarios) im Abschnitt **Szenarien und Empfehlungen**.

## Gründe für die Verwendung von Web-Apps

Hier sind einige wichtige Features von App Service aufgeführt, die für Web-Apps gelten:

- **Mehrere Sprachen und Frameworks** – App Service bietet erstklassige Unterstützung für ASP.NET, Node.js, Java, PHP und Python. Sie können auf virtuellen App Service-Computern auch [PowerShell und weitere Skripts oder ausführbare Dateien](../app-service-web/web-sites-create-web-jobs.md) ausführen.

- **DevOps-Optimierung**: Richten Sie mit Visual Studio Team Services, GitHub oder BitBucket die [fortlaufende Integration und Bereitstellung](../app-service-web/app-service-continuous-deployment.md) ein. Stufen Sie Updates über [Test- und Stagingumgebungen](../app-service-web/web-sites-staged-publishing.md) herauf. Führen Sie [A/B-Tests](../app-service-web/app-service-web-test-in-production-get-start.md) aus. Verwalten Sie Ihre Apps in App Service mithilfe von [Azure PowerShell](../powershell-install-configure.md) oder der [plattformübergreifenden Befehlszeilenschnittstelle (Command-Line Interface, CLI)](../xplat-cli-install.md).
 
- **Globale Skalierung mit hoher Verfügbarkeit** – Skalieren Sie manuell oder automatisch [zentral](../app-service-web/web-sites-scale.md) oder [horizontal](../azure-portal/insights-how-to-scale.md) hoch. Hosten Sie Ihre Apps überall in der globalen Rechenzentrumsinfrastruktur von Microsoft. Die App Service-[SLA](https://azure.microsoft.com/support/legal/sla/app-service/) garantiert eine hohe Verfügbarkeit.

- **Verbindungen mit SaaS-Plattformen und lokalen Daten** – Wählen Sie aus über 50 [Connectors](../connectors/apis-list.md) für Unternehmenssysteme (z.B. SAP, Siebel und Oracle), SaaS-Dienste (z.B. Salesforce und Office 365) sowie Internetdienste (z.B. Facebook und Twitter). Greifen Sie über [Hybridverbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und [Azure Virtual Networks](../app-service-web/web-sites-integrate-with-vnet.md) auf lokale Daten zu.

- **Sicherheit und Compliance** – App Service ist [ISO-, SOC- und PCI-konform](https://www.microsoft.com/TrustCenter/).

- **Anwendungsvorlagen** – Wählen Sie im [Azure Marketplace](https://azure.microsoft.com/marketplace/) aus zahlreichen Anwendungsvorlagen, die die Verwendung eines Assistenten zum Installieren gängiger Open Source-Software wie WordPress, Joomla und Drupal ermöglichen.

- **Visual Studio-Integration** – Dedizierte Tools in Visual Studio optimieren das Erstellen, Bereitstellen und Debuggen.

Außerdem können für eine Web-App die Features verwendet werden, die von [API-Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (z.B. CORS-Unterstützung) und [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (z.B. Pushbenachrichtigungen) bereitgestellt werden. Weitere Informationen zu App-Typen in App Service finden Sie unter [Übersicht: App Service](../app-service/app-service-value-prop-what-is.md).

Neben Web-Apps in App Service bietet Azure noch andere Dienste an, die zum Hosten von Websites und Webanwendungen verwendet werden können. In den meisten Fällen sind Web-Apps die beste Wahl. Erwägen Sie für eine Microservice-Architektur die Verwendung von [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric). Wenn Sie eine bessere Kontrolle über die virtuellen Computer benötigen, auf denen Ihr Code ausgeführt wird, können Sie [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) einsetzen. Weitere Informationen zur Wahl zwischen diesen Azure-Diensten finden Sie unter [Azure App Service, Azure Cloud Services, Azure Virtual Machines und Azure Service Fabric im Vergleich](choose-web-site-cloud-service-vm.md).

## Erste Schritte

Zunächst müssen Sie Beispielcode für eine neue Web-App in App-Service bereitstellen. Hierzu führen Sie die Schritte im Tutorial [Bereitstellen Ihrer ersten Web-App für Azure in fünf Minuten](app-service-web-get-started.md) aus. Sie benötigen ein kostenloses Azure-Konto.

Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.

<!---HONumber=AcomDC_0831_2016-->