# Configure-Profile.ps1

Thursday, August 4, 2016
2:41 PM

```PowerShell
$ErrorActionPreference = "Stop"

Import-Module C:\NotBackedUp\Public\Toolbox\PowerShell\WASP\WASP.dll

Function Toggle-StartMenuPinnedItem
{
    [CmdletBinding()]
    Param(
        [Parameter(Position = 1, Mandatory = $true)]
        $appName
    )

    Write-Verbose "Toggling Start menu pinned item ($appName)..."

    Select-Window -ActiveWindow |
        Send-Keys -Keys "^{ESC}"  # open Start menu

    Sleep -Milliseconds 200

    $activeWindow = Select-Window -ActiveWindow

    If ($activeWindow.Title -eq "Start menu")
    {
        $activeWindow | Send-Keys -Keys $appName # type app name
        Sleep -Milliseconds 300

        $activeWindow | Send-Keys -Keys "+{F10}" # open context menu
        Sleep -Milliseconds 200

        # select "Pin to Start" or "Unpin from Start"
        $activeWindow | Send-Keys -Keys "{DOWN}"
        $activeWindow | Send-Keys -Keys "{ENTER}"

        $activeWindow | Send-Keys -Keys "{ESC 3}" # close Start menu
    }
    Else
    {
        Throw "Unexpected window ($($activeWindow.Title))"
    }
}

Function Toggle-TaskbarPinnedItem
{
    [CmdletBinding()]
    Param(
        [Parameter(Position = 1, Mandatory = $true)]
        $appName
    )

    Write-Verbose "Toggling Taskbar menu pinned item ($appName)..."

    Select-Window -ActiveWindow |
        Send-Keys -Keys "^{ESC}"  # open Start menu

    Sleep -Milliseconds 200

    $activeWindow = Select-Window -ActiveWindow

    If ($activeWindow.Title -eq "Start menu")
    {
        $activeWindow | Send-Keys -Keys $appName # type app name
        Sleep -Milliseconds 300

        $activeWindow | Send-Keys -Keys "+{F10}" # open context menu
        Sleep -Milliseconds 200

        # select "Pin to Taskbar" or "Unpin from Taskbar"
        $activeWindow | Send-Keys -Keys "{DOWN 2}"
        $activeWindow | Send-Keys -Keys "{ENTER}"

        $activeWindow | Send-Keys -Keys "{ESC 3}" # close Start menu
    }
    Else
    {
        Throw "Unexpected window ($($activeWindow.Title))"
    }
}

Function Configure-Profile
{
    Toggle-StartMenuPinnedItem "Prince" -Verbose
    Toggle-StartMenuPinnedItem "Microsoft Expression Web 4" -Verbose
    Toggle-StartMenuPinnedItem "SQL Server 2014 Management Studio" -Verbose

    Toggle-TaskbarPinnedItem "Developer Command Prompt for VS2015" -Verbose
    Toggle-TaskbarPinnedItem "Internet Explorer" -Verbose
    Toggle-TaskbarPinnedItem "Mozilla Firefox" -Verbose
    Toggle-TaskbarPinnedItem "Visual Studio 2015" -Verbose
}

Configure-Profile
```

## Configure trusted sites for Visual Studio 2015

The following items are required and are configured automatically by Visual Studio

[https://go.microsoft.com](https://go.microsoft.com)\
[https://login.microsoftonline.com](https://login.microsoftonline.com)\
about://security_devenv.exe\
[https://app.vssps.visualstudio.com](https://app.vssps.visualstudio.com)\
[https://tfsprodch1acs01.accesscontrol.windows.net](https://tfsprodch1acs01.accesscontrol.windows.net)

The following items are required -- but are not configured automatically by Visual Studio

[https://secure.aadcdn.microsoftonline-p.com](https://secure.aadcdn.microsoftonline-p.com)\
[https://auth.gfx.ms](https://auth.gfx.ms)

The following items are required when using a Windows Live account -- but are not configured automatically by Visual Studio

[https://client.hip.live.com](https://client.hip.live.com)\
[https://login.live.com](https://login.live.com)


