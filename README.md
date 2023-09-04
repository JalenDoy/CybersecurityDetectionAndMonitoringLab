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


<p align="center">
Launch the utility: <br/>
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
