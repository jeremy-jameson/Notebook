# Install-ExpressionStudio.ps1

Thursday, August 4, 2016
5:33 PM

```Text
12345678901234567890123456789012345678901234567890123456789012345678901234567890
```

---

**WOLVERINE**

```PowerShell
cls
```

## # Copy installation media from internal file server

```PowerShell
$isoFile = "en_expression_studio_4_ultimate_x86_dvd_537032.iso"

$source = "\\TT-FS01\Products\Microsoft\Expression Studio"

$destination = "\\EXT-FOOBAR4.extranet.technologytoolbox.com" `
    + "\C$\NotBackedUp\Temp"

robocopy $source $destination $isoFile
```

---

```PowerShell
cls
```

## # Install Expression Studio

```PowerShell
Import-Module Storage
Import-Module C:\NotBackedUp\Public\Toolbox\PowerShell\WASP\WASP.dll

$VerbosePreference = "Continue"

Function Ensure-ActiveWindow
{
    [CmdletBinding()]
    Param(
        [Parameter(Position = 1, Mandatory = $true)]
        [string] $WindowTitle
    )

    Write-Verbose "Ensuring active window ($WindowTitle)..."

    Do
    {
        $activeWindow = Select-Window -ActiveWindow

        If ($activeWindow.Title -eq $WindowTitle)
        {
            return $activeWindow
        }

        Write-Debug "Waiting for window ($WindowTitle) to be active..."
        Start-Sleep -Milliseconds 500
    } While($true)
}

Function Ensure-MountedDiskImage
{
    [CmdletBinding()]
    Param(
        [Parameter(Position = 1, Mandatory = $true)]
        [string] $ImagePath
    )

    $imageDriveLetter = (Get-DiskImage -ImagePath $ImagePath|
        Get-Volume).DriveLetter

    If ($imageDriveLetter -eq $null)
    {
        Write-Verbose "Mounting disk image ($ImagePath)..."
        $imageDriveLetter = (Mount-DiskImage -ImagePath $ImagePath -PassThru |
            Get-Volume).DriveLetter
    }

    return $imageDriveLetter
}

Function Install-ExpressionStudio
{
    [CmdletBinding()]
    Param(
        [Parameter(Position = 1, Mandatory = $true)]
        [string] $ProductKey
    )

    $imagePath = 'C:\NotBackedUp\Temp' `
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
