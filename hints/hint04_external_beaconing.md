# Task 4: External Beaconing / C2 Simulation

## Simulated Activity
Simulate command-and-control (C2) style beaconing from a Windows system to an external IP, mimicking malware behavior calling back home. Detect the activity using Wazuh (Sysmon Event ID 3) and network observations.

---

## ✅ Method 1: PowerShell HTTP Beacon (Simple)

### Steps Performed:

1. On **Kali**, start an HTTP server:
   ```bash
   python3 -m http.server 9001
   ```
2. On Windows PowerShell, execute the loop:
   ```powershell
   for ($i = 1; $i -le 10; $i++) {
    Invoke-WebRequest -Uri "http://192.168.1.44:9001/beacon" -UseBasicParsing
    Start-Sleep -Seconds 10
   }
   ```
> This simulates 10 C2-style beacon requests to port 9001 with a 10-second interval.

### Observation 1 – Initial Failure (404)
- The Kali server responded with:
  Error 404 - File Not Found
- Even though the file didn’t exist, network connection still occurred
- Sysmon captured Sysmon Event ID 3 for each attempt
### Observation 2 – Success (200 OK)
1. Created a dummy file on Kali:
  ```bash
  echo "Hello C2" > beacon
  ```
2. Re-ran the PowerShell script → this time, Kali responded with:
  ```css
  Status Code: 200 OK
  ```
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint04_external_beaconing_simulated.png)
Sysmon Logs of **Network Connection** using PowerShell.exe
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint04_external_beaconing_sysmon_log_powershell.png)
## ✅ Method 2: Python Script Beacon (Advanced)
beacon.py
  ```python
  import time
  import random
  import requests

  url = "http://192.168.1.44:9001/beacon"

  for i in range(10):
      try:
          r = requests.get(url)
          print(f"[{i+1}] Beacon sent - Status: {r.status_code}")
      except Exception as e:
          print(f"[{i+1}] Beacon failed: {e}")
      time.sleep(random.randint(5, 15))
  ```
- Adds random intervals between beacons
- Uses Python + requests to simulate malware more realistically
- Logs its own status like real C2 malware
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint04_external_beaconing_script_simulated.png)
Sysmon logged the network connection using python.exe
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint04_external_beaconing_sysmon_log_python.png)

## Detection & Logs:
|Source|Event ID|Description|
|------|--------|-----------|
|Sysmon|3|Network connection created|
|Kali Logs|Accessed|/beacon endpoint hit repeatedly|
|Wireshark|Traffic|HTTP Traffic captured|

Kali Log
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint04_external_beaconing_traffic.png)
Wireshark Traffic
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint04_external_beaconing_packets_captured.png)
![](https://github.com/alj-v/cyber-intern-phase-2/blob/main/screenshots/hint04_external_beaconing_wireshark_monitored.png)

### 🧠Insights
- Repeated connections to same external IP/port at a fixed/random interval mimic real-world malware beacons
- Even failed HTTP requests (404) are valuable for detection via Sysmon
- Custom ports and script automation increase realism
- Wazuh provides full visibility into both PowerShell and Python-based attack vectors
