# Tugboat - Main Branch

Wednesday, August 08, 2012\
3:57 PM

```PowerShell
Add-PSSnapin Microsoft.SharePoint.PowerShell
cd "C:\NotBackedUp\Tugboat\Main\Source\Deployment Files\Scripts"
```

```PowerShell
cls
& '.\Redeploy Features.ps1'
```

```PowerShell
cls
& '.\Upgrade Solutions.ps1'
```

```PowerShell
cls
& '.\Deactivate Features.ps1'
& '.\Retract Solutions.ps1'
& '.\Delete Solutions.ps1'
```

```PowerShell
cls
& '.\Add Solutions.ps1'
& '.\Deploy Solutions.ps1'
& '.\Activate Features.ps1'
```

```PowerShell
cls
& '.\Rebuild Web Application.ps1' -Confirm:$false
```
