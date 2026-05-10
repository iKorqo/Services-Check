# Windows Service Checker

## How to Use

1. Open PowerShell as Administrator (right-click → Run as administrator)
2. Run:

```powershell
powershell -ExecutionPolicy Bypass -Command "Invoke-Expression (Invoke-RestMethod 'https://raw.githubusercontent.com/iKorqo/Services-Check/refs/heads/main/Services%20Check')"
```

---

## Detection Layers

| Layer    | Method                                                                 | Property Checked        |
|----------|------------------------------------------------------------------------|-------------------------|
| Registry | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\<Name>` | `Start` DWORD = 4       |
| SCM      | `Get-Service -Name <Name>`                                             | `.StartType = Disabled` |

If sources disagree (e.g. Registry flags it but SCM does not), the registry entry is stale and SCM has not synced — or the value was tampered with directly without going through SCM.

---

## Service Regedit Paths

All paths are under `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\`
Check the `Start` DWORD — value `4` = Disabled.

| Service | Full Path |
|---|---|
| SysMain | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SysMain` |
| PcaSvc | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\PcaSvc` |
| DPS | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DPS` |
| EventLog | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog` |
| Schedule | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Schedule` |
| SearchIndexer | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SearchIndexer` |
| BAM | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\BAM` |
| DusmSvc | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DusmSvc` |
| Appinfo | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Appinfo` |
| DcomLaunch | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DcomLaunch` |
| CDPUserSvc (instance) | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\CDPUserSvc_<suffix>` |
| CDPUserSvc (template) | `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\CDPUserSvc` |

---

## Output

| Color | Meaning |
|-------|-------------------------------------------------------------------------|
| Green | Service exists and is not disabled across all checked layers |
| Red   | Service is missing, fully removed, or disabled in one or more layers |

When disabled, the Issue column reports which layers flagged it, e.g. `Disabled [Registry, SCM]`.

---

## `Start` DWORD Reference

| Value | Meaning   |
|-------|-----------|
| 2     | Automatic |
| 3     | Manual    |
| 4     | Disabled  |
