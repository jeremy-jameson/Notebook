# Snapshots

Tuesday, May 19, 2015
11:03 AM

## # Reset machine password

### # FABRIKAM domain

```PowerShell
netdom resetpwd /s:FAB-DC01 /ud:FABRIKAM\jjameson-admin /pd:*
```

### # EXTRANET domain

```PowerShell
netdom resetpwd /s:EXT-DC08 /ud:EXTRANET\jjameson-admin /pd:*
```

### # TECHTOOLBOX domain

```PowerShell
netdom resetpwd /s:TT-DC06 /ud:TECHTOOLBOX\jjameson-admin /pd:*
```

Pasted from <[http://www.technologytoolbox.com/blog/jjameson/archive/2011/03/12/resolving-issues-after-applying-hyper-v-snapshot.aspx](http://www.technologytoolbox.com/blog/jjameson/archive/2011/03/12/resolving-issues-after-applying-hyper-v-snapshot.aspx)>

### # PowerShell 3.0 and later

#### # FABRIKAM domain

```PowerShell
Reset-ComputerMachinePassword -Credential (Get-Credential FABRIKAM\jjameson-admin)
```

#### # EXTRANET domain

```PowerShell
Reset-ComputerMachinePassword -Credential (Get-Credential EXTRANET\jjameson-admin)
```

#### # TECHTOOLBOX domain

```PowerShell
Reset-ComputerMachinePassword -Credential (Get-Credential TECHTOOLBOX\jjameson-admin)
```

## # Copy Toolbox content

### # Authenticate to file server

```PowerShell
net use \\tt-fs01.corp.technologytoolbox.com\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
cls
```

### # Mirror Toolbox content

```PowerShell
$source = "\\tt-fs01.corp.technologytoolbox.com\Public\Toolbox"
$destination = "C:\NotBackedUp\Public\Toolbox"

robocopy $source $destination /E /MIR /XD git-for-windows "Microsoft SDKs"
```

## # Clean up WinSxS folder

```PowerShell
Dism.exe /Online /Cleanup-Image /StartComponentCleanup /ResetBase
```

## # Clean up Windows Update files

```PowerShell
Stop-Service wuauserv

Remove-Item C:\Windows\SoftwareDistribution -Recurse
```

## Update checkpoint

---

**FOOBAR10**

```PowerShell
cls
```

#### # Stop VM and update "Baseline" checkpoint

```PowerShell
$vmHost = "WOLVERINE"
$vmName = "EXT-FOOBAR4"

C:\NotBackedUp\Public\Toolbox\PowerShell\Update-VMBaseline.ps1 `
    -ComputerName $vmHost `
    -Name $vmName

Start-VM -ComputerName $vmHost -Name $vmName
```

---
