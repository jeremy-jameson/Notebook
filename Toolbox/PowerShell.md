# PowerShell

Tuesday, May 19, 2015
11:08 AM

## PowerShell profile

```Console
If (-not (Test-Path $PROFILE))
{
    New-Item -Path $PROFILE -ItemType File -Force
}

Notepad.exe $PROFILE
```

---


**Copy/paste the following content into Notepad:**

```PowerShell
$Host.PrivateData.VerboseForegroundColor = "DarkGray"
$Host.PrivateData.DebugForegroundColor = "Cyan"

Set-Alias -Name sgdm -Value C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe

Function desktop { Push-Location \\iceman\Users$\jjameson\Desktop }

Function temp { Push-Location C:\NotBackedUp\Temp }

Function toolbox { Push-Location C:\NotBackedUp\Public\Toolbox }
```

---


```PowerShell
$cert = Get-ChildItem Cert:\CurrentUser\My -CodeSigningCert

Set-AuthenticodeSignature -FilePath $PROFILE -Certificate $cert
```

**TODO:**

```PowerShell
[Console]::CursorSize = 25
```

### Reference

**How to show cursor in (Azure) Powershell**\
From <[http://stackoverflow.com/questions/25394160/how-to-show-cursor-in-powershell](http://stackoverflow.com/questions/25394160/how-to-show-cursor-in-powershell)>


