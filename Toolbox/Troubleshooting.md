# Troubleshooting

Tuesday, June 02, 2015
7:40 AM

## Fiddler debugging

```XML
  <system.net>
    <defaultProxy>
<!-- Use the following element to debug using Fiddler -->
      <proxy
        usesystemdefault="False"
        bypassonlocal="True"
        proxyaddress="http://127.0.0.1:8888"/>
<!-- -->
    </defaultProxy>
  </system.net>
```

## Network capture

```PowerShell
cls
```

### # Start network capture

```PowerShell
If ((Test-Path C:\NotBackedUp\Temp\Captures) -eq $false)
{
    New-Item -ItemType Directory -Path C:\NotBackedUp\Temp\Captures
}

$traceFile = "C:\NotBackedUp\Temp\Captures\$env:COMPUTERNAME.etl"

netsh trace start persistent=yes capture=yes tracefile=$traceFile maxsize=500 overwrite=yes
```

```PowerShell
cls
```

### # Stop network capture

```PowerShell
netsh trace stop
```

---


**WOLVERINE**

```PowerShell
cls
```

### # Copy network capture for analysis

```PowerShell
$source = "\\EXT-SQL01A.extranet.technologytoolbox.com" `
    + "\C$\NotBackedUp\Temp\Captures"

$destination = "C:\NotBackedUp\Temp\Captures"

robocopy $source $destination /MOVE
```

---


## # Copy Web server config files to Temp

```PowerShell
$webServers = @(
    "EXT-APP01A"
    , "EXT-WEB01A"
    , "EXT-WEB01B"
)

$webServers | ForEach-Object {
    Write-Host $_

    $source = "\\$_\C$\Inetpub\wwwroot"
    $destination = "C:\NotBackedUp\Temp\WebServers\$_\Inetpub\wwwroot"

    robocopy $source $destination *.config /S
}
```

## # Enable Windows Installer verbose logging (e.g. to troubleshoot SharePoint installation)

```PowerShell
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer" /v Debug /t REG_DWORD /d 7 /f

reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer" /v Logging /t REG_SZ /d voicewarmupx! /f
```

### References

**Sharepoint Server 2013 installation: why ArpWrite action fails?**\
Pasted from <[http://sharepoint.stackexchange.com/questions/68620/sharepoint-server-2013-installation-why-arpwrite-action-fails](http://sharepoint.stackexchange.com/questions/68620/sharepoint-server-2013-installation-why-arpwrite-action-fails)>

"...steps you can use to gather a Windows Installer verbose log file.."\
Pasted from <[http://blogs.msdn.com/b/astebner/archive/2005/03/29/403575.aspx](http://blogs.msdn.com/b/astebner/archive/2005/03/29/403575.aspx)>

## # Disable Windows Installer verbose logging

```PowerShell
reg delete "HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer" /v Debug /f

reg delete "HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer" /v Logging /f
```
