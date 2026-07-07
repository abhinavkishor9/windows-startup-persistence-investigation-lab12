# windows-startup-persistence-investigation-lab12
## Overview

This lab demonstrates how Windows Startup Folder persistence works and how a SOC analyst can investigate startup applications using native Windows tools.

A benign application (Notepad shortcut) is added to the user's Startup folder to simulate persistence. The investigation validates the startup item using PowerShell, Event Viewer, Registry Editor, File Explorer, and Task Manager.

This technique is commonly abused by attackers to automatically execute malware after user logon.

---

## Objectives

- Understand Windows Startup Folder persistence
- Investigate startup items using PowerShell
- Review Startup Folder contents
- Review Startup Registry Keys
- Investigate startup-related Windows Event Logs
- Review startup applications in Task Manager
- Practice SOC investigation methodology
- Map activity to MITRE ATT&CK

---

## Tools Used

- Windows 10
- Windows Event Viewer
- Windows PowerShell
- Registry Editor
- Task Manager
- File Explorer

---

## Lab Steps

### Step 1 — Review Existing Startup Applications

Open Task Manager.

Navigate to:

Startup Tab

Review currently configured startup applications.

---

### Step 2 — Review Existing Startup Folder

Open:

shell:startup

or

```
%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup
```

Review existing startup files.

---

### Step 3 — Create Benign Startup Persistence

Create a shortcut to:

```
C:\Windows\System32\notepad.exe
```

Move the shortcut into:

```
%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup
```

This simulates a persistence mechanism used by attackers.

---

### Step 4 — Verify Startup Item using PowerShell

Run:

```powershell
Get-CimInstance Win32_StartupCommand | Select Name,Command,Location,User
```

Verify that the newly created startup item appears in the output.

---

### Step 5 — Review Startup Registry Keys

Open Registry Editor.

Navigate to:

```
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
```

Review startup registry entries.

Observe that the Startup Folder shortcut does not create a Run registry value.

---

### Step 6 — Investigate Startup Events

Open Event Viewer.

Navigate to:

```
Applications and Services Logs
Microsoft
Windows
Shell-Core
Operational
```

Review startup-related events.

Example:

- Event ID 9706
- Event ID 9707
- Event ID 9708

These events show Windows processing startup items during logon.

---

### Step 7 — Cleanup

Delete the shortcut from:

```
%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup
```

Verify removal using:

```powershell
Get-CimInstance Win32_StartupCommand | Select Name,Command,Location,User
```

Confirm the startup item no longer exists.

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Boot or Logon Autostart Execution: Startup Items | T1547.001 |
| Persistence | TA0003 |

---

## Detection Opportunities

- New files created inside Startup folders
- Shortcut (.lnk) creation in Startup directory
- Unexpected startup entries discovered through PowerShell
- New startup applications observed in Task Manager
- Shell-Core Operational events during startup processing
- Correlation with suspicious process execution after user logon

---

## Learning Outcomes

After completing this lab, you will understand:

- Windows Startup persistence
- Startup Folder investigation
- Startup Registry investigation
- Startup application enumeration
- Event Viewer startup logs
- PowerShell startup enumeration
- MITRE ATT&CK persistence techniques
