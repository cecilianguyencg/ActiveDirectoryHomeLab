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

1. Log back in after it restarts. In **Server Manager**, **Add roles and features**.

![Add roles and features](https://i.imgur.com/4rC9LPp.png)

2. Click **Next**, **Next**, **Next**, then check **Active Directory Domain Services** and click **Add Features**.

![Active Directory Domain Services](https://i.imgur.com/MAXcMGy.png)

3. Click **Next**, **Next**, **Next**, **Install**. After it finishes installing, click **Close** 
4. Click the **flag** icon with the yellow warning symbol. Notice the **Post-deployment Configuration**. Click **Promote this server to a domain controller**.
> This warning means that the software for Active Directory domain services has been installed, but the actual domain hasn't been created yet.

![Post Warning](https://i.imgur.com/XTOilyg.png)

5. Select **Add a new forest** and choose a **Root domain name**. For this lab, I'll use **mydomain.com** and then click **Next**.

![Forest](https://i.imgur.com/WDByvwl.png)

6. Enter in a password, although you probably will never have to use it. Keep clicking **Next** until you can click **Install**. It'll automatically restart your computer. Unlock your screen and log back in.
> Notice it now says MYDOMAIN\Administrator account since you're part of the MYDOMAIN server.

![MYDOMAIN Login](https://i.imgur.com/RMPWi6U.png)

7. Now, we'll create our own dedicated Domain Admin account instead of using the built-in Administrator account. Access this either by clicking:
   + **Start** menu, **Windows Administrative Tools** folder, **Active Directory Users and Computers**
   + **Server Manager**, **Tools**, **Active Directory Users and Computers**

![StartUsers](https://i.imgur.com/iXc1UsD.png)

![ServerUsers](https://i.imgur.com/bpjyfVK.png)

8. Expand the newly created mydomain.com in the left pane, then right-click on it, **New**, **Organization Unit**.
> Organizational Units are like folders in the Active Directory.

![New OU](https://i.imgur.com/ucZKw6x.png)

9. Let's name it **_ADMINS** and uncheck **Protect container from accidental deletion**, then click **OK**.

![OU Name](https://i.imgur.com/1yR5YfL.png)

10. Right-click the newly created **_ADMINS** OU, **NEW**, **USER** and enter in **First name, Last name**, and **User Logon name**. Click **Next**. 
> For the **User logon name**, a common naming convention in many organizations is a- to represent it’s an admin account, and whatever naming convention follows. So in this case, first name initial followed by last name.

![New User](https://i.imgur.com/h3sO71i.png)

11. Enter in a password and uncheck **User must change password at next logon** and check **Password never expires**. Click **Next** then **Finish**.
> Since this is a lab environment, it's safe to enable **Password never expires**, but in the real world, you'll want to leave **User must change password at next logon** checked on so the new user can create their own password that only they know. Not even administrators should have access to another user's password.

12. See your new account in the right pane, but it’s not an admin yet. To make it a domain admin, right-click **Properties, Add to a group...**.
![Add to a group](https://i.imgur.com/rrr7DjV.png)

13. In **Enter the object names to select**, type: domain admins, then click **Check Names** so that it resolves to Domain Admins, indicating it's an existing group. Click **Apply, Ok, Ok**.
![Domain Admins](https://i.imgur.com/UXTUMWB.png)

14. Now you have your very own domain admin account. To use this, click **Start** menu, **Profile** icon, **Sign out**.

![Sign out](https://i.imgur.com/AKl49Yi.png)

15. Instead of logging into the **Administrator** account, click **Other user** and use your newly created domain admin account. In my case, that's **a-cnguyen**.

### Install RAS/NAT
+ RAS = remote access server
+ NAT = network address translation
+ Setting up RAS/NAT allows client machines to be on a virtual private network or VPN, but still be able to access the internet through the Domain Controller.


1. In **Server Manager**, click **Add roles and features**, **Next**, **Next**, **Next**, then select **Remote Access**. 

![Remote Access](https://i.imgur.com/IFM8Vp3.png)

2. Click **Next**, **Next**, **Next**, select **Routing**, then click **Add Features**. DirectAccess and VPN (RAS) will automatically be selected for you.

![Routing](https://i.imgur.com/c12yapg.png)

3. Click **Next**, **Next**, **Next**, **Install**. Once it's done installing, click **Close** then **Tools, **Routing and Remote Access**.

![Routing and Remote Access](https://i.imgur.com/51TPHXK.png)

4. Right-click **DC (local), Configure and Enable Routing and Remote Access, Next**, then select **Network address translation (NAT), Next**
> If **Network address translation (NAT)** is grayed out, click **Cancel**, close the **Routing and Remote Access** window and reopen it again from **Tools**. 

5. Select **INTERNET**, then click **Next** and **Finish**.

![INTERNET](https://i.imgur.com/kBlcp6u.png)

6. Now you’ll see the DC icon with a green arrow pointing, which means **Routing and Remote Access Is Configured on This Server**.

![DC Configured](https://i.imgur.com/jGF4C2J.png)

### Install DHCP
+ The whole purpose of DHCP is to allow client computers on the network to automatically get their IP addresses instead of manually inputting them in.
+ We'll set up the DHCP server on the Domain Controller with scope information. This allows Windows 10 clients to get IP addresses (within a range we specify) and lets them browse on the Internet even though they’re on a private Internal network, just like in an office or school.

1. Back in **Server Manager**, click **Add roles and features, Next, Next, Next**, select **DHCP Server, Add Features, Next, Next, Next, Install**.

![DHCP Server](https://i.imgur.com/lyqdp8G.png)

2. After it installs, let's set up our scope by clicking **Close, Tools, DHCP**. 

![Tools DHCP](https://i.imgur.com/mggcPdZ.png)

3. In the left pane, expand **dc.mydomain.com** and **IPv4**. Right-click on **dc.mydomain.com** and click **Authorize**. 
4. Right-click on **IPv4** and select **New Scope...**

![New Scope](https://i.imgur.com/qWxfDcL.png)

5. Click **Next** and **Name** it the scope or desired IP range and leave the description blank. Click **Next**.

![Scope Name](https://i.imgur.com/DRwoTG7.png)

6. Enter in **Start IP address, End IP address, Length, and Subnet mask** as seen below:

![IP Address Range](https://i.imgur.com/yREJw00.png)

7. Click **Next, Next, Next** then enter in the **IP address: 172.16.0.1 ** on the **Router (Default Gateway)** page and click **Add**:
> 172.16.0.1 is the Domain Controller's IP address that has NAT configured on it.

8. Click **Next, Next, Next, Finish**
> If the arrows on both IPv4 and IPv6 aren't green, right-click **dc.mydomain.com, Authorize** then right-click **Refresh**.

9. If you expand **IPv4, Scope, Address Leases**, there's no leases yet because client computers haven't been created. DNS is now set up!

![Address Leases](https://i.imgur.com/QWpLPH5.png)

### Add Users Using PowerShell
+ Before creating a client computer and joining it to the domain, let's first use a PowerShell script to create a whole bunch of users in Active Directory created for instead of doing it manually.

1. Open up Edge browser from the toolbar. Download the zip file containing the scripts from my GitHub [here](https://github.com/cecilianguyencg/ActiveDirectoryHomeLab/archive/master.zip) by coping and pasting the link into Edge. 

![Download Zip](https://i.imgur.com/Uke6jil.png)

2. Click the **Download** icon and then the **Show in folder** icon to open up the zip file's location. 

![Zip Location](https://i.imgur.com/TvJkK5v.png)

3. Double-click the Zip, then drag the folder onto the **Desktop** to unzip the file and for easier access when we go into PowerShell. 

![Desktop Zip](https://i.imgur.com/Xdb2sSD.png)

4. In the unzipped folder, double-click the **names.txt** file to open it in Notepad. Add your name to the top of the list, then click **File, Save** then close the file.
> These list of names were generated using the PowerShell script **Generate-Names-Create_Users.ps1**

![Names](https://i.imgur.com/IqSDU4W.png)

5. Open up **PowerShell** by typing in **powershell** in the search bar in the taskbar. Right-click **Windows PowerShell ISE, Run as administrator**. Click **Yes**. 

![Open PowerShell](https://i.imgur.com/hraTe8m.png)

6. Click the **Folder** icon under **Edit** to open the script. Navigate to the **Desktop** or the location of the unzipped folder previously downloaded, and select **1_CREATE_USERS**.
> **1_CREATE_USERS** will create about 1,000 new accounts based on the **names.txt** file. When this script is run, it'll create users based on this code. A new Organization Unit will be created called **_USERS** with the **Protect container from accidental deletion box** unchecked. All new users will be added in this OU and the user accounts will all be created to have the password of **Password1**. The **user logon** will be created by looking at the first name initial and joining it with the last name from the **names.txt**.

7. Before running this script, we have to enable the execution of all scripts on this server, otherwise an error will occur. In PowerShell, type then hit **Enter**: `Set-ExecutionPolicy Unrestricted`. Select **Yes to All**.

![Execute](https://i.imgur.com/Kp08vqj.png)

8.  Before running the script, we need to change the directory so that it points to where the list of names is. In my case, I will type then hit **Enter**: `cd C:\users\a-cnguyen\Desktop\ActiveDirectoryHomeLab-main`

![Run Script](https://i.imgur.com/NW6CioN.png)

9.  The directory has now changed and we can click the **green arrow* in the top menu to **Run Script**. Click **Run Once**
> This scirpt will import the Active Directory module and start creating users. The blue text that shows in PowerShell is a result of line 13 in our script and can be manipulated any way you want, since this is just a visual indicator. PowerShell will repeat this script to create new Users until it's gone through all the names on the **names.txt**. 

![names](https://i.imgur.com/PxOPsFO.png)

10. Open up **Active Directory Users and Computers** to see the newly created **_USERS** OU and all the new users populated inside it. Right-click **_USERS, Find...**.
11. Change dropdown option **_USERS** to **Entire Directory** and type in your first name then click **Find Now**. Two accounts should show up with your first name, the administrator one we created and one we added to the **names.txt** earlier. In my case, a third account with a first name of Cecilia because it was created from the **Generate-Names-Create-Users** script. Crazy!

![Find](https://i.imgur.com/FFi06kZ.png)

### Create Client Machine
+ So far, pretty much everuthing is set up. Internet is connected, NICs are st up, domain server with users, NAT/RAS, and DHCP. Last step is to create a Windows 10 virtual machine in VirtualBox so that it can use internal NIC and get its IP address from DHCP server that we configured in our first virtual machine, the Domain Controller (DC).

1. Go back to VirtualBox and click **New** to create a new virtual machine. This will be our client machine.
   + Name: CLIENT1
   + ISO Image: Windows 10 ISO
   + Edition: Windows 10 Pro
   + Uncheck Skip Unattended Installation

![Client](https://i.imgur.com/NNDgwhx.png)

2. Click **Next**, increase Base Memory: 4096 MB, increase Processors: 2, **Next, Next, Finish**.
3. Click **Settings**, Network, Adapter 1,** and change the default **Attached to: NAT** to **Internal Network**. Click **OK**
> Instead of using NAT and connecting to our home network, choosen Internal Network so the client machine can get a DHCP address from the Domain Controller (DC).

![Internal Network](https://i.imgur.com/luavjAi.png)

4. Start or double-click **CLIENT1**. The installation window appears, click **Next** and then **Install now**. Click **I don't have a product key** at the bottom.
5. Select **Windows 10 Pro** then **Next**. Check **I accept the license terms** then **Next**, **Custom: Install Windows only (advanced)**, and then finally **Next**.
> Make sure to choose Windows 10 Pro because Windows 10 Home can't join a Domain.

![10 Home](https://i.imgur.com/AuEXhRg.png)

6. Select your region, then click **Yes**, choose your keyboard layout, click **Yes**, and then **Skip**.
7. Choose **Set up for an organizaiton** since this client machine will be joining a domain, then click **Next**.

![organizaiton](https://i.imgur.com/EZAT1XD.png)

8. Click **Domain join instead** towards the bottom of the screen.
![domain join](https://i.imgur.com/8JPsYMZ.png)

9. Next, it'll ask what name to use. Since this is going to be the local username name, we'll call it: **user**. Enter in a password, confirm password, then enter in answers for security questions.
10. Continue with the installation, choosing to either accept or deny certain settings, and then installation will begin.
11. After installation, let's check if the internet is working by opening up **Command Prompt** by typing **cmd** into the search bar in the taskbar.
12. In command prompt, type `ipconfig` to check for internet settings. 

![ipconfig](https://i.imgur.com/eamvn6g.png)

13. Internet is working so let's **ping** a website like Google by typing `ping google.com` in **Command Prompt**. Google.com resolved, meaning their DNS server is working.

![piing]([https://i.imgur.com/eamvn6g.png](https://i.imgur.com/e7lZNvl.png)

14. We can also ping our domain by typing `mydomain.com` in **Command Prompt**. The ping responds.

![ping mydomain](https://i.imgur.com/S5SrZPY.png)

15. Find the name of your PC in Command Prompt by typing `hostname`.

![hostname](https://i.imgur.com/XggV6nU.png)

16. Let's rename this PC. Right-click **Start Menu, System**. Instead of clicking **Rename this PC**, scroll down and click on **Rename this PC (advanced) so that we can rename the PC and join a domain at the same time.

![rename advanced](https://i.imgur.com/bxTbe9F.png)

17. Don't enter a **Computer description**, instead click **Change** towards the bottom. Let's change Computer name to **CLIENT 1** and under Member of, choose **Domain** and type in **mydomain.com**. 

![computer name domain](https://i.imgur.com/tCIuS2i.png)

18. Click **OK** and then enter in a user name and password that was created on our first virtual machine, the **Domain Controller**. A window will appear indicated this client machine is part of **mydomain.com** domain. Click **OK**, **OK**, then restart for changes to go into effect. 

![permission to join](https://i.imgur.com/otj4uzM.png)

19. Go back to our **DC** VM and open up **DHCP** in the **Server Manager**. Expand **dc.mydomain.com, IPv4, Scope** then click on **Address Leases** to see 1 lease from our **CLIENT1** computer.
> When you created a client computer, **CLIENT1** and joined it to the network, it reached out to the DHCP server automatically and requested an address. The DHCP server gave it an address and now you see the lease located in the right pane.

![address leases](https://i.imgur.com/5GUFG0d.png)

20. Let's open up **Active Directory Users and Computers** again. Expand **mydomain.com** and click on the **Computers** OU. We can now see that after joining the client computer to the domain, it automatically shows up here and is a member of the domain.
> If you were to delete CLIENT1 here, and tried to log back into **CLIENT1**, you wouldn't be able to use a normal user account that was created. Since it joined the domain, you can use any accounts to log into the computer.

![Computers](https://i.imgur.com/Wbv8Bbe.png)

21. Going back to our **CLIENT1** VM, click **Other user**. Instead of logging in with the local user account when we made when installing Windows 10, we can use one of the user accounts created on the **Domain Controller**. I logged in with **cnguyen**. Now any user part of **MYDOMAIN** can log into **CLIENT1**.

You've successfully created an Active Directory home lab environment, creating two virtual machines, and adding users with PowerShell. Thank you for joining me on this tutorial walk-through! 😊  
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
