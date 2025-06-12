# 🛡️Cybersecurity SIEM Internship Program - Phase 2 Report

**Intern Name:** Aleena Joy  
**Phase:** 2 - Attack Simulation
**Tasks Completed:** 8 out of 10  
**Mentor:** Rajendra Bodda  

---

## Objective

The goal of **Phase 2** was to simulate real-world cyber attack behaviors and analyze their detection through security monitoring tools such as **Sysmon**, **Winlogbeat**, **Event Viewer**, and **Wazuh SIEM**. Each task represented a different threat technique aligned with MITRE ATT&CK tactics.

---

## ✅Tasks Completed

Below are the 8 tasks successfully completed and documented:

### 1. Suspicious ZIP or RAR File Downloads
- Simulated downloading a password-protected `.zip` archive containing a dummy executable.
- Observed file creation events using Sysmon and validated logs.
- Screenshots included in `/screenshots/` and full task in `/hints/hint01_suspicious_zip_file_download.md`.

### 2. Malicious PowerShell Execution
- Executed base64-encoded PowerShell commands.
- Detected via Sysmon Event ID 1 (process creation) and reviewed encoded payloads.
- Documented under `/hints/hint02_malicious_powershell_execution.md`.

### 3. Credential Dumping Tools
- Used `mimikatz` to simulate credential dumping.
- Observed related Event IDs and endpoint protection behavior.
- Logs and evidence covered in `/hints/hint03_credential_dumping_tools.md`.

### 4. External Beaconing or C2 Communication
- Created a Python beacon to simulate periodic external communication.
- Monitored outbound connections using Wireshark and Windows logs.
- Detailed in `/hints/hint04_external_beaconing.md`.

### 5. Privilege Escalation Simulation
- Created a new local user and added it to the Administrators group.
- Verified Event IDs 4728 and 4732 in Windows Security logs.
- Covered in `/hints/hint05_privilege_escalation_simulation.md`.

### 6. Unauthorized Remote Desktop Access
- Enabled RDP and simulated a login from Kali using `xfreerdp`.
- Successfully triggered Event ID 4624 with Logon Type 10.
- Captured in `/hints/hint06_unautorized_remote_desktop_access.md`.

### 7. Vulnerability Scan Simulation
- Used `nmap` from Kali to scan Windows 10 VM.
- Observed Event ID 5152 (dropped packets) and Sysmon Event ID 3 (network connection).
- Log insights and challenges documented in `/hints/hint07_vulnerability_scan_simulation.md`.

### 9. Malicious Archive Execution
- Created a fake `.exe` and zipped it using a password, then downloaded it onto Windows via Python HTTP server.
- Observed execution behavior and logging via Event Viewer and Sysmon.
- Logged in `/hints/hint09_malicious_archive_file_execution.md`.

---

## Observations

- **Sysmon + Wazuh** provided reliable detection across most simulated attacks.
- **Winlogbeat setup** encountered issues on Amazon Linux-based Wazuh Manager due to service limitations.
- Screenshots served as primary log evidence, stored under `/screenshots/`.
- Some Sysmon events were not captured in Wazuh due to either configuration gaps or unsupported rules.

---

## Conclusion

All tasks were approached with a red-team mindset while maintaining defensive awareness. Tools like Sysmon, Event Viewer, and Wazuh were instrumental in tracking, interpreting, and validating malicious activities.  
With Phase 2 completed, the project is now ready to progress to **Phase 3**.

---

**Report by:** Aleena Joy  
**LinkedIn:** [Aleena Joy](https://www.linkedin.com/aleenjoyvettiyadan/)
