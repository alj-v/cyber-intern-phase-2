# Hint 06 - Unauthorized Remote Desktop Access

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

