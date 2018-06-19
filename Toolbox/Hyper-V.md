# Hyper-V

Tuesday, May 19, 2015
11:07 AM

## Manage VM checkpoints

---


**WOLVERINE**

```PowerShell
cls
```

### # Revert all VMs to most recent checkpoint

```PowerShell
Get-VM | % {
    Get-VMSnapshot -VM $_ |
    Where-Object { $_.SnapshotType -eq "Standard" } |
    Sort-Object CreationTime |
    Select-Object -Last 1 |
    Restore-VMSnapshot -Confirm:$false -Verbose
}
```

---


---


**FOOBAR8**

```PowerShell
cls
```

### # Checkpoint VM

```PowerShell
$checkpointName = "Baseline SharePoint Server 2013 configuration"
$vmHost = "STORM"
$vmName = "POLARIS-DEV"

Stop-VM -ComputerName $vmHost -Name $vmName

Checkpoint-VM `
    -ComputerName $vmHost `
    -Name $vmName `
    -SnapshotName $checkpointName

Start-VM -ComputerName $vmHost -Name $vmName
```

---


---


**FOOBAR8**

```PowerShell
cls
```

### # Delete VM checkpoint

```PowerShell
$checkpointName = "Baseline SharePoint Server 2013 configuration"
$vmHost = "STORM"
$vmName = "POLARIS-DEV"

Stop-VM -ComputerName $vmHost -Name $vmName

Remove-VMSnapshot -ComputerName $vmHost -VMName $vmName -Name $checkpointName

while (Get-VM -ComputerName $vmHost -Name $vmName | Where Status -eq "Merging disks") {
    Write-Host "." -NoNewline
    Start-Sleep -Seconds 5
}

Write-Host

Start-VM -ComputerName $vmHost -Name $vmName
```

---


## Misc

---


**WOLVERINE - Run as Administrator**

```PowerShell
cls
```

### # Backup VMs

```PowerShell
$exportPath = "Z:\Backup\VMs"

If ((Test-Path $exportPath) -eq $false)
{
    New-Item -ItemType Directory -Path $exportPath
}

Get-VM | Export-VM -Path $exportPath -Verbose
```

---


---


**FOOBAR8**

```PowerShell
cls
```

### # Remove media from virtual DVD drives

```PowerShell
$vmHosts = "BEAST", "FORGE", "STORM"

$vmHosts | ForEach-Object {
    $vmHost = $_

    Get-VM -ComputerName $vmHost |
        ForEach-Object {
            $vmName = $_.Name

            Set-VMDvdDrive -ComputerName $vmHost -VMName $vmName -Path $null
        }
    }
```

---


---


**FOOBAR8**

```PowerShell
cls
```

### # Replace VHD (and preserve permissions)

```PowerShell
cd 'D:\NotBackedUp\VMs\EXT-FOOBAR7\Virtual Hard Disks'

icacls .\EXT-FOOBAR7.vhdx /save AclFile

Copy-Item 'Z:\NotBackedUp\VMs\EXT-FOOBAR7\Virtual Hard Disks\EXT-FOOBAR7.vhdx' `
    'D:\NotBackedUp\VMs\EXT-FOOBAR7\Virtual Hard Disks' -Force

icacls . /restore AclFile
```

---


## VM startup configuration

---


**FOOBAR8**

```PowerShell
cls
```

### # List VM startup configuration

```PowerShell
#$vmHosts = "ANGEL", "BEAST", "FORGE", "ICEMAN", "ROGUE", "STORM"

$vmHosts = "BEAST", "FORGE", "STORM"

$vmHosts | ForEach-Object {
    $vmHost = $_

    Get-VM -ComputerName $vmHost |
        Select-Object `
            @{Expression={$vmHost};Label="Host"},
            Name,
            Path,
            AutomaticStartDelay |
        Sort-Object AutomaticStartDelay
    } |
    Format-Table -AutoSize
```

---


---


**FOOBAR8**

```PowerShell
cls
```

### # Set VM startup delay

```PowerShell
$vmHost = "STORM"
$vmName = "DEVOPS2012"

Set-VM -ComputerName $vmHost `
    -VMName $vmName `
    -AutomaticStartDelay 180
```

---


## # Expand D: (Data01) drive

---


**FOOBAR10 - Run as TECHTOOLBOX\\jjameson-admin**

```PowerShell
cls
```

### # Increase the size of "Data01" VHD

```PowerShell
$vmHost = "FORGE"
$vmName = "POLARIS-DEV"

Resize-VHD `
    -ComputerName $vmHost `
    -Path ("C:\NotBackedUp\VMs\$vmName\Virtual Hard Disks\" `
        + $vmName + "_Data01.vhdx") `
    -SizeBytes 5GB
```

---


```PowerShell
cls
```

### # Extend partition

```PowerShell
$driveLetter = "D"

$partition = Get-Partition -DriveLetter $driveLetter |
    where { $_.DiskNumber -ne $null }

$size = (Get-PartitionSupportedSize `
    -DiskNumber $partition.DiskNumber `
    -PartitionNumber $partition.PartitionNumber)

Resize-Partition `
    -DiskNumber $partition.DiskNumber `
    -PartitionNumber $partition.PartitionNumber `
    -Size $size.SizeMax
```

## Issue - Not enough free space to install patches using Windows Update

4 GB of free space, but unable to install **2017-07 Cumulative Update for Windows Server 2016 for x64-based Systems (KB4025339)**.

### Expand C: 

---


**FOOBAR8**

```PowerShell
cls
```

#### # Increase size of VHD

```PowerShell
$vmHost = "TT-HV05C"
$vmName = "TT-DPM02"

Stop-VM -ComputerName $vmHost -Name $vmName

Resize-VHD `
    -ComputerName $vmHost `
    -Path ("E:\NotBackedUp\VMs\$vmName\Virtual Hard Disks\" `
        + $vmName + ".vhdx") `
    -SizeBytes 50GB

Start-VM -ComputerName $vmHost -Name $vmName
```

---


```PowerShell
cls
```

#### # Extend partition

```PowerShell
$driveLetter = "C"

$partition = Get-Partition -DriveLetter $driveLetter |
    where { $_.DiskNumber -ne $null }

$size = (Get-PartitionSupportedSize `
    -DiskNumber $partition.DiskNumber `
    -PartitionNumber $partition.PartitionNumber)

Resize-Partition `
    -DiskNumber $partition.DiskNumber `
    -PartitionNumber $partition.PartitionNumber `
    -Size $size.SizeMax
```

## Move virtual machines

---


**TT-HV02C**

```PowerShell
cls
```

### # Move virtual machines (including storage)

```PowerShell
Function MoveVirtualMachine($vmName, $destHost, $destStoragePath, $startVM)
{
    Write-Host "Moving virtual machine ($vmName)..."

    Stop-VM -Name $vmName

    Move-VM `
        -Name $vmName `
        -DestinationHost $destHost `
        -IncludeStorage `
        -DestinationStoragePath $destStoragePath

    If ($startVM -eq $true)
    {
        Start-VM -ComputerName $destHost -Name $vmName
    }
}

#$virtualMachines = Get-VM *A |
#    ? { $_.Name -notin @('EXT-APP01A', 'EXT-APP02A') } |
#    select -ExpandProperty Name

$virtualMachines = @(
    'FAB-ADFS02',
    'HAVOK-TEST',
    'POLARIS-TEST',
    'TT-MGMT01',
    'TT-WIN10-TEST1',
    'WIN7-TEST1',
    'WIN7-TEST2',
    'WIN7-TEST3',
    'WIN8-TEST1',
    'XAVIER1',
    'EXT-FOOBAR8')

$destHost = "TT-HV02A"
$startVM = $true

$virtualMachines |
    % {
        $vmName = $_

        $destStoragePath = "E:\NotBackedUp\VMs\$vmName"

        MoveVirtualMachine $vmName $destHost $destStoragePath $startVM
    }
```

---


---


**TT-VMM01A**

```PowerShell
cls
```

### # Live migrate virtual machine (including storage)

```PowerShell
$vmHost = Get-SCVMHost -ComputerName "TT-HV02A.corp.technologytoolbox.com"

$vm = Get-SCVirtualMachine -Name "EXT-APP02A"

Move-SCVirtualMachine `
    -VM $vm `
    -VMHost $vmHost `
    -Path "D:\NotBackedUp\VMs" `
    -UseDiffDiskOptimization
```

---


---


**TT-VMM01A**

```PowerShell
cls
```

### # Make virtual machines highly available

```PowerShell
@('CYCLOPS-TEST', 'EXT-WAC02A', 'FAB-WEB01', 'POLARIS-TEST') |
    % {
        $vm = Get-SCVirtualMachine -Name $_
        $vmHost = $vm.VMHost

        Move-SCVirtualMachine `
            -VM $vm `
            -VMHost $vmHost `
            -HighlyAvailable $true `
            -Path "\\TT-SOFS01.corp.technologytoolbox.com\VM-Storage-Silver" `
            -UseDiffDiskOptimization

    }
```

---


## Create VM

---


**FOOBAR10 - Run as TECHTOOLBOX\\jjameson-admin**

```PowerShell
cls
```

### # Create virtual machine

```PowerShell
$vmHost = "TT-HV02A"
$vmName = "EXT-WAP02A"
$vmPath = "E:\NotBackedUp\VMs"
$vhdPath = "$vmPath\$vmName\Virtual Hard Disks\$vmName.vhdx"
$sysPrepedImage = "\\TT-FS01\VM-Library\VHDs\WS2016-Std.vhdx"

$vhdUncPath = "\\$vmHost\" + $vhdPath.Replace(":", "`$")

New-VM `
    -ComputerName $vmHost `
    -Name $vmName `
    -Path $vmPath `
    -NewVHDPath $vhdPath `
    -NewVHDSizeBytes 32GB `
    -MemoryStartupBytes 2GB `
    -SwitchName "Embedded Team Switch"

Copy-Item $sysPrepedImage $vhdUncPath

Set-VM `
    -ComputerName $vmHost `
    -Name $vmName `
    -ProcessorCount 2 `
    -DynamicMemory `
    -MemoryMinimumBytes 2GB `
    -MemoryMaximumBytes 4GB

Start-VM -ComputerName $vmHost -Name $vmName
```

---


```PowerShell
cls
```

### # Create virtual machines

```PowerShell
Function GetRandomMachineName([String] $prefix){    $name = [System.IO.Path]::GetRandomFileName()
    $name = $name.ToUpper()
    $name = [System.IO.Path]::GetFileNameWithoutExtension($name)

    If ([String]::IsNullOrWhiteSpace($name) -eq $false)    {        $name = $prefix + $name    }

    return $name}

1..2 |
% {
$vmName = GetRandomMachineName "VM-"
$vmPath = "\\TT-SOFS01\VM-Storage-Silver"
$vhdFolderPath = "$vmPath\$vmName\Virtual Hard Disks"
$vhdPath = "$vhdFolderPath\$vmName.vhdx"

$sysPrepedImage = "\\TT-FS01\VM-Library\VHDs\WS2016-Std.vhdx"

New-VM `
    -Name $vmName `
    -Path $vmPath `
    -NewVHDPath $vhdPath `
    -NewVHDSizeBytes 32GB `
    -MemoryStartupBytes 2GB `
    -SwitchName "Embedded Team Switch"

Copy-Item $sysPrepedImage $vhdPath

Set-VM `
    -Name $vmName `
    -ProcessorCount 2 `
    -DynamicMemory `
    -MemoryMaximumBytes 4GB

Start-VM -Name $vmName
}
```
