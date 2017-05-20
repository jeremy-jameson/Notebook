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

### Start network capture

```Console
mkdir C:\NotBackedUp\Temp\Captures

netsh trace start persistent=yes capture=yes tracefile=C:\NotBackedUp\Temp\Captures\%COMPUTERNAME%.etl maxsize=500 overwrite=yes
```

### Stop network capture

```Console
netsh trace stop
```

### Copy network capture for analysis

```Console
robocopy C:\NotBackedUp\Temp\Captures \\WOLVERINE\C$\NotBackedUp\Temp\Captures
```

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
