## Detection Use Case: Brute Force Attack Detection

### Scenario Description
An attacker attempts multiple failed login attempts on a Windows system using a brute-force method.

### Objective
Detect a high volume of failed login attempts (Event ID 4625) from the same IP or for the same account within a short time.

### Tools Used
- **SIEM**: ELK Stack
- **Log Source**: Windows Event Logs
- **Lab Setup**: Windows with Winlogbeat forwarding logs to ELK.

### Event ID / Data Source Mapping
| Source        | Event ID / Field | Description          |
|---------------|------------------|----------------------|
| Windows Logs  | 4625             | Failed login attempt |
