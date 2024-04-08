# Active Directory Home Lab | Add Users With PowerShell

## [YouTube Demonstration](https://youtu.be/)

## Description
In this lab, I'll walk you through creating an Active Directory (AD) home lab environment using Oracle VirtualBox and how to add users with PowerShell. Two virtual machines will be made - the first is the DC or Domain controller and the second is the client computer that connects to the DC. This lab will also walk through installing Active Directory, setting up a domain server, configuring internet settings, installing RAS/NAT and DHCP, and lastly using PowerShell to add users. This is a great way to get hands-on experience and understand how Active Directory and Windows networking works. 
<br />

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
3. Open up VirtualBox and click **New** icon to create our first virtual machine, the domain controller. We'll name it **DC**. Select the Windows Server ISO image under
4. Click on Settings, General, Advanced, change Shared Clipboard and Drag'n'Drop to **Bidirectional**. This lets you copy and paste and drag and drop between your actual computer and virtual machine.
5. Click on Network, Adapter 1 - NAT and Adapter 2 - Internal Network.
6. Open the DC VM. Go through the installation and choose Windows Server 2019 Standard Evaluation (Desktop Experience) to have the GUI. Let it install and when a black screen pops up asking to press any key, don't press anything and let it boot up itself.
7. After it boots up, go to Input, Keyboard, Insert Ctrl+Alt+Del to login. Sign in with your Administrator credentials.
8. Click Yes to discoverable by other computers. Go to This PC, run VBoxWindowsAdditions-amd64 to install then let it reboot.

### Internet Settings

1. Once it reboots again, click the Network icon in the lower right taskbar, Change Adapter Options.
2. 
3. 
4. 

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
