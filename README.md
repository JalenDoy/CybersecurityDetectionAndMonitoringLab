# <h1>Cybersecurity Detection and Threat Monitoring Lab</h1>

<h2>Description</h2>
Project consists of a simple PowerShell script that walks the user through "zeroing out" (wiping) any drives that are connected to the system. The utility allows you to select the target disk and choose the number of passes that are performed. The PowerShell script will configure a diskpart script file based on the user's selections and then launch Diskpart to perform the disk sanitization.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Diskpart</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)

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
26. The next screen will show all the services we have access to, make sure to take a screenshot of this page and save it. Press "TAB" to Select "Yes" to continue. This Installation will take a long time so grab some coffee and take a break!

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
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
