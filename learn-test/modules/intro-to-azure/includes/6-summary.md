Great work! With your first virtual machine under your belt, you now have a sense of how Azure works and how easy it is to bring up a system, configure some software, and tear everything down when it's no longer needed.

Azure provides services that can help transform the way your organization delivers new features to your users in ways you simply can't do without the power of the cloud.

## Cleanup

You can experiment more with your VM now, if you'd like.

Once you're done, the easiest way to clean up your VM is to delete its parent resource group.

::: zone pivot="windows-cloud"

1. Start by verifying your resource group's name. From Cloud Shell, run the `Get-AzureRmResourceGroup` to list your resource groups. 

    ```powershell
    Get-AzureRmResourceGroup | select ResourceGroupName
    ```
    You see something similar to this.
    ```console
    ResourceGroupName
    -----------------
    cloud-shell-storage-westus
    myResourceGroup
    ```
    You see your VM's resource group, **myResourceGroup**, in the output. You also see a second resource group. That resource group supports the Cloud Shell session you're currently working in.
1. Delete the resource group named **myResourceGroup**. Here's how.

    ```powershell
    Get-AzureRmResourceGroup `
      -Name "myResourceGroup" |`
    Remove-AzureRmResourceGroup `
      -Verbose `
      -Force
    ```
1. Run `Get-AzureRmResourceGroup` to list all resource groups a second time.
    ```powershell
    Get-AzureRmResourceGroup | select ResourceGroupName
    ```
    You see that **myResourceGroup** no longer exists.
    ```console
    ResourceGroupName
    -----------------
    cloud-shell-storage-westus
    ```

::: zone-end

::: zone pivot="linux-cloud"

1. Start by verifying your resource group's name. From Cloud Shell, run the `az group list` to list your resource groups.

    ```powershell
    az group list --query "[].name" -o tsv
    ```
    You see something similar to this.
    ```console
    cloud-shell-storage-westus
    myResourceGroup
    ```
    You see your VM's resource group, **myResourceGroup**, in the output. You also see a second resource group. That resource group supports the Cloud Shell session you're currently working in.
1. Run `az group delete` to delete the resource group named **myResourceGroup**.

    ```powershell
    az group delete -n myResourceGroup -y
    ```
1. Run `az group list` to list all resource groups a second time.
    ```powershell
    az group list --query "[].name" -o tsv
    ```
    You see that **myResourceGroup** no longer exists.
    ```console
    cloud-shell-storage-westus
    ```

::: zone-end

## Continue your Azure journey

We provide learning paths based on your role and interests.

A great next step is to check out the
[Cloud foundations](https://docs.microsoft.com/learn/paths/cloud-foundations/) learning path. You'll learn the fundamental concepts behind cloud computing and how Azure services such as compute, storage, networking, and security can help you unlock the power of the cloud and create that next great breakthrough idea.