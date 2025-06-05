# Task 2 - Malicious PowerShell Execution
## ✅Simulated Activity:
Simulate execution of encoded/malicious PowerShell commands, and detect the behavior using Sysmon logs in Wazuh. 
## Approch: Encoded PowerShell Command
### Payload used:
```powershell
Start-Process notepad.exe
```

### Encoded:
Use PowerShell to generate Base64:
```powershell
$command = 'Start-Process notepad.exe'
$bytes = [System.Text.Encoding]::Unicode.GetBytes($command)
$encoded = [Convert]::ToBase64String($bytes)
```

You’ll get
```powershell
UwB0AGEAcgB0AC0AUAByAG8AYwBlAHMAcwAgAG4AbwB0AGUAcABhAGQALgBlAHgAZQA=
```

### Execution:
```powershell
powershell.exe -EncodedCommand UwB0AGEAcgB0AC0AUAByAG8AYwBlAHMAcwAgAG4AbwB0AGUAcABhAGQALgBlAHgAZQA=
```
![Malicious PowerShell command executed](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint02_malicious_powershell_executed.png)

---

## Logs Collected
- ### **PowerShell Log**: Enabled **PowerShell Script block logging** (Event ID: 4104)
![malicious powershell command executed powershell log](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint02_malicious_powershell_exec_script_block_log.png)
- ### Sysmon Log: Event ID 1 **Process Creation** log was generated when command notepad.exe was executed
![malicious powershell command executed sysmon log](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint02_malicious_powershell_exec_sysmon_log.png)
- ### Sysmon Log captured in Wazuh SIEM
![malicious powershell command executed wazuh log](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint02_malicious_powershell_exec_wazuh_log.png)
