# Powershell-Practice
Practicing Powershell Automations


Working in powershell today on my Windows VM


# 1) Confirm current user & elevation


```bash
Whoami
```


Next confirm OS version and host name, this is useful when documenting environment or matching known CVEs (Common Vulnerabilities and Exposures).


```bash
Get-ComputerInfo | Select-Object CsName, WindowsVersion, OsBuildNumber
```

<img width="645" height="83" alt="image" src="https://github.com/user-attachments/assets/587853be-61bd-436a-ae6c-5987817461de" />

### IP Configuration

During an incident investigation, a SOC or incident response role, I might be looking at suspicious network logs or alerts in SIEM tools.

Those logs will show IP addresses — but which one belongs to the host?

Use Get-NetIPAddress whenever you need to confirm how a host is talking on the network — what its addresses are, what interfaces exist, and if anything looks out of place.

```bash
Get-NetIPAddress | Where-Object { $_.AddressState -eq "Preferred" } | Select-Object IPAddress, InterfaceAlias, AddressFamily
```

### Active TCP Connections (Short)

This shows me all the active TCP network connections on the computer — both incoming and outgoing.
Run this to see who your host is communicating with — and to spot weird or unauthorized connections.

IE: 

SIEM fires an alert:
“Outbound connection to known malicious IP 178.62.56.31 detected.”

```bash
Get-NetTCPConnection | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State | Select -First 20
```


### Automation 

```bash
notepad C:\SOC_System_Report.ps1
```


<img width="748" height="499" alt="image" src="https://github.com/user-attachments/assets/bc61623c-51cb-4f42-8c13-013e127ad4a9" />

I wrote the follwoing 

<img width="801" height="611" alt="image" src="https://github.com/user-attachments/assets/5a8e1a46-9dbb-42f6-9635-b4d14d5d026b" />

Closed and saved the notepad

Ran 

```bash
notepad C:\SOC_System_Report.ps1
```

<img width="540" height="115" alt="image" src="https://github.com/user-attachments/assets/ab758a3d-2428-4db2-8ab7-c045d54babe0" />


That means your script successfully:

Grabbed system info
Logged network connections
Pulled security event logs
Wrote it all into a timestamped report file


Now to read it I will run

```bash 
notepad C:\SOC_Report_20251028_155431.txt 
```


<img width="603" height="432" alt="image" src="https://github.com/user-attachments/assets/f601f400-6be3-4b96-b528-3a034579223b" />


And this it shows me what we asked!


### Why did I do this?

In SOC and forensics work, you usually don’t want info scrolling by — you want a clean log or report you can attach to a ticket or case.
So we automate data collection into a file (timestamped for evidence).



