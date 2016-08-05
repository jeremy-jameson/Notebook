# Install-ExpressionWeb

Thursday, August 4, 2016
5:33 PM

```PowerShell
Import-Module Storage
Import-Module C:\NotBackedUp\Public\Toolbox\PowerShell\WASP\WASP.dll

Function Ensure-ActiveWindow
{
    Param(
        $windowTitle
    )

    Do
    {
        $activeWindow = Select-Window -ActiveWindow

        If ($activeWindow.Title -eq $windowTitle)
        {
            return $activeWindow
        }

        Write-Debug "Waiting for window ($windowTitle) to be active..."
        Start-Sleep -Milliseconds 500
    } While($true)
}

Function Ensure-MountedDiskImage
{
    Param(
        $imagePath
    )

    $imageDriveLetter = (Get-DiskImage -ImagePath $imagePath |
        Get-Volume).DriveLetter

    If ($imageDriveLetter -eq $null)
    {
        $imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
            Get-Volume).DriveLetter
    }

    return $imageDriveLetter
}

$imagePath = '\\ICEMAN\Products\Microsoft\Expression Studio' `
    + '\en_expression_studio_4_ultimate_x86_dvd_537032.iso'

$imageDriveLetter = Ensure-MountedDiskImage $imagePath

If ((Get-Process -Name "Setup" -ErrorAction SilentlyContinue) -eq $null)
{
    $setupPath = $imageDriveLetter + ':\Setup.exe'

    Write-Debug "Starting setup..."
    & $setupPath

    Start-Sleep -Seconds 15
}

$setupWindow = Ensure-ActiveWindow "Microsoft Expression Studio 4 Ultimate Setup"
Write-Debug "Active window: $($setupWindow.Title)"

Write-Debug "Accepting license agreement..."
$setupWindow | Send-Keys "%a"
Sleep -Seconds 1

Write-Debug "Entering license key..."
$setupWindow | Send-Keys "*****-T74X9-R7D63-JYMY8-*****"
Sleep -Milliseconds 500
$setupWindow | Send-Keys "%n"
Sleep -Milliseconds 500

Write-Debug "Joining CEIP..."
$setupWindow | Send-Keys "{Tab 7}"
Sleep -Milliseconds 500
$setupWindow | Send-Keys "{Enter}"
Sleep -Milliseconds 500

Write-Debug "Installing..."
$setupWindow | Send-Keys "%i"

Write-Debug "Waiting for setup to complete..."
Wait-Process -Name Setup

Dismount-DiskImage -ImagePath $imagePath
```
