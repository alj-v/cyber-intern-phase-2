# Task 6 - Unauthorized Remote Desktop Access

## ✅Simulated Activity:
> Attempt an RDP login from a different IP within your LAN or VM setup.  
> **Focus**: What events trigger during failed/successful RDP logins?  
> Look for **Event ID 4624** + **Logon Type 10**

##  Initial Setup
### Windows 10 in VMware
- Network: Host-Only Adapter
- RDP Enabled
- ICMP (ping) and RDP allowed through firewall
### Kali in VirtualBox
- Network: Host-Only Adapter
- xfreerdp tool installed

Since they were in different virtual environment, they couldn't talk eachother.
## Final Setup
### Windows 10 installed in VirtualBox
- Network: Host-Only Adapter
- RDP Enabled
- ICMP (ping) and RDP allowed through firewall
- Wazuh agent installed and connected to manager
- Enabled Windows Security Logs
### Kali in VirtualBox
- Network: Host-Only Adapter
- xfreerdp tool installed

Steps to Configure RDP Access

### 1. Enable RDP on Windows 10
- Open **System Properties → Remote**
- Check: `Allow remote connections to this computer`

### 2. Allow RDP Through Firewall
```powershell
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
```
### 3. Enable ICMP (ping) to verify connectivity
```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -IcmpType 8 -Action Allow
```
### 4. Verify RDP is Listening
```powershell
netstat -an | findstr 3389
```
## RDP Simulation (Attack Step)
Step:
1. Installed xfreerdp on Kali:
```bash
sudo apt install freerdp2-x11
```
2. Launched RDP session from Kali:
```bash
xfreerdp /u:<username> /p:<password> /v:<windows-ip>
```
with wrong password
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint06_unauthorized_remote_desktop_access_simulated_kali.png)
with correct password
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint06_unauthorized_remote_desktop_accessed.png)
- Launched GUI remote session inside Kali Linux!
- Windows desktop loaded in a separate window.
- RDP session captured by Sysmon with Logon Type: 10
- Wazuh successfully logged:
  - Source IP (Kali)
  - Target IP (Windows)
  - User
  - Logon success timestamp
 
## Logs and Observation
Windows Security Login failed Event ID:4625
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint06_unauthorized_remote_desktop_access_security_log_kali.png)
Wazuh logs of audit failiure Event ID:4625
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint06_unauthorized_remote_desktop_access_simulation.png)
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint06_unauthorized_remote_desktop_access_simulation_log_4625.png)
Wazuh log of Successful Login Event ID: 4624 Logon Type: 10
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint06_unauthorized_remote_desktop_access_wazuh_log.png)

### Insights
- Successfully simulated RDP logon from Kali to Windows
- Validated detection using Sysmon and Wazuh
- Learned about RDP, event IDs, and how attackers could misuse this technique
- This is NOT hacking — it’s simulation for defensive security learning

