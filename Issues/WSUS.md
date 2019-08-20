# WSUS

Saturday, January 12, 2019
9:26 AM

Slow cleanup

[https://www.google.com/search?client=firefox-b-1-ab&q=spdeleteupdate+slow](https://www.google.com/search?client=firefox-b-1-ab&q=spdeleteupdate+slow)

**Enhancing WSUS database cleanup performance SQL script**\
From <[https://stevethompsonmvp.wordpress.com/2018/05/01/enhancing-wsus-database-cleanup-performance-sql-script/](https://stevethompsonmvp.wordpress.com/2018/05/01/enhancing-wsus-database-cleanup-performance-sql-script/)>

**WSUS Delete Obsolete Updates**\
From <[https://kickthatcomputer.wordpress.com/2017/08/15/wsus-delete-obsolete-updates/](https://kickthatcomputer.wordpress.com/2017/08/15/wsus-delete-obsolete-updates/)>

**SCCM/WSUS – Clean-up WSUS Strategy & Maintenance**\
From <[https://www.walshamsolutions.com/SCCM-WSUS-Cleanup-WSUS-Strategy-maintenance](https://www.walshamsolutions.com/SCCM-WSUS-Cleanup-WSUS-Strategy-maintenance)>

**Cleaning a Stuck WSUS Server: Too Many Unapproved Updates**\
From <[http://pnijjar.freeshell.org/2016/wsus-stuck/](http://pnijjar.freeshell.org/2016/wsus-stuck/)>

USE [SUSDB]\
GO\
CREATE NONCLUSTERED INDEX [IX_tbRevisionSupersedesUpdate] ON [dbo].[tbRevisionSupersedesUpdate]([SupersededUpdateID])\
GO\
CREATE NONCLUSTERED INDEX [IX_tbLocalizedPropertyForRevision] ON [dbo].[tbLocalizedPropertyForRevision]([LocalizedPropertyID])\
GO

## Decline superseded updates

```PowerShell
# Option 1
# ------------------------------------------------------------------------------

$stopwatch = C:\NotBackedUp\Public\Toolbox\PowerShell\Get-Stopwatch.ps1

$supersededUpdates = Get-WsusUpdate -Approval AnyExceptDeclined -Classification All -Status Any |
    Where-Object -Property UpdatesSupersedingThisUpdate -NE -Value 'None'

C:\NotBackedUp\Public\Toolbox\PowerShell\Write-ElapsedTime.ps1 -Stopwatch $stopwatch -Prefix "Option 1 - "

Write-Host ("Option 1 count: " + $supersededUpdates.Count)

# Option 2
# ------------------------------------------------------------------------------

$stopwatch = C:\NotBackedUp\Public\Toolbox\PowerShell\Get-Stopwatch.ps1

$supersededUpdates = Get-WSUSUpdate -Approval AnyExceptDeclined -Classification All -Status Any `
    | Where-Object { $_.Update.GetRelatedUpdates(([Microsoft.UpdateServices.Administration.UpdateRelationship]::UpdatesThatSupersedeThisUpdate)).Count -gt 0 } `

C:\NotBackedUp\Public\Toolbox\PowerShell\Write-ElapsedTime.ps1 -Stopwatch $stopwatch -Prefix "Option 2 - "

Write-Host ("Option 2 count: " + $supersededUpdates.Count)

# Option 3
# ------------------------------------------------------------------------------

$stopwatch = C:\NotBackedUp\Public\Toolbox\PowerShell\Get-Stopwatch.ps1

$supersededUpdates = Get-WsusUpdate -Approval Approved -Status InstalledOrNotApplicableOrNoStatus |
    ? {$_.Update.IsSuperseded -eq $true -and $_.ComputersNeedingThisUpdate -eq 0}

C:\NotBackedUp\Public\Toolbox\PowerShell\Write-ElapsedTime.ps1 -Stopwatch $stopwatch -Prefix "Option 3 - "

Write-Host ("Option 3 count: " + $supersededUpdates.Count)

Option 1 - Elapsed time: 00:23:56.098
Option 1 count: 186
Option 2 - Elapsed time: 00:25:05.847
Option 2 count: 186
Option 3 - Elapsed time: 00:06:19.584
Option 3 count: 146

# Option 4
# ------------------------------------------------------------------------------

$stopwatch = C:\NotBackedUp\Public\Toolbox\PowerShell\Get-Stopwatch.ps1

$supersededUpdates =
Get-WsusUpdate -Approval Approved -Status InstalledOrNotApplicableOrNoStatus |
    Where-Object -Property UpdatesSupersedingThisUpdate -NE -Value 'None' |
    Where-Object -Property ComputersNeedingThisUpdate -EQ 0

C:\NotBackedUp\Public\Toolbox\PowerShell\Write-ElapsedTime.ps1 -Stopwatch $stopwatch -Prefix "Option 4 - "

Write-Host ("Option 4 count: " + $supersededUpdates.Count)

Option 4 - Elapsed time: 00:06:10.966
Option 4 count: 146
```
