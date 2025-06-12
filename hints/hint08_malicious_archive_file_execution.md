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
   @echo off
   echo Hello from a fake executable!
   pause
   ```

2. Zipped the `fake.exe` file:
   ```bash
   zip --password infected archive.zip fake.exe
   ```

3. Transferred archive to Windows and extracted it using windows explorer.
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint08__password_protected_malicious_archive_extracted.png)

4. Executed the `fake.exe` manually.
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint08_malicious_archive_av_warning.png)
### NOTE:
> Initial attempt using a fake `.exe` failed because the file was not a valid Windows PE format.
> **Fixed** by generating a real executable using `msfvenom` in kali.

STEPS:
1. CreateD a Real (But Harmless) `.exe` File in kali using `msfvenom` (comes with Metasploit Framework):
```bash
msfvenom -p windows/exec CMD="calc.exe" -f exe -o harmless.exe
```
> This creates an executable that launches Calculator — completely safe.
2. Zip this with a password:
```bash
zip --password infected malicious_archive.zip harmless.exe
```
3. Re-upload/download it on Windows via your local Python HTTP server:
```bash
python3 -m http.server 8080
```
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint08_fake_malicious_archive_zip_file_creation.png)
#### - The file malicious_archive.zip was downloaded to windows via browser
#### - The zip file was then extracted to the system using the password 'infected'
#### - When tried to execute `harmless.exe` Windows Defender *warned* and the file was *immediately flagged and quarantined*
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint08_malicious_archive_executivon_av_warning.png)
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint08_malicious_archive_file_flagged_av.png)
#### - Manually *restored* the `harmless.exe` file from Windows Defender and *executed* it manually through PowerShell
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint08_malicious_archive_file_restored_and_executed.png)

---

## Logs Observed

### Event Viewer (Windows)
- **Sysmon Event ID 1**: Process Creation
  - Image: `fake.exe`
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint08_malicious_archive_created_security_log.png)
  - Image: `harmless.exe`
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint08_malicious_archive_file_created.png)
  - Parent Process: `explorer.exe`
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint08_malicious_file_executed_sysmon_log.png)

### Wazuh Dashboard
- Under **Sysmon → Event ID 1**
- Log showing `.exe` execution

---

## Detection Notes
- AV alerts were observed
- Windows Defender warned the execution of `fake.exe` file
- Windows Defender flagged and quarantined the file `harmless.exe` right after it was extracted to the system
- Process creation and metadata captured by Sysmon
- Archive password prevented AV scanning prior to extraction

---

### Conclusion
Successfully simulated malicious archive execution. Logged behavior matched expectations, confirming visibility in Sysmon and Wazuh dashboards.

