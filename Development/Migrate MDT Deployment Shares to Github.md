# Migrate MDT Deployment Shares to Github

Saturday, June 1, 2019
4:01 PM

## Compare folders to ensure identical copies of files

```PowerShell
cd C:\NotBackedUp\GitHub\technology-toolbox

$leftFolderName = 'MDT'
$rightFolderName = 'MDT-Deploy'

$leftFolderPath = "C:\NotBackedUp\GitHub\technology-toolbox\$leftFolderName"
$rightFolderPath = "C:\NotBackedUp\GitHub\technology-toolbox\$rightFolderName"

Get-ChildItem $leftFolderPath -Recurse |
    Get-FileHash |
    select @{Label="Path";Expression={$_.Path.Replace($leftFolderPath,"")}},Hash |
    Export-Csv -NoTypeInformation -Path "$leftFolderName.csv"

Get-ChildItem $rightFolderPath -Recurse |
    Get-FileHash |
    select @{Label="Path";Expression={$_.Path.Replace($rightFolderPath,"")}},Hash |
    Export-Csv -NoTypeInformation -Path "$rightFolderName.csv"

C:\NotBackedUp\Public\Toolbox\DiffMerge\x64\sgdm.exe "$leftFolderName.csv" "$rightFolderName.csv"
```
