<h1>Azure Sentinel Tutorial Map with live Cyber Attacks | SIEM</h1>

<h2>Description</h2>
<b>The Powershell script in this repository parses Windows Event Log information for failed RDP attacks and uses a third-party API to collect geographic information about the attacker's location.
</b>
<br />
<br />
The script is used in this demo where I set up Azure Sentinel (SIEM) and connect it to a live virtual machine acting as a honey pot.
We will observe live attacks (RDP Brute Force) from all around the world. I will use a custom PowerShell script to look up the attacker's Geolocation information and plot it on an Azure Sentinel Map!
<br />
<br />

<h2>Languages and Utilities Used</h2>

- <b>PowerShell:</b> Extract RDP failed logon logs from Windows Event Viewer 
- <b>ipgeolocation.io:</b> IP Address to Geolocation API
- <b>Kusto Query Language (KQL): </b> Ingest logs and create visualizations in Azure


<h2>Setup</h2>
The first series of steps involves setting up an Azure account and creating a virtual machine.

Change the firewall settings of the VM so that it allows all incoming traffic.

<p align="center">
<img src="https://i.imgur.com/p0t1k2r.png" height="50%" width="50%" alt="Image Analysis Dataflow"/>
</p>

Create a log analytics workspace that will allow all the logs from the vm to be stored.
<p align="center">
<img src="https://i.imgur.com/VNIYPTR.png" height="50%" width="50%" alt="Image Analysis Dataflow"/>
</p>

Next up is to set data collection to 'All Events' so that it captures everything. Go to Microsoft Defender for Cloud and go to 
the environment settings so set this.

<p align="center">
<img src="https://i.imgur.com/6fr5N1e.png" height="50%" width="50%" alt="Image Analysis Dataflow"/>
</p>


Connect the log analytics workspace to the VM.

<p align="center">
<img src="https://i.imgur.com/OKop1j8.png" height="50%" width="50%" alt="Image Analysis Dataflow"/>
</p>


<h2>Virtual Machine</h2>
Remote Desktop Protocol into the created VM. For the first attempt, purposely failed the login attempt so we will see this information in the logs. When successfully 
rdp into the VM, go to Event Viewer and you can see the failed login attempts in the logs.
<p align="center">
<img src="https://i.imgur.com/cSuwUht.png" height="50%" width="50%" alt="Image Analysis Dataflow"/>
</p>

We can check the IP address and use it to search for the location then.

<p align="center">
<img src="https://i.imgur.com/jMN4V38.png" height="50%" width="50%" alt="Image Analysis Dataflow"/>
</p>

<h2>Powershell</h2>
Created a PowerShell script to search the security logs for failed login attempts, this script runs indefinitely.

<p align="center">
<img src="https://i.imgur.com/zj4gYjz.png" height="50%" width="50%" alt="Image Analysis Dataflow"/>
</p>

<h2>Attacks from Indonesia coming in; Custom logs being output with geodata</h2>


<p align="center">
<img src="https://i.imgur.com/9vvffez.png" height="50%" width="50%" alt="Image Analysis Dataflow"/>
</p>

<h2> Create Custom Logs</h2>
Create a custom log in Log Analytics Workspace. Once created we query the custom table

<p align="center">
<img src="https://i.imgur.com/dVojaGb.png" height="50%" width="50%" alt="Image Analysis Dataflow"/>
</p>

<h2>Create a World Map Workbook in Microsoft Sentinel</h2>

We now need to create a visual representation to show the world map of where the login attempts are originating from. Create a new workbook
in Azure Sentinel and add the same query as used in the Log Analytics Workspace.

<p align="center">
<img src="https://i.imgur.com/oWRMgEE.png" height="50%" width="50%" alt="Image Analysis Dataflow"/>
</p>

Set the type to map visualisation. Once created it is now time to wait and see who discovers the newly created VM and tries to log in.

<h2>World map of incoming attacks after 24 hours</h2>


<p align="center">
<img src="https://i.imgur.com/bWS9Dks.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>

<h2>Most common usernames used</h2>

Queried the username value used by the attackers. The most common username used was 
"administrator". Shows the importance of not having standard usernames and passwords.

<p align="center">
<img src="https://i.imgur.com/j4xq0Vi.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
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
