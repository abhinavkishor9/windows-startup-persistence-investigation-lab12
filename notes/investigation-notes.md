# Investigation Notes

## Incident Summary

A new application was configured to launch automatically during user logon by placing a shortcut inside the Windows Startup Folder.

The objective was to investigate how startup persistence can be identified using native Windows tools.

---

## Investigation Timeline

### 1. Baseline Review

Reviewed existing startup applications using:

- Task Manager
- Startup Folder

No suspicious entries were present.

---

### 2. Persistence Creation

A Notepad shortcut was created and placed inside:

%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup

---

### 3. PowerShell Verification

Executed:

```powershell
Get-CimInstance Win32_StartupCommand | Select Name,Command,Location,User
```

Observation:

- Startup shortcut successfully detected
- User context identified
- Startup command displayed

---

### 4. Registry Review

Reviewed:

HKCU\Software\Microsoft\Windows\CurrentVersion\Run

Observation:

No additional Run registry value was created because persistence relied on the Startup Folder instead of Registry Run Keys.

---

### 5. Event Viewer Review

Reviewed:

Microsoft-Windows-Shell-Core/Operational

Observed:

- Event ID 9706
- Event ID 9707
- Event ID 9708

These events indicate Windows processing startup commands during user logon.

---

### 6. Cleanup Verification

Deleted the Startup shortcut.

Verified removal using PowerShell.

No startup persistence remained.

---

## Key Findings

Persistence Method:

Startup Folder Shortcut

Persistence Scope:

Current User

Registry Modification:

None

PowerShell Enumeration:

Successful

Event Logs:

Shell-Core Operational Events

MITRE Technique:

T1547.001

---

## Analyst Conclusion

The startup application was successfully detected and validated using multiple Windows artifacts.

Although benign, the same persistence technique is frequently abused by malware to maintain execution after user logon.
