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
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
