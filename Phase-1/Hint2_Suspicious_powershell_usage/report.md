##Detection Use Case: Suspicious PowerShell Execution

###Scenario Description
An attacker uses encoded or obfuscated PowerShell scripts to execute malicious commands.

###Objective
Identify the use of encoded PowerShell commands.

###Tools Used
- **SIEM**: ELK Stack
- **Log Source**: Sysmon
- **Lab Setup**: Sysmon logging process creation.

###Event ID / Data Source Mapping
|Source| Event ID / Field|	Description    |
|------|-----------------|-----------------|
|Sysmon|        1        | Process creation|

###Detection Logic / Query
```Kibana
event.code:1 AND process.name:"powershell.exe" AND process.command_line:*EncodedCommand*
```
