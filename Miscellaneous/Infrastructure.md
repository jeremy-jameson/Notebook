# Infrastructure

Wednesday, April 08, 2015\
1:41 PM

```PowerShell
cls
```

## # Set password for local Administrator account

```PowerShell
$adminUser = [ADSI] "WinNT://./Administrator,User"
$adminUser.SetPassword("{password}")
```

## # Set MaxPatchCacheSize to 0

```PowerShell
reg add HKLM\Software\Policies\Microsoft\Windows\Installer /v MaxPatchCacheSize /t REG_DWORD /d 0 /f
```

## # Copy Toolbox content

```PowerShell
net use \\ICEMAN\ipc$ /USER:TECHTOOLBOX\jjameson

robocopy \\ICEMAN\Public\Toolbox C:\NotBackedUp\Public\Toolbox /E
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

## # Join domain

```PowerShell
Add-Computer -DomainName corp.technologytoolbox.com -Credential (Get-Credential)

Restart-Computer
```

## # Increase the size of VHD

```PowerShell
$vmName = "FOOBAR8"

Stop-VM $vmName

$vhdPath = "C:\NotBackedUp\VMs\$vmName\$vmName.vhdx"

Resize-VHD -Path $vhdPath -SizeBytes 40GB

Start-VM $vmName
```

## # Expand C: drive

```PowerShell
$size = (Get-PartitionSupportedSize -DiskNumber 0 -PartitionNumber 2)
Resize-Partition -DiskNumber 0 -PartitionNumber 2 -Size $size.SizeMax
```

## Install Microsoft Office Professional Plus 2010 (x86)

## Install Microsoft SharePoint Designer 2010 (x86)

## Install Microsoft Visio Premium 2010 (x86)

## # Install Adobe Reader 8.3

```PowerShell
msiexec /i "\\ICEMAN\Products\Adobe\AdbeRdr830_en_US.msi" /q

msiexec /update "\\ICEMAN\Products\Adobe\AdbeRdrUpd831_all_incr.msp" /q
```

## # Install Mozilla Firefox 36.0

## # Install Mozilla Thunderbird 31.3

"[\\\\ICEMAN\\Products\\Mozilla\\Thunderbird\\Thunderbird](\\ICEMAN\Products\Mozilla\Thunderbird\Thunderbird) Setup 31.3.0.exe" -ms

## # Install Google Chrome

```PowerShell
msiexec /i "\\ICEMAN\Products\Google\Chrome\googlechromestandaloneenterprise.msi" /q
```

## Install Remote Server Administration Tools for Windows 7 SP1

## Install Microsoft Security Essentials

## Install updates

```PowerShell
# Delete C:\Windows\SoftwareDistribution folder

Stop-Service wuauserv

Remove-Item C:\Windows\SoftwareDistribution -Recurse

Restart-Computer
```

Configure server settings

On the **Settings** page:

1. Ensure the following default values are selected:
   1. **Country or region: United States**
   2. **App language: English (United States)**
   3. **Keyboard layout: US**
2. Click **Next**.
3. Type the product key and then click **Next**.
4. Review the software license terms and then click **I accept**.
5. Type a password for the built-in administrator account and then click **Finish**.

## # Rename computer and join domain

```PowerShell
$computerName = "MIMIC"

Rename-Computer -NewName $computerName -Restart
```

Wait for the VM to restart and then execute the following command to join the **TECHTOOLBOX **domain:

```PowerShell
Add-Computer -DomainName corp.technologytoolbox.com -Restart
```

## # Enter a product key and activate Windows

```PowerShell
slmgr /ipk {product key}
```

**Note:** Click **OK** to dismiss dialog box.

```Console
slmgr /ato
```

## # Windows Features

# Default features installed with Windows Server 2012 R2 Standard

```PowerShell
Get-WindowsFeature | ? {  $_.Installed } | select Name
```
