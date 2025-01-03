# PowerShell

Tuesday, May 19, 2015\
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

Set-Alias `
    -Name code `
    -Value "C:\Program Files\Microsoft VS Code\Code.exe"

Set-Alias `
    -Name fciv `
    -Value C:\NotBackedUp\Public\Toolbox\FCIV\fciv.exe

If ($env:PROCESSOR_ARCHITECTURE -eq "AMD64")
{
    Set-Alias `
        -Name sgdm `
        -Value C:\NotBackedUp\Public\Toolbox\DiffMerge\x64\sgdm.exe
}
Else
{
    Set-Alias `
        -Name sgdm `
        -Value C:\NotBackedUp\Public\Toolbox\DiffMerge\x86\sgdm.exe
}

Function desktop
{
    [String] $regPath = 'HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer'

    [String] $desktopPath = Get-ItemProperty `
        -Path "$regPath\User Shell Folders" `
        -Name Desktop |
        select -ExpandProperty Desktop

    If ([String]::IsNullOrEmpty($desktopPath) -eq $true)
    {
        $desktopPath = Get-ItemProperty `
            -Path "$regPath\Shell Folders" `
            -Name Desktop |
            select -ExpandProperty Desktop
    }

    If ([String]::IsNullOrEmpty($desktopPath) -eq $true)
    {
        Throw "Unable to determine Desktop path."
    }

    Push-Location $desktopPath
}

Function temp { Push-Location C:\NotBackedUp\Temp }

Function toolbox { Push-Location C:\NotBackedUp\Public\Toolbox }

Function Enable-SharePointCmdlets
{
    If ((Get-PSSnapin Microsoft.SharePoint.PowerShell `
        -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Debug "Adding snapin (Microsoft.SharePoint.PowerShell)..."

        $ver = $host | select version

        If ($ver.Version.Major -gt 1)
        {
            $Host.Runspace.ThreadOptions = "ReuseThread"
        }

        Add-PSSnapin Microsoft.SharePoint.PowerShell
    }
}
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
