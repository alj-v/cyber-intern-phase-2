# Task 1: Suspicious Zip File Download Simulation
### Objective: Simulate the download of a .zip file from an external source and monitor system logs for suspicious activity.

---

## Method Used: Hosting Malicious File on Kali (Attacker VM)
### 1. Created a fake malicious archive:
```bash
echo "this is fake malware" > malware.txt
zip suspicious.zip malware.txt
```
### 2. Hosted it with:
```bash
python3 -m http.server 8000
```
![fake file created for the task](https://github.com/alj-v/cyber-intern-phase2/blob/main/screenshots/hint01_suspicious_file_creation_simulation.png)
### 3. From Windows VM:
```powershell
Invoke-WebRequest -Uri "http://192.168.56.101:8000/suspicious.zip" -OutFile "$env:USERPROFILE\Downloads\suspicious-kali.zip"
```
![suspicious zip file download simulation](https://github.com/alj-v/cyber-intern-phase2/blob/main/screenshots/hint01_suspicious_zip_file_downloaded.png)

## Logs generated
 - Sysmon logs in local system
   - Event ID:3 Network Connection
![suspicious zip file download network connection sysmon log](https://github.com/alj-v/cyber-intern-phase2/blob/main/screenshots/hint01_suspicious_zip_file_download_network_connection_log.png)
   - Event ID:11 File Creation
![suspicious zip file downloaded](https://github.com/alj-v/cyber-intern-phase2/blob/main/screenshots/hint01_suspicious_zip_file_creation_log.png)
 - Wazuh SIEM
  - Wazuh logged sysmon logs of Event ID:3 from windows
![suspicious zip file downloaded wazuh log](https://github.com/alj-v/cyber-intern-phase2/blob/main/screenshots/hint01_suspicious_file_creation_sysmon_log_in_wazuh.png)
