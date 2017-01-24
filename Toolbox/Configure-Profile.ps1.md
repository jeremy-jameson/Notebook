# Configure-Profile.ps1

Thursday, August 4, 2016
2:41 PM

```Console
12345678901234567890123456789012345678901234567890123456789012345678901234567890
```

```Console
cls
```

## # Mirror Toolbox content

```PowerShell
$source = "\\tt-fs01.corp.technologytoolbox.com\Public\Toolbox"
$dest = "C:\NotBackedUp\Public\Toolbox"

robocopy $source $dest /E /MIR
```

## # Configure profile

```PowerShell
$ErrorActionPreference = "Stop"

Function Configure-Profile
{
    Push-Location C:\NotBackedUp\Public\Toolbox\PowerShell

    .\Toggle-StartMenuPinnedItem.ps1 "Prince" -Verbose
    .\Toggle-StartMenuPinnedItem.ps1 "Microsoft Expression Web 4" -Verbose
    .\Toggle-StartMenuPinnedItem.ps1 `
        "SQL Server 2014 Management Studio" `
        -Verbose

    .\Toggle-TaskbarPinnedItem.ps1 `
        "Developer Command Prompt for VS2015" `
        -Verbose

    .\Toggle-TaskbarPinnedItem.ps1 "Internet Explorer" -Verbose
    .\Toggle-TaskbarPinnedItem.ps1 "Mozilla Firefox" -Verbose
    .\Toggle-TaskbarPinnedItem.ps1 "Visual Studio 2015" -Verbose

    Pop-Location
}

Configure-Profile
```

```PowerShell
cls
```

## # Configure trusted sites for Visual Studio 2015

```PowerShell
$trustedSites = @(
    # The following items are required and are configured automatically by
    # Visual Studio:
    #"https://go.microsoft.com",
    #"https://login.microsoftonline.com",
    #"about://security_devenv.exe",
    #"https://app.vssps.visualstudio.com",
    #"https://tfsprodch1acs01.accesscontrol.windows.net",
    # The following items are required -- but are not configured automatically
    # by Visual Studio:
    "https://secure.aadcdn.microsoftonline-p.com",
    "https://auth.gfx.ms",
    # The following items are required when using a Windows Live account -- but
    # are not configured automatically by Visual Studio:
    "https://client.hip.live.com",
    "https://login.live.com"
)

C:\NotBackedUp\Public\Toolbox\PowerShell\Add-InternetSecurityZoneMapping.ps1 `
    -Zone TrustedSites `
    -Patterns $trustedSites
```
