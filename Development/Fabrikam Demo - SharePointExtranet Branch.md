# Fabrikam Demo - SharePointExtranet Branch

Saturday, March 31, 2012\
5:29 AM

## Setup

### REM Visual Studio command prompt

### REM Get latest and rebuild WSPs

```Console
mkdir C:\NotBackedUp\Fabrikam
cd C:\NotBackedUp\Fabrikam
tf get Demo\Dev\SharePointExtranet /recursive /force
cd Demo\Dev\SharePointExtranet\Source
msbuild Fabrikam.Demo.sln /p:IsPackaging=true
```

## # Set environment variables

```PowerShell
[Environment]::SetEnvironmentVariable(
    "FABRIKAM_EXTRANET_URL",
    "http://extranet-local.fabrikam.com",
    "Machine")

[Environment]::SetEnvironmentVariable(
    "FABRIKAM_BUILD_CONFIGURATION",
    "Debug",
    "Machine")
```

### # EXT-FOOBAR2/FAB-FOOBAR2

```PowerShell
Add-PSSnapin Microsoft.SharePoint.PowerShell
cd "C:\NotBackedUp\Fabrikam\Demo\Dev\SharePointExtranet\Source\Deployment Files\Scripts"
```

```PowerShell
cls
```

## # Upgrade solution (major)

```PowerShell
& '.\Redeploy Features.ps1'
```

```PowerShell
cls
```

## # Upgrade solution (minor)

```PowerShell
& '.\Upgrade Solutions.ps1'
```

```PowerShell
cls
```

## # Upgrade solution (step-by-step)

```PowerShell
& '.\Deactivate Features.ps1'
& '.\Retract Solutions.ps1'
& '.\Delete Solutions.ps1'
& '.\Add Solutions.ps1'
& '.\Deploy Solutions.ps1'
& '.\Activate Features.ps1'
```

```PowerShell
cls
```

## # Rebuild Fabrikam Extranet Web app

```PowerShell
& '.\Rebuild Web Application.ps1' -Confirm:$false

pushd ..\..\Tools\TestConsole\bin\Debug
.\Fabrikam.Demo.Tools.TestConsole.exe
popd
```

```PowerShell
cls
```

## # Recyle the app pool for the Fabrikam Extranet Web app

```PowerShell
C:\Windows\System32\inetsrv\appcmd start apppool "SharePoint - extranet-local.fabrikam.com80"
```

```PowerShell
cls
```

## # Recreate Test site collection

```PowerShell
Remove-SPSite http://extranet-local.fabrikam.com/sites/Test -Confirm:$false
& '.\Create Site Collections.ps1'
```

```PowerShell
cls
```

## # Recreate main site collection

```PowerShell
Remove-SPSite http://extranet-local.fabrikam.com/ -Confirm:$false
& '.\Create Site Collections.ps1'
& '.\Redeploy Features.ps1'
```

## Check-in comments

- Merge latest version of \$/Demo/Main branch into Dev/SharePointExtranet branch
- Merge latest version of \$/Demo/Dev/SharePointExtranet branch into Main branch
- Discard merge of extranet-specific changes from \$/Demo/Dev/SharePointExtranet branch to Main branch
