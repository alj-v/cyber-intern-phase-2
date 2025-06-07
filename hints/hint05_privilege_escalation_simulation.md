# ⚠️Task 5: Privilege Escalation Simulation

## Simulated Activity:
Simulate local privilege escalation by creating a new user and adding them to the Administrators group. Observe how Windows logs and Wazuh capture this critical behavior.

---

## Steps Performed
### 1. Enable logging for Windows Security Event IDs 4720 (User Account Creation), 4732 (User added to Administrators group):
Navigate to:

Computer Configuration > Windows Settings > Security Settings > Advanced Audit Policy Configuration > Audit Policies > Account Management.

Enable **"Audit Security Group Management"** and **"Audit User Account Management"**
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint05_privilege_escalation_detection_logs_enabled.png)
### 2. Created new user:
```powershell
net user testUser P4ssw0rd /add
```
### 3. Added user to administrators group:
```powershell
net localgroup administrators testUser /add
```
### 4. Optional Cleanup
```powershell
net user testUser /delete
```
## Log Detection
Event ID|Description
--------|-----------
4720|A new user account was created
4732|User was added to the Administrators group
> Both logs were visible in Windows Event Viewer and forwarded to Wazuh.

Wazuh Logs
1. User Created
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint05_privilege_escalation_wazuh_log_4720.png)
2. User addedd to Administrators group
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint05_privilege_escalation_wazuh_log_4732.png)

Windows Security Logs
1. User created
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint05_privilege_escalation_4720_security_log.png)
2. User added to Administrators group
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint05_privilege_escalation_4732_security_log.png)

#### 🧠 Insights:
- Attackers often create or elevate users silently to gain persistence.
- Event IDs 4720 and 4732 are key detection points for lateral movement and privilege escalation.
- Monitoring group membership changes is essential for catching these actions early.

Before and after Privilege escalation Simulation
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint05_privilege_escalation_before_after_admin.png)

