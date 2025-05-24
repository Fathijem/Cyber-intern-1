# 🚨 SIEM Internship Program – Phase 1: Cyber Defense Lab

Phase 1 of the SIEM Internship Program. This project establishes foundational skills in Security Information and Event Management (SIEM) by building a detection lab, simulating attacks, and using Elastic Stack to identify threats.

## 🎯 Objectives

- Build and configure a SIEM lab with Elastic Stack and Wazuh.
- Simulate 10 real-world attack techniques.
- Forward and analyze logs from Windows and Linux systems.
- Detect threats using Sysmon, Winlogbeat, and Elastic SIEM.

## 🖥️ Lab Architecture
+----------------+ +----------------+ +--------------------+
| Windows VM | <---> | Kali Linux VM | <---> | ELK + Wazuh Server |
| (Target) | | (Attacker) | | (SIEM/Log Storage) |
+----------------+ +----------------+ +--------------------+



**Components:**
- Windows 10 Target (Sysmon + Winlogbeat)
- Kali Linux and Windows Attacker (Hydra, PowerShell scripts, netcat)
- ELK Stack + Wazuh (SIEM Dashboard)

## ⚙️ Tools Used

- **SIEM:** Elastic Stack (Elasticsearch, Logstash, Kibana)
- **Endpoint Logging:** Sysmon, Winlogbeat
- **Attack Simulation:** Kali Linux tools (Hydra), Windows tools (Powershell)
- **Forwarders:** Winlogbeat (Windows)

## 🧪 Detection Scenarios

### 1. Brute Force Login
- **Simulated with:** Hydra, RDP script
- **Event IDs:** 4625 (failed login), 4624 (success)
- **Detection:** Multiple failures from one IP → alert

### 2. Suspicious PowerShell Activity
- **Simulated with:** PowerShell -nop -w hidden -c IEX
- **Event IDs:** 4104 (script block logging), Sysmon 1 (process create)
- **Detection:** Keywords: `IEX`, `DownloadString`, `Invoke-*`

### 3. USB Drive Detection
- **Simulated with:** Plug-in USB and run .exe
- **Event IDs:** 6414 (device), Sysmon 1 & 11
- **Detection:** Monitor file creation from removable devices

### 4. Malware Execution
- **Simulated with:** evil.exe from startup or USB
- **Sysmon:** ID 1, 3 
- **Detection:** Suspicious processes from temp/startup paths

### 5. Registry Modification
- **Simulated with:** Reg add for Run keys
- **Sysmon:** ID 13
- **Detection:** Monitor Run/RunOnce and UAC keys

### 6. Suspicious Network Connections
- **Simulated with:** PowerShell web client
- **Sysmon:** ID 3
- **Detection:** Powershell.exe or cmd.exe reaching out to unknown IPs

### 7. Fileless Attack
- **Simulated with:** PowerShell loading remote script in memory
- **Event IDs:** 4104, Sysmon 1
- **Detection:** `IEX`, encoded commands, memory-only payloads

### 8. Scheduled Task Creation
- **Simulated with:** schtasks /create
- **Event ID:** 4698 (task creation)
- **Detection:** Monitor new tasks with odd user paths

### 9. Persistence Simulation
- **Simulated with:** Copy .exe to Startup folder, Run key
- **Sysmon:** ID 13 (registry), ID 1 (execution)
- **Detection:** Watch startup paths, autorun keys

### 10. Privilege Escalation
- **Simulated with:** UAC bypass or admin shell
- **Event IDs:** 4672 (admin rights), 4688 (new process)
- **Detection:** Unexpected use of elevated permissions
  

## 📊 SIEM Detection in Kibana

- **Index Patterns:** `winlogbeat-*`, `sysmon-*`, `filebeat-*`
- Use **Discover** for raw log inspection.
- Use **Security > Timelines** for correlation views.
- Use **KQL** queries like:
  ```kql
  event.code:1 AND process.name:powershell.exe AND command_line:(*IEX* OR *Invoke-Expression*)
  ```

## Sample KQL queries used:
- event.code:4625 AND winlog.event_data.LogonType:10
- process.name:"powershell.exe" AND command_line.keyword:*DownloadString*
- registry.path:*Run* AND event.code:13
- network.protocol:http AND process.name:"powershell.exe"

## 🔁 Future Enhancements
- Add log tampering and data exfiltration detection.
- Integrate Security Onion or Zeek for network monitoring.
- Automate alerts with ElastAlert or Wazuh rules.

## 🙋 Reflection Questions
Here are my thoughts and experiences based on building and working with the SIEM lab during Phase 1 of the internship program.

### 1. What is the role of SIEM in modern cybersecurity?
SIEM acts as the central nervous system for cybersecurity operations. It collects, normalizes, and analyzes logs from multiple sources to detect, investigate, and respond to threats in real-time. It helps reduce the noise and focus on the signals that matter most.

---

### 2. What challenges did you face while setting up your lab?
One of the biggest challenges was getting logs to properly flow into the Elastic Stack, especially when configuring Winlogbeat and Sysmon. Index visibility in Kibana wasn’t always immediate, and troubleshooting those forwarding issues took time and patience. Network configurations between VMs also needed careful setup to simulate real-world attacks.

---

### 3. What are the differences between Sysmon logs and Windows Security logs?
Windows Security logs focus on system-level and user authentication events (like login attempts), while Sysmon provides deeper visibility into system activity such as process creation, network connections, and file modifications. Sysmon is more behavior-based and better for detecting stealthy or advanced threats.

---

### 4. How does a brute force attack appear in logs? Mention specific Event IDs.
A brute force attack shows up as repeated `4625` (failed login) events from the same IP or username. If successful, it ends with a `4624` (successful login) event. These logs can reveal patterns like rapid-fire login attempts, which are clear signs of brute force behavior.

---

### 5. How would you detect a login outside normal business hours?
By defining business hours (e.g., 9 AM to 7 PM) and monitoring Event ID `4624` for successful logins, I filtered logins occurring late at night or early morning. Admin logins during these off-hours are red flags and should be investigated further.

---

### 6. Describe how RDP lateral movement is tracked in event logs.
Lateral movement over RDP shows up as Event ID `4624` with `LogonType` 10 (remote interactive login). If the same user logs into multiple machines across the network using RDP, it's a strong indicator of lateral movement. Monitoring these patterns helps detect the spread of an attack.

---

### 7. What is the risk of log tampering, and how can we detect it?
If an attacker deletes logs or disables logging, it can hide their actions and make detection harder. Tools like `wevtutil cl` or `auditpol` can clear logs—this activity should trigger alerts. We can detect tampering through Sysmon or by monitoring for sudden gaps or missing logs.

---

### 8. What improvements would you make in your lab setup if given more time?
I'd automate more of the log forwarding setup, integrate Security Onion or Zeek for full network packet capture, and simulate more advanced attacks like privilege escalation chains or exfiltration via DNS. I’d also work on visualizing detection logic more clearly in dashboards.

---

### 9. How will this phase help you in real-world interviews or jobs?
This phase helped me speak confidently about how threats appear in logs and how detection logic works. Knowing how to use tools like Sysmon, Winlogbeat, and Kibana gives me a strong foundation to talk about real-world use cases and incident response in interviews.

---

### 10. What was your biggest takeaway from Phase 1?
The biggest takeaway was understanding that threat detection isn’t about identifying huge malware—it’s about recognizing subtle changes and behaviors in logs. The value of a properly configured SIEM and good log hygiene cannot be overstated.

