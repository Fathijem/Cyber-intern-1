## Detection Use Case: USB Drive Detection

### Scenario Description
An external USB drive is connected to a system which could be used for data exfiltration or malware delivery.

### Objective
Detect USB drive insertions.

### Tools Used
- **SIEM**: ELK Stack
- **Log Source**: Sysmon
- **Lab Setup**: USB insertion on monitored Windows host.

### Event ID / Data Source Mapping
|Source|	Event ID / Field |	Description |
|------|-------------------|--------------|
|Sysmon|	      6          | Driver loaded|
|Sysmon|	     11	         | File created |

### Detection Logic / Query
```dsl
event.code:11 
```
