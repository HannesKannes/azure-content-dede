<properties
	pageTitle="CORS-Unterstützung in App Service | Microsoft Azure"
	description="Erfahren Sie, wie Sie die CORS-Unterstützung in Azure App Service verwenden."
	services="app-service\api"
	documentationCenter=".net"
	authors="tdykstra"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-api"
	ms.workload="na"
	ms.tgt_pltfrm="dotnet"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="08/27/2016"
	ms.author="rachelap"/>

# Nutzen einer API-App aus JavaScript mit CORS

App Service bietet integrierte Unterstützung für [CORS (Cross Origin Resource Sharing)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), das JavaScript-Clients domänenübergreifende Aufrufe von APIs ermöglicht, die in API-Apps gehostet werden. Unter App Service können Sie den CORS-Zugriff auf Ihre API konfigurieren, ohne Code in Ihrer API zu schreiben.

Dieser Artikel enthält zwei Abschnitte:

* Im Abschnitt [Konfigurieren von CORS](#corsconfig) wird allgemein beschrieben, wie Sie CORS für eine API-App, Web-App oder mobile App konfigurieren. Diese Informationen gelten gleichermaßen für alle Frameworks, die von App Service unterstützt werden, z.B. .NET, Node.js und Java.

* Der Artikel beginnt mit dem Abschnitt [Fortsetzen der Erste-Schritte-Tutorials für .NET](#tutorialstart) und ist ein Tutorial, mit dem die CORS-Unterstützung veranschaulicht wird. Hierfür wird fortgeführt, was Sie im [ersten Tutorial zu den ersten Schritten mit API-Apps](app-service-api-dotnet-get-started.md) begonnen haben.

## <a id="corsconfig"></a> Konfigurieren von CORS in Azure App Service

Sie können CORS im Azure-Portal oder über [Azure Resource Manager](../resource-group-overview.md)-Tools konfigurieren.

#### Konfigurieren von CORS im Azure-Portal

8. Wechseln Sie in einem Browser zum [Azure-Portal](https://portal.azure.com/).

2. Klicken Sie auf **App Services** und dann auf den Namen der API-App.

	![API-App im Portal auswählen](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Suchen Sie auf dem Blatt **Einstellungen**, das rechts neben dem Blatt **API-App** angezeigt wird, den Abschnitt **API**, und klicken Sie dann auf **CORS**.

	![CORS im Blatt „Einstellungen“ auswählen](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. Geben Sie im Textfeld die URLs ein, von denen JavaScript-Aufrufe ausgehen sollen.


	Wenn Sie die JavaScript-Anwendung beispielsweise für eine Web-App mit dem Namen „todolistangular“ bereitgestellt haben, geben Sie „https://todolistangular.azurewebsites.net“ ein. Als Alternative können Sie auch ein Sternchen (*) eingeben, um anzugeben, dass alle Ursprungsdomänen akzeptiert werden.


13. Klicken Sie auf **Speichern**.

	![Klicken Sie auf Speichern.](./media/app-service-api-cors-consume-javascript/corsinportal.png)

	Nachdem Sie auf **Speichern** geklickt haben, akzeptiert die API-App JavaScript-Aufrufe von den angegebenen URLs.

#### Konfigurieren von CORS mit Azure-Ressourcen-Manager-Tools

Sie können CORS für eine API-App auch konfigurieren, indem Sie [Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md) in Befehlszeilentools verwenden, z.B. [Azure PowerShell](../powershell-install-configure.md) und die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md).

Öffnen Sie die [Datei „azuredeploy.json“ im Repository für die Beispielanwendung dieses Tutorials](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json), um ein Beispiel für eine Azure Resource Manager-Vorlage zum Festlegen der CORS-Eigenschaft anzuzeigen. Suchen Sie nach dem Abschnitt der Vorlage, die wie im folgenden Beispiel aussieht:

		"cors": {
		    "allowedOrigins": [
		        "todolistangular.azurewebsites.net"
		    ]
		}

## <a id="tutorialstart"></a> Fortsetzen des Erste-Schritte-Tutorials für .NET

Falls Sie die API-App-Reihe „Erste Schritte“ für Node.js oder Java durcharbeiten, haben Sie die Reihe „Erste Schritte“ abgeschlossen. Im Abschnitt [Nächste Schritte](#next-steps) erfahren Sie, wo Sie sich noch ausführlicher über API-Apps informieren können.

Der Rest dieses Artikels ist eine Fortsetzung der Reihe „Erste Schritte“ für .NET. Es wird vorausgesetzt, dass Sie das [erste Tutorial](app-service-api-dotnet-get-started.md) erfolgreich abgeschlossen haben.

## Bereitstellen des Projekts ToDoListAngular für eine neue Web-App

Im [ersten Tutorial](app-service-api-dotnet-get-started.md) haben Sie eine API-App der mittleren Ebene und eine API-App für die Datenebene erstellt. In diesem Tutorial erstellen Sie eine Web-App vom Typ Single-Page-Anwendung (SPA), mit der die API-App der mittleren Ebene aufgerufen wird. Damit die SPA funktioniert, müssen Sie CORS für die API-App der mittleren Ebene aktivieren.

In der [ToDoList-Beispielanwendung](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) ist das Projekt ToDoListAngular ein einfacher AngularJS-Client, mit dem das Web-API-Projekt ToDoListAPI aufgerufen wird. Mit dem JavaScript-Code in der Datei *app/scripts/todoListSvc.js* wird die API mithilfe des AngularJS-HTTP-Anbieters aufgerufen.

		angular.module('todoApp')
		.factory('todoListSvc', ['$http', function ($http) {

		    $http.defaults.useXDomain = true;
		    delete $http.defaults.headers.common['X-Requested-With']; 
		
		    return {
		        getItems : function(){
		            return $http.get(apiEndpoint + '/api/TodoList');
		        },

		        /* Get by ID, Put, and Delete methods not shown */

		        postItem : function(item){
		            return $http.post(apiEndpoint + '/api/TodoList', item);
		        }
		    };
		}]);

### Erstellen einer neuen Web-App für das Projekt ToDoListAngular

Das Verfahren zum Erstellen einer neuen App Service-Web-App und Bereitstellen eines Projekts dafür ähnelt der Vorgehensweise beim [Erstellen und Bereitstellen einer API-App im ersten Tutorial dieser Reihe](app-service-api-dotnet-get-started.md#createapiapp). Der einzige Unterschied besteht darin, dass der App-Typ **Web-App** lautet , und nicht **API-App**. Screenshots der Dialogfelder:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt ToDoListAngular, und klicken Sie dann auf **Veröffentlichen**.

3.  Klicken Sie auf der Registerkarte **Profil** des Assistenten **Web veröffentlichen** auf **Microsoft Azure App Service**.

5. Klicken Sie im Dialogfeld **App Service** auf **Neu**.

3. Geben Sie im Dialogfeld **App Service erstellen** auf der Registerkarte **Hosten** einen **Web-App-Namen** ein, der in der Domäne *azurewebsites.net* eindeutig ist.

5. Wählen Sie das gewünschte Azure-**Abonnement** aus.

6. Wählen Sie in der Dropdownliste **Ressourcengruppe** die Ressourcengruppe aus, die Sie weiter oben erstellt haben.

4. Wählen Sie in der Dropdownliste **App Service-Plan** den Plan aus, den Sie bereits erstellt haben.

7. Klicken Sie auf **Erstellen**.

	Visual Studio erstellt die Web-App und das dazugehörige Veröffentlichungsprofil und zeigt den Schritt **Verbindung** des Assistenten **Web veröffentlichen** an.

	Klicken Sie noch nicht auf **Veröffentlichen**. Im folgenden Abschnitt konfigurieren Sie die neue Web-App für das Aufrufen der API-App der mittleren Ebene, die in App Service ausgeführt wird.

### Festlegen der URL für die mittlere Ebene in den Web-App-Einstellungen

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/) und dann zum Blatt **Web-App** für die Web-App, die Sie zum Hosten des Projekts TodoListAngular (Front-End) erstellt haben.

2. Klicken Sie auf **Einstellungen > Anwendungseinstellungen**.

3. Fügen Sie im Abschnitt **App-Einstellungen** folgenden Schlüssel und Wert hinzu:

	|Schlüssel|Wert|Beispiel
	|---|---|---|
	|toDoListAPIURL|https://{your middle tier API app name = Name Ihrer API-App der mittleren Ebene}.azurewebsites.net|https://todolistapi0121.azurewebsites.net|

4. Klicken Sie auf **Speichern**.

	Wenn der Code in Azure ausgeführt wird, überschreibt dieser Wert die localhost-URL in der Datei *Web.config*.

	Der Code, mit dem der Einstellungswert abgerufen wird, ist in *index.cshtml* enthalten:

		<script type="text/javascript">
		    var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
		</script>
		<script src="app/scripts/todoListSvc.js"></script>

	Im Code in *todoListSvc.js* wird die Einstellung verwendet:

		return {
		    getItems : function(){
		        return $http.get(apiEndpoint + '/api/TodoList');
		    },
		    getItem : function(id){
		        return $http.get(apiEndpoint + '/api/TodoList/' + id);
		    },
		    postItem : function(item){
		        return $http.post(apiEndpoint + '/api/TodoList', item);
		    },
		    putItem : function(item){
		        return $http.put(apiEndpoint + '/api/TodoList/', item);
		    },
		    deleteItem : function(id){
		        return $http({
		            method: 'DELETE',
		            url: apiEndpoint + '/api/TodoList/' + id
		        });
		    }
		};

### Bereitstellen des Webprojekts ToDoListAngular für die neue Web-App

*  Klicken Sie in Visual Studio im Schritt **Verbindung** des Assistenten **Web veröffentlichen** auf **Veröffentlichen**.

	Visual Studio stellt das Projekt ToDoListAngular in der neuen Web-App bereit und öffnet einen Browser mit der URL der Web-App.

### Testen der Anwendung ohne Aktivierung von CORS 

2. Öffnen Sie im Entwicklertools-Browser das Konsolenfenster.

3. Klicken Sie im Browserfenster, in dem die AngularJS-Benutzeroberfläche angezeigt wird, auf den Link **Aufgabenliste**.

	Der JavaScript-Code versucht, die API-App der mittleren Ebene aufzurufen. Der Aufruf ist aber nicht erfolgreich, da das Front-End in einer anderen Domäne als das Back-End ausgeführt wird. Im Entwicklertools-Konsolenfenster des Browsers wird eine Fehlermeldung vom Typ „ursprungsübergreifend“ angezeigt.

	![„Ursprungsübergreifende“ Fehlermeldung](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## Konfigurieren Sie CORS für die API-App der mittleren Ebene

In diesem Abschnitt konfigurieren Sie die CORS-Einstellung in Azure für die ToDoListAPI-API-App der mittleren Ebene. Mit dieser Einstellung kann die API-App der mittleren Ebene JavaScript-Aufrufe von der Web-App empfangen, die Sie für das Projekt ToDoListAngular erstellt haben.

8. Wechseln Sie in einem Browser zum [Azure-Portal](https://portal.azure.com/).

2. Klicken Sie auf **App Services** und dann auf die API-App ToDoListAPI (mittlere Ebene).

	![API-App im Portal auswählen](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Suchen Sie auf dem Blatt **Einstellungen**, das rechts neben dem Blatt **API-App** angezeigt wird, den Abschnitt **API**, und klicken Sie dann auf **CORS**.

	![CORS im Portal auswählen](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. Geben Sie im Textfeld die URL für die Web-App ToDoListAngular (Front-End) ein. Beispiel: Wenn Sie das Projekt ToDoListAngular für eine Web-App mit dem Namen „todolistangular0121“ bereitgestellt haben, können Sie Aufrufe über die URL `https://todolistangular0121.azurewebsites.net` zulassen.

	Als Alternative können Sie auch ein Sternchen (*) eingeben, um anzugeben, dass alle Ursprungsdomänen akzeptiert werden.

13. Klicken Sie auf **Speichern**.

	![Klicken Sie auf Speichern.](./media/app-service-api-cors-consume-javascript/corsinportal.png)

	Nachdem Sie auf **Speichern** geklickt haben, akzeptiert die API-App JavaScript-Aufrufe von der angegebenen URL. In diesem Screenshot akzeptiert die API-App ToDoListAPI0223 Aufrufe von JavaScript-Clients der Web-App ToDoListAngular.

### Testen der Anwendung mit Aktivierung von CORS

* Öffnen Sie einen Browser mit der HTTPS-URL der Web-App.

	Dieses Mal können Sie für die Anwendung Aufgabenelemente anzeigen, hinzufügen, bearbeiten und löschen.

	![Seite „Aufgabenliste“ der Beispiel-App](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## App Service-CORS im Vergleich zu Web-API-CORS

In einem Web-API-Projekt können Sie das NuGet-Paket [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) installieren, um im Code anzugeben, aus welchen Domänen Ihre API JavaScript-Aufrufe akzeptiert.
 
Web-API-CORS-Unterstützung ist flexibler als App Service-CORS-Unterstützung. Im Code können Sie beispielsweise verschiedene zulässige Ursprünge für unterschiedliche Aktionsmethoden angeben, während Sie für App Service-CORS einen Satz von zulässigen Ursprüngen für alle Methoden einer API-App festlegen.

> [AZURE.NOTE] Versuchen Sie nicht, Web-API-CORS und App Service-CORS in einer API-App zu verwenden. App Service-CORS hat Vorrang, und Web-API-CORS hat keine Auswirkung. Beispiel: Wenn Sie eine Ursprungsdomäne in App Service und alle Ursprungsdomänen in Ihrem Web-API-Code aktivieren, akzeptiert Ihre Azure-API-App nur Aufrufe von der in Azure angegebenen Domäne.

### Gewusst wie: Aktivieren von CORS im Web-API-Code

In den folgenden Schritten ist der Prozess zum Aktivieren der Web-API-Unterstützung zusammengefasst. Weitere Informationen finden Sie unter [Enabling Cross-Origin Requests in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api) (Aktivieren von ursprungsübergreifenden Anforderungen in ASP.NET-Web-API 2).

1. Installieren Sie in einem Web-API-Projekt das NuGet-Paket [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/).

1. Fügen Sie wie im folgenden Beispiel gezeigt eine `config.EnableCors()`-Codezeile in die **Register**-Methode der **WebApiConfig**-Klasse ein.

		public static class WebApiConfig
		{
		    public static void Register(HttpConfiguration config)
		    {
		        // Web API configuration and services
	            
		        // The following line enables you to control CORS by using Web API code
		        config.EnableCors();
	
		        // Web API routes
		        config.MapHttpAttributeRoutes();
	
		        config.Routes.MapHttpRoute(
		            name: "DefaultApi",
		            routeTemplate: "api/{controller}/{id}",
		            defaults: new { id = RouteParameter.Optional }
		        );
		    }
		}

1. Fügen Sie im Web-API-Controller eine `using`-Anweisung für den `System.Web.Http.Cors`-Namespace und der Controllerklasse oder einzelnen Aktionsmethoden das `EnableCors`-Attribut hinzu. Im folgenden Beispiel gilt die CORS-Unterstützung für den gesamten Controller.

		namespace ToDoListAPI.Controllers 
		{
		    [HttpOperationExceptionFilterAttribute]
		    [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
		    public class ToDoListController : ApiController
 
## Verwenden von Azure API Management mit API-Apps

Wenn Sie Azure API Management mit einer API-App verwenden, konfigurieren Sie CORS in API Management und nicht in der API-App. Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Azure API Management Overview](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/) (Übersicht über Azure API Management) (Video: CORS beginnt bei 12:10)
* [API Management cross domain policies](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS) (Domänenübergreifende API Management-Richtlinien)
 
## Problembehandlung

Falls beim Durcharbeiten dieses Tutorials Probleme auftreten, helfen Ihnen die unten angegebenen Hinweise zur Problembehandlung weiter.

* Stellen Sie sicher, dass Sie die aktuelle Version des [Azure SDK für .NET für Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003) verwenden.

* Stellen Sie sicher, dass Sie `https` in der CORS-Einstellung eingegeben haben und `https` zum Ausführen der Front-End-Web-App verwenden.

* Vergewissern Sie sich, dass Sie die CORS-Einstellung in der API-App der mittleren Ebene eingegeben haben, nicht in der Front-End-Web-App.

* Wenn Sie CORS sowohl im Anwendungscode als auch in Azure App Service konfigurieren, sollten Sie berücksichtigen, dass die App Service-CORS-Einstellung alles überschreibt, was Sie im Anwendungscode durchführen.

Weitere Informationen zu Visual Studio-Features, mit denen die Problembehandlung vereinfacht wird, finden Sie unter [Problembehandlung von Azure App Service-Apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## Nächste Schritte 

In diesem Artikel haben Sie erfahren, wie Sie App Service-CORS-Unterstützung aktivieren, sodass der JavaScript-Clientcode eine API in einer anderen Domäne aufrufen kann. Wenn Sie mehr über API-Apps erfahren möchten, lesen Sie die [Einführung in die Authentifizierung in App Service](../app-service/app-service-authentication-overview.md), und wechseln Sie dann zum Tutorial [Benutzerauthentifizierung für API-Apps](app-service-api-dotnet-user-principal-auth.md).

<!---HONumber=AcomDC_0824_2016-->