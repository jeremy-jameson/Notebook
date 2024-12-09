# Toolbox

Tuesday, April 09, 2013\
6:08 AM

**7 Must-Have (Free) Mobile Apps to do Your Job Better**\
Pasted from <[http://www.linkedin.com/today/post/article/20130312182519-2967511-7-must-have-free-mobile-apps-to-do-your-job-better](http://www.linkedin.com/today/post/article/20130312182519-2967511-7-must-have-free-mobile-apps-to-do-your-job-better)>

## # Authenticate to file server

```PowerShell
net use \\TT-FS01.corp.technologytoolbox.com\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
> 
> When prompted, type the password to connect to the file share.

## # Copy Toolbox content

```PowerShell
$source = "\\TT-FS01.corp.technologytoolbox.com\Public\Toolbox"
$dest = "C:\NotBackedUp\Public\Toolbox"

robocopy $source $dest /E /XD git-for-windows "Microsoft SDKs"
```

```PowerShell
cls
```

## # Mirror Toolbox content

```PowerShell
$source = "\\TT-FS01.corp.technologytoolbox.com\Public\Toolbox"
$dest = "C:\NotBackedUp\Public\Toolbox"

robocopy $source $dest /E /MIR /XD git-for-windows "Microsoft SDKs"
```

```PowerShell
cls
```

## # Mirror Toolbox content across all domain computers

```PowerShell
$source = "\\TT-FS01.corp.technologytoolbox.com\Public\Toolbox"

$computers = Get-ADComputer -Filter * |
    where { $_.Name -notin @(
        'TT-HV05',
        'TT-HV05-FC',
        'TT-SOFS01',
        'TT-SOFS01-FC',
        'TT-SQL01',
        'TT-SQL01-FC',
        'TT-VMM01',
        'TT-VMM01-FC',
        'WOLVERINE') } |
    select -ExpandProperty Name

$computers | ForEach-Object {
    Write-Host "Mirroring Toolbox content on computer ($_)..."

    $dest = '\\' + $_ + '\C$\NotBackedUp\Public\Toolbox'

    robocopy $source $dest /E /MIR /XD git-for-windows "Microsoft SDKs" /R:1 /W:1
}
```

```PowerShell
cls
```

## # Upload Toolbox content to Azure

```PowerShell
$key = {key}
$source = "\\TT-FS01.corp.technologytoolbox.com\Public\Toolbox"
$destination = "https://techtoolbox.file.core.windows.net/toolbox"

& 'C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe' `
    /Source:$source `
    /Dest:$destination `
    /DestKey:$key `
    /S /XO
```

```PowerShell
cls
```

## # Download Toolbox content from Azure

```PowerShell
$key = {key}
$source = "https://techtoolbox.file.core.windows.net/toolbox"
$destination = "C:\NotBackedUp\Public\Toolbox"

& 'C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe' `
    /Source:$source `
    /SourceKey:$key `
    /Dest:$destination `
    /S /XO
```

```PowerShell
cls
```

## # Set password for local Administrator account

```PowerShell
$adminUser = [ADSI] "WinNT://./Administrator,User"
$adminUser.SetPassword("{password}")
```

```PowerShell
cls
```

## # Get disk usage

```PowerShell
function Format-HumanReadable
        {
            param ($size)
            switch ($size)
            {
                {$_ -ge 1PB}{"{0:#.#' PB'}" -f ($size / 1PB); break}
                {$_ -ge 1TB}{"{0:#.#' TB'}" -f ($size / 1TB); break}
                {$_ -ge 1GB}{"{0:#.#' GB'}" -f ($size / 1GB); break}
                {$_ -ge 1MB}{"{0:#.#' MB'}" -f ($size / 1MB); break}
                {$_ -ge 1KB}{"{0:#' KB'}" -f ($size / 1KB); break}
                default {"{0}" -f ($size) + " B"}
            }
        }

$computers = @('TT-HV02A', 'TT-HV02B', 'TT-HV02C')

$computers |
    ForEach-Object {
        Invoke-Command -ComputerName $_ {Get-PSDrive -PSProvider FileSystem} |
            Select-Object PSComputerName, Name,
                @{n='Used';e={Format-HumanReadable $_.Used}},
                @{n='Free';e={Format-HumanReadable $_.Free}}
    }
```

```PowerShell
cls
```

## # Change drive letter for DVD-ROM

```PowerShell
$cdrom = Get-WmiObject -Class Win32_CDROMDrive
$driveLetter = $cdrom.Drive

$volumeId = mountvol $driveLetter /L
$volumeId = $volumeId.Trim()

mountvol $driveLetter /D

mountvol X: $volumeId
```

### Reference

**Change CD ROM Drive Letter in Newly Built VM's to Z:\\ Drive**\
Pasted from <[http://www.vinithmenon.com/2012/10/change-cd-rom-drive-letter-in-newly.html](http://www.vinithmenon.com/2012/10/change-cd-rom-drive-letter-in-newly.html)>

## # Add hostnames

```PowerShell
C:\NotBackedUp\Public\Toolbox\PowerShell\Add-Hostnames.ps1 `
    -IPAddress 192.168.10.216 `
    -Hostnames `
        ext-foobar2,
        ext-foobar2.extranet.technologytoolbox.com,
        client-local.securitasinc.com,
        cloud-local.securitasinc.com
```

## REM Find hidden network adapters

```Console
set devmgr_show_nonpresent_devices=1
start devmgmt.msc
```

Pasted from <[http://www.technologytoolbox.com/blog/jjameson/archive/2011/03/14/removing-quot-stale-quot-network-adapters-in-hyper-v-vm.aspx](http://www.technologytoolbox.com/blog/jjameson/archive/2011/03/14/removing-quot-stale-quot-network-adapters-in-hyper-v-vm.aspx)>

## # Clear event logs and restart computer

```PowerShell
C:\NotBackedUp\Public\Toolbox\PowerShell\Clear-EventLogs.ps1

Restart-Computer
```

## Use .NET assembly in PowerShell

```Console
[void] [Reflection.Assembly]::LoadWithPartialName("Fabrikam.Demo.CoreServices.SharePoint")

[Fabrikam.Demo.CoreServices.SharePoint.Diagnostics.SPLogger]::Log("General", "High", "Test", $null)
```

```Console
cls
```

## # Monitor BITS transfer (e.g. WSUS download)

```PowerShell
Get-BitsTransfer -AllUsers | ? { $_.JobState -eq "Transferring" } | fl FileList, @{Name='Size';Expression={"{0:N1} MB" -f ($_.BytesTotal / 1MB)}}, @{Name='Progress';Expression={"{0:0.0%}" -f ($_.BytesTransferred / $_.BytesTotal)}}


FileList : {http://wsus.ds.download.windowsupdate.com/d/msdownload/update/software/svpk/2014/06/sqlserver2012sp2-kb2958429-x86-enu_63fa1d5ace194b8ebca1c085f0acb3ff9e608e04.exe}
Size     : 713 MB
Progress : 49.1%
```

## Download files using BITS

### Create Download.csv file using Excel

#### A1 - Source

#### B1 - Destination

#### C1 - Index

#### A2 - {Paste download URL}

#### B2 - Formula

```Text
=MID(A2,C2+1, 1000)
```

#### C2 - Formula

```Text
=SEARCH("@",SUBSTITUTE(A2,"/","@",LEN(A2)-LEN(SUBSTITUTE(A2,"/",""))))
```

#### Reference

**Excel Last Index Of**\
Pasted from <[http://geekswithblogs.net/prash/archive/2011/11/02/excel-last-index-of.aspx](http://geekswithblogs.net/prash/archive/2011/11/02/excel-last-index-of.aspx)>

### # Start BITS job (PowerShell)

```PowerShell
Import-Module BitsTransfer
Import-Csv .\Download.csv | Start-BitsTransfer
```

### # Suspend BITS jobs

```PowerShell
Import-Module BitsTransfer
Get-BitsTransfer | ? { $_.JobState -eq "Transferring" } | Suspend-BitsTransfer
```

### # Resume BITS jobs

```PowerShell
Import-Module BitsTransfer
Get-BitsTransfer | ? { $_.JobState -eq "Suspended" } |
    Resume-BitsTransfer -Asynchronous
```

## Parse Channel 9 videos

```PowerShell
$(".entry-meta").each(function () { '"' + console.log('"' + $(this).children("a.title").text() + '",' + $(this).find("ul.media a").filter(function() { return this.innerHTML == 'MP4 (High)'; }).attr('href')); })
```

## Backup VMs on WOLVERINE

```Console
robocopy C:\NotBackedUp\VMs F:\NotBackedUp\VMs /E
robocopy D:\NotBackedUp\VMs F:\NotBackedUp\VMs /E
robocopy F:\NotBackedUp\VMs \\iceman\Backups\VirtualBox /E
```

## # Configure startup delays on VMs

```PowerShell
Get-VM | sort AutomaticStartDelay | select Name, AutomaticStartDelay

Get-VM | sort AutomaticStartDelay | % { $i = 0 } { Set-VM $_ -AutomaticStartDelay ($i * 15); $i++ }
```

## Provision new VM

---


**FORGE**

### # Create virtual machine

```PowerShell
$vmName = "POLARIS-DEV"

New-VM `
    -Name $vmName `
    -Path C:\NotBackedUp\VMs `
    -MemoryStartupBytes 8GB `
    -SwitchName "Virtual LAN 2 - 192.168.10.x"

Set-VM -VMName $vmName -ProcessorCount 4

New-Item -ItemType Directory "C:\NotBackedUp\VMs\$vmName\Virtual Hard Disks"

$vhdPath = "C:\NotBackedUp\VMs\$vmName\Virtual Hard Disks\$vmName.vhdx"

$sysPrepedImage = "\\ICEMAN\VM-Library\VHDs\WS2012-R2-STD.vhdx"

Copy-Item $sysPrepedImage $vhdPath

Add-VMHardDiskDrive -VMName $vmName -Path $vhdPath

Start-VM $vmName
```

---


### Configure server settings

On the **Settings** page:

1. Ensure the following default values are selected:
   1. **Country or region: United States**
   2. **App language: English (United States)**
   3. **Keyboard layout: US**
2. Click **Next**.
3. Type the product key and then click **Next**.
4. Review the software license terms and then click **I accept**.
5. Type a password for the built-in administrator account and then click **Finish**.

### # Rename server and join domain

```PowerShell
Rename-Computer -NewName POLARIS-DEV -Restart
```

Wait for the VM to restart and then execute the following command to join the **TECHTOOLBOX **domain:

```PowerShell
Add-Computer -DomainName corp.technologytoolbox.com -Restart
```

## # Empty Active Directory Recycle Bin

```PowerShell
Get-ADObject -Filter 'isDeleted -eq $true -and Name -like "*DEL:*"' -IncludeDeletedObjects |
    Remove-ADObject -Confirm:$false
```

### Reference

**How to Empty the Active Directory Recycling Bin**\
From <[https://bdwyertech.net/2013/01/22/how-to-empty-the-active-directory-recycling-bin/](https://bdwyertech.net/2013/01/22/how-to-empty-the-active-directory-recycling-bin/)>

```PowerShell
cls
```

## # Get random machine name

```PowerShell
Function GetRandomMachineName([String] $prefix)
{
    $name = [System.IO.Path]::GetRandomFileName()

    $name = $name.ToUpper()

    $name = [System.IO.Path]::GetFileNameWithoutExtension($name)

    If ([String]::IsNullOrWhiteSpace($name) -eq $false)
    {
        $name = $prefix + $name
    }

    return $name
}
```

```PowerShell
cls
```

## # Configure networking

### # Configure network team

#### # Create NIC team

```PowerShell
$interfaceAlias = "Team 1"
$teamMembers = "Team 1A", "Team 1B"

New-NetLbfoTeam `
    -Name $interfaceAlias `
    -TeamMembers $teamMembers `
    -Confirm:$false
```

#### # Verify NIC team status

```PowerShell
Write-Host "Waiting for NIC team to initialize..."
Start-Sleep -Seconds 5

do {
    If (Get-NetLbfoTeam -Name $interfaceAlias | Where Status -eq "Up")
    {
        return
    }

    Write-Host "." -NoNewline
    Start-Sleep -Seconds 5
```

}  while (\$true)

> **Important**
> 
> Ensure the **Status** property of the network team is **Up**.

```PowerShell
cls
```

### # Configure static IP addresses

```PowerShell
$interfaceAlias = "Management"
```

#### # Disable DHCP and router discovery (to avoid getting IPv6 address and gateway)

```PowerShell
Set-NetIPInterface `
    -InterfaceAlias $interfaceAlias `
    -Dhcp Disabled `
    -RouterDiscovery Disabled
```

#### # Configure static IPv4 address

```PowerShell
$ipAddress = "10.1.10.8"

New-NetIPAddress `
    -InterfaceAlias $interfaceAlias `
    -IPAddress $ipAddress `
    -PrefixLength 24
```

#### # Configure static IPv6 address

**# Note:** Private IPv6 address range (fd87:77eb:097e:95a1::/64) generated by [http://simpledns.com/private-ipv6.aspx](http://simpledns.com/private-ipv6.aspx)

```PowerShell
$ipAddress = "fd87:77eb:097e:95a1::8"

New-NetIPAddress `
    -InterfaceAlias $interfaceAlias `
    -IPAddress $ipAddress `
    -PrefixLength 64
```

#### # Configure IPv4 DNS servers

```PowerShell
Set-DNSClientServerAddress `
    -InterfaceAlias $interfaceAlias `
    -ServerAddresses 192.168.10.103,192.168.10.104
```

#### # Configure IPv6 DNS servers

```PowerShell
Set-DnsClientServerAddress `
    -InterfaceAlias $interfaceAlias `
    -ServerAddresses 2603:300b:802:8900::103, 2603:300b:802:8900::104
```

```PowerShell
cls
```

## # Shutdown VMs on SOFS

```PowerShell
@('EXT-APP02A', 'EXT-WEB02A', 'EXT-WEB02B') |
    % {
        Get-SCVirtualMachine -Name $_ |
            ? { $_.Status -ne 'PowerOff' } |
            Stop-SCVirtualMachine -Shutdown
    }

$domainControllers = @(
    "CON-DC1",
    "CON-DC2",
    "EXT-DC04",
    "EXT-DC05",
    "FAB-DC01",
    "FAB-DC02")

Get-SCVirtualMachine |
    ? { $_.Location -like '\\TT-SOFS01*' } |
    ? { $_.Name -notin $domainControllers }  |
    ? { $_.Status -ne 'PowerOff' } |
    Stop-SCVirtualMachine -Shutdown

Get-SCVirtualMachine |
    ? { $_.Location -like '\\TT-SOFS01*' } |
    ? { $_.Name -in $domainControllers }  |
    ? { $_.Status -ne 'PowerOff' } |
    Stop-SCVirtualMachine -Shutdown
```

```PowerShell
cls
```

## # Start VMs on SOFS

```PowerShell
$domainControllers = @(
    "CON-DC1",
    "CON-DC2",
    "EXT-DC04",
    "EXT-DC05",
    "FAB-DC01",
    "FAB-DC02")

Get-SCVirtualMachine |
    ? { $_.Location -like '\\TT-SOFS01*' } |
    ? { $_.Name -in $domainControllers }  |
    ? { $_.Status -ne 'Running' } |
    % {
        Start-SCVirtualMachine $_

        Start-Sleep -Seconds 30
    }

Get-SCVirtualMachine |
    ? { $_.Location -like '\\TT-SOFS01*' } |
    ? { $_.Name -notin $domainControllers }  |
    ? { $_.Status -ne 'Running' } |
    % {
        Start-SCVirtualMachine $_

        Start-Sleep -Seconds 30
    }

@('EXT-APP02A', 'EXT-WEB02A', 'EXT-WEB02B') |
    % {
        Get-SCVirtualMachine -Name $_ |
            ? { $_.Status -ne 'Running' } |
        Start-SCVirtualMachine $_
    }
```

## Assign static IPv4 address from address pool managed by VMM

```PowerShell
$macAddressPool = Get-SCMACAddressPool -Name "Default MAC address pool"
$ipAddressPool = Get-SCStaticIPAddressPool -Name "Storage Address Pool"

$networkAdapter = Get-SCVirtualMachine EXT-SQL011 |
    Get-SCVirtualNetworkAdapter |
    ? { $_.SlotId -eq 1 }

$macAddress = Grant-SCMACAddress `
    -MACAddressPool $macAddressPool `
    -Description $vm.Name `
    -VirtualNetworkAdapter $networkAdapter

$ipAddress = Grant-SCIPAddress `
    -GrantToObjectType VirtualNetworkAdapter `
    -GrantToObjectID $networkAdapter.ID `
    -StaticIPAddressPool $ipAddressPool `
    -Description $vm.Name

Set-SCVirtualNetworkAdapter `
    -VirtualNetworkAdapter $networkAdapter `
    -MACAddressType Static `
    -MACAddress $macAddress `
    -IPv4AddressType Static `
    -IPv4Address $ipAddress
```

## Compare folders to ensure identical copies of files

```PowerShell
cd C:\NotBackedUp\Securitas-TT-TFS02\EmployeePortal

$leftFolderName = "Main"
$rightFolderName = "Lab4"

$leftFolderPath = "C:\NotBackedUp\Securitas-TT-TFS02\EmployeePortal\$leftFolderName"
$rightFolderPath = "C:\NotBackedUp\Securitas-TT-TFS02\EmployeePortal\Dev\$rightFolderName"

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

### Reference

**Compare contents of two folders using PowerShell Get-FileHash**\
From <[http://almoselhy.azurewebsites.net/2014/12/compare-contents-of-two-folders-using-powershell-get-filehash/](http://almoselhy.azurewebsites.net/2014/12/compare-contents-of-two-folders-using-powershell-get-filehash/)>

```PowerShell
cls
```

## # Delete empty folders

```PowerShell
dir C:\NotBackedUp -Directory -Recurse |
    where { -not $_.GetFiles("*","AllDirectories") } |
    del -Recurse -WhatIf
```

### Reference

**PowerShell Problem Solver: Delete Empty Folders with PowerShell**\
From <[https://www.petri.com/powershell-problem-solver-delete-empty-folders](https://www.petri.com/powershell-problem-solver-delete-empty-folders)>

```PowerShell
cls
```

## # Copy cmder configuration

```PowerShell
$computerName = "EXT-FOOBAR9.extranet.technologytoolbox.com"

$source = "C:\NotBackedUp\Public\Toolbox\cmder"
```

\$destination = "[\\\\\$computerName\\C`\$\\NotBackedUp\\Public\\Toolbox\\cmder](\\$computerName\C`$\NotBackedUp\Public\Toolbox\cmder)"

```Console
robocopy $source $destination /E /XD "git-for-windows"
```
