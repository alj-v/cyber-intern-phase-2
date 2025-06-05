# Credential Dumping Detection

## ✅Simulated Activity:
Simulate use of a credential dumping tool (Mimikatz) and observe how system defenses and SIEM respond.

### What I Did:
- Used Kali to zip and host `mimikatz.exe`
- Downloaded `mimikatz.zip` on Windows VM
![mimikatz downloaded](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint03_mimikatz_downloaded.png)

### **Unexpected behavior observed:**
- Upon extracting, Windows Defender instantly detected `mimikatz.exe` as **high risk**
- The file was immediately **quarantined by Defender**
![defender flahhed and quarantined](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint03_mimikatz_flagged_and_quarantined_by_windows_defender.png)

### ⚔️ Manual Execution (Post-Quarantine)

Out of curiosity, I restored the quarantined file and executed it **without disabling Defender or Firewall**, while my system was still online.
> I acknowledge this was risky, and I took action to ensure cleanup right after. Documentation of that is below.

---

### Commands Executed in Mimikatz:
```mimikatz
privilege::debug
log
sekurlsa::logonpasswords
```
![credentials dumped using mimikatz](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint03_credential_dumped_using_mimikatz.png)

---

### Result:
- The command executed successfully but returned Password : (null)
- This is expected on modern Windows due to LSASS protection and Credential Guard
- No sensitive data or credentials were visible in output

## Detection & Logging
### Wazuh logs of Event ID 11 File Creation
![mimikatz.zip file created](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint03_mimikatz_zip_file_creation_wazuh_log.png)
### Sysmom Logged
- File Creation Event ID 11 when mimikatz.zip was extracted
![mimikatz.exe created](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint03_mimikatz_extracted_sysmon_log.png)
### Windows Security Log
- Precess Creation log Event ID 4688 when mimikatz executed
![mimikatz executed](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint03_credential_dumping_mimikatz_exec_security_log.png)

### Defender Response:

- Threat: HackTool:Win32/Mimikatz!pz
- Severity: High
- Status: Quarantined
- 
## Post-Test Cleanup
To ensure complete system safety, I followed the steps below:

- Deleted all instances of mimikatz.zip and extracted files
- Ran Full System Scan via Windows Defender
- Cleared temporary and prefetch files using PowerShell
- Reset network settings with ipconfig /release & renew
- Verified LSASS process health and system integrity
- Changed Windows login password as precaution

## Key Insights
- Credential dumping tools are immediately detected by Defender, even before execution
- Sysmon captured partial activity (file drop), but Defender neutralized the binary before full behavior was logged
- Simulating such tools gives a deeper understanding of blue team detection surfaces and attacker workflows
- Execution in live environments (especially online) should never be done casually — cleanup is essential

> Ethical Note:
> This simulation was conducted in a controlled virtual lab environment, with the intent to learn and observe endpoint security behavior.
> No real credentials were exposed. No production or internet-facing systems were affected.

