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

## # Configure firewall rule for POSHPAIG (http://poshpaig.codeplex.com/)

---


**FOOBAR8**

```PowerShell
$cred = Get-Credential FABRIKAM\jjameson-admin

$computer = 'FAB-DC02'

$script = "New-NetFirewallRule ``
    -Name 'Remote Windows Update (Dynamic RPC)' ``
    -DisplayName 'Remote Windows Update (Dynamic RPC)' ``
    -Description 'Allows remote auditing and installation of Windows updates via POSHPAIG (http://poshpaig.codeplex.com/)' ``
    -Group 'Technology Toolbox (Custom)' ``
    -Program '%windir%\system32\dllhost.exe' ``
    -Direction Inbound ``
    -Protocol TCP ``
    -LocalPort RPC ``
    -Profile Domain ``
    -Action Allow"

$scriptBlock = [scriptblock]::Create($script)

Invoke-Command -ComputerName $computer -Credential $cred -ScriptBlock $scriptBlock
```

---


## # Configure firewall rules for POSHPAIG (http://poshpaig.codeplex.com/)

```PowerShell
$script = "
Remove-NetFirewallRule -DisplayName 'Remote Windows Update (Dynamic RPC)'

New-NetFirewallRule ``
    -Name 'Remote Windows Update (DCOM-In)' ``
    -DisplayName 'Remote Windows Update (DCOM-In)' ``
    -Description 'Allows remote auditing and installation of Windows updates via POSHPAIG (http://poshpaig.codeplex.com/)' ``
    -Group 'Remote Windows Update' ``
    -Direction Inbound ``
    -Protocol TCP ``
    -LocalPort 135 ``
    -Profile Domain ``
    -Action Allow

New-NetFirewallRule ``
    -Name 'Remote Windows Update (Dynamic RPC)' ``
    -DisplayName 'Remote Windows Update (Dynamic RPC)' ``
    -Description 'Allows remote auditing and installation of Windows updates via POSHPAIG (http://poshpaig.codeplex.com/)' ``
    -Group 'Remote Windows Update' ``
    -Program '%windir%\system32\dllhost.exe' ``
    -Direction Inbound ``
    -Protocol TCP ``
    -LocalPort RPC ``
    -Profile Domain ``
    -Action Allow

New-NetFirewallRule ``
    -Name 'Remote Windows Update (SMB-In)' ``
    -DisplayName 'Remote Windows Update (SMB-In)' ``
    -Description 'Allows remote auditing and installation of Windows updates via POSHPAIG (http://poshpaig.codeplex.com/)' ``
    -Group 'Remote Windows Update' ``
    -Direction Inbound ``
    -Protocol TCP ``
    -LocalPort 445 ``
    -Profile Domain ``
    -Action Allow

Enable-NetFirewallRule ``
    -DisplayName 'File and Printer Sharing (Echo Request - ICMPv4-In)'

Enable-NetFirewallRule ``
    -DisplayName 'File and Printer Sharing (Echo Request - ICMPv6-In)'
```

## # Disable firewall rules for POSHPAIG (http://poshpaig.codeplex.com/)

```PowerShell
Disable-NetFirewallRule -Group 'Remote Windows Update'
"


@('FAB-FOOBAR4', 'FAB-TEST1') |
    ForEach-Object {
        $vmName = $_

        Write-Host "Enabling firewall rules on $vmName..."

        Invoke-Command -ComputerName $vmName -ScriptBlock {
            # Note: Enable-NetFirewallRule is not available on Windows
            # Server 2008 R2 or Windows 7

            netsh advfirewall firewall set rule `
                name="File and Printer Sharing (SMB-In)" `
                new enable=yes

            netsh advfirewall firewall set rule `
                name="Windows Management Instrumentation (WMI-In)" `
                new enable=yes
        }
    }
```

ANGEL\
BEAST\
CIPHER01\
COLOSSUS\
CYCLOPS\
CYCLOPS-TEST\
DAZZLER\
FOOBAR10\
FOOBAR8\
HAVOK\
HAVOK-TEST\
ICEMAN\
JUBILEE\
JUGGERNAUT\
MIMIC\
MOONSTAR\
POLARIS\
POLARIS-DEV\
POLARIS-TEST\
STORM\
WIN10-DEV1\
WIN7-TEST3\
WIN8-DEV1\
WIN8-TEST1\
WOLVERINE\
XAVIER1\
XAVIER2
