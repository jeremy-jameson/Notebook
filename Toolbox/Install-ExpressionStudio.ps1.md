# Install-ExpressionStudio.ps1

Thursday, August 4, 2016
5:33 PM

```PowerShell
12345678901234567890123456789012345678901234567890123456789012345678901234567890

Import-Module Storage
Import-Module C:\NotBackedUp\Public\Toolbox\PowerShell\WASP\WASP.dll

$VerbosePreference = "Continue"

Function Ensure-ActiveWindow
{
    Param(
        $windowTitle
    )

    Write-Verbose "Ensuring active window ($windowTitle)..."

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
        Write-Verbose "Mounting disk image ($imagePath)..."
        $imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
            Get-Volume).DriveLetter
    }

    return $imageDriveLetter
}

Function Install-ExpressionStudio
{
    Param(
        $ProductKey
    )

    $imagePath = '\\ICEMAN\Products\Microsoft\Expression Studio' `
        + '\en_expression_studio_4_ultimate_x86_dvd_537032.iso'

    $imageDriveLetter = Ensure-MountedDiskImage $imagePath

    If ((Get-Process -Name "Setup" -ErrorAction SilentlyContinue) -eq $null)
    {
        $setupPath = $imageDriveLetter + ':\Setup.exe'

        Write-Verbose "Starting setup..."
        & $setupPath

        Start-Sleep -Seconds 15
    }

    $setupWindow = Ensure-ActiveWindow `
        "Microsoft Expression Studio 4 Ultimate Setup"

    Write-Debug "Active window: $($setupWindow.Title)"

    Write-Verbose "Accepting license agreement..."
    $setupWindow | Send-Keys "%a"
    Sleep -Seconds 1

    Write-Verbose "Entering license key..."
    $setupWindow | Send-Keys $productKey
    Sleep -Seconds 10
    $setupWindow | Send-Keys "%n"
    Sleep -Milliseconds 500

    Write-Verbose "Joining CEIP..."
    $setupWindow | Send-Keys "{Tab 7}"
    Sleep -Milliseconds 500
    $setupWindow | Send-Keys "{Enter}"
    Sleep -Milliseconds 500

    Write-Verbose "Installing..."
    $setupWindow | Send-Keys "%i"

    Write-Verbose "Waiting for setup to complete..."
    Sleep -Seconds 60

    Write-Verbose "Finishing setup..."
    $setupWindow | Send-Keys "%f"

    Wait-Process -Name Setup

    Dismount-DiskImage -ImagePath $imagePath
}

Install-ExpressionStudio -ProductKey "*****-T74X9-R7D63-JYMY8-*****"
```
