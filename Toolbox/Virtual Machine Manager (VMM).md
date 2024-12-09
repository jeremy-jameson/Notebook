# Virtual Machine Manager (VMM)

Friday, July 21, 2017\
10:01 AM

---

**FOOBAR10 - Run as TECHTOOLBOX\\jjameson-admin**

```PowerShell
cls
```

#### # Insert Windows Server 2016 installation media

```PowerShell
$vmName = "TT-DPM02"
$isoName = "en_windows_server_2016_x64_dvd_9718492.iso"

$iso = Get-SCISO | where { $_.Name -eq $isoName }

Get-SCVirtualDVDDrive -VM $vmName |
    Set-SCVirtualDVDDrive -ISO $iso -Link
```

---
