# Troubleshooting Notes

## Problem

Startup item does not appear in PowerShell.

### Resolution

Verify the shortcut exists inside:

%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup

Restart Explorer or log off/log on.

---

## Problem

Startup folder is empty.

### Resolution

Open using:

shell:startup

instead of navigating manually.

---

## Problem

Shortcut launches but is not shown in Task Manager.

### Resolution

Some Startup Folder items may not immediately appear in Task Manager until after logon.

Use PowerShell as the primary validation method.

---

## Problem

Registry Run key is empty.

### Resolution

Expected behavior.

Startup Folder persistence does not require Run registry entries.

---

## Problem

No Shell-Core events appear.

### Resolution

Ensure the following log is enabled:

Applications and Services Logs

Microsoft

Windows

Shell-Core

Operational

---

## Useful Commands

Enumerate startup items:

```powershell
Get-CimInstance Win32_StartupCommand | Select Name,Command,Location,User
```

Open Startup Folder:

```
shell:startup
```

Open Registry Run Key:

```
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

---

## Lessons Learned

- Startup Folder persistence differs from Registry Run Key persistence.
- PowerShell provides reliable startup enumeration.
- Event Viewer offers visibility into startup processing.
- Multiple artifacts should always be correlated during persistence investigations.
