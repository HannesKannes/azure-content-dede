<properties
	pageTitle="Bereitstellen von Azure-Ressourcen mithilfe von C# | Microsoft Azure"
	description="Erfahren Sie, wie Sie C# und Azure Resource Manager zum Erstellen von Microsoft Azure-Ressourcen verwenden können."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="davidmu1"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="na"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/14/2016"
	ms.author="davidmu"/>

# Bereitstellen von Azure-Ressourcen mit C# 

In diesem Artikel erfahren Sie, wie Sie Azure-Ressourcen mithilfe von C# erstellen können.

Zuerst müssen Sie sicherstellen, dass Sie Folgendes erledigt haben:

- Installieren von [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Überprüfen der Installation von [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Abrufen eines [Authentifizierungstokens](../resource-group-authenticate-service-principal.md)

Die Durchführung dieser Schritte dauert etwa 30 Minuten.

## Schritt 1: Erstellen eines Visual Studio-Projekts und Installieren der Bibliotheken

NuGet-Pakete sind die einfachste Möglichkeit, um die Bibliotheken zu installieren, die Sie zum Abschluss dieses Lernprogramms benötigen. Sie müssen die Azure-Ressourcenverwaltungsbibliothek, die Azure Active Directory-Authentifizierungsbibliothek und die Computerressourcenanbieter-Bibliothek installieren. Führen Sie folgende Schritte aus, um diese Bibliotheken in Visual Studio abzurufen:

1. Klicken Sie auf **Datei** > **Neu** > **Projekt**.

2. Wählen Sie unter **Vorlagen** > **Visual C#** **Konsolenanwendung** aus, geben Sie den Namen und Speicherort des Projekts ein, und klicken Sie dann auf **OK**.

3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen und anschließend auf **NuGet-Pakete verwalten**.

4. Geben Sie *Active Directory* in das Suchfeld ein, klicken Sie für das Active Directory-Authentifizierungsbibliothek-Paket auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

5. Wählen Sie am oberen Rand der Seite **Vorabversion einschließen** aus. Geben Sie *Microsoft.Azure.Management.Compute* in das Suchfeld ein, klicken Sie für die .NET-Computebibliotheken auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

6. Geben Sie *Microsoft.Azure.Management.Network* in das Suchfeld ein, klicken Sie für die .NET-Netzwerkbibliotheken auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

7. Geben Sie *Microsoft.Azure.Management.Storage* in das Suchfeld ein, klicken Sie für die .NET-Speicherbibliotheken auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

8. Geben Sie *Microsoft.Azure.Management.ResourceManager* in das Suchfeld ein, und klicken Sie für die Ressourcenverwaltungsbibliotheken auf **Installieren**.

Sie können nun die Bibliotheken verwenden, um Ihre Anwendung zu erstellen.

## Schritt 2: Erstellen der Anmeldeinformationen zum Authentifizieren von Anforderungen

Nachdem die Azure Active Directory-Anwendung erstellt und die Authentifizierungsbibliothek installiert wurde, formatieren Sie nun die Anwendungsinformationen als Anmeldeinformationen, die zum Authentifizieren von Anforderungen an den Azure Resource Manager verwendet werden. Gehen Sie folgendermaßen vor:

1. Öffnen Sie die Datei „Program.cs“ für das von Ihnen erstellte Projekt, und fügen Sie die folgenden using-Anweisungen am Anfang der Datei hinzu:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. Fügen Sie der Program-Klasse die folgende Methode hinzu, um das Token abzurufen, das zum Erstellen der Anmeldeinformationen benötigt wird:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

	Ersetzen Sie {client-id} durch den Bezeichner der Azure Active Directory-Anwendung, {-client-secret} durch den Zugriffsschlüssel der AD-Anwendung und {tenant-id} durch die Mandanten-ID für Ihr Abonnement. Sie erhalten die Mandanten-ID durch Ausführen von Get-AzureRmSubscription. Den Zugriffsschlüssel finden Sie mithilfe des Azure-Portals.

3. Fügen Sie der Main-Methode in der Datei „Program.cs“ den folgenden Code hinzu, um die Anmeldeinformationen zu erstellen:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Speichern Sie die Datei "Program.cs".

## Schritt 3: Hinzufügen von Code zum Registrieren des Anbieters und Erstellen von Ressourcen

### Registrieren Sie die Anbieter, und erstellen Sie eine Ressourcengruppe.

Alle Ressourcen müssen in einer Ressourcengruppe enthalten sein. Bevor Sie Ressourcen zu einer Gruppe hinzufügen können, muss Ihr Abonnement bei Ressourcenanbietern registriert werden.

1. Fügen Sie der Main-Methode der Program-Klasse Variablen hinzu, um die Namen, die Sie für die Ressourcen verwenden möchten, den Speicherort der Ressourcen (z. B. „USA, Mitte“), Administratorkontoinformationen und Ihre Abonnement-ID anzugeben:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    Ersetzen Sie alle Variablenwerte durch die Namen und den Bezeichner, die Sie verwenden möchten. Sie erhalten die Abonnement-ID durch Ausführen von Get-AzureRmSubscription.

2. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Ressourcengruppe zu erstellen, und registrieren Sie die Anbieter:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### Speicherkonto erstellen

Ein [Speicherkonto](../storage/storage-create-storage-account.md) wird benötigt, um die für den virtuellen Computer erstellte virtuelle Festplattendatei zu speichern.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um das Speicherkonto zu erstellen:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential);
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Fügen Sie der Main-Methode der Program-Klasse den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### Erstellen der öffentlichen IP-Adresse

Eine öffentliche IP-Adresse ist für die Kommunikation mit der virtuellen Maschine erforderlich.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die öffentliche IP-Adresse des virtuellen Computers zu erstellen:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Fügen Sie der Main-Methode der Program-Klasse den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### Erstellen des virtuellen Netzwerks

Eine virtuelle Maschine, die mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt wurde, muss sich in einem virtuellen Netzwerk befinden.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um ein Subnetz und ein virtuelles Netzwerk zu erstellen:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Fügen Sie der Main-Methode der Program-Klasse den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### Erstellen der Netzwerkschnittstelle

Ein virtueller Computer benötigt eine Netzwerkschnittstelle, um in dem virtuellen Netzwerk zu kommunizieren, das Sie gerade erstellt haben.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um eine Netzwerkschnittstelle zu erstellen:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Fügen Sie der Main-Methode der Program-Klasse den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### Verfügbarkeitsgruppe erstellen

Verfügbarkeitsgruppen erleichtern die Verwaltung der Wartung der virtuellen Maschinen, die von Ihrer Anwendung verwendet werden.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Verfügbarkeitsgruppe zu erstellen:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Fügen Sie der Main-Methode der Program-Klasse den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### Erstellen eines virtuellen Computers

Nachdem Sie alle unterstützenden Ressourcen erstellt haben, können Sie nun einen virtuellen Computer erstellen.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die virtuelle Maschine zu erstellen:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential);
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
          Console.WriteLine(vm.ProvisioningState);
        }

	>[AZURE.NOTE] In diesem Tutorial wird eine virtuelle Maschine erstellt, auf dem eine Version des Windows Server-Betriebssystems ausgeführt wird. Weitere Informationen zur Auswahl von anderen Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle oder PowerShell](virtual-machines-linux-cli-ps-findimage.md).

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avsetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##Schritt 4: Hinzufügen von Code zum Löschen der Ressourcen

Da in Azure die genutzten Ressourcen in Rechnung gestellt werden, empfiehlt es sich grundsätzlich, nicht mehr benötigte Ressourcen zu löschen. Wenn Sie die virtuellen Computer und alle unterstützenden Ressourcen löschen möchten, müssen Sie lediglich die Ressourcengruppe löschen.

1.	Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Ressourcengruppe zu löschen:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.	Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## Schritt 5: Ausführen der Konsolenanwendung

1. Klicken Sie zum Ausführen der Konsolenanwendung in Visual Studio auf **Starten**, und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

2. Drücken Sie nach der Rückgabe jedes Statuscodes die **EINGABETASTE**, um jede Ressource zu erstellen. Nachdem der virtuelle Computer erstellt wurde, führen Sie den nächsten Schritt aus, bevor Sie die EINGABETASTE drücken, um alle Ressourcen zu löschen.

	Die vollständige Ausführung dieser Konsolenanwendung von Anfang bis zum Ende sollte etwa 5 Minuten dauern. Bevor Sie die EINGABETASTE drücken, um das Löschen der Ressourcen zu starten, können Sie sich ein paar Minuten Zeit nehmen, um die Erstellung der Ressourcen im Azure-Portal zu überprüfen, bevor Sie diese löschen.

3. Navigieren Sie im Azure-Portal zu den Überwachungsprotokollen, um den Status der Ressourcen anzuzeigen:

	![Durchsuchen von Überwachungsprotokollen im Azure-Portal](./media/virtual-machines-windows-csharp/crpportal.png)
    
## Nächste Schritte

- Nutzen Sie die Vorteile der Erstellung eines virtuellen Computers per Vorlage, indem Sie sich die Informationen unter [Bereitstellen eines virtuellen Azure-Computers mit C# und einer Resource Manager-Vorlage](virtual-machines-windows-csharp-template.md) durchlesen.
- Informationen zum Verwalten des gerade erstellten virtuellen Computers finden Sie unter [Verwalten von virtuellen Computern mit Azure Resource Manager und PowerShell](virtual-machines-windows-csharp-manage.md).

<!---HONumber=AcomDC_0720_2016-->