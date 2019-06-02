# Migrate MDT Deployment Shares to Github

Saturday, June 1, 2019
4:01 PM

```PowerShell
cls
$utcOffsetHours = [TimeZoneInfo]::Local |
    select -ExpandProperty BaseUtcOffset |
    select -ExpandProperty Hours

$timezoneOffset = $utcOffsetHours.ToString("00") + "00"

$commitDate = (Get-Date).Subtract([TimeSpan]::FromDays((365*4 + 154.2)))

$commitDateInRfc2822Format = `
    $commitDate.ToString("R").Replace("GMT", $timezoneOffset)

$env:GIT_AUTHOR_DATE = "$commitDateInRfc2822Format"
$env:GIT_COMMITTER_DATE = "$commitDateInRfc2822Format"

$env:GIT_AUTHOR_DATE

git status

#git commit -m ""
```
