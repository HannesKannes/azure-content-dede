<properties
   pageTitle="Bereitstellen einer Node.js-Anwendung mit MongoDB | Microsoft Azure"
   description="Exemplarische Vorgehensweise zum Packen mehrerer ausführbarer Gastanwendungsdateien, um sie in einem Azure Service Fabric-Cluster bereitzustellen"
   services="service-fabric"
   documentationCenter=".net"
   authors="bmscholl"
   manager=""
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/20/2016"
   ms.author="bscholl;mikhegn"/>


# Bereitstellen mehrerer ausführbarer Gastanwendungsdateien

In diesem Artikel wird das Packen mehrerer ausführbarer Gastanwendungsdateien sowie ihre Bereitstellung in Azure Service Fabric mithilfe der Vorschauversion des Service Fabric-Packtools erläutert, die unter [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool) verfügbar ist.

Informationen zum manuellen Erstellen eines Service Fabric-Pakets finden Sie im Abschnitt zum [Bereitstellen einer ausführbaren Gastanwendungsdatei in Service Fabric](service-fabric-deploy-existing-app.md).

In dieser exemplarischen Vorgehensweise wird zwar das Bereitstellen einer Anwendung mit einem Node.js-Front-End gezeigt, die MongoDB als Datenspeicher nutzt, aber diese Schritte gelten für alle Anwendungen mit Abhängigkeiten von einer anderen Anwendung.

## Packen der Node.js-Anwendung

In diesem Artikel wird vorausgesetzt, dass Node.js nicht auf den Knoten im Service Fabric-Cluster installiert ist. Daher müssen Sie vor dem Packen „Node.exe“ dem Stammverzeichnis Ihrer Node.js-Anwendung hinzufügen. Die Verzeichnisstruktur der Node.js-Anwendung (mit dem Webframework Express und Vorlagenmodul Jade) sollte wie folgt aussehen:

```
|-- NodeApplication
	|-- bin
        |-- www
	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
	|-- public
        |-- images
        |-- etc.
	|-- routes
        |-- index.js
        |-- users.js
    |-- views
        |-- index.jade
        |-- etc.
    |-- app.js
    |-- package.json
    |-- node.exe
```

Im nächsten Schritt erstellen Sie ein Anwendungspaket für die Node.js-Anwendung. Der folgende Code erstellt ein Service Fabric-Anwendungspaket, das die Node.js-Anwendung enthält.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Es folgt eine Beschreibung der verwendeten Parameter:

- **/source**: Zeigt auf das Verzeichnis der Anwendung, das gepackt werden soll.
- **/target**: Definiert das Verzeichnis, in dem das Paket erstellt werden soll. Dieses Verzeichnis darf nicht mit dem Quellverzeichnis identisch sein.
- **/appname**: Gibt den Anwendungsnamen der vorhandenen Anwendung an. Wichtig ist der Hinweis, dass dieser Name in den Dienstnamen im Manifest und nicht in den Service Fabric-Anwendungsnamen übersetzt wird.
- **/exe**: Bestimmt die ausführbare Datei, die Service Fabric starten soll. In diesem Fall ist dies `node.exe`.
- **/ma**: Gibt das Argument an, das verwendet wird, um die ausführbare Datei zu starten. Da Node.js nicht installiert ist, muss Service Fabric den Node.js-Webserver durch Ausführen von `node.exe bin/www` starten. `/ma:'bin/www'` weist das Packtool an, `bin/ma` als Argument für „node.exe“ zu verwenden.
- **/AppType**: Bestimmt den Namen des Service Fabric-Anwendungstyps.

>[AZURE.NOTE] Das Anwendungspaket kann auch mithilfe von Visual Studio als Teil eines Anwendungsprojekts erstellt werden. Wenn Sie die Quelle im Visual Studio-Projekt verknüpfen und die Visual Studio-Projektmappe erstellen, wird Ihr Anwendungspaket mit den Änderungen in der Quelle aktualisiert. [Verwenden von Visual Studio zum Packen einer vorhandenen Anwendung](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-application)

Wenn Sie zum Verzeichnis navigieren, das im Parameter „/target“ angegeben wurde, sehen Sie, dass das Tool ein voll funktionsfähiges Service Fabric-Paket erstellt hat (siehe unten):

```
|--[yourtargetdirectory]
    |-- NodeApplication
        |-- C
		      |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
		      |--Settings.xml
	    |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
Die generierte Datei „ServiceManifest.xml“ enthält nun einen Abschnitt, der beschreibt, wie der Node.js-Webserver gestartet werden soll. Dies wird im folgenden Codeausschnitt gezeigt:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
In diesem Beispiel lauscht der Node.js-Webserver an Port 3000. Deshalb müssen Sie die Endpunktinformationen in der Datei „ServiceManifest.xml“ wie unten dargestellt aktualisieren.

```xml
<Resources>
      <Endpoints>
     	<Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
Nachdem Sie das Node.js-Anwendungspaket erstellt haben, können Sie nun das MongoDB-Paket erstellen. Wie bereits erwähnt, gelten die jetzt auszuführenden Schritte nicht speziell für Node.js und MongoDB. Sie gelten für alle Anwendungen, die zu einer Service Fabric-Anwendung gepackt werden sollen.

Für das MongoDB-Paket müssen Sie „Mongod.exe“ und „Mongo.exe“ packen. Beide Binärdateien befinden sich im Verzeichnis `bin` des MongoDB-Installationsverzeichnisses. Die Verzeichnisstruktur sieht ähnlich wie folgt aus.

```
|-- MongoDB
	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service Fabric muss MongoDB mit einem Befehl starten, der dem unten angegebenen Befehl ähnelt. Deshalb müssen Sie den Parameter `/ma` beim Packen von MongoDB verwenden.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] Die Daten werden bei einem Ausfall eines Knotens nicht beibehalten, wenn Sie das MongoDB-Datenverzeichnis in das lokale Verzeichnis des Knotens einfügen. Sie müssen entweder beständigen Speicher verwenden oder eine MongoDB-Replikatgruppe implementieren, um Datenverluste zu vermeiden.

Über PowerShell oder die Befehlsshell führen wir das Packtool mit den folgenden Parametern aus:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Um MongoDB Ihrem Service Fabric-Anwendungspaket hinzuzufügen, müssen Sie sicherstellen, dass der Parameter „/target“ auf dasselbe Verzeichnis verweist, das bereits das Anwendungsmanifest und die Node.js-Anwendung enthält. Sie müssen auch sicherstellen, dass Sie denselben Namen für „ApplicationType“ verwenden.

>[AZURE.NOTE] Das Anwendungspaket kann auch mithilfe von Visual Studio als Teil eines Anwendungsprojekts erstellt werden. Wenn Sie die Quelle im Visual Studio-Projekt verknüpfen und die Visual Studio-Projektmappe erstellen, wird Ihr Anwendungspaket mit den Änderungen in der Quelle aktualisiert. [Verwenden von Visual Studio zum Packen einer vorhandenen Anwendung](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-application)

Lassen Sie uns zum Verzeichnis wechseln und untersuchen, was das Tool erstellt hat.

```
|--[yourtargetdirectory]
    |-- MyNodeApplication
    |-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
		    |--Settings.xml
	    |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
Wie Sie sehen, hat das Tool dem Verzeichnis einen neuen Ordner namens „MongoDB“ hinzugefügt, das die MongoDB-Binärdateien enthält. Wenn Sie die Datei `ApplicationManifest.xml` öffnen, sehen Sie, dass das Paket jetzt sowohl die Node.js-Anwendung als auch MongoDB enthält. Der folgende Code zeigt den Inhalt des Anwendungsmanifests.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

Der letzte Schritt ist das Veröffentlichen der Anwendung im lokalen Service Fabric-Cluster mithilfe des folgenden PowerShell-Skripts:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

>[AZURE.NOTE] Mithilfe von Visual Studio können Sie die Anwendung lokal veröffentlichen, indem Sie sie debuggen (F5) oder den Veröffentlichungs-Assistenten verwenden.

Nach der erfolgreichen Veröffentlichung der Anwendung im lokalen Cluster können Sie auf die Node.js-Anwendung an dem Port zugreifen, den Sie in das Dienstmanifest der Node.js-Anwendung eingegeben haben, z. B.: http://localhost:3000.

In diesem Tutorial haben Sie gelernt, wie Sie zwei vorhandene Anwendungen leicht zu einer Service Fabric-Anwendung Packen können. Sie haben auch gelernt, wie Sie die Bereitstellung für Service Fabric durchführen, um von den Service Fabric-Features zu profitieren. Beispiele hierfür sind hohe Verfügbarkeit und die Integration des Integritätssystems.

## Nächste Schritte

- Informieren Sie sich darüber, wie Sie [eine Gastanwendung manuell packen](service-fabric-deploy-existing-app.md).

<!---HONumber=AcomDC_0622_2016-->