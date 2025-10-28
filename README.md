# Powershell-Practice
Practicing Powershell Automations


Working in powershell today on my Windows VM


first im going to 
# 1) Confirm current user & elevation


```bash
Whoami
```


Next confirm OS version and host name, this is useful when documenting environment or matching known CVEs (Common Vulnerabilities and Exposures).


```bash
Get-ComputerInfo | Select-Object CsName, WindowsVersion, OsBuildNumber
```

<img width="645" height="83" alt="image" src="https://github.com/user-attachments/assets/587853be-61bd-436a-ae6c-5987817461de" />

###IP Configuration###

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
“Outbound connection to known malicious IP 178.62.56.31 detected.”<img width="792" height="53" alt="image" src="https://github.com/user-attachments/assets/f55ba7cd-d71b-4074-a199-c66541b526e3" />

```bash
Get-NetTCPConnection | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State | Select -First 20
```


### Automation ###
