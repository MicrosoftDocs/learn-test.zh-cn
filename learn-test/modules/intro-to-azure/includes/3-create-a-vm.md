As a technology professional, you likely have expertise in a specific area. Perhaps you're a storage admin or virtualization expert, or maybe you focus on the latest security practices. If you're a student, you may still be exploring what interests you most.

鹅、鹅、鹅
曲项向天歌。
白毛浮绿水，
红掌拨清波

There are many ways to create a virtual machine on Azure. Many users prefer the portal because you can visually create and manage Azure resources right from your web browser. Here, you'll bring up a Windows or Linux VM using an interactive terminal called Cloud Shell. If you work from the terminal on a daily basis, you know this is often the fastest way to get the job done.

> [!TIP]
> Be sure to select from the top of this page whether you want to run a Windows or Linux VM.
> You can return here later if you want to try something new!

Let's review some basic terms and get your first virtual machine up and running.

## What is a virtual machine?

A virtual machine, or VM, is a software emulation of a physical computer. Because VMs exist as software, dozens, hundreds, or even thousands of Azure VMs can be generated in minutes, then deleted when you don't need them anymore. With the low-cost, per-minute billing, you pay only for the compute resources you use, for as long as you are using them. Plus, there are many ways to configure the VMs to fit your needs.

::: zone pivot="windows-cloud"
A snapshot of a running VM is called an _image_. Azure provides images for Windows and several flavors of Linux. You can also create your own preconfigured images to make deployments go faster. Here you'll bring up a Windows Server 2016 VM, provided by Microsoft.
::: zone-end

::: zone pivot="linux-cloud"
A snapshot of a running VM is called an _image_. Azure provides images for Windows and several flavors of Linux. You can also create your own preconfigured images to make deployments go faster. Here you'll bring up an Ubuntu 16.04 VM, provided by Canonical.
::: zone-end

## What defines a virtual machine on Azure?

A virtual machine is defined by a number of factors, including its size and location. Before you bring up your VM, let's briefly cover what's involved.

:::row:::
    :::column:::
        **Size**
    :::column-end:::
    :::column span="3":::
A VM's _size_ defines its processor speed, amount of memory, initial amount of storage, and expected network bandwidth. Some sizes even include specialized hardware such as GPUs for heavy graphics rendering and video editing.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Region**
    :::column-end:::
    :::column span="3":::
A _region_ is an Azure data center at a specific geographic location. Azure is made up of data centers distributed throughout the world. Every virtual machine runs in one of these regions. East US and North Europe are examples of regions.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Network**
    :::column-end:::
    :::column span="3":::
A _virtual network_ is a logically isolated network on Azure. Each virtual machine on Azure is associated with a virtual network. Azure provides cloud-level firewalls for your virtual networks called _network security groups_.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Resource groups**
    :::column-end:::
    :::column span="3":::
Virtual machines and other cloud resources are grouped into logical containers called _resource groups_. You refer to a resource group by its name.
    :::column-end:::
:::row-end:::

## What is Azure Cloud Shell?

Azure Cloud Shell is a browser-based shell experience to manage and develop Azure resources. Think of Cloud Shell as an interactive console that you run in the cloud.

Behind the scenes, Cloud Shell runs on Linux. This Linux environment provides popular open-source tools you'll need to work with cloud resources.

Cloud Shell provides two experiences to choose from: Bash and PowerShell. Either way, you have access to the Azure CLI &ndash; the command-line interface for Azure.

In addition to Windows, you can run PowerShell on Linux and macOS &ndash; that's why you can run PowerShell from Cloud Shell. Azure PowerShell is an extension of PowerShell that helps you manage Azure resources.

You can use either the Azure CLI or PowerShell to work with both Windows and Linux VMs. For learning purposes, here you'll use PowerShell if you're creating a Windows VM, or the Azure CLI if you're creating a Linux VM.

::: zone pivot="windows-cloud"

## Create a Windows VM

Here, you'll bring up a Windows VM using Cloud Shell.

1. From Cloud Shell on the right side of this page, run the `Get-Credential` PowerShell cmdlet to generate a credential object.
    When prompted, enter a username and password.

    > [!NOTE]
    > We recommend **azureuser** for your username and a password you'll remember later. Choose a password that contains at least 8 characters with a combination of upper and lowercase letters, numbers, and symbols. Don't use a password you use elsewhere.

    ```powershell
    $cred = Get-Credential
    ```

1. Run the `New-AzureRmVm` cmdlet to create your VM.

    ```powershell
    New-AzureRmVm `
      -Image "Win2016Datacenter" `
      -ResourceGroupName "myResourceGroup" `
      -Name "myVM" `
      -Size "Standard_DS1_v2" `
      -Location "East US" `
      -VirtualNetworkName "myVnet" `
      -SubnetName "mySubnet" `
      -SecurityGroupName "myNetworkSecurityGroup" `
      -PublicIpAddressName "myPublicIpAddress" `
      -OpenPorts 80 `
      -Credential $cred
    ```

    > [!TIP]
    > This is a long command. You can use the **Copy** button to copy it. To paste it,  right click on the new line in the Cloud Shell window and select **Paste**.

Your VM will take about five minutes to come up. Compare that to the time it takes to purchase, rack, and configure a system in your data center. Quite a difference!

While you're waiting, let's review the command you just ran.

* **Win2016Datacenter** specifies the Windows Server 2016 VM image.
* The resource group, or the VM's logical container, is named **myResourceGroup**.
* The VM is named **myVM**. This name identifies the VM in Azure. It also becomes the VM's internal hostname, or computer name.
* **Standard_DS1_v2** refers to the size of the VM. This size has one virtual CPU and 3.5 GB of memory.
* The VM exists in the **East US** location, or region.
* The command also assigns a public IP address to the VM. You can configure a VM to be accessible from the Internet or only from the internal network.
* The network firewall allows inbound traffic on port 80 to allow HTTP traffic to your web server.
* The credential object specifies your username and password.

You can also check out this short video about some of the options you have to create and manage VMs.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

When the process completes, you see information in Cloud Shell about your new VM. Here's an example.

```console
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
VmId                     : 6684cc9a-ef9f-47dd-92ed-ce1dbcd98396
Name                     : myVM
Type                     : Microsoft.Compute/virtualMachines
Location                 : eastus
Tags                     : {}
HardwareProfile          : {VmSize}
NetworkProfile           : {NetworkInterfaces}
OSProfile                : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
ProvisioningState        : Succeeded
StorageProfile           : {ImageReference, OsDisk, DataDisks}
FullyQualifiedDomainName : myvm-edce6d.East US.cloudapp.azure.com
```

::: zone-end

::: zone pivot="linux-cloud"

## Create a Linux VM

Here, you'll bring up an Ubuntu VM using Cloud Shell.

1. From Cloud Shell on the right side of this page, run the `az group create` command to create a resource group named **myResourceGroup** in the East US region.

    ```azurecli
    az group create --location eastus --name myResourceGroup
    ```

    > [!TIP]
    > If you don't feel like typing the command, you can use the **Copy** button to copy it. To paste it, right click on the new line in the Cloud Shell window and select **Paste**.

1. Run the `az group create` command to create a resource group named **myResourceGroup** in the East US region.

    ```azurecli
    az vm create -n myVM -g myResourceGroup --image UbuntuServer --size Standard_DS1_v2 --generate-ssh-keys
    ```

Your VM will take about two minutes to come up. Compare that to the time it takes to purchase, rack, and configure a system in your data center. Quite a difference!

While you're waiting, let's review the command you just ran.

* **UbuntuServer** specifies the Ubuntu 16.04 LTS VM image.
* The resource group, or the VM's logical container, is named **myResourceGroup**.
* The VM is named **myVM**. This name identifies the VM in Azure. It also becomes the VM's internal hostname, or computer name.
* **Standard_DS1_v2** refers to the size of the VM. This size has one virtual CPU and 3.5 GB of memory.
* The VM exists in the **East US** location, or region.
* The command also assigns a public IP address to the VM. You can configure a VM to be accessible from the Internet or only from the internal network.
* The network firewall allows inbound traffic on port 80 to allow HTTP traffic to your web server.
* The `--generate-ssh-keys` option creates an SSH key pair to enable you to log in to the VM.

You can also check out this short video about some of the options you have to create and manage VMs.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

When the VM is ready, you see information about it. Here's an example.

```console
{
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
    "location": "eastus",
    "macAddress": "00-0D-3A-1D-EB-02",
    "powerState": "VM running",
    "privateIpAddress": "10.0.0.4",
    "publicIpAddress": "137.135.110.210",
    "resourceGroup": "myResourceGroup",
    "zones": ""
}
```

::: zone-end

## Summary

With just a few concepts under your belt, you're able to spin up a VM on Azure in just a few minutes. Many of these concepts, such as a VM's size and firewall rules, are likely familiar to you already.

Next, you'll connect to your VM, install a web server, and configure your web server to serve up a basic web site.