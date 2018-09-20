Now that your VM is up and running, let's do something with it. Here you'll install a web server and serve up a basic web page that displays the VM's hostname.

鹅、鹅、鹅
曲项向天歌。
白毛浮绿水，
红掌拨清波

To configure a VM, you have several choices. You can connect directly and interactively configure your system. For example, on Windows systems you can create a Remote Desktop session to connect to the UI of your remote Windows computer as if you were seated at it.

Manual configuration is a good start, but as you add systems, you can automate your deployments. Automation involves running repeatable processes such as programs and scripts that take care of the heavy lifting for you.

::: zone pivot="windows-cloud"

Here, you'll interactively log into your VM and configure IIS.

::: zone-end

::: zone pivot="linux-cloud"

Here, you'll interactively log into your VM and configure Nginx.

::: zone-end

::: zone pivot="windows-cloud"

## What is IIS?

Internet Information Services, or IIS, is a web server that runs on Windows. You can use IIS to serve standard web content (HTML, CSS, and JavaScript) or run ASP.NET web applications. IIS comes with Windows Server, but you need to activate it to start serving web pages.

## What is WinRM?

WinRM stands for Windows Remote Management. It's a communication protocol that helps you accomplish common management tasks. Although they're not quite the same, you can think of a WinRM connection to a Windows system similar to how SSH works on Linux.

## Connect to your VM from Cloud Shell

Let's connect to your Windows VM and run a few commands to get a feel for how things work.

1. From Cloud Shell, run the following `Get-Credential` cmdlet a second time to generate a credential object.
    When prompted, enter the same username and password as you did previously. Remember that we recommend **azureuser** for your username and a password you'll remember later.

    ```powershell
    $cred = Get-Credential
    ```
1. Run the `Enter-AzureRmVM` cmdlet to start an interactive session with your VM.

    ```powershell
    Enter-AzureRmVM `
      -Name "myVM" `
      -ResourceGroupName "myResourceGroup" `
      -Credential $cred `
      -EnableRemoting
    ```
    It may take a few moments to begin your session.

    > [!NOTE]
    > Don't confuse this with creating a Remote Desktop, or RDP, session. This process creates a remote session so you can work with your Windows system from the command line.

1. Once logged in, you can explore the system. For example, run `dir` to print the contents of the C: drive.
    ```powershell
    dir C:\ | select Name
    ```
    You see this.
    ```console
    Name
    ----
    Logs
    Packages
    PerfLogs
    Program Files
    Program Files (x86)
    Users
    Windows
    WindowsAzure
    ```
1. As a second example, print out the environment variable that holds the computer name.
    ```powershell
    $env:computername
    ```
    You see the name of your VM, **myVM**.
    ```console
    myVM
    ```

## Configure IIS

Now that you're logged in, let's install IIS and configure a basic home page.

1. From your interactive session, run this `Add-WindowsFeature` cmdlet to activate IIS.

    ```powershell
    Add-WindowsFeature Web-Server
    ```
1. Run this command to add some initial content to the default home page, `Default.htm`.
    ```powershell
    Set-Content `
      -Path "C:\\inetpub\\wwwroot\\Default.htm" `
      -Value "<h1>Hello, Azure! My name is $($env:computername).</h1>"
    ```
    The home page displays a welcome message, along with the VM's computer name.

## Verify the configuration

Now that IIS is set up, let's verify that it's running. You'll start by verifying things locally, then remotely.

1. From your interactive session, run the `Invoke-WebRequest` cmdlet to fetch the homepage from localhost and print its contents to the console.

    ```powershell
    (Invoke-WebRequest -useb localhost).Content
    ```
    You see this.
    ```console
    <h1>Hello, Azure! My name is myVM.</h1>
    ```
1. Run `exit` to leave your interactive session.
    ```powershell
    exit
    ```
1. From Cloud Shell, run this cmdlet to get your VM's public IP address.

    ```powershell
    $ipaddress = Get-AzureRmPublicIPAddress `
      -ResourceGroupName "myResourceGroup" `
      -Name "myPublicIPAddress" `
      | select IpAddress
    ```
1. Run `Invoke-WebRequest` a second time, this time specifying your VM's IP address.

    ```powershell
    (Invoke-WebRequest $ipaddress.IpAddress).Content
    ```
    Again, you see this.
    ```console
    <h1>Hello, Azure! My name is myVM.</h1>
    ```
1. Print the IP address to the console.

    ```powershell
    $ipaddress
    ```
    You see output similar to this.
    ```console
    IpAddress
    ---------
    40.117.84.233
    ```

From a second browser tab, navigate to your VM's IP address. You see your welcome message and your VM's name.

![](../media/web-server-browser.png)

::: zone-end

::: zone pivot="linux-cloud"

## What is Nginx?

Nginx is a popular web server that runs on Unix, Linux, and Windows. Here you'll use Nginx to serve a basic web page.

## What is SSH?

SSH stands for Secure Shell. It's a communication protocol that helps you connect to a remote system and work with it as if you were seated at it.

SSH runs over a secure connection. There are two common ways to authenticate access: _password authentication_ and _key-based authentication_. Here you'll use key-based authentication because most consider it to be more secure.

We won't go into the details of how key-based authentication works here. All you need to know for now is that key-based authentication involves two files &ndash; a _private key_ that exists on your workstation and a _public key_ that exists on the computer you want to connect to. When you created your VM, you specified the `--generate-ssh-keys` option. This option sets up these files for you. 

## Connect to your VM from Cloud Shell

Let's connect to your Linux VM over SSH and run a few commands to get a feel for how things work.

1. First, run `whoami` to get your username.

    ```bash
    whoami
    ```
    You see your username. Here's an example.
    ```console
    thomas
    ```
    You'll use this name to connect to your VM. By default, when you create a Linux VM from Cloud Shell, the username on the VM is the same as your Cloud Shell username.

1. Run this `az vm list-ip-addresses` command to list your VM's public IP addresses.

    ```azurecli
    az vm list-ip-addresses -g myResourceGroup -n myVM --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" -o tsv
    ```

    > [!NOTE]
    > This command is pretty complex, especially `--query` argument. But you'll be at pro at this as you dig in and explore Azure.

    You see your VM's public IP address. Here's an example.
    ```console
    137.135.110.210
    ```

1. Create an SSH connection to your VM. Replace the username and IP address shown in this example with yours.

    ```bash
    ssh thomas@137.135.110.210
    ```
    When prompted, enter "y" to continue connecting.

1. Once logged in, you can explore the system. For example, run `ls` to print the contents of the root directory.

    ```bash
    ls -w 40 /
    ```
    You see the contents of the root directory.
    ```console
    bin   initrd.img  mnt   sbin  usr
    boot  lib         opt   snap  var
    dev   lib64       proc  srv   vmlinuz
    etc   lost+found  root  sys
    home  media       run   tmp
    ```
1. As a second example, run `hostname` to print out your VM's name.
 
    ```bash
    hostname
    ```
    You see the name of your VM, **myVM**.
    ```console
    myVM
    ```

## Configure Nginx

Now that you're logged in, let's install Nginx and configure a basic home page.

1. From your SSH connection, start by updating the `apt` cache.

    ```bash
    sudo apt-get update
    ```

    This command updates the system's package index. Keeping the package index up to date helps ensure you're getting the most up-to-date software.

    > [!NOTE]
    > The `sudo` part enables you to run commands as the `root` user. Running as `root` is required to make system changes such as installing software.

1. Next, install Nginx.

    ```bash
    sudo apt-get install nginx -y
    ```
1. Run this command to add some initial content to the default home page, `/var/www/html/index.html`.

    ```bash
    echo "Hello, Azure! My name is $(hostname)." | sudo tee -a /var/www/html/index.html
    ```
    The home page displays a welcome message, along with the VM's computer name.

## Verify the configuration

Now that Nginx is set up, let's verify that it's running. You'll start by verifying things locally, then remotely.

1. From your SSH connection, run `curl localhost` to fetch the homepage from localhost and print its contents to the console.

    ```bash
    curl localhost
    ```
    You see this.
    ```console
    Hello, Azure! My name is myVM.
    ```
1. Run `exit` to leave your interactive session.

    ```bash
    exit
    ```
1. From Cloud Shell, run this `az vm open-port` command to open port 80 (HTTP) through the firewall.

    ```azurecli
    az vm open-port --name myVM --port 80 --resource-group myResourceGroup
    ```

1. Run `curl` a second time, this time specifying your VM's IP address.

    ```bash
    curl 137.135.110.210
    ```
    Again, you see this.
    ```console
    Hello, Azure! My name is myVM.
    ```

From a second browser tab, navigate to your VM's IP address. You see your welcome message and your VM's name.

![](../media/web-server-browser.png)

::: zone-end

## Summary

Your VM is running and can now serve up web pages, but what does that mean for you?

Remember, every journey starts with the basics, and almost any great innovation born in the cloud, from companies big and small, started with a similar setup to yours. As your idea evolves, it begins making a positive impact on your business and your users.