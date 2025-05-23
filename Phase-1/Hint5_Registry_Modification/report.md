## Detection Use Case: Persistence via Registry Modification

### Scenario Description
Registry keys are modified to achieve persistence (e.g., Run/RunOnce).

### Objective
Detect registry key modifications under autostart locations.

### Tools Used
- **SIEM**: ELK Stack
- **Log Source**: Sysmon
- **Lab Setup**: Registry keys manually edited under HKCU\Software\Microsoft\Windows\CurrentVersion\Run.

### Event ID / Data Source Mapping
Source	Event ID / Field	Description
Sysmon	13	Registry value set

### Detection Logic / Query
```dls
event.code:13 AND registry.path:(*\\Run\\* OR *\\RunOnce\\*)
```
