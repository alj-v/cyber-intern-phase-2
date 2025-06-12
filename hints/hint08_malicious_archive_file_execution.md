# Task 9: Malicious Archive Execution

## ✅Simulated Activity:
Simulated execution of a potentially malicious executable bundled inside a password-protected archive, and observed how it is logged by Sysmon and Wazuh.

---

## Setup
- **Kali Linux**
- **Windows 10**
- **Wazuh Agent** installed and connected
- **Sysmon** installed with XML config

---

## Simulation Steps

1. Created a new `fake.exe` file on Kali:
   ```bash
   nano fake.exe
   
   @echo off
   echo Hello from a fake executable!
   pause
   ```

2. Zipped the `fake.exe` file:
   ```bash
   zip --password infected archive.zip fake.exe
   ```

3. Transferred archive to Windows and extracted it using 7-Zip.

4. Executed the `fake.exe` manually.

---

## Logs Observed

### Event Viewer (Windows)
- **Sysmon Event ID 1**: Process Creation
  - Image: `fake.exe`
  - Parent Process: `explorer.exe` (or 7zFM.exe)
  - Hash: [SHA1]
  - User: `ALJV`
  - IP: [if accessed via share]

### Wazuh Dashboard
- Under **Sysmon → Event ID 1**
- Alert showing `.exe` execution
- Parent-child relationship of processes

---

## Detection Notes
- No AV alerts were observed (if Defender was disabled)
- Process creation and metadata captured by Sysmon
- Archive password prevented AV scanning prior to extraction

---

### Conclusion
Successfully simulated malicious archive execution. Logged behavior matched expectations, confirming visibility in Sysmon and Wazuh dashboards.

