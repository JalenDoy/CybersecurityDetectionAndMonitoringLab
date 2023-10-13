# <h1>Cybersecurity Detection and Threat Monitoring Lab</h1>

<h2>Description</h2>
Project consists of a simple PowerShell script that walks the user through "zeroing out" (wiping) any drives that are connected to the system. The utility allows you to select the target disk and choose the number of passes that are performed. The PowerShell script will configure a diskpart script file based on the user's selections and then launch Diskpart to perform the disk sanitization.
<br />


<h2>Languages and Utilities Used</h2>

- <b>pfSense</b>
- <b>Security Onion</b>
- <b>Splunk</b>
- <b>Splunk Unvierseral Forwarder</b>
- <b>Windows Domain Controller</b>
- <b>Windows Event Forwarder</b>

<h2>Environments Used </h2>

- <b>Windows 10</b>
- <b>Windows Server 2019</b>
- <b>Ubuntu Desktop</b>
- <b>Ubuntu Server</b>
- <b>Kali Linux</b>

<h2>Lab Walk-Through:</h2>

<h2>Installing and Configuring pfsense:</h2>

1. Installed a community edition pfSense ISO file from https://www.pfsense.org/download/
2. Launched VMware Workstation Pro 16 and clicked on "Create a New Virtual Machine"
3. Extracted the downloaded iso zip file to a seperate folder and selected the iso image in Workstation Pro
4. Selected the location where I wanted to save the virtual machine and renamed it "pfSense"
5. Chose the listed options for the virtual machine:
    - 20GB Maximum Disk Space
    - Split virtual disk into multiple files
    - 2GB Ram
    - 6 Network Adapters (Clicked "customize hardware", "add", and then select "Network Adapter")
6. After 5 Network adapters were added, connected each one to a VMnet by number. Network Adapter 2 connected to VMnet2, Network Adapter 3 connected to VMnet3, and so on. 
<p align="center">
Linking the Network Adapters to VMnet(s): <br/>
<img src="https://i.imgur.com/nAhOl5o.png"/>

7. Clicked finish and launched the virtual machine
8. Selected all the default options when booting the pfSense installed and rebooted
9. Assigned the interfaces (See screenshot below)
<p align="center">
Linking the Network Adapters to VMnet(s): <br/>
<img src="https://i.imgur.com/EWqodQw.png"/>

10. Selected "y" to proceed and then configured the LAN IPv4 address as 192.168.1.1
11. Typed "24" as the LAN IPv4 subnet bit count (255.255.255.0) and entered the start address range as "192.168.1.11" and the end address range as "192.168.1.200"
<p align="center">
Configuring IP address for the LAN: <br/>
<img src="https://i.imgur.com/FwNolCC.png"/>

12. Repeated the same steps as step 10 for the rest of the interfaces but changed the IP addresses. em4 was left blank as it will serve as the span port. 
<p align="center">
Configuring IP address for the interfaces: <br/>
<img src="https://i.imgur.com/rFUothA.png"/>

<h2>Installing and Configuring Security Onion:</h2>

1. Downloaded the Security Onion ISO file from https://github.com/Security-Onion-Solutions/securityonion/blob/master/VERIFY_ISO.md
2. Launched VMware Workstation Pro and selected "Create a New Virtual Machine" and selected the Security Onion ISO file when prompted
3. Changed the virtual machine name to  "SecOnion" and chose my portable SSD drive as the save location
4. Selected the following requirements for the virtual machine:
   - 250GB Maximum Disk Space (Security Onion recommends 200GB)
    - Single Virtual Disk File
    - 16GM of ram (Security Onion recommends 12GB)
    - 2 Additional Network Adapters (Network Adapter 2 is set to VMnet4 and Network Adapter 3 is set to VMnet 5)

<p align="center">
Final configuration of Security Onion Virtual Machine: <br/>
<img src="https://i.imgur.com/lRwmFUQ.png"/>

5. Launch the SecOnion Virtual Machine
6. Press "Enter" to start the installation
7. The installationg process will start and once the scripts are installed it will ask me to create a username and password
8. After creating the username and password, the system will reboot and login to continue to the Security Onion Setup process

<p align="center">
Security Onion Setup: <br/>
<img src="https://i.imgur.com/3kmJ2tA.png"/>

9. When asked "Would you like to continue" I will select "Yes"
10. Select "Install Run the standard Security Onion installation"
11. Choose "EVAL" as the install type
12. Type "Enter" when prompted to agree to the terms of the Elastic License
13. Type in the hostname of your choosing. I chose 'seconion"
14. Selected "ens33" (first interface that was available" as the Management Link
15. At the "Choose how to set up your management interface", press spacebar on "DHCP" and them press "Enter". At the warning screen go ahead and select "Yes" and then "Ok".
16. At the "How should this manager be installed?" screen, select "Standard" and press "Enter". At the bottom left "Restarting Docker" should appear.
17. At the "How would you like to connect to the Internet?" screen, select "Direct" and press "Enter".
18. At the "Please add NICs to the Monitor Interface" select "ens34".
19. Choose "Automatic" for the OS patch schedule
20. Leave the default options when asked about Home Networks and CIDR blocks.
21. Press "Enter" when notifed that installing more services requires using more RAM.
22. Press "Enter" to keep the default Docker IP range.
23. Enter a email address for the Administrator Account for the Web Interface. I chose my personal email address. When prompted to enter a password, ensure that it is something that you can remember.
24. When prompted on how to access the Web Interfaces, choose the "IP" option and then click "Yes" to configure NTP servers and keep the default options.
25. Select "No" when asked to run "so-allow". This will be configured later.
26. The next screen will show all the services we have access to, make sure to take a screenshot of this page and save it. Press "TAB" to Select "Yes" to continue. This Installation will take a long time so head to the "Installing Ubuntu Desktop" section that is located below before the Security Onion installion completes. 
27. After installing Ubuntu Desktop, go back to the SecOnion virtual machine so that so-allow can be "configured"

<p align="center">
Security Onion Virtual Machine: <br/>
<img src="https://i.imgur.com/pfi9tus.png"/>

28. Type the following command "sudo so-allow" and enter the password. When asked for the role, type "A" and then the IP address of the Ubuntu Desktop Machine. When completed the following screen should appear.

<p align="center">
Ubuntu Desktop allowed access: <br/>
<img src="https://i.imgur.com/JhpVheP.png"/>

29. Go to the Ubuntu machine, open a web browser and enter the IP address of the Security Onion Virtual Machine. Select "Accept Risk and Continue"
30. You should be at the Security Onion website where there is a prompt asking for a email address and password. These are the credentials that we setup earlier. Enter them in and you should be able to access your Security Onion Web Interface.

<p align="center">
Security Onion Web Interface: <br/>
<img src="https://i.imgur.com/blvcD80.png"/>  

<h2>Installing Ubuntu Desktop:</h2>
While Security Onion is installing, Ubuntu Desktop will be downloaded. Ubuntu will be used to access Security Onion. 

1. Download Ubuntu Desktop here: https://ubuntu.com/download/desktop
2. Once downloaded, navigate to VMWorkstation PRO and click "Create a New Virtual Machine".
3. Select the Ubuntu ISO image that was downloaded and enter the following:
    - Full Name: SecOnionMgmt
    - Username: Username of your choosing
    - Password: Password of your choosing
4. Enter "SecurityOnionMgmt" as the Virtual Machine net and enter the following settings:
    - Maximum Disk Size: 20GB
    - Split Virtual Disk into Multiple Files
    - Keep the default hardware settings and click "Finish". The Ubuntu machine will boot up after creation.
  5. Once installed, open a terminal and type the command "sudo apt install net-tools". Enter the password for you account and once the installation is done, type "ifconfig" and make a not of the "inet" IP address.
  6. Procced to the rest of the Security Onion Installation that is located above.

<h2>Installing Kali Linux:</h2>

1. Download the Kali Linux VMWare 64-bit ISO image from here: https://www.kali.org/get-kali/#kali-virtual-machines
2. Once downloaded, search through the Kali linux folder contents, and double click on the vmx file. This will load a prebuilt Kali Linux machine in VMWare Workstation PRO.

<p align="center">
Kali Linux vmx file: <br/>
<img src="https://i.imgur.com/zhY606m.png"/>  

3. Once loaded, click "Edit Virtual Machine Settings" and click "add", and choose "Network Adapter"
4. Click on the added network adapter (Network Adapter 2), Under "Network Connection" section click "custom" and in the drop down menu select "VMnet2".
5. If the virtual machine is not using 4GB of RAM. Click on "Memory" and adjust the slider to "4GB" or "4096MB" and then click "OK".

<p align="center">
Kali Linux Configuration Settings: <br/>
<img src="https://i.imgur.com/Uivnoeb.png"/>  

6. Now "Power On" the virtual machine
7. To login, enter the following credentials:
    - Username: kali
    - Password" Kali
8. Now we have a fully installed Kali Linux virtual machine for our network!

<h2> Configuring pfSense Interfaces: </h2>

1. In WMWare Workstation Pro, boot up the Kali Linux and pfSense virtual machines.
2. Once booted, on the Kali machine open a new browser and enter "192.168.1.1" in the url search bar which should bring up the pfSense login page. Enter the following:
    - Username: admin
    - Password: pfsense
3. Once logged in, click the "system" drop down menu and then click on "setup wizard". From here click next twice until you reach the **General Information** page. On this page you will enter the following:
    - Primary DNS Server: 8.8.8.8
    - Secondary DNS Server: 4.4.4.4
4. Click "Next". On the **Time Information** page, select your time zone. Click "Next".
5. On the **Configure WAN Interface** page, **uncheck** the following at the bottom:
    - Block private networks from entering via WAN
    - Block non-internet routed networks from entering via WAN
6. Click "Next". On the **Configure Lan Interface** page click "Next"
7. On the **Set Admin WebGUI Password** page, Enter a new password. Once done click "Next". If asked to save password click "Dont Save". Next is to click on "Reload" so the new changes can be put into effect. Once reloaded, click "Finish".
8. At the top of the pfSense web application, click on the "Interfaces" drop down menu and click on "Lan". From here we will enter the following information for the Kali Linux machine:
   - Description: Kali
   - IPv6 Configuration Type: Static IPv6
   - IPv6 Address: ::1
9. Click "Save Changes". Next click on "Services" drop down menu in the top right and click on "DHCPv6 Server & RA". Click on "Router Advertisements" and change the following:
     - Router Mode: Disabled
10. Next we will go back to our kali interface and changed **IPv6 Configuration Type** to "None". Click "Save".
11. At the top of the pfSense web application, click on the "Interfaces" drop down menu and click on "OPT1". From here we will enter the following information:
    - Description: VictimNetwork
12. Click "Save" and then "Save Changes". 
13. At the top of the pfSense web application, click on the "Interfaces" drop down menu and click on "OPT2". From here we will enter the following information:
    - Description: SecOnion
14. Click "Save" and then "Save Changes".
15. At the top of the pfSense web application, click on the "Interfaces" drop down menu and click on "OPT3". From here we will enter the following information:
    - Enable: Enable Interface (Check the box)
    - Description: SpanPort
16. Click "Save" and then "Save Changes".
17. At the top of the pfSense web application, click on the "Interfaces" drop down menu and click on "OPT4". From here we will enter the following information:
    - Description: Splunk
18.  Click "Save" and then "Save Changes".
19. At the top of the pfSense web application, click on the "Interfaces" drop down menu and click on "Assignments". From here we will check to ensure all the interfaces are correctly configured.

<p align="center">
pfSense Interfaces: <br/>
<img src="https://i.imgur.com/nLeYDsw.png"/>    

20. At the top of the pfSense web application, click on the "Bridges" drop down menu and click on "Add". From here we will enter the following information:
    - Member Interface: VICTIMNETWORK
21. Click on "Show Advanced". Enter the following:
    - Span Port: SPANPORT
22. Click "Save".
23. At the top of the pfSense web application, click on the "Firewall" drop down menu and click on "Rules". Under **WAN**, click on "Add". From here we will enter the following information:
    - Protocol: Any
24. Click "Save" and then "Apply Changes". 

<p align="center">
Firewall Rule: <br/>
<img src="https://i.imgur.com/KCOTzxP.png"/> 

<h2> Installing Windows Server 2019 and Creating an Active Directory Environment: </h2>

1. Downloaded Windows Server 2019 ISO image Evaluation from: https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019.
2. Open VMWare Workstation Pro and select "Create a New Virtual Machine".
3. Click "Next" and select the Windows Server ISO image that was just downloaded. Ignore the screen asking for a product key and click "next" and then "next" again.
4. Choose any location to install the Virtual Machine. Now choose 60GB for Maximum Disk Size and ensure the radio button next to "Split virtual disk into multiple files" is selected. Click "next". 
5. Select customize harder and click on "Network Adapter section in the left pane" then select "Custom: Specific virtual network" and select "VMnet3". Click "close" and then "finish".
6. At the home screen of VMWare Workstation Pro, On the Windows Server 2019 machine. Click "edit virtual machine settings" and remove "Floppy" from the settings. Now power on the Virtual Machine.
7. When booting the machine, ensure to press any key when prompted to, to start the installation process. Click "next" on the language and time screen and then click "install now".
8. When choosing which operating system to install, select "Windows Server 2019 Standard Evaluation (Desktop Experiance)". Click the box to check "I accept the license terms".
9. On the next screen click "custom". Now select "New" at the bottom of the screen and then click "apply". Click "ok". THe following screen should appear,  if it does then click "finish". Windows Server should now start the installation process. 

<p align="center">
Windows Drive Partition Setup: <br/>
<img src="https://i.imgur.com/tdf60fL.png"/> 

10. When prompted, create a password for the windwows administrator account and then click "next". Now sign into the account.
11. In the left ov VMware. Click on "VM" and then select "Install VM Tools". CLick on the "DVD Drive (D:) VMware Tools popup in the bottom right of the screen and double click "Run setup64.exe".
12. Now follow the next few steps. Click "next". Choose "Complete" for the type of install, then "next", and then "finish". Aftwards the resolution should be fixed and fit the entire screen. Now restart the Virtual Machine.
13. Login after the restart and click the start menu and then select settings in the bottom left. Search for "PC Name" and then click on "Rename your PC". Rename the PC to anything but put "-DC" at the end to signify that this host is the Windows Domain Controller. Restart the machine.
14. Login again. In the top right, click "Manage" and then "Add roles and features". Click "next" until the **Select server roles** page appears. Click on "Active Directory Domain Features" and then click "Add features". Now click "next" until the **Confirm installation selections** page appears and click "Install".
15. When the Installaion is finished, click on the flag icon in the top right and then click on "Promote this server to a domain controller". Now select "Add a new forest" and specify a root domain name. It can be any name aslong as it is followed by ".local". Click "Next".
16. Create a password and then select "next" until the **Installation** screen appears and then select "Install". The service will now reboot.
17. Login again. In the top right, click "Manage" and then "Add roles and features". Click "next" until the **Select server roles** page appears. Click on "Active Directory Certificate Services" and then click "Add features". Now click "next" until the **Confirm installation selections** page appears and click "Restart the destination server automatically if required". Then select "Yes" and then "Install".
18. Refresh the dashboard and then click the flag in the top right and select "Configure Active Directory Certificate Services on the Destination server". On the **Role Services** screen select "certificate authority" and then click "next".
19. Click next until the **Validity Period** screen appears". Change it to "99" years and then click next until the **Confirmation** screen appears and then select "Configure". Once it finishes, manually restart windows be click on the start menu on the bottom left and then select "restart". 
20. Now a Active Directory user will be created by selection "Tools" in the top right and then clicking on "Active Directory Users and Computers". On the left pane, select the domain that was created, right click on "Users", and then select "New" and lastly "User". Enter any information for the new user, an Example is listed below.

<p align="center">
Active Directory User: <br/>
<img src="https://i.imgur.com/uoRd0Wf.png"/> 

21. Ensure to choose a easy and simple password. Nothing complicated for brute force exercises. Uncheck "User must change password at next logon" and check "Password never expires". Click "next" and then "finish".
22. Once done, the firewall must be turned off. This is to ensure that the lab has weak configuration and vulnerabilities to be exploited as this is a lab environment built for learning. Search for "Firewall" in the Windows search bar and then click on "Windows Defender Firewall". Now Turn everything off and click "Okay".
23. Now its time to add pfSense as the default gateway. Search for "Control Panel" in the Windows search bar and click on "Control Panel". Next click on "Network and Internet", then "Network and Sharing Center", and on the left hand panel choose "Change adapter settings". Right click on **Ethernet0** go to **Properties** and double click on **Internet Protocol Version 4 (TCP/IPv4) Properties**.
24. Select **Use the following IP address:** and enter the following configurations:
    - IP Address: 192.168.2.10
    - Subnet Mask: 255.255.255.0
    - Default Gateway: 192.168.2.1
    - Preferred DNS server: 192.168.2.1
25. Now click "Okay" until all of the popups are gone. 

<h2>Installing and Configuring a Windows 10 Host:</h2>

1. Boot up the Kali Linux and pfSense virtual machines and access the pfSense interface using the Kali machine. Open a web browser and type in **192.168.1.1** and then login to the pfSense admin account.
2. Go to **Servers** and then click on **DHCP Server** and select **VICTIMNETWORK**. From here, go to the **Server** section and add the IP address (192.168.2.10) of the Domain Controller as the DNS server. Next scroll down to **Other Options** section and next to **Domain Name** enter the name of the Domain Controller that was created. 
3. On the homepage of VMware click on **Create a New Virtual Machine** and choose the **Typical** option. Select the Windows 10 Enterprise ISO image that can be downloaded from here" https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise.
4. Click **Next**, **Next**, and then **Okay**. Change the name to **Windows 10**, and click **Next** and **Next** again. Now click on **Customize Hardware**. Set the **Network Adapter** to **VMnet3** and click **Okay**. Now uncheck **Power on this virtual machine after creation** and click **Finish**.
5. Once the Windows 10 machine is created, click on **Edit virtual machine settings**, click on **Floppy**, and then select the **Remove** button at the bottom and click **Ok**.
6. Now boot up the machine and prepare to press any key on the keyboard when prompted to. Choose a language and then press **Next**. Check the box next to **I accept the license terms** and click **next**. Now select **Custom: Install Windows only (Advanced)** and click **next**. The installation process should start and the machine will reboot.
7. Once restarted, choose a region when prompted to. Choose the correct keyboard layout. Select in the bottom left **I do not have internet**. Create a name and Password for the PC and click **Next**. When prompted to select security questions, make every answer the same and simple so that it is easy to remember.
8. Turn off all the privacy settings for the device when prompted and click **Accept**. Click **Not now** when prompted about Cortana.
9. Once the machine loads, right click on the Windows 10 virtual machine in VMware and select **Install VMware Tools** and then click on **Install**. Click on the **DVD Drive (D:) VMware Tools** popup and double click on **Run setup64.exe**. Click **Yes**.
10. When the **VMware Tools Setup** page appears, click **Next**, **Complete**, and **Install**. Once it is done, click **Finish** and then **Yes** to restart the machine. Now Login.
11. In the Windows search bar, search for "View your PC Name" and then click on it. Now click on **Rename your PC** and rename the PC to the user that was created on the Windows 10 Machine. Click **Next** and then **Restart Now**.
12. Once rebooted, in the bottom right click on internet icon and then select **Network & Internet Settings**. Click on **Change Adapter Options**. Right click on **Ethernet0** and double click on **Internet Protocol Version 4 (TCP/IPv4) Properties**. Click on **Use the following IP address** and enter the following configurations:
    - IP Address: 192.168.2.31
    - Subnet Mask: 255.255.255.0
    - Default Gateway: 192.168.2.1
    - Preferred DNS Server: 192.168.2.10
13. Press **Ok** and then **OK** again. In the Windows search bar, search for "Access work or School" and click on it. Next, click on the **Connect** button. When prompted to enter a email address, click **Join this device to a local Active Directory Domain** at the bottomn of the **Microsoft Account** pop-up.
14. Now enter the name of the Domain that was created and click **Next**. When asked to enter a domain account, enter the following username and password"
    - Username: Administrator
    - Password: (Any passowrd of your choosing as long as it is easy to remember)
15. Click **Ok** and then **Skip** and lastly **Restart Now**. Once rebooted, log back in and then click on the Windows Server 2019 Virtual Machine. In the top right click on **Tools**, and then **Active Directory Users and Computers** and then double click on **Computers** to ensure that the new user was added. 

<h2>Installing and Configuring Splunk:</h2>

1. Download Ubuntu Server ISO image from here: https://ubuntu.com/download/server
2. Launch VMware Workstation Pro and click on **Create a New Virtual Machine**. Choose **Typical** and then select that Ubuntu Server ISO image that was just downloaded. Change the Virutal Machine name to "Splunk". Click **Next**.
3. Choose **100GB** as the maximum disk size and then select **Store Virtual Disk as a single file**. Click **Next**.
4. Click on **Customize Hardware** and add an additional network adapter and set it to **VMnet6**. Click **Close** and then select **Power on this virtual machine after creation**. Click **Finish**.
5. On the Splunk machine, continue with the installation process by selecting the correct language and keyboard layout. Select **Install Ubuntu**. Press **Enter** when at the network connections screen. Press **Enter** at the Proxy server screen.
6. Press **Enter** on the Mirror address screen. On the **Guided Storage Configuration** screen, press down on the arrow keys and hover over **Done** and press **Enter**. On the next page press **Done** and then **Continue**.
7. Choose any name, username, and password for the **Profile setup** screen but make sure the server name is "Splunk". Scroll down and press **Enter** on Done.
8. Press **Enter** to skip upgrading to Ubuntu pro. Check the box asking to install **SSH** and then hover over **Done** and press **Enter**. Arrow key down and Press **Enter** as no Server Snaps will be downloaded. The installation will start and the Virtual Machine will prompt to be rebooted. Under **View full log**, hover over **Reboot Now** and press **Enter**.

<p align="center">
Reboot Now: <br/>
<img src="https://i.imgur.com/JKLV4yY.png"/> 

9. When rebooted, log into Splunk user the credentials created. Once logged in, enter the command "sudo apt install taskel". Enter "y" when prompted to. **Tasksel** will allow for a **Graphical User Interface (GUI)** to be installed.
10. Next enter the command "sudo apt install ubuntu-desktop". Enter **y** when prompted to.
11. At the **Services to be restarted** screen, press **enter** and then type "Reboot" to restart the Virtual Machine. Once rebooted, ensure to log in again.
12. Open a new web browser and navigate to **www.splunk.com**. In the top right corner of the page, click on **Free Splunk** and sign in or create an account. Once logged in, in the top left, click on the **Products** drop down menu and then select **Splunk Enterprise**. Click on the **Free Trial** button and select the **.tgz** file.

<p align="center">
Splunk Homepage: <br/>
<img src="https://i.imgur.com/2WTArFY.png"/> 

13. Open a brand new terminal and navigate to the **Downloads** directory by typing in "cd Downloads". Now type "ls -l" to ensure the downloaded splunk file is there.

<p align="center">
Splunk File in Downloads Directory: <br/>
<img src="https://i.imgur.com/kHgQUTa.png"/>  

14. Next type "tar xvzf" and then the name of the downloaded splunk file and then press **Enter**. Once done, type "cd splunk", and then "cd bin" to change into the **splunk/bin** directory. Now splunk will be started be typing "./splunk start".
15. Press **Spacebar** until prompted to enter "y" to continue. Now enter a administrator username and password of any choosing. Once done the followin screen should appear.

<p align="center">
Splunk installation finished: <br/>
<img src="https://i.imgur.com/vJNc9co.png"/>  

16. In a web browser, type in **splunk:8000** and then log into the splunk interface using the login credentials that was just created. A **Splunk** instance has now been created.

<p align="center">
Splunk Instance <br/>
<img src="https://i.imgur.com/VbqXOgr.png"/>  

<h2>Installing Universal Forwarder:</h2>

1. Start by booting up the Splunk machine and navigate to **Downloads/Splunk/Bin** Directory and type "./splunk start" to start **Splunk**. 
2. When **Splunk** is started. Access **Splunk** by typing in "splunk:8000" into a web browser and sign in. Once signed in, in the top right click on **Settings** and then **Forwarding and Receiving**. 
3. Click on **Configure Recieving** and then select **New Recieving Port** in the top right. Type in "9997" and then click **Save** when asked to enter a port to listen on. 
4. Click on **Settings** in the top right and then click on **Indexes**. Click on **New Index** in the top right and then enter the index name as "wineventlog". Scroll to the bottom and click **Save**. 
5. Open the Domain Controller machine and navigate to https://www.splunk.com/en_us/download/universal-forwarder.html. (It may be easier to use Google Chrome). Once at the Splunk website, login. 
6. Download the 64-bit file that supports Windows 10, Windows 11, and Windows Server. Once downloaded navigate to the **Downloads** folder and double click the splunk forwarder. Check the **License Agreement** box and ensure that **An on-premises Splunk Enterprise Instance** option is selected and then click **Customize Options**

<p align="center">
Universal Forwarder Correct File <br/>
<img src="https://i.imgur.com/zmxan4y.png"/>  

7. Click **Next** and set the username and password the same as the **Splunk** instance. Set the **Hostname or IP** of the **Deployment Server** to the IP address of the **Splunk** instance and enter "8089". Do the same for the **Receiving Indexer** but use "9997" instead. Click **Next** and then **Install**. When it is done click **Finish**.
8. Go back to the **Splunk** instance and then click on **Services** and on the left hand side select **Add Data**. Under **Available Host(s)** click the domain that was created and enter "Domain Controller" as the **New Server Class Name**. Click **Next"**. 

<p align="center">
Select Forwarders <br/>
<img src="https://i.imgur.com/hv2iahQ.png"/> 

9. Click on **Local Event Logs** and select all of the available items (Application, ForwardedEvents, Security, Setup, System) and make sure they transfer over to Selected Items. Click **Next**.
10. Under **Default** drop down menu, select **wineventlog**. Click **Review** and ensure the following setups are selected and then click **Submit**.

<p align="center">
Review Settings <br/>
<img src="https://i.imgur.com/GkLWBSW.png"/> 

11. Click **Start Searching**. Now the lab is complete and is your own playground to learn different aspects of cybersecurity.

<p align="center">
Finished Configuration of Splunk and the Cybersecurity Detection and Monitoring Lab <br/>
<img src="https://i.imgur.com/krWcusU.png"/> 

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
