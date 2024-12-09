# Storage

Tuesday, May 19, 2015\
11:08 AM

## Configure VM storage

| Disk | Drive Letter | Volume Size | Allocation Unit Size | Volume Label |
| ---- | ------------ | ----------- | -------------------- | ------------ |
| 0    | C:           | 40 GB       | 4K                   | OSDisk       |
| 1    | D:           | 2 GB        | 64K                  | Data01       |

---

**FORGE**

### # Expand primary VHD for virtual machine

```PowerShell
$vmName = "POLARIS-DEV"

Stop-VM $vmName

$vhdPath = "C:\NotBackedUp\VMs\$vmName\Virtual Hard Disks\$vmName.vhdx"

Resize-VHD -Path $vhdPath -SizeBytes 40GB
```

### # Create Data01 VHD

```PowerShell
$vhdPath = "C:\NotBackedUp\VMs\$vmName\Virtual Hard Disks\$vmName" `
    + "_Data01.vhdx"

New-VHD -Path $vhdPath -SizeBytes 2GB
Add-VMHardDiskDrive -VMName $vmName -ControllerType SCSI -Path $vhdPath
```

---

### # Expand C: partition

```PowerShell
$maxSize = (Get-PartitionSupportedSize -DriveLetter C).SizeMax

Resize-Partition -DriveLetter C -Size $maxSize
```

### # Format Data01 drive

```PowerShell
Get-Disk 1 |
    Initialize-Disk -PartitionStyle MBR -PassThru |
    New-Partition -DriveLetter D -UseMaximumSize |
    Format-Volume `
        -AllocationUnitSize 64KB `
        -FileSystem NTFS `
        -NewFileSystemLabel "Data01" `
        -Confirm:$false
```
