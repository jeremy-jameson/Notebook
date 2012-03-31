# Fabrikam Demo - EmployeePortal Branch

Saturday, March 31, 2012
5:29 AM

## Visual Studio command prompt

### REM Get latest and rebuild WSPs

```Console
mkdir C:\NotBackedUp\Fabrikam\Demo
tf get C:\NotBackedUp\Fabrikam\Demo\Dev\EmployeePortal /recursive /force
tf unshelve "Demo - Temp - EmployeePortal" /noprompt
cd C:\NotBackedUp\Fabrikam\Demo\Dev\EmployeePortal\Source
msbuild Fabrikam.Demo.sln /p:IsPackaging=true
```

## # Setup

```PowerShell
Add-PSSnapin Microsoft.SharePoint.PowerShell
cd "C:\NotBackedUp\Fabrikam\Demo\Dev\EmployeePortal\Source\Deployment Files\Scripts"
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

## # Start Fabrikam Demo Test Console

```PowerShell
pushd ..\..\Tools\TestConsole\bin\Debug
.\Fabrikam.Demo.Tools.TestConsole.exe
popd
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
Remove-SPSite http://fabrikam-local/ -Confirm:$false
& '.\Create Site Collections.ps1'
& '.\Redeploy Features.ps1'
```

```PowerShell
cls
Remove-SPSite http://fabrikam-local/sites/Test -Confirm:$false
& '.\Create Site Collections.ps1'
& '.\Redeploy Features.ps1'
```

```PowerShell
cls
```

## # Rebuild Fabrikam Web app

```PowerShell
& '.\Rebuild Web Application.ps1' -Confirm:$false

pushd ..\..\Tools\TestConsole\bin\Debug
.\Fabrikam.Demo.Tools.TestConsole.exe
popd
```

```PowerShell
cls
C:\Windows\System32\inetsrv\appcmd recycle apppool "SharePoint - portal.fabrikam.com80"
```

```PowerShell
cls
& '.\Create Web Application.ps1'
& '.\Configure Object Cache User Accounts.ps1'
& '.\Create Site Collections.ps1'
& '.\Enable Anonymous Access.ps1'
& '.\Add Solutions.ps1'
& '.\Deploy Solutions.ps1'
& '.\Activate Features.ps1'
```

### REM Configure the People Picker to support searches across one-way trust

#### REM Specify the credentials for accessing the trusted forest

```Console
stsadm -o setproperty -pn peoplepicker-searchadforests -pv "domain:extranet.technologytoolbox.com,EXTRANET\s-web-fabrikam-dev,{password};domain:corp.fabrikam.com,FABRIKAM\s-web-fabrikam-dev,{password}" -url http://portal.fabrikam.com
```

```Console
cls
```

## # Set environment variables

```PowerShell
[Environment]::SetEnvironmentVariable(
    "FABRIKAM_PORTAL_URL",
    "http://portal-local.fabrikam.com",
    "Machine")

[Environment]::SetEnvironmentVariable(
    "FABRIKAM_BUILD_CONFIGURATION",
    "Debug",
    "Machine")
```

## Shelveset names

- Demo - Temp - EmployeePortal

## Check-in comments

- Merge latest version of \$/Demo/Main branch into Dev/EmployeePortal branch
- Merge latest version of \$/Demo/Dev/EmployeePortal branch into Main branch
- Discard merge of portal-specific changes from \$/Demo/Dev/EmployeePortal branch to Main branch
