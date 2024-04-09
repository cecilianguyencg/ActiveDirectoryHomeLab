# Active Directory Home Lab | Add Users With PowerShell

## Description
In this lab, I'll walk you through creating an Active Directory (AD) home lab environment using Oracle VirtualBox and how to add users with PowerShell. Two virtual machines will be made - the first is the DC or Domain controller which will have 2 NICs or network interface controllers, the first connects to our home internet and the second is connected to a private network that the second virtual machine, which is our client, can connect to. This lab will also walk through installing Active Directory, setting up a domain server, configuring internet settings, installing RAS/NAT and DHCP, and lastly using PowerShell to add users. This is a great way to get hands-on experience and understand how Active Directory and Windows networking works. 

## Languages and Utilities Used

- Oracle VirtualBox
- PowerShell

## Environments Used

- Windows 10
- Server 2022

## Program Walk-Through

### Creating the Domain Controller (DC)

1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) for your respective platform. Then install the [Extension Pack](https://download.virtualbox.org/virtualbox/7.0.14/Oracle_VM_VirtualBox_Extension_Pack-7.0.14.vbox-extpack).
2. Download [Windows Server ISO](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022) and [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10).
3. Open up VirtualBox and click **New** starburst icon to create our first virtual machine, the domain controller.
   + Name: DC
   + ISO Image: Windows Server ISO.
   + Edition: Windows Server 2022 Standard Evaluation (Desktop Experience)
   + Uncheck Skip Unattended Installation.
>  If you select one of the non-Desktop Experience (Evaluation), after it installs, this version will have a command line with no GUI, making it more difficult to navigate.

![New Settings Server](https://i.imgur.com/dZtepoL.png)

4. Click Next, Base Memory: 4096 MB and Processors: 2
> If your computer has enough memory and cores, it's recommended to increase it from the default settings to speed up using the virtual machine.

![Hardware](https://i.imgur.com/GJGxuWz.png)

5. Keep Virtual hard disk settings, click Next, then Finish.
6. Click on **Settings** cog wheel icon, General, Advanced, change Shared Clipboard and Drag'n'Drop to **Bidirectional**
> Bidirectional setting lets you copy and paste and drag and drop between your actual computer and virtual machine.

![General Advanced Settings](https://i.imgur.com/cJrU8hE.png)

7. Click on Network, and keep Adapter 1 - NAT and change Adapter 2 - Internal Network. click Ok.
> Adapter 1 connects to our home internet and automatically gets an IP address. Adapter 2 is the internal network that we'll need to setup the IP address manually in a bit.
8. Open the DC VM by either clicking the **Start** green arrow icon or double-clicking on the DC VM in the left pane. Click Next to start Server installation and choose Windows Server 2022 Standard Evaluation (Desktop Experience).

![Desktop Experience](https://i.imgur.com/FJBBfYi.png))

9. Check to accept the Microsoft Software License Terms, Next, Custom, Next, and then let it install. This will take a few minutes.
10. After it boots up, go to Input, Keyboard, Insert Ctrl+Alt+Del to login and then type in your Administrator password.

![Unlock Screen](https://i.imgur.com/2NCMY6M.png)

11. If the blue Network pane on the right appears, click **Yes** to discoverable by other computers.
12. Click Devices, Insert Guest Additions CD image... Then navigate to **This PC** by clicking the **File Explorer** folder icon in the taskbar. Double-click CD Drive (D:) VirtualBox Guest Additions then double-click VBoxWindowsAdditions-amd64 to install. Click Next, Next, Install and then let it reboot.
> Installing Guest Additions gives a better experience and speeds up everything.

![Insert Guest](https://i.imgur.com/pEp9muN.png)
![CD Drive](https://i.imgur.com/SUML8gU.png) 

### Internet Settings

1. Once the DC reboots, unlock the screen and log in. Click the Network icon in the lower right taskbar, Network Connected, Change Adapter Options.
2. Two Ethernet Networks will appear. Right-click on one of them, **Status**, **Details...**
> The IP looks ok here. This is connected to our home internet.

![Ethernet](https://i.imgur.com/bbLNjLC.png)

3. Close this window and the Ethernet Status one. Right-click, **Rename**, and type **INTERNET**.
4. Right-click on Ethernet 2, **Status**, **Details...**
> Autoconfiguration IPv4: 169.254.192.140, means that this adapter was looking for a DHCP server to try to get an IP address from somewhere but it was unable to find one, so this one was automatically assigned to it. This is our Internal network.

![Ethernet 2](https://i.imgur.com/sIxSGvR.png)

5. Close this window and the Ethernet Status one. Right-click, **Rename**, and type **INTERNAL**.
6. Right-click on **INTERNAL**, **Properties**, and double-click **Internet Protocol Version 4 (TCP/IPv4).

![INTERNAL Properties](https://i.imgur.com/NmvBEOn.png)

7. Check **Use the following IP address:** and enter the settings as seen below:
> We won't use a Default gateway because the Domain Controller itself will serve as the Default gateway. For the DNS server, when we installed Active Directory, it automatically installed DNS so this will use itself as the DNS server. To do this, either enter its own IP address or use the loopback address of 127.0.0.1, which is a generic address that refers to itself. Whenever a computer pings this address, they're essentially pinging themself automatically.

![Internet Protocol Version 4 Properties](https://i.imgur.com/SuimBXD.png)

8. Click **Ok**, then **Ok** again to close out the Network windows.
9. Next, we'll rename the PC. Right-click **Start Menu**, **System**, **Rename this PC**. Rename it **DC**, **Next**, **Restart now**, **Continue**.

![Rename PC](https://i.imgur.com/wm1e9FT.png)

### Install Active Directory and Create Domain Server

1. 
2. 
3. 
4. 

### Install RAS/NAT

1. 
2. 
3. 
4.

### Install DHCP

1. 
2. 
3. 
4. 

### Add Users Using PowerShell

1. 
2. 
3. 
4. 

### Create Client Machine

1. Go back to VirtualBox and click **New** to create a new virtual machine. This will be our client machine.
2.  
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
