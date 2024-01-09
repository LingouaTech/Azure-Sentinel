# Azure Sentinel Geolocation RDP (brute force attacks)
<h2>Description</h2>
The purpose of this project is to set up Azure Sentinel (SIEM) and connect it to a live virtual machine (VM) acting as a honey pot. Make the VM vulnerable, and monitor and log different global attacks. We will use a PowerShell script to programmatically extract the IP address from the Windows EventLogs and send it to a third-party API (ipgeolocation.io). The API will derive the latitude, longitude, state, province, etc of the failed Remote Desktop Portal attacks and send it back to our VM which we will use to create a custom log with geographical data. Then we will configure Azure Sentinel Workbook where we will display the global attack data (RDP brute force) on a world map according to physical location and magnitude of attacks.
<br />

<h2>Languages Used</h2>

- <b>PowerShell</b>: Extract RDP failed login logs from Windows Event Viewer

<h2>Utilities Used</h2>

- <b>Azure</b>

- <b>ipgeolocation.io</b>: IP Address to Geolocation API
  
<h2>Environments Used </h2>

- <b>Windows 10</b> 

- <b>Virtual Machine</b>

<h2>Walk-through:</h2>

<p align="center">
Create VM: Configure the SECURITY setting and create our own inbound rule that will allow ANY traffic from the internet into the VM: <br/>
<img src="https://i.imgur.com/tPYKqpc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Create our Log Analytics Workspace (LAW) and connect it to the VM. Then create Sentinel (SIEM) and connect it to the VM:  <br/>
<img src="https://i.imgur.com/M2vrel6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
In VM, turn off the firewall, and ping the VM IP from our personal computer to test for response. Retrieve API key (Geospatial) to insert into PowerShell Script: <br/>
<img src="https://i.imgur.com/X6vfzYG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
The script loops and looks through the event logs (security logs) looks at the failed logins, grabs their IP address, and gets the geo data to create a new log file. 
The script looks through the Event Viewer for failed '4625' events > sends the IP address to API > API outputs country and geo data > data gets sent to the 'failed_rdp' folder. 
Afterward, copy the content from the “failed_rdp" folder in the VM to our personal desktop and save it to later upload to Log Analytics Workspace in Azure.
:  <br/>
<img src="https://i.imgur.com/UG8C5wa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
In LAW, create a custom log that will allow us to bring in the custom log with the geo-analytics. Uploaded the copied Txt file from our personal desktop and route the “Failed_rdp” file address from our VM to the custom log. Use the script to categorize the event data:  <br/>
<img src="https://i.imgur.com/zfIHzEF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Set up a new workbook for Azure Sentinel that will allow us to create a Geo map. Add new query (previous script used to categorize the event data), configure settings, and visual as a map
<br />
:  <br/>
<img src="https://i.imgur.com/ly40do8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Snapshot of our Geo map with the location of incoming RDP Brute attack attempts:  <br/>
<img src="https://i.imgur.com/rJZvmkN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
